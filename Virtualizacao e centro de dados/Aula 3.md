# 1. Edificação da Infraestrutura Cloud

**Construir, implementar, conceber ou projetar** uma infraestrutura Cloud é uma jornada longa e faseada, que começa pela gestão da infraestrutura e pela implementação de ferramentas de criação de serviços (Figura 1). O primeiro passo consiste em compreender a infraestrutura existente da organização, seguido da sua virtualização, que permite agregar os recursos disponíveis e garantir uma elasticidade rápida. Posteriormente, é essencial implementar processos e ferramentas de gestão de serviços Cloud, com vista à automatização da prestação de serviços, possibilitando o auto-serviço dos recursos informáticos a pedido, através da rede. Por fim, torna-se fundamental monitorizar e medir os serviços para assegurar a sua eficiência e qualidade contínua.

![](https://i.imgur.com/zQw0Ll8.png)  
Fig 1. Infraestrutura Cloud.

**A infraestrutura deve satisfazer as características essenciais da Cloud.**

Existe um caminho, uma ==**abordagem faseada**==, que deve ser adoptada para a construção ou edificação de infraestruturas Cloud (veja-se Figura 2).

![](https://i.imgur.com/w49mtWw.png)  
Fig 2. Edificação de uma Infraestrutura Cloud - Abordagem faseada.

  

**Compreender a Infraestrutura** de um centro de dados clássico existente é o primeiro passo na transição para um modelo baseado na Cloud, sendo o Centro de Dados Clássico (CDC) o modelo tradicional utilizado pelas organizações.

A Infraestrutura corresponde ao conjunto de hardware, software, redes, instalações e equipamentos da empresa que suportam as operações, o desenvolvimento de aplicações, a monitorização e todo o suporte de tecnologias de informação ou de processamento de dados.

Os **elementos nucleares** de um **CDC** incluem:

- As Aplicações.
- Os sistemas de gestão de bases de dados (DBMS).
- Os servidores responsáveis pela computação.
- Os sistemas de armazenamento.
- A rede que interliga todos estes componentes.

Por fim, a ==**virtualização da infraestrutura**== entende-se como o processo de virtualizar os elementos nucleares do Centro de Dados, nomeadamente nas camadas de computação, armazenamento, rede e/ou aplicação. Assim, um Centro de Dados Virtualizado ou _Virtualized Data center (VDC)_ é um centro de dados em que estes recursos são virtualizados, que representa uma evolução significativa face ao modelo clássico.

A virtualização é uma técnica de abstracção dos recursos físicos, permitindo que estes funcionem como recursos lógicos, independentes do hardware subjacente.

Entre os **principais benefícios** da virtualização destaca-se a otimização da utilização da infraestrutura de TI, a redução dos custos e da complexidade de gestão, a diminuição do tempo de implantação de serviços e o aumento da flexibilidade operacional.

Voltar a **destacar** que a **transformação de um centro de dados clássico (CDC) num centro de dados virtualizado (VDC) requer a virtualização dos elementos _core_ (centrais) do centro de dados.**

Para virtualizar a infraestrutura, recomenda-se uma abordagem faseada, de modo a garantir uma transição suave do Centro de Dados Clássico para o Centro de Dados Virtualizado (Figura 3).

![](https://i.imgur.com/s3EF90R.png)  
Fig 3. Transição faseada de um Centro de Dados Clássico para um Centro de Dados Virtualizado.

# 2. Gestão Eficiente de Serviços na Nuvem

A implementação de processos e ferramentas de gestão de serviços Cloud corresponde à criação de um verdadeiro Centro de Comando Cloud.

Estas ferramentas e processos têm como principal função apoiar a criação, disponibilização e otimização dos serviços Cloud. Um dos elementos centrais é o **Catálogo de Serviços**, que define os serviços disponíveis para consumo. Exemplos práticos incluem portais Web ou dashboards que funcionam como painéis de controlo, permitindo configurar e gerir a infraestrutura Cloud de forma centralizada. Estas soluções permitem automatizar a oferta de serviços e o provisionamento, incluindo funcionalidades como auto-provisionamento e auto-escalabilidade. Além disso, facilitam a monitorização e medição do consumo de recursos, bem como a respetiva taxação, e contribuem para a gestão e otimização dos recursos físicos e virtuais utilizados na criação de serviços Cloud.

# 3. Conceitos Fundamentais de Virtualização

A virtualização é uma tecnologia que permite simular plataformas de _hardware_ via um componente de _software_ - O _software_ cria uma máquina virtual (_Virtual Machine_ ou VM).

Uma VM simula um computador tradicional, com todas as suas características. Assim sendo, nesta máquina virtual é possível instalar um sistema operativo e software relacionado – Este irá funcionar da mesma forma que um SO instalado numa máquina tradicional.

O _software_ instalado nestas máquinas virtuais, sistemas operativos e aplicações, executa de uma forma isolada de outras aplicações e/ou sistemas físicos que lhe são adjacentes (i.e., isolado do _hardware_ da máquina hospedeira).

Existem vários ==**tipos de virtualização**==, no contexto de como a virtualização é feita, como o _software_ executa nas máquinas virtuais e acede aos recursos disponibilizados pelo _hardware_, o isolamento dos recursos, o desempenho obtido, etc.

**A virtualização é uma tecnologia essencial na _cloud_**, pois esta permite a divisão de uma plataforma de recursos em sub-plataformas mais pequenas e divisíveis. Estas são utilizadas de forma independente por diferentes utilizadores onde existe assim uma elevada otimização dos recursos disponíveis.

**Virtualização e _cloud_ são dois conceitos/tecnologias completamente distintos e diferentes**. Ao analisar as características essenciais da _cloud_ verificamos que a virtualização apenas se enquadra no _resource pooling_ (acesso partilhado a recursos), deixando assim de parte outras características essenciais como _on-demand self-service_, _broad network access_, etc.

## 3.1. Componentes de um Sistema/Arquitetura de Virtualização

Uma arquitetura ou sistema de virtualização é composta por três componentes essenciais, nomeadamente:

1. **_Hardware_ Físico** ou **Hospedeiro** (_Host Machine_ / _Infrastructure Layer_).
2. _**Hypervisor**_ (_Virtual Machine Monitor_ - VMM).
3. **Máquinas Virtuais** ou _**Guests**_ (_Virtual Machines_ ou _Guest Machines_).

Opcionalmente, podemos ter ainda um componente que permite a gestão e orquestração de máquinas virtuais ou _containers_ (criar, configurar, iniciar, parar e monitorizar). - Em ambiente cloud OpenStack, vCenter, Kubernetes para _containers_, etc.

O componente principal é o ==**Hardware Físico**== a ser virtualizado, e este _hardware_ é frequentemente designado por _**host machine**_, _**host físico**_ ou simplesmente **hospedeiro**.

- Tradicionalmente num ambiente cloud um hospedeiro é um servidor de grande capacidade computacional.

Adicionalmente, e dependendo da tecnologia de virtualização utilizada, temos o _hypervisor_. O ==_**Hypervisor**_== é responsável pela emulação/simulação de diversos componentes através da partilha de recursos existentes na máquina hospedeira. Em outros tipos de tecnologias de virtualização o componente de _**hypervisor**_ pode não existir, no seu lugar existe um sistema operativo com funções próprias ou mecanismos de virtualização. Por fim, temos as ==**Máquinas Virtuais**== que funcionam como se se tratassem de sistemas independentes, que correm “em cima” de um _host físico_ e do _hypervisor_ ou equivalente (a estas máquinas virtuais podemos também chamar _**guests**_).

No hardware físico pode existir suporte a extensões que permitem acelerar a virtualização (Intel VT-x, AMD-V, IOMMU).

> **Resumo** (veja Figura 4)
> 
> - Sistema **Hospedeiro** (do inglês, _Host System_): Define um sistema real com um SO no qual um _Hypervisor_, por exemplo, do tipo 2, existe e é executado.
> - Sistema **Convidado** (do inglês, _Guest System_): Define uma máquina virtual que possui um sistema operativo e _software_ relacionado.

![](https://i.imgur.com/xlv1zIv.png)  
Fig 4. Distinção entre _Guest_ e _Host System_.

## 3.2. Tipos de _Hypervisors_

Um programa de baixo nível (do inglês, _low-level program_), é necessário para fornecer acesso aos recursos do sistema às máquinas virtuais, este programa é tipicamente designado por _hypervisor_ ou _Virtual Machine Monitor_ (VMM).

De uma forma geral, existem dois tipos de _hypervisors_ (Figura 5):

- **Tipo 1 / Nativos / _Bare Metal_**: Hypervisors que correm diretamente no _hardware_.
    - As máquinas virtuais criadas num _hypervisor_ do tipo 1 não têm sistema operativo hospedeiro, uma vez que são criadas/instaladas num sistema “nu”. Um sistema operativo a correr numa VM do tipo 1 é considerada uma **virtualização completa**, uma vez que é feita a virtualização completa do hardware existe na máquina hospedeira.
    - Exemplos: KVM c/s Proxmox, XenServer, Microsoft Hyper-V, LynxSecure, RTS Hypervisor, Oracle VM, Sun xVM Server, VirtualLogix VLX, VMware ESX, entre outros.

> O Hyper-V é um _hypervisor_ de tipo 1 porque é carregado diretamente sobre o hardware, antes mesmo do Windows. O Windows, embora pareça o sistema “principal”, na realidade corre por cima do Hyper-V como uma partição especial (a raiz). Assim, mesmo estando integrado no Windows, o Hyper-V é considerado _bare metal_ (tipo 1).

- **Tipo 2 / Baseados em _Host_**: _Hypervisors_ instalados sobre um sistema operativo.
    - As máquinas virtuais criadas num _hypervisor_ do tipo 2 são instaladas sobre um sistema operativo hospedeiro.
    - Exemplos: Containers, Parallels, VirtualBox, VMWare Fusion, entre outros.

![](https://i.imgur.com/muckPnL.png)  
Fig 5. Tipos de _Hypervisors_.

## 3.3. Máquinas Virtuais

Por oposição ao termo físico, uma máquina virtual é um servidor, ou plataforma, disponível a sistemas ou utilizadores. Estas correm um sistema operativo e aplicações através da utilização de recursos virtuais “gerados” a partir de recursos físicos existentes num sistema hospedeiro (veja-se Figura 6)

![](https://i.imgur.com/FY7PeQp.png)  
Fig 6. Comparação de uma cenário computacional tradicional _versus_ um cenário baseada em máquinas virtuais.

Observando a Figura 6, podemos identificar que existem `n` máquinas virtuais executadas sobre um _hypervisor_. O mesmo não acontece num cenário tradicional, onde existe apenas uma máquina física, um sistema operativo e `n` aplicações a correr sobre a mesma.

Em termos de vantagens podemos considerar que as máquinas virtuais oferecem soluções inovadoras para:

- Recursos subutilizados.
- Inflexibilidade ao nível das aplicações.
- Gestão de _software_.
- Preocupações de segurança nas máquinas físicas existentes.

==**Gestão do Ciclo de Vida das VMs**==

A crescente adoção de ambientes distribuídos e infraestruturas em nuvem tem impulsionado o desenvolvimento de técnicas avançadas de virtualização, essenciais para garantir a eficiência, escalabilidade e flexibilidade dos sistemas computacionais modernos. Neste contexto, a ==**gestão do ciclo de vida**== de máquinas virtuais assume um papel central, englobando as seguintes **operações em ambientes distribuídos**:

- **Multiplexagem** (Figura 7, subfigura ==a==): Criação de múltiplos objetos virtuais a partir de uma instância de um objeto físico (i.e., _one to n_).
- **Suspensão** (Figura 7, subfigura ==b==): Colocação em _storage_ instâncias de máquinas virtuais criadas sobre um _hypervisor_ ou _Virtual Machine Monitor_ (VMM).
- **Fornecimento, Restauro ou Resumo** (Figura 7, subfigura ==c==): Resumo ou provisão de máquinas, ou objetos virtuais que se encontravam em suspensão.
- **Migração** (Figura 7, subfigura ==d==): Migração dos serviços (apps e OSs) das máquinas virtuais para diferentes _hypervisors_ ou VMMs.

![](https://i.imgur.com/BtstSgI.png)  
Fig 7. Operações de gestão do ciclo de vida das máquinas virtuais em ambientes distribuídos.

==**Servidores de Máquinas Virtuais**==

Um _Virtual Machine Server_, ou servidor de máquinas virtuais, refere-se a um computador com um software (_hypervisor_) em execução que aloja, cria e controla máquinas virtuais (veja-se Figura 8).

![](https://i.imgur.com/wdqBrYI.png)  
Fig 8. Representação do _Virtual Machine Server_.

## 3.4. Modelos de Execução de Máquinas Virtuais

Além da distinção prática entre _hypervisors_ de Tipo 1 e Tipo 2, a **literatura clássica** em sistemas operativos descreve também diferentes modelos de execução de máquinas virtuais: nativa, alojada e em modo duplo. Estes termos ajudam a compreender como a virtualização foi inicialmente conceptualizada, relacionando-se diretamente com os tipos de _hypervisors_, mas também com as técnicas usadas para gerir instruções privilegiadas e garantir o isolamento entre máquinas virtuais. Nomeadamente:

- **VM Nativa** → Corresponde ao _hypervisor Tipo 1_: A máquina virtual é instalada através de um _hypervisor_ em modo privilegiado, executada diretamente nos recursos da máquina.
    - O _hypervisor_ corre diretamente sobre o hardware físico, sem sistema operativo anfitrião. Oferece maior desempenho.
- **VM Alojada** → Corresponde ao _hypervisor Tipo 2_: A máquina virtual é instalada através de um _hypervisor_ e modo **não** privilegiado, executada sobre um sistema operativo hospedeiro ou _guest OS_.
    - O _hypervisor_ corre como aplicação num sistema operativo anfitrião. Mais simples de usar, mas com _overhead_ maior.
- **VM em Modo Duplo** → Não é um tipo de _hypervisor_, mas sim um mecanismo interno de como a virtualização funciona.
    - Aproveita os dois modos de execução da CPU (user mode e kernel/supervisor mode).
    - **O _hypervisor_ (seja do Tipo 1 ou Tipo 2) utiliza essa técnica** para capturar instruções privilegiadas e garantir o isolamento.
        
        O “modo duplo” descreve como a virtualização funciona dentro do processador, não uma categoria de _hypervisor_.
        

_**Virtualização Total (Full Virtualization)**_ É um método de virtualização onde o _hypervisor_ fornece ao sistema operativo convidado (_guest_ OS) uma simulação completa do _hardware_.

- O ponto essencial é que o _guest_ não sabe que está a correr numa VM.
- Ele acredita que tem acesso direto ao hardware físico.

> Pode ser instalado sem qualquer modificação.

O diagrama ilustrado na Figura 9, apresenta os modelos de execução, representadas nas subfiguras ==b==, ==c== e ==d==, comparadas com uma máquina física, ilustrada em ==a==.

![](https://i.imgur.com/Olr8T8o.png)  
Fig 9. Modelos de execução de máquinas virtuais.

## 3.5. Tipos de Virtualização

No contexto da virtualização, existem **cinco tipos principais de virtualização** que permitem maximizar a eficiência de recursos, melhorar a flexibilidade e a escalabilidade, e simplificar a gestão das infraestruturas físicas ou virtuais:

- **Virtualização Completa** (_Full Virtualization_) → _Guest_ não sabe que está virtualizado.
- **Paravirtualização** (_Paravirtualization_) → _Guest_ é modificado para cooperar com o _hypervisor_.
- **Emulação** (_Emulation_) → _Guest_ pode ser de uma arquitetura diferente da do _host_.
- **Virtualização ao Nível do Sistema Operativo** (_Containers_) → Existe a partilha do _kernel_ e isolamento dos ambientes.

==**Virtualização Completa**==

Todos os sistemas operativos convidados em virtualização completa comunicam diretamente com o _hypervisor_, por sua vez este comunica com o _hardware_ da máquina física (veja-se Figura 10).

- Os sistemas operativos dos _guests_ não necessitam de qualquer tipo de alteração.
- Normalmente, o _guest operating system_ em _full virtualization_ são mais rápidos do que em outros esquemas de virtualização existentes.

![](https://i.imgur.com/WCzPgNj.png)  
Fig 10. Representação do esquema da virtualização completa.

==**Paravirtualização**==

Na paravirtualização, o sistema operativo hospedeiro fornece uma interface virtual que permite ao sistema operativo convidado o acesso ao _hardware_ subjacente.

É o próprio sistema operativo hospedeiro (_host_) que executa as operações de I/O mediante uma API (Figura 11).

![](https://i.imgur.com/nmpve2h.png)  
Fig 11. Representação do esquema da paravirtualização.

==**Emulação**==

Num esquema baseado em emulação, a máquina virtual simula o _hardware_ de forma a ser independente do _hardware_ existente na máquina (veja-se Figura 12).

O sistema operativo convidado (_guest_) que utiliza um esquema de virtualização do tipo _emulation_ não necessita de ser modificado de forma alguma.

![](https://i.imgur.com/SDDRXLC.png)  
Fig 12. Representação do esquema de emulação.

==**Virtualização ao Nível do Sistema Operativo (_Containers_)**==

Num esquema baseado em virtualização ao nível do sistema operativo, ocorre a partilha do kernel do host e a criação de múltiplos ambientes isolados assentes em _containers_ (e.g., Docker, LXC).

## 3.6. Níveis de Implementação da Virtualização

No âmbito da virtualização, é possível identificar diferentes tipos de tecnologias de virtualização ou níveis de implementação da virtualização. Nomeadamente:

- _**Hardware-level Virtualization**_
- _**OS-level Virtualization**_
- _**Process-level Virtualization**, Software-level Virtualization_ ou _Application-level Virtualization_

O termo _software-level virtualization_ é frequentemente utilizado como sinónimo de _process-level virtualization_, referindo-se à virtualização de processos ou aplicações através de software, como acontece com a _Java Virtual Machine_ (JVM).

==**_Hardware-level Virtualization_ (Figure 13)**==

Esta tecnologia de virtualização é a mais utilizada e consiste na existência de um _hypervisor_ que é instalado “em cima” ou no topo de um hospedeiro. O _hypervisor_ comunica com o hospedeiro para controlar todos os recursos físicos disponíveis ao sistema para que os mesmos possam ser virtualizados e disponibilizados a uma ou `n` máquinas virtuais. Adicionalmente, a gestão dos acessos aos recursos é também realizada pelo _hypervisor_.

![](https://i.imgur.com/L3AT40Z.png)  
Fig 13. Hardware-level virtualization.

O fornecedor mais reconhecido neste tipo de tecnologia de virtualização é a empresa VMware, cujo produto principal também se denomina VMware. No entanto, existem outras soluções relevantes no mercado, tais como:

- Microsoft – Hyper-V
- Citrix – XenServer
- Oracle – Oracle VM (OVM)
- Open Source – KVM (Kernel-based Virtual Machine)
- (…)

==**_OS-level Virtualization_ ou _Containerization_ (Figura 14)**==

Esta tecnologia assume a utilização de apenas um sistema operativo instalado no _hardware_ onde existem mecanismos ao nível do sistema operativo que permitem dividir os recursos computacionais da máquina - Cada utilizador possui um domínio distinto e independente com os seus próprios recursos computacionais. Na tecnologia anterior a gestão e alocação dos recursos físicos em virtuais era feita pelo _hypervisor_, nesta tecnologia esta gestão e alocação é feita diretamente pelo sistema operativo (i.e., via o _kernel_ partilhado do SO).

Como exemplos deste tipo de tecnologia temos o OpenVZ, o FreeBSD _jails_ e o Virtuozzo da Parallels. Neste tipo de tecnologia não é utilizado o termo máquina virtual, mas sim, **ambientes independentes**, _jails_ ou _**containers**_.

![](https://i.imgur.com/zx9j4WO.png)  
Fig 14. Os-level virtualization.

**Jails Vs. Containers**  
_**Jails**_ (FreeBSD) → Mais simples, nativos do FreeBSD, muito seguros por defeito, ideais para isolar serviços num único servidor.

_**Containers**_ (Linux) → Mais flexíveis, com um ecossistema enorme (Docker, Kubernetes), perfeitos para DevOps e cloud, mas exigem mais cuidado na configuração de segurança.

Os _jails_ garantem segurança e isolamento robusto no FreeBSD, enquanto os _containers_ privilegiam portabilidade, automação e escala no Linux.

==**_Process-level Virtualization_ (Figura 15)**==

Esta tecnologia a máquina virtual executa apenas e exclusivamente uma aplicação/processo. Esta aplicação tem de ser especificamente escrita para esta máquina virtual. Por sua vez, esta máquina virtual pode ser instalada em diferentes sistemas operativos.

A _**Java Virtual Machine (JVM)**_ é o exemplo-chave desta tecnologia de virtualização, sendo que, permite que as aplicações possam correr em diferentes ambientes ou sistemas operativos.

![](https://i.imgur.com/dp7dWCC.png)  
Fig 15. Process-level virtualization.

**Resumo**  

- **Virtualização a nível de hardware** → Implementada com _hypervisors_ Tipo 1 (_bare metal_), que interagem diretamente com o hardware.
- **Virtualização a nível do sistema operativo** → _Containers_ e _jails_ que partilham o kernel do host, como Docker e LXC.
- **Virtualização a nível de aplicação ou processo** → Isola aplicações em _sandboxes_ (e.g., Flatpak, Snap, AppImage, App-V, ThinApp, Citrix)

# 4. Virtualização por Domínio de Recursos

A virtualização por domínio de recursos consiste em aplicar técnicas de virtualização a diferentes componentes do sistema, como computação, rede, armazenamento, aplicações e desktops.

## 4.1. Virtualização da Computação

Nas secções anteriores abordámos a virtualização da computação.

A virtualização da computação é uma técnica que mascara ou abstrai o hardware físico de processamento, permitindo que múltiplos sistemas operativos (SO) sejam executados simultaneamente numa única máquina física ou num conjunto de máquinas físicas em _cluster_ (Figura 20).

Podemos considerar os seguintes principais elementos desta tecnologia:

- **Criação de múltiplas máquinas virtuais (VMs)**, cada uma com o seu próprio sistema operativo e aplicações.
    - Cada VM é uma entidade lógica que se comporta como uma máquina física real.
- A **camada de virtualização** encontra-se entre o hardware físico e as VMs.
    - Esta camada é conhecida como _hypervisor_.
- **O _hypervisor_ fornece hardware padronizado** às VMs, permitindo que estas operem de forma independente do hardware físico subjacente.

![](https://i.imgur.com/Z3I19Mu.png)  
Fig 20. Virtualização da Computação.

## 4.2. Virtualização da Rede

A **virtualização da rede** consiste em segmentar logicamente uma ou várias redes físicas, para ser possível operar como uma ou múltiplas redes independentes, designadas por redes virtuais. Esta abordagem permite que a infraestrutura física seja abstraída e organizada em unidades lógicas mais flexíveis e eficientes.

Podemos considerar os seguintes **benefícios** principais da utilização de redes virtualizadas:

- Partilha eficiente de recursos da infraestrutura de rede entre várias redes virtuais.
- Comunicação facilitada entre os nós numa mesma rede virtual.
- Encaminhamento controlado para as comunicações entre diferentes redes virtuais.
- Restrição do tráfego de gestão (como transmissões de broadcast) a cada rede virtual, evitando a sua propagação indesejada.
- Agrupamento funcional de nós, permitindo organizar dispositivos de forma lógica e independente da sua localização física.

==**Virtualização da Rede num _Virtual Data Center_ (VDC)**==

A virtualização da rede envolve não apenas as redes virtuais, mas também a integração entre redes físicas e redes de máquinas virtuais (VMs).

Na **rede física** podemos observar a existências de adaptadores de rede, switches, routers, bridges, repetidores e hubs (Figura 16). Estes dispositivos asseguram a conectividade fundamental para o funcionamento da infraestrutura.

- **Esta conectividade é essencial em vários níveis:** permite a comunicação entre servidores físicos que executam _hypervisors_, garante a ligação entre servidores e os clientes, e ainda estabelece a comunicação entre servidores e sistemas de armazenamento.
- A rede física serve de base para suportar e interligar os diferentes elementos que, em conjunto com a virtualização, tornam o ambiente de rede mais flexível e eficiente.

Por fim, na **rede de máquinas virtuais** (VM _Network_) podemos observar a existência de um servidor físico e _switches_ lógicos, conhecidos como _virtual switches_ (Figura 17).

- Esta rede de máquinas virtuais assegura a ligação entre as várias VMs alojadas no mesmo servidor, permitindo igualmente a comunicação direta com o _kernel_ do _hypervisor_, e estabelece ainda a ligação entre as VMs e a rede física, atuando como uma ponte que integra os ambientes virtuais e físicos.

![](https://i.imgur.com/4CwsXwE.png)  
Fig 16. Representação de uma Rede Física.

![](https://i.imgur.com/fl4RNuC.png)  
Fig 17. Representação de uma rede de máquinas virtuais.

  

_**Network Interface Controller**_ (**NIC**), ou placa de rede, é um componente de _hardware_ que permite a um computador enviar e receber dados através de uma rede, seja por ligação física (cabo _Ethernet_) ou mediante uma rede sem fios.

==**Ferramentas de Virtualização da Rede**==

Tanto os **equipamentos físicos** como as plataformas de virtualização devem dispor de **funcionalidades** específicas que suportem a **virtualização** da rede (Figura 18). Nesse sentido, o papel do sistema operativo dos _switches_ físicos e do _hypervisor_ é determinante, pois são eles que fornecem as bases para a criação e gestão de redes virtuais.

- O _hypervisor pode utilizar_ as suas funcionalidades nativas de rede e virtualização para criar switches virtuais e configurar redes virtuais.

**ou**

- O _hypervisor_ pode recorrer a software de terceiros, instalados sobre este, que irá substituir as funcionalidade nativas de rede do _hypervisor_.  
    

![](https://i.imgur.com/pOP6y22.png)  
Fig 18. Exemplo de Configuração de uma Rede Virtual no VirtualBox (_Hypervisor_ Tipo 2).

==**Benefícios da Virtualização da Rede**==

A virtualização de redes traz consigo um conjunto de benefícios que vão muito além da simples redução de custos. Ao permitir a criação de redes lógicas independentes sobre a mesma infraestrutura física, esta abordagem potencia ganhos ao nível da segurança, do desempenho, da gestão e da eficiência dos recursos de rede.

Podemos assim considerar os seguintes **benefícios da virtualização de redes**:

- Reforço da segurança
    - Restringe o acesso entre nós de diferentes redes virtuais.
    - Isola dados sensíveis de uma rede virtual em relação a outra.
- Melhoria do desempenho
    - Restringe o tráfego de difusão na rede (broadcast), aumentando a performance das redes virtuais.
- Maior facilidade de gestão
    - Permite configurar redes virtuais a partir de uma estação centralizada de gestão, utilizando software dedicado.
    - Facilita a organização e reorganização de nós.
- Melhor aproveitamento e redução do CAPEX
    - Possibilita que várias redes virtuais partilhem a mesma infraestrutura física, otimizando a utilização dos recursos de rede.
    - Reduz a necessidade de implementar redes físicas separadas para diferentes grupos de nós.

**CAPEX** → _Capital Expenditure_ (Despesa de Capital)  
Refere-se ao investimento inicial em bens de capital, como compra de hardware, construção de infraestruturas ou aquisição de equipamentos de rede físicos.

Na prática, ao permitir que várias redes virtuais partilhem a mesma infraestrutura física, a virtualização reduz a necessidade de adquirir novos equipamentos (switches, routers, cabos, etc.), levando a uma diminuição dos custos de capital.

## 4.3. Virtualização do Armazenamento

A virtualização de armazenamento é uma técnica utilizada em ambientes informáticos para simplificar a gestão dos recursos de armazenamento. Em vez de os sistemas de computação acederem diretamente aos dispositivos físicos (como discos rígidos ou SSDs), acedem a uma **representação lógica desses recursos**. Esta abordagem facilita a administração, aumenta a flexibilidade e melhora a eficiência na utilização do espaço disponível.

- O mapeamento entre o armazenamento lógico e o físico é realizado pela camada de virtualização.
- A camada de virtualização abstrai a identidade dos dispositivos físicos de armazenamento.
- Os volumes virtuais são criados a partir dos pools de armazenamento (i.e., _storage pools_) e são atribuídos ao sistema de computação.

As máquinas virtuais (VMs) são armazenadas como um conjunto de ficheiros no espaço de armazenamento disponível alocado ao _hypervisor_ (Figura 19). Um dos principais ficheiros é um ficheiro que representa um disco virtual, que por sua vez é utilizador por uma VM para guardar os seus dados.

O tamanho do ficheiro de disco virtual corresponde ao espaço de armazenamento alocado para esse.

- Este espaço é definido no momento da criação da VM ou pode ser ajustado posteriormente, conforme necessário.

![](https://i.imgur.com/ngYgbra.png)  
Fig 19. Virtualização do Armazenamento - Máquinas Virtuais armazenadas num conjunto de ficheiros.

==**Benefícios da Virtualização do Armazenamento**==

A virtualização de armazenamento oferece várias vantagens que tornam a gestão de infraestruturas mais eficiente, flexível e económica. Entre os principais benefícios destacam-se:

- **Adição ou remoção de armazenamento sem tempo de inatividade**, permitindo alterações na capacidade sem afetar os serviços em funcionamento.
- **Maior utilização do armazenamento disponível**, o que contribui para a redução do Custo Total de Propriedade - _Total Cost of Ownership_ (TCO).
- **Migração de dados entre dispositivos de armazenamento de forma não disruptiva**, facilitando atualizações e mudanças de infraestrutura sem impacto nos utilizadores.
- **Compatibilidade com plataformas de armazenamento heterogéneas e de múltiplos fornecedores**, promovendo flexibilidade e evitando dependência de marcas específicas.
- **Simplificação da gestão de armazenamento**, tornando os processos mais ágeis e menos complexos para os administradores.