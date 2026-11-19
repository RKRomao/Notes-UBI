Atividade de Grupo — Camada de Ligação de Dados Cada questão corresponde a uma ronda de 20 minutos de pesquisa, discussão e preparação da resposta. No final de cada ronda, é selecionado aleatoriamente um grupo para apresentar a sua resposta à turma, durante cerca de 5 minutos, incluindo discussão. 
1. Camada de Ligação de Dados 
	- Onde atua a camada de ligação de dados? 
		Atua entre a camada de Rede e a camada Fisica 
	
	- Qual é a sua principal função? 
		A sua principal função é o Framing, que consiste em adicionar os pacotes recebidos e o endereçamento fisico , atraves do MAC para identificar de forma unica as placas de rede

2. Serviços da Camada de Ligação 
	- Identifiquem os seis serviços da camada de ligação de dados. 
		o enquadramento ou framing, o endereçamento físico adiciona o MAC, o controlo de acesso ao meio, o controlo de fluxo, o controlo de erros e o controlo de ligação lógica

	- Descrevam sucintamente cada serviço.
		o Framing:
			divide o fuxo de bits recebidos em unidades estrutoradas chamadas frames
			
		o enderecamento fisico:
			Adiciona o endereco fisico (MAC) de origem e de destino ao cabecalho do frame, permite o encamenhamento dentro de aparalhos da mesma rede local
			
		o controlo de acesso ao meio:
			Determina  qual o dispositivo que tem permissão para transmitir dados num canal partilhado a cada momento. Utiliza protocolos como o CSMA/CD ou o CSMA/CA para evitar ou gerir colisões entre sinais
			
		o controlo de fluxo:
			Regula a velocidade de transmissão entre o emissor e o recetor. Evita que um transmissor rápido sobrecarregue a capacidade de processamento ou de memória de um recetor mais lento.
			
		o Controlo de ligação lógica
			Gere a interface entre a camada física e a camada de rede superior, identificando qual o protocolo de rede encapsulado na trama. Permite também estabelecer comunicações orientadas ou não orientadas à ligação no enlace local


3. Deteção de Erros 
	- Quais são os dois principais mecanismos de deteção de erros estudados na camada de ligação? 
		Verificação de Pridade e o CRC (Cyclic Redundancy Check)

	- Expliquem resumidamente como funcionam. 
		- Funcionamento básico do Parity Check:
			Adiciona-se um único bit extra (bit de paridade) ao conjunto de bits original. Na paridade par, o bit extra garante que o total de uns seja um número par, enquanto na paridade ímpar o total deve ser ímpar. 
			ainda tem a paridade bidimensional que é feita em matiz
		- Funcionamento do CRC
			Trata o bloco de bits dos dados como um polinómio binário. O emissor divide este polinómio por um polinómio gerador predefinido e anexa o resto da divisão (chamado de FCS ou _Frame Check Sequence_) ao final da trama.
			O dispositivo de destino realiza exatamente a mesma divisão polinomial sobre a trama recebida (dados + FCS). Se o resto da divisão for igual a zero, a trama é considerada íntegra e aceite

	- Qual é o mais utilizado e porquê? 

		O **CRC** é o mais utilizado, porque deteta com elevada probabilidade erros em rajada (burst errors), que são o tipo de erro dominante em redes reais, com um custo computacional moderado e implementação trivial em hardware.

| Característica | Verificação de Paridade | CRC (*Cyclic Redundancy Check*) |
|---|---|---|
| **Complexidade** | Muito baixa (contagem simples de bits). | Moderada (divisão polinomial binária). |
| **Fiabilidade** | Baixa (falha em erros múltiplos pares). | Muito alta (excelente para erros em rajada). |
| **Uso típico** | Comunicações série simples e memórias. | Redes Ethernet, Wi-Fi e protocolos modernos. |


4. Protocolo de Acesso Múltiplo Ideal 
	- Identifiquem os quatro requisitos fundamentais de um protocolo de acesso múltiplo ideal. 
		
		Para um canal de transmissão (*broadcast*) com débito de **R bps**, um protocolo de acesso múltiplo ideal tem de cumprir quatro requisitos:

		1. **Aproveitamento total quando só um nó transmite** — se apenas um nó tem dados, deve poder transmitir à taxa máxima **R bps**.
		2. **Repartição justa quando vários nós transmitem** — se **M** nós têm dados, cada um deve conseguir, em média, **R/M bps**.
		3. **Funcionamento descentralizado** — não pode depender de um nó “mestre” a coordenar; não deve haver um ponto único de falha.
		4. **Simplicidade** — implementação barata, sem necessidade de sincronização de relógios ou de coordenação complexa entre nós.

	- Expliquem sucintamente a importância de cada requisito. 

		**1. Aproveitamento total (um só nó):** 
		o protocolo não deve desperdiçar capacidade. Se um nó é o único com tráfego, qualquer mecanismo de espera, *slots* vazios ou *overhead* de coordenação traduz-se diretamente em largura de banda perdida. É o critério de **eficiência** em carga baixa.

		**2. Repartição justa (M nós):** garante **equidade**:
		nenhum nó fica indefinidamente sem transmitir (*starvation*). Sem este requisito, um nó agressivo poderia monopolizar o canal e os restantes nunca convergiriam. Note-se que “R/M em média” admite variação instantânea; o que se exige é justiça a longo prazo.

		**3. Descentralização:** 
		evita o **ponto único de falha** e o **gargalo** de um coordenador central. Um mestre teria de ser eleito, sincronizado e protegido; se falhasse, a rede parava. Protocolos descentralizados escalam melhor e sobrevivem a partidas e entradas de nós sem reconfiguração global.

		**4. Simplicidade:** 
		reduz **custo de implementação** e **overhead de controlo**. Exigir relógios sincronizados ao microssegundo (como o TDMA puro) ou troca constante de mensagens de coordenação (como *polling*) consome recursos e é frágil em redes grandes e heterogéneas. A simplicidade torna o protocolo viável em hardware barato e fácil de depurar.

