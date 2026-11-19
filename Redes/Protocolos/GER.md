---
disciplina: Redes
tags:
  - redes
---

# GER

> **Nota de rigor:** “GER” está na lista da disciplina sem expansão e não corresponde, com esta sigla, a um standard de redes amplamente conhecido. Não vou inventar conteúdo. Abaixo ficam as hipóteses e um guião para completar a partir das notas de aula.

## Hipóteses plausíveis
- **GER** como abreviatura usada na cadeira (ex.: “Gestão de Redes”/“Gerenciamento de Rede” — em PT-BR *Gerenciamento*; ou “Gateway de Rede”).
- **GER** = **GMPLS**/**GER**? — sigla de um protocolo de transporte visto em aula.
- Erro de transcrição de outra sigla da lista (ex.: **GRE** — *Generic Routing Encapsulation*, ou **GÉANT**).

## Se for GRE (Generic Routing Encapsulation)
Caso seja **GRE** (RFC 2784/2890), o resumo seria:
- Túnel L3 que encapsula qualquer protocolo sobre IP.
- Cabeçalho de 4 bytes mínimo + opções (checksum, key, sequence).
- Base de muitas VPNs (GRE + IPsec), de *overlay* em SD-WAN e de mobilidade.
- Não cifra nem autentica por si — precisa de IPsec.

## Guião para completar
1. Confirmar a expansão exata com o docente / slides.
2. Preencher: **problema que resolve**, **camada**, **formato de PDU**, **mecanismos**, **segurança**, **comparação**, **casos de uso**.
3. Ligar a [[Arquiteturas Avançadas Redes (AAR)]].

## Referências
- (a confirmar)
