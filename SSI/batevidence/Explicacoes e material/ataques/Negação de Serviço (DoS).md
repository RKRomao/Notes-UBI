> [!info] Definição
> Um ataque de **negação de serviço** (_Denial of Service_, DoS) tenta **tornar um serviço indisponível** para os utilizadores legítimos. Em vez de roubar ou alterar dados, o atacante esgota recursos — rede, CPU, memória, ligações — até o sistema deixar de responder.

## Em linguagem simples

É como alguém que bloqueia a porta da loja com carrinhos de compras. Não leva nada, não arromba nada: simplesmente **ninguém consegue entrar**. A loja está intacta, mas inutilizável.

## Como o ataque funciona

1. O atacante envia um grande volume de pedidos, ou pedidos especialmente pesados.
2. O servidor gasta CPU, memória ou largura de banda a respondê-los.
3. Os utilizadores legítimos ficam sem capacidade de resposta.
4. Em versões distribuídas (**DDoS**), muitos dispositivos atacam ao mesmo tempo.

## Exemplo no BatEvidence

Um atacante inunda o *endpoint* de pedidos de consulta. O agente no terreno deixa de conseguir depositar provas ou consultar casos. Numa ligação satélite, a indisponibilidade pode ser agravada por **jamming** ou por latência extrema.

## Porque é perigoso

- Impede **depósitos no terreno**, precisamente quando são urgentes.
- Pode esconder outros ataques (o ruído serve de distração).
- Afeta a **disponibilidade** e a confiança operacional.

## Como o BatEvidence se protege

- **Rate limiting e quotas** por identidade e por IP.
- **Firewalls e segmentação** (RNS18) para filtrar tráfego.
- **Redundância** com servidor principal, *hot backup* e *cold backup* (RNS17).
- **Buffer local no terreno:** o agente pode continuar a recolher e assinar, entregando quando a rede voltar.
- **Monitorização e alertas** de anomalias de volume.
- **Envelope efémero** não resolve DoS por si só — a mitigação é de perímetro (RNS13 documenta esta limitação).

## Como testar

- **TC-RNS17-01** em [[Intregavel 4]] — failover durante um depósito.
- **TC-RNS22-01** — comportamento sob latência elevada e perda de pacotes.
- Vetor **v2** (link satélite) em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Man-in-the-Middle (MitM)]], [[Bat (couf couf) Evidence A Wayne]]