> **Nota:** nenhum protocolo real cumpre os quatro requisitos em simultâneo é justamente essa tensão (eficiência vs. justiça vs. simplicidade) que explica a existência de três famílias de protocolos: **partição de canal** (TDMA, FDMA, CDMA), **acesso aleatório** (ALOHA, CSMA/CD, CSMA/CA) e **acesso por revezamento** (*polling*, *token passing*). Cada uma otimiza requisitos diferentes.

5. Protocolos de Acesso Múltiplo Identifiquem as três principais categorias de protocolos de acesso múltiplo. 

	As três categorias são:

	1. **Partição de canal** (*channel partitioning*)
	2. **Acesso aleatório** (*random access*)
	3. **Acesso por revezamento** (*taking turns*)

	- Descrevam sucintamente o funcionamento de cada categoria. 
		
		**1. Partição de canal** — o canal é dividido *a priori* em porções (tempo, frequência ou código) e cada nó recebe uma fatia fixa, mesmo que não a use. Não há colisões por construção, mas há desperdício quando um nó está inativo.
		- **TDMA** (*Time Division Multiple Access*): o tempo é dividido em *slots* e cada nó transmite no seu *slot*; exige sincronização.
		- **FDMA** (*Frequency Division Multiple Access*): o espectro é dividido em bandas e cada nó transmite na sua frequência.
		- **CDMA** (*Code Division Multiple Access*): todos transmitem ao mesmo tempo e na mesma banda, mas com **códigos** ortogonais; o recetor separa os sinais por correlação.

		**2. Acesso aleatório** — o canal não é dividido; qualquer nó transmite quando tem dados, com risco de **colisão**. Quando colidem, os nós detetam (ou não) e retransmitem após um atraso aleatório. É simples e eficiente com poucos nós, mas degrada com carga alta.
		- **ALOHA** (puro e *slotted*): transmite sem ouvir o canal; eficiência máxima de ~18 % (puro) e ~37 % (*slotted*).
		- **CSMA** (*Carrier Sense Multiple Access*): ouve o canal antes de transmitir (“*listen before talk*”), reduzindo colisões.
		- **CSMA/CD** (*Collision Detection*): além de ouvir, deteta a colisão durante a transmissão e aborta cedo (Ethernet clássica).
		- **CSMA/CA** (*Collision Avoidance*): evita colisões por *backoff* e ACKs explícitos, pois não consegue detetar colisões no ar (Wi-Fi).

		**3. Acesso por revezamento** — os nós coordenam-se para que transmita **um de cada vez**, sem colisões e sem desperdício de *slots* vazios. A coordenação pode ser centralizada ou distribuída.
		- **Polling**: um nó **mestre** inquire cada nó por ordem; só o inquirido transmite. Sem colisões, mas com *overhead* de *polling* e latência; o mestre é ponto único de falha.
		- **Token passing**: um **token** circula pela rede em ordem lógica; quem o tem transmite. Justo e sem colisões, mas o token pode perder-se e há latência de espera.

	- Apresentem exemplos de protocolos pertencentes a cada categoria.

| Categoria | Mecanismo-base | Exemplos |
|---|---|---|
| **Partição de canal** | fatia fixa de tempo/frequência/código | TDMA (GSM), FDMA, CDMA (3G), OFDMA (LTE/Wi-Fi 6) |
| **Acesso aleatório** | transmitir e recuperar de colisões | ALOHA, Slotted ALOHA, CSMA, CSMA/CD (Ethernet), CSMA/CA (Wi-Fi) |
| **Acesso por revezamento** | coordenação explícita da vez | *Polling* (Bluetooth, *master/slave*), *Token Ring*/FDDI, *token bus* |

> 	**Síntese:** a partição de canal é ótima em **carga alta e tráfego constante**, mas desperdiça em carga baixa; o acesso aleatório é ótimo em **carga baixa e tráfego esporádico**, mas colide em carga alta; o revezamento procura o melhor dos dois, ao custo de **overhead de coordenação**.


