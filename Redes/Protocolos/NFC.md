---
disciplina: Redes
tags:
  - redes
  - nfc
  - sem-fios
---
# NFC (Near Field Communication)

Tecnologia de comunicação **sem fios de muito curto alcance**, que funciona por **acoplamento indutivo** à frequência de **13,56 MHz**. É uma evolução do RFID de alta frequência (HF), normalizada pelo **ISO/IEC 18092** (NFCIP-1) e pelo **ISO/IEC 14443**, e promovida pelo **NFC Forum**.

## Funcionalidade
- **Frequência:** 13,56 MHz (banda ISM, não licenciada).
- **Alcance:** tipicamente **≤ 4 cm**; na prática nunca excede ~10 cm.
- **Débitos:** **106, 212 ou 424 kbit/s** (ISO/IEC 18092).
- **Acoplamento indutivo:** o iniciador gera um campo magnético; o alvo passivo é **alimentado por esse campo**, sem bateria.
- **Dois modos de comunicação:**
  - **Passivo** — um dispositivo gera o campo, o outro (tag/cartão) responde por modulação da carga.
  - **Ativo** — ambos os dispositivos geram o seu próprio campo, alternando quem transmite.
- **Três modos de operação:**
  1. **Emulação de cartão** — o telemóvel comporta-se como um cartão contactless (pagamentos, acessos).
  2. **Leitor/escritor** — lê e escreve tags NFC (cartazes inteligentes, configuração).
  3. **Peer-to-peer** — dois dispositivos trocam dados diretamente (NDEF, LLCP).

| Característica     | Valor                                                   |
| ------------------ | ------------------------------------------------------- |
| Frequência         | 13,56 MHz                                               |
| Distância          | ~4 cm (máx. ~10 cm)                                     |
| Débito             | 106 / 212 / 424 kbit/s                                  |
| Alimentação da tag | Campo do leitor (passivo)                               |
| Normas             | ISO/IEC 18092, ISO/IEC 14443 A/B, JIS X 6319-4 (FeliCa) |

## Problema que resolve

O NFC resolve o atrito das interações de proximidade:

- **Substitui códigos de barras e QR codes** por uma leitura sem linha de vista nem foco ótico.
- **Elimina o emparelhamento manual** de Bluetooth e Wi-Fi: um toque troca as credenciais e liga os dispositivos.
- **Torna os pagamentos e acessos mais rápidos** do que inserir um cartão ou introduzir um PIN em cada utilização.
- **Permite tags sem bateria**, alimentadas pelo leitor — ideais para cartazes, bilhetes e inventário.
- **Usa a proximidade como fator de segurança**: o alcance de poucos centímetros dificulta a interceção à distância (embora não elimine ataques de *relay*).

## Contexto

O NFC está presente em:

- **Pagamentos contactless** (cartões EMV e telemóveis com *secure element* / tokenização);
- **Bilhética de transportes** e cartões de acesso a edifícios;
- **Smart posters** e museus (um toque abre uma página ou áudio);
- **Emparelhamento rápido** de auscultadores, colunas e câmaras;
- **Aprovisionamento de IoT** (Wi-Fi, credenciais, configuração inicial);
- **Cartões de saúde, passaportes** e autenticação de documentos.

## Finalidade

Permitir a troca **simples, intuitiva e segura de pequenas quantidades de dados** entre dois dispositivos próximos, com configuração mínima e sem infraestrutura própria — funcionando muitas vezes como **ponte** para outra tecnologia (por exemplo, passar as credenciais de Wi-Fi ou Bluetooth).

## História

### Origem

As raízes estão no **RFID HF** a 13,56 MHz e nas tecnologias de cartões sem contacto **MIFARE** (Philips/NXP) e **FeliCa** (Sony). Em **2002**, Philips e Sony acordaram desenvolver uma especificação comum, submetendo-a à ECMA e à ISO.

### Marco histórico

| Ano      | Marco                                                       |
| -------- | ----------------------------------------------------------- |
| **2002** | Philips e Sony acordam a especificação NFC                  |
| **2003** | Aprovação como **ISO/IEC 18092** (NFCIP-1), também ECMA-340 |
| **2004** | Fundação do **NFC Forum** (Nokia, Philips/NXP e Sony)       |
| **2006** | Especificações das **tags NFC** e dos *SmartPosters*        |
| **2011** | **Android Beam** — peer-to-peer por NFC em smartphones      |
| **2014** | **Apple Pay** populariza o pagamento por NFC em telemóveis  |

### Curiosidade documentada

- **NFC não é sinónimo de RFID.** É um subconjunto do RFID de alta frequência, com regras próprias de inicialização, *anti-colisão* e segurança.
- As **tags passivas não têm bateria**: são alimentadas pelo campo magnético do leitor.
- O alcance de ~4 cm **é intencional** — funciona como barreira de segurança e obriga a uma ação deliberada do utilizador.
- O **NDEF** (*NFC Data Exchange Format*) é o formato standard dos dados gravados nas tags; o **LLCP** é o protocolo de transporte para o modo peer-to-peer.

## Exemplo de utilização

**Pagamento contactless num café:**

1. O cliente aproxima o telemóvel do terminal (a menos de 4 cm).
2. O telemóvel entra em **emulação de cartão**; o terminal alimenta o circuito por indução.
3. Há uma troca de mensagens NDEF/APDU e o terminal lê os dados de pagamento (token, não o número real do cartão).
4. A transação conclui-se em menos de um segundo, sem abrir aplicações nem introduzir PIN (para valores baixos).

**Outro exemplo:** aproximar o telemóvel de uma etiqueta NFC num router para receber automaticamente as credenciais de Wi-Fi.

## NFC vs outras tecnologias

| | NFC | RFID HF | Bluetooth |
| --- | --- | --- | --- |
| Alcance | ~4 cm | até ~1 m | ~10–100 m |
| Débito | 106–424 kbit/s | variável | Mbps |
| Emparelhamento | Um toque | Leitura | Manual (ou NFC) |
| Alimentação da tag | Pelo leitor | Pelo leitor | N/A |
| Uso típico | Pagamentos, acessos, *pairing* | Inventário, bilhética | Áudio, dados |
Fonte:
## Ligações

- [[Arquiteturas Avançadas Redes (AAR)]] — índice da disciplina
- [[Pilha TCP-IP]] — o NFC atua nas camadas de acesso à rede/aplicação, fora da pilha TCP/IP clássica
- [[Trabalhos e Avaliação]]
