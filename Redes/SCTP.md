---
disciplina: Redes
tags:
  - redes
  - transporte
  - sctp
---
# SCTP (Stream Control Transmission Protocol)

O SCTP destaca-se por ser **orientado a mensagens** e por oferecer duas funcionalidades cruciais:

- **Multi-homing** — permite que os dois extremos da conexão tenham múltiplos endereços IP, garantindo redundância.
- **Multi-streaming** — permite transmitir vários fluxos de dados independentes dentro da mesma conexão.

## Problema que resolve

Resolve duas grandes limitações dos protocolos tradicionais:

- **Head-of-Line Blocking (TCP):** no TCP, se um pacote é perdido, todo o fluxo de dados para até que ele seja retransmitido. O *multi-streaming* do SCTP resolve isso, pois a perda de um pacote num fluxo não bloqueia a entrega dos outros fluxos.
- **Falta de redundância nativa:** o TCP estabelece ligações entre um único IP de origem e um único IP de destino. Se essa interface de rede falhar, a ligação cai. O UDP, por sua vez, não tem garantia de entrega nem controlo de congestionamento.

## Contexto

Foi desenvolvido no contexto crítico das redes de telecomunicações. À medida que as operadoras começaram a migrar a infraestrutura telefónica analógica tradicional para redes baseadas em pacotes (IP), precisavam de um protocolo de transporte que garantisse a mesma fiabilidade, baixíssima latência e tolerância a falhas que as redes telefónicas dedicadas possuíam.

## Finalidade

Garantir a entrega fiável, ordenada e segura de mensagens de sinalização (dados de controlo de chamadas e tráfego de rede) sobre redes IP, mantendo a conexão estabelecida e operacional mesmo em caso de falha de uma interface de rede ou caminho de roteamento.

## História

### Origem

Foi desenvolvido inicialmente pelo grupo de trabalho **SIGTRAN** (*Signaling Transport*) da IETF. O objetivo primário era transportar as mensagens de sinalização telefónica do protocolo **SS7** (*Signaling System 7*) — o sistema que controla as chamadas telefónicas e mensagens SMS a nível mundial — através de redes de Internet convencionais.

### Marco histórico

O SCTP foi oficializado como protocolo padrão da Internet em **outubro de 2000**, com a publicação da **RFC 2960** pela IETF (mais tarde atualizada e substituída pela **RFC 4960**). Isto elevou o SCTP ao estatuto de **terceiro grande protocolo de transporte** da pilha TCP/IP.

### Curiosidade documentada

Ao contrário do TCP, que utiliza um estabelecimento de ligação de 3 vias (*3-way handshake*), o SCTP utiliza um aperto de mão de **4 vias** com troca de *cookies* (INIT → INIT-ACK → COOKIE-ECHO → COOKIE-ACK). Esta alteração foi desenhada especificamente para tornar o SCTP imune a ataques de negação de serviço (DoS) do tipo **SYN flood**, não alocando recursos no servidor até que a identidade e a intenção do cliente sejam totalmente confirmadas criptograficamente.

## Exemplos de utilização

- **Redes 4G e 5G:** o SCTP é o protocolo padrão para a sinalização entre as antenas (eNodeB / gNodeB) e o núcleo da rede da operadora (*Core Network*), gerindo a mobilidade dos telemóveis.
- **WebRTC:** é o protocolo responsável pelos *Data Channels*, permitindo comunicação *peer-to-peer* diretamente entre navegadores para partilha de ficheiros ou jogos multiplayer em tempo real, aproveitando a sua fiabilidade e capacidade de evitar atrasos no fluxo.

## Ligações

- [[Camada de Transporte]]
- [[Pilha TCP-IP]]
- [[QUIC]] — também resolve o HOL com fluxos independentes, mas sobre UDP
