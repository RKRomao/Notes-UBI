---
disciplina: Redes
tags:
  - redes
  - seguranca
---

# maxsec

> **Nota de rigor:** o termo “maxsec” aparece na lista da disciplina sem expansão. Não corresponde a um standard conhecido com esse nome exato. Há duas leituras plausíveis:
> **(a)** é um *typo* de **MACsec** (IEEE 802.1AE) — muito provável numa lista com MPLS, LISP e RPKI-RTR;
> **(b)** é uma designação interna da cadeira para um mecanismo de “segurança máxima” em enlaces restritos.
> Abaixo detalha-se **MACsec**, que é a interpretação com standard verificável. Confirmar com as notas de aula.

# MACsec (IEEE 802.1AE) — segurança na camada 2

## O que é e que problema resolve
O **MACsec** protege tramas Ethernet **ponto-a-ponto** na camada 2: autenticidade, integridade e (opcionalmente) confidencialidade. Resolve o facto de, num domínio L2 partilhado (edifício, campus, operador), qualquer porta poder ouvir ou injetar tráfego — o IPsec só protege acima do IP e não evita ataques L2 como *MAC spoofing* ou *ARP poisoning*.

Como é **hop-by-hop**, cada salto desencripta e reencripta; não é fim-a-fim (para isso há IPsec/TLS).

## Formato da trama
O MACsec insere um **SecTAG** entre o endereço MAC de origem e o EtherType original, e acrescenta um **ICV** no fim:

```
| DA | SA | SecTAG | [EtherType original] | payload | ICV |
                    (8–16 bytes)                    (8/16 bytes)
```

Campos do **SecTAG**:
- **TAG Control Information (TCI)** — bits de versão, *ES/SC*, *SCB*, *E* (confidencialidade), *C* (é SCI válido), *AN* (association number).
- **SL** — *short length* (opcional).
- **PN** — **Packet Number** (32 ou 64 bits), anti-replay.
- **SCI** — **Secure Channel Identifier** (MAC + porta), opcional.

## Conceitos
- **CA (Connectivity Association)** — grupo de sistemas que partilham chaves; identificado por **CKN** (nome) e **CAK** (chave).
- **SA (Secure Association)** — chave simétrica (SAK) e número de associação (AN) usados para cifrar; rodam periodicamente.
- **SC (Secure Channel)** — canal unidirecional entre dois pontos, identificado por **SCI**.
- **SAK** — chave de sessão derivada do CAK.

## Cifra e integridade
- **AES-GCM-128** ou **AES-GCM-256** (também AES-GCM-XPN para PN de 64 bits).
- O **ICV** garante integridade e autenticidade; a confidencialidade é opcional (bit *E*).
- **Anti-replay** pelo PN: tramas com PN repetido ou demasiado antigo são descartadas.

## Gestão de chaves — MKA
O **MKA (MACsec Key Agreement, IEEE 802.1X-2010)** negocia CA e SA:
1. Descoberta de peers por **EAPOL-MKA**.
2. Eleição de **Key Server** (normalmente quem tem prioridade).
3. Distribuição da **SAK** cifrada com o CAK.
4. Rotação de SAK (por tempo ou número de pacotes).

Autenticação inicial pode usar **EAP** (802.1X) ou chave pré-partilhada (PSK).

## Vantagens
- Proteção **L2 transparente** para IP e protocolos superiores.
- Baixa latência (hardware ASIC), alto débito (10/40/100 G).
- Não exige mudanças na aplicação nem no endereçamento IP.
- Anti-replay e deteção de intrusão L2.

## Limitações
- **Hop-by-hop** — cada switch intermédio vê o tráfego em claro.
- Só Ethernet (não Wi-Fi; para Wi-Fi há 802.11i/WPA3).
- Requer hardware/MACsec-capable e gestão de chaves (MKA).
- Overhead de ~16–32 bytes por trama.

## Comparação
| | MACsec | IPsec | TLS |
|---|---|---|---|
| Camada | 2 | 3 | 4–7 |
| Âmbito | hop-by-hop | fim-a-fim (túnel/transporte) | fim-a-fim |
| Transparência | total p/ IP | exige config | exige app |
| Caso típico | campus, DC, operador | VPN | Web |

## Casos de uso
- Ligação switch–switch e switch–router em campus/DC.
- Backhaul de operador (alternativa a IPsec em alguns pontos).
- Proteção de tráfego L2 em redes críticas (energia, transporte).

## Pontos-chave para exame
1. MACsec = segurança **L2 ponto-a-ponto** (802.1AE).
2. **SecTAG + ICV**, AES-GCM, **PN anti-replay**.
3. Chaves geridas por **MKA**; CAK → SAK.
4. **Hop-by-hop**, não fim-a-fim.
5. Overhead e necessidade de hardware são os custos.

## Referências
- IEEE 802.1AE (MACsec), IEEE 802.1X-2010 (MKA), RFC 7621? (não) — ver IEEE.
