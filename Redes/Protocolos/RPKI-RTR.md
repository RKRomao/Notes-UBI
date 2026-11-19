---
disciplina: Redes
tags:
  - redes
  - routing
  - seguranca
---

# RPKI-RTR — RPKI to Router Protocol

## O que é e que problema resolve
O **RPKI-RTR** (RFC 8210, v1; RFC 6810, v0) é o protocolo que transporta **dados de validação RPKI** de um **cache RPKI** para os **routers**, para que estes possam fazer **ROV** (*Route Origin Validation*) sobre anúncios BGP.

Problema: o BGP confia cegamente em quem anuncia um prefixo — daí *hijacking* e *route leaks*. A RPKI resolve com certificados e **ROAs** (*Route Origin Authorizations*): “o AS X está autorizado a anunciar o prefixo P com máscara até /L”. O router precisa de saber, em tempo real, que ROAs existem. Não pode fazer validação criptográfica pesada nem falar o protocolo de repositórios (rsync/RRDP) — daí o RTR: um protocolo **simples e incremental** entre cache e router.

## Arquitetura
```
Repositórios RPKI (rsync/RRDP)
        │
   Validator (Routinator, OctoRPKI, ...)
        │  RPKI-RTR (TCP 323)
   ┌────┴────┬────────┐
 Router A  Router B  Router C
```

- **Validator/cache** — recolhe objetos, valida assinaturas, produz a lista de prefixos válidos.
- **Router** — consome a lista via RTR e aplica política BGP.
- O router **não valida criptografia**; confia no cache (relação de confiança explícita, normalmente com **SSH/TLS** ou rede dedicada).

## Funcionamento
1. O router abre **TCP porta 323** para o cache.
2. Envia **Reset Query** (pede a tabela completa) ou **Serial Query** (pede só o que mudou desde a versão N).
3. O cache responde com **Cache Response** + uma sequência de **Prefix PDUs** + **End of Data** (com o novo serial).
4. Quando há alterações, o cache envia **Serial Notify** — o router faz *Serial Query*.
5. Se o serial for antigo/inválido, o router faz **Reset Query** (tabela completa).

É **stateful e incremental**: só se transferem deltas.

## PDUs
Todos começam com header `version | type | session_id | length`.

| PDU | Direção | Função |
|---|---|---|
| **Serial Notify** | cache→router | “há nova versão N” |
| **Serial Query** | router→cache | “dá-me o delta desde N” |
| **Reset Query** | router→cache | “dá-me tudo” |
| **Cache Response** | cache→router | início da resposta |
| **IPv4 Prefix** | cache→router | prefixo IPv4 + ASN + maxLength |
| **IPv6 Prefix** | cache→router | idem IPv6 |
| **End of Data** | cache→router | fim + serial atual |
| **Cache Reset** | cache→router | “não consigo servir esse serial; reinicia” |
| **Error Report** | ambos | erro de protocolo/aplicação |

## Prefix PDU em detalhe
```
flags(1) | prefixLength(1) | maxLength(1) | zero(1) | prefix | ASN(4)
```
- **flags** bit 0: **1 = announce**, **0 = withdraw**.
- **prefixLength** — máscara do prefixo.
- **maxLength** — maior máscara autorizada (evita anúncios de subprefixos não autorizados).
- **ASN** — origem autorizada.

## Estados de validação (RFC 6811)
Para cada rota BGP, o router classifica:
- **Valid** — existe ROA que cobre prefixo e origem.
- **Invalid** — existe ROA para o prefixo mas a origem/máscara não corresponde (ou maxLength excedido).
- **NotFound** — não há ROA.

Política típica (RFC 7115 / MANRS):
- **Invalid** → preferência baixa ou *discard*;
- **Valid/NotFound** → normal.

## Sessão e robustez
- **Session ID** identifica a instância do cache; muda → router faz reset.
- **Serial number** versiona a tabela.
- *Timeouts* e reconexão; o router mantém a última tabela conhecida se o cache cair.
- Autenticação: **TCP-AO**, **SSH**, **IPsec** ou TLS (perfis de implementação).

## Segurança
- O RTR em si não autentica os ROAs — a confiança vem da cadeia RPKI validada no cache.
- Riscos: cache comprometido, *spoofing* de sessão (mitigar com TCP-AO/TLS), *downgrade*.
- **ROV** reduz *hijacking* por origem, mas não protege o caminho (para isso há BGPsec/ASPA).

## Comparação
| | RPKI-RTR | BGP | IRR |
|---|---|---|---|
| Transporte | TCP 323 | TCP 179 | whois/HTTP |
| Conteúdo | ROAs validadas | rotas | registos manuais |
| Validação | criptográfica (no cache) | nenhuma | nenhuma |
| Uso | ROV | encaminhamento | filtros |

## Casos de uso
- Operadores que fazem **ROV** em larga escala.
- IXPs com *route servers* a validar origem.
- Redes empresariais que filtram *route leaks*.

## Pontos-chave para exame
1. RPKI-RTR liga **cache RPKI → router**, TCP **323**.
2. Protocolo **incremental** (serial) com *Reset/Serial Query*.
3. **Prefix PDU**: announce/withdraw + prefixLength + maxLength + ASN.
4. Permite **ROV**: Valid / Invalid / NotFound.
5. Não substitui BGP — alimenta a sua política.

## Referências
- RFC 8210 (RPKI-RTR v1), RFC 6810 (v0), RFC 6480 (RPKI), RFC 6811 (validação de origem), RFC 7115 (BGP operations).
