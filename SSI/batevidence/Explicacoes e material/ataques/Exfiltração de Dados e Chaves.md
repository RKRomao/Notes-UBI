> [!info] Definição
> **Exfiltração** é a **saída não autorizada de dados ou chaves** de dentro do sistema. O atacante pode tirar provas, notas, a chave AES de um caso, a chave privada de um agente ou o próprio segredo de Shamir. Muitas vezes não altera nada: apenas **copia**.

## Em linguagem simples

É como alguém que entra numa casa, não leva a televisão, mas **fotografa todos os documentos** e sai sem deixar marcas. A casa fica igual; o segredo já não está só lá dentro.

## Como o ataque funciona

1. O atacante obtém execução de código, acesso a um backup, a um log ou à RAM do processo.
2. Procura segredos: chaves, *tokens*, variáveis de ambiente, ficheiros de configuração, *crash dumps*.
3. Copia para fora por um canal permitido (HTTPS, DNS, e-mail, USB).
4. Em muitos casos, **apaga os rastos** ou usa canais pouco monitorizados.

## Exemplo no BatEvidence

A chave AES do caso é reconstruída em RAM durante uma consulta. Se ficasse em *swap*, num log ou num *crash dump*, um atacante com acesso ao servidor podia copiá-la e decifrar a prova sem passar pelo quórum.

O mesmo vale para as chaves privadas dos agentes: se estiverem no disco do telemóvel em vez do TPM/Secure Enclave, um malware pode extraí-las.

## Porque é perigoso

- Quebra a **confidencialidade** de forma duradoura (a cópia fica fora do controlo).
- Pode permitir **decifra futura** de provas antigas.
- Compromete **identidades** e permite impersonação.

## Como o BatEvidence se protege

- **Chaves em hardware (RNS10):** TPM 2.0 / Secure Enclave, não exportáveis.
- **Zeroização (RNS03/RNS04):** a chave AES e o segredo ECDH são apagados da RAM após uso.
- **Sem segredos em logs:** a auditoria guarda apenas `request_id` e hash do nonce (RF14).
- **Enclave isolado (RNS21):** as chaves nunca passam pelo backend.
- **Cifra em repouso (RNS02)** e **WORM cego:** o binário é inútil sem a chave do caso.
- **Menor privilégio (RNS19)** e **segmentação de rede (RNS18)** limitam o alcance.

## Como testar

- **TC-RNS03-01**, **TC-RNS04-01**, **TC-RNS10-01** e **TC-RNS21-01** em [[Intregavel 4]].
- Vetores **v1a/v11** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Movimento Lateral]], [[Adulteração de Registos (Tampering)]], [[Bat (couf couf) Evidence A Wayne]]
