# 1. Virtual Clusters

Os clusters virtuais são construídos a partir de máquinas virtuais (VMs) instaladas em servidores distribuídos pertencentes a um ou mais clusters físicos. As VMs de um cluster virtual estão interligadas logicamente através de uma rede virtual que atravessa várias redes físicas.

Cada cluster virtual é, portanto, constituído por máquinas físicas ou por VMs alojadas em múltiplos clusters físicos.

A Figura 1 ilustra uma plataforma em nuvem composta por **três clusters físicos** e **quatro clusters virtuais** onde cada cluster físico contém várias máquinas que podem hospedar **máquinas virtuais** (VMs).

Estas VMs são agrupadas logicamente para formar os clusters virtuais, que podem abranger máquinas provenientes de diferentes clusters físicos. Assim, a figura demonstra como a virtualização permite criar estruturas lógicas (clusters virtuais) sobre infraestruturas físicas distintas, garantindo flexibilidade e melhor aproveitamento dos recursos disponíveis.

![](https://i.imgur.com/76GRhtA.png)  
Fig 1. Uma plataforma em nuvem com quatro clusters virtuais sobre três clusters físicos.

  

Um _cluster_ virtual pode ser composto por mais do que um _cluster_ físico.

==**As Propriedades dos _Virtual Clusters_**==

Podemos considerar as seguintes propriedades de um _cluster_ virtual:

- Os nós de um _virtual cluster_ podem ser tanto físicos como VMs.
    - Múltiplas VMs que executam diferentes sistemas operativos podem ser implementadas num mesmo nó físico.
- As VMs permitem consolidar múltiplas funcionalidades no mesmo servidor (isto aumenta a utilização do servidor e a flexibilidade das aplicações).
- As VMs podem ser colonizadas (replicadas) por múltiplos servidores, promovendo o paralelismo distribuído, tolerância a falhas e o _Disaster Recovery_ (DR).
- O tamanho (número de nós) de um _virtual cluster_ pode crescer ou diminuir dinamicamente, de forma semelhante ao que ocorre numa rede P2P (_peer-to-peer_).
- A falha de um nó físico pode desativar algumas VMs instaladas no nó em falha. Mas a falha de VMs não irá desativar o sistema hospedeiro.

==**Implementação Rápida e Escalonamento Eficaz**==

Para garantir um funcionamento eficiente e flexível dos clusters virtuais, é essencial adotar mecanismos que permitam uma distribuição ou _deployment_ rápido de software, uma gestão dinâmica de recursos e uma otimização contínua do desempenho.

- O sistema deve permitir a **rápida construção e distribuição de software**, incluindo sistemas operativos, bibliotecas e aplicações, para qualquer nó físico do cluster.
- Deve também possibilitar uma **mudança ou comutação rápida de ambiente de execução entre utilizadores**.
    - Quando um utilizador deixa de utilizar o seu cluster virtual, este deve ser rapidamente suspenso ou desligado, libertando recursos para outras VMs.
- É necessária a **adoção de técnicas que evitem a sobrecarga do cluster** e que promovam a eficiência energética (_green computing_).
- Estas medidas influenciam diretamente parâmetros fundamentais como a utilização global do sistema, o _throughput_ e a qualidade de serviço (QoS).

**Relembrar** Nas redes de comunicação, o termo _throughput_, também designado _throughput_ de rede ou simplesmente taxa de transferência, corresponde à quantidade de dados transmitidos de um ponto para outro, ou à quantidade de dados processados num determinado intervalo de tempo.

==**O Papel da virtualização**==

A virtualização desempenha um papel central neste processo, proporcionando vantagens significativas, nomeadamente:

- **Balanceamento de carga**, utilizando índices de carga (_load indexes_) e a frequência dos logins dos utilizadores;
- **Migração dinâmica de máquinas virtuais** (_Live VM Migration_), permitindo redistribuir recursos de forma eficiente e sem interrupção do serviço.

==**Armazenamento Virtual de Alto Desempenho**==

O **armazenamento virtual** é um elemento essencial para o bom funcionamento dos clusters virtuais, permitindo uma gestão eficiente dos dados e das imagens de máquinas virtuais distribuídas por múltiplos servidores. O objetivo é garantir elevado desempenho, otimização do espaço em disco e rapidez na personalização e execução das VMs

- Um **_template_ de máquina virtual** pode ser **distribuída por vários hosts físicos** do cluster, permitindo a customização das VMs conforme as necessidades de cada utilizador.
- É fundamental gerir eficientemente o espaço em disco, especialmente aquele espaço ocupado pelos pacotes de software associados aos _templates_.
    - Então, em termos de gestão e otimização do armazenamento:
        - O sistema deve **detetar e reduzir blocos duplicados** no sistema de ficheiros distribuído do cluster virtual, utilizando, por exemplo, funções de _hash_ para comparar o conteúdo dos blocos de dados.
        - **Novos blocos** devem ser criados apenas quando ocorre uma **modificação dos dados correspondentes**, evitando redundâncias desnecessárias.
        - Os **identificadores dos blocos** pertencentes a cada utilizador devem ser armazenados no respetivo perfil, garantindo uma gestão personalizada e eficiente do armazenamento.

==**Grupos de Máquinas Virtuais num Cluster**==

A criação de um grupo de VMs num cluster alvo requer um conjunto de passos cuidadosamente planeados para garantir **eficiência**, **rapidez de distribuição ou _deployment_ e **otimização do espaço em disco**.

O processo de criação do grupo de máquinas virtuais, compreende:

1. **Preparação da imagem de disco (_disk image)_:** recorre-se normalmente a _templates_, que são imagens pré-configuradas com um sistema operativo e aplicações-base (e.g., _Windows Datacenter 2019 + IIS_ ou _MySQL_).
    - Cada utilizador seleciona o **_template_ mais adequado** aos seus requisitos e duplica-a para criar a sua própria imagem de disco.
    - Os _templates_ costumam implementar o formato _Copy-on-Write_ (COW), que permite reduzir o consumo de espaço em disco e diminuir o tempo de *deployment das máquinas virtuais.
2. **Configuração das máquinas virtuais:** inclui a atribuição de um nome, imagem de disco, definições de rede e alocação de recursos (CPU e memória).
    - O processo torna-se mais eficiente com o uso _profiles_, que permitem ao sistema autoconfigurar automaticamente os principais parâmetros da VM.
3. **Seleção dos nós de destino:** os nós físicos que irão hospedar as máquinas virtuais devem ser escolhidos de modo a equilibrar a carga de trabalho (i.e., load balancing) entre os diferentes servidores do cluster.
4. **_Deployment_ final:** após a configuração, são executados os comandos de _deployment_ das máquinas virtuais em cada host, concluindo o processo de criação do grupo de máquinas virtuais.

# 2. Gestão de Um _Virtual Cluster_

Existem **quatro abordagens** principais que permitem a **gestão de um cluster virtual**, que diferem consoante a forma como o gestor de cluster (_cluster manager_) se encontra localizado e integrado entre os sistemas anfitriões e os sistemas convidados.

1. **Gestão baseada em sistema convidado (_guest-based manager_)**: Nesta abordagem, o gestor de cluster é executado dentro de um sistema convidado (_guest system_).
    - As máquinas virtuais (VMs) formam, assim, o cluster virtual.
    - Exemplo → `openMosix` – Um cluster baseado em Linux de código aberto que executa diferentes sistemas convidados sobre o _hypervisor_ Xen.
2. **Gestão baseada em sistema anfitrião (_host-based manager_):** O gestor de cluster é executado nos sistemas anfitriões (_host systems_), supervisionando as VMs.
    - Este gestor pode reiniciar um sistema convidado noutro servidor físico em caso de falha.
    - Exemplo → VMware HA (_High Availability_), que reinicia automaticamente máquinas virtuais após uma falha.
3. **Gestão independente nos sistemas anfitrião e convidado (i_ndependent cluster manager_):** Nesta configuração mais complexa, existe um gestor de cluster independente tanto nos sistemas anfitriões como nos convidados.
    - A coordenação entre ambos requer maior esforço de configuração e sincronização.
4. **Gestão integrada entre sistemas anfitrião e convidado (_integrated cluster manager_):** O gestor de cluster é integrado entre os dois níveis (i) anfitrião e (ii) convidado
    - Este deve ser projetado para distinguir entre recursos virtualizados e físicos, garantindo uma gestão coerente e eficiente de todo o ambiente virtualizado.

# 3. Etapas da Migração das Máquinas Virtuais

A migração de uma máquina virtual (_**Live VM Migration**_) envolve a cópia do ficheiro de estado da VM, localizado na área de armazenamento, para o host de destino.

Este processo permite transferir a execução de uma VM de um servidor físico para outro, garantindo continuidade de operação e equilíbrio de carga entre servidores.

Durante a migração, uma máquina virtual pode encontrar-se em diferentes ==**estados operacionais**==:

- **Estado Inativo**: a máquina virtual não está ativa, o que significa que ainda não foi instanciada na plataforma de virtualização.
- **Estado Ativo**: a máquina virtual já foi instanciada e está a desempenhar uma função real.
- **Estado Pausado**: a máquina virtual foi instanciada, mas encontra-se temporariamente parada ou em pausa.
- **Estado Suspenso**: o ficheiro da máquina virtual e os respetivos recursos virtuais foram guardados no disco, permitindo retomar a execução posteriormente.

==**A Figura 2 demonstra o processo de migração em tempo real**== (_live migration_) de uma máquina virtual de um host para outro. Este processo é composto por **seis etapas principais**:

1. **Pré-Migração** (_Stage 0 – Pre-Migration_): A VM está ativa no Host AA. Um host físico alternativo é selecionado para a migração, e os dispositivos de bloco são espelhados, mantendo os recursos prontos.
2. **Reserva** (_Stage 1 – Reservation_): É inicializado um _container_ no host de destino (Host BB), preparando o ambiente para receber a VM.
3. **Cópia Iterativa** (_Stage 2 – Iterative Pre-Copy_): Ativa-se o mecanismo de _shadow paging_, e as páginas de memória modificadas (_dirty pages_) são copiadas em várias rondas sucessivas enquanto a VM continua ativa.
4. **Paragem e Cópia** (_Stage 3 – Stop and Copy_): A VM é suspensa no Host AA. É gerado um pedido ARP para redirecionar o tráfego para o Host BB, e o estado restante da VM é sincronizado. Durante este passo, ocorre o _downtime_ (a VM fica temporariamente fora de serviço).
5. **Confirmação** (_Stage 4 – Commitment_): O estado da VM no Host AA é libertado, concluindo a transferência.
6. **Ativação** (_Stage 5 – Activation_): A VM é iniciada no Host BB, liga-se aos dispositivos locais e existe a retoma do funcionamento normal.

Em resumo, o processo assegura que a VM passa de um host para outro (_Host_ AA → _Host_ BB) com o mínimo de interrupção, mantendo a continuidade do serviço.

![](https://i.imgur.com/fwvRhoV.png)  
Fig 2. Processo de migração em tempo real (live migration) de uma máquina virtual (VM) de um host para outro.  
C. Clark et. al.

  

==**A Figura 3 apresenta o efeito de uma migração**== em tempo real na taxa de transmissão de dados de um servidor web, funcionando como complemento prático do processo descrito anteriormente (veja-se Figura 2).

Durante a migração de uma máquina virtual entre dois hosts, é inevitável algum impacto no desempenho, especialmente na taxa de transferência da rede (_throughput_).

- O gráfico apresentado na Figura 3 demonstra como essa taxa varia ao longo do tempo durante as diferentes fases da migração.

Ao **observar a Figura 3**, podemos concluir que:

- Inicialmente, o servidor web (VM no Host AA) apresenta uma taxa de transmissão estável de cerca de 870 Mbit/s.
- Durante a fase de pré-cópia (Pré-Migração, Reserva e Cópia Iterativa), a taxa reduz-se ligeiramente para 765 Mbit/s, devido à transferência contínua de memória para o Host BB.
- Nas iterações seguintes, a taxa mantém-se estável com pequenas flutuações, até ao início da fase de Paragem e Cópia (_Stop and Copy_), por volta dos 90 segundos.
- Nesta fase, a VM é suspensa no Host AA e ocorre a maior queda de desempenho (694 Mbit/s), com 165ms de _downtime_.
- Após as fases de Confirmação e Ativação, a VM inicia no Host BB, a taxa estabiliza e o serviço retoma o funcionamento normal, demonstrando o impacto mínimo da migração.

![](https://i.imgur.com/LPUmEsv.png)  
Fig 3. Efeito da migração no desempenho da taxa de transmissão de um servidor Web.  
Cortesia de C. Clark et al.

  

Em suma, a Figura 3 ilustra que o processo de migração em tempo real permite transferir uma máquina virtual (VM) ativa entre servidores, com interrupções mínimas e controladas, demonstrando a eficácia da técnica na garantia de alta disponibilidade dos serviços.