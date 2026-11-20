> [!info] Definição
> **Phishing** é enganar a **pessoa** para que ela própria entregue credenciais ou execute uma ação perigosa. **Spoofing de interface** é mostrar-lhe um ecrã ou uma notificação **falsa**, que parece vir do sistema verdadeiro. Estes ataques exploram a confiança humana, não uma falha matemática.

## Em linguagem simples

É como colocar à frente de um multibanco uma máquina que parece igual à verdadeira. Introduzes o teu cartão e o PIN, e a máquina falsa guarda-os. A tecnologia do banco pode estar perfeita — o engano foi contigo.

## Como o ataque funciona

1. O atacante cria uma app, página ou notificação muito parecida com a legítima.
2. Pede o PIN, a palavra-passe ou a aprovação de um pedido.
3. A vítima introduz os dados ou aprova a operação.
4. O atacante usa esses dados para se autenticar ou assinar em nome dela.

## Exemplo no BatEvidence

Um agente no terreno recebe uma notificação "BatEvidence: sessão expirada, introduza o PIN do certificado". Introduz o PIN numa app falsa, que o guarda e o usa para desbloquear o certificado verdadeiro. Outra variante: o atacante devolve ao cliente dados não verificados e a interface mostra-os como se fossem autênticos.

## Porque é perigoso

- Contorna mecanismos técnicos fortes, porque **o utilizador colabora**.
- Pode comprometer **chaves privadas** e **identidades**.
- É difícil de detetar apenas com criptografia.

## Como o BatEvidence se protege

- **App assinada e atualizações verificadas**, com certificado *pinned* ao backend.
- **PIN nunca sai do Secure Enclave/TPM** (RNS10); a app nunca o envia para o servidor.
- **Verificação de assinaturas antes de mostrar dados** (RF20/RNS09): a interface só apresenta o que foi validado.
- **Confirmação de operações sensíveis** e alertas fora de banda.
- **Formação dos utilizadores** e procedimentos de reporte.

## Como testar

- **TC-RF20-01**, **TC-RNS10-01** e **TC-RF11-01** em [[Intregavel 4]] — verificar que dados não validados não são apresentados e que o PIN não é exportável.
- Vetores **v15/v16** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Falsificação de Assinatura]], [[Man-in-the-Middle (MitM)]], [[Exfiltração de Dados e Chaves]], [[Bat (couf couf) Evidence A Wayne]]
