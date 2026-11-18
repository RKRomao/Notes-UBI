# 1. Tipos ou Modelos de Virtualização

**Relembrar**

Dependendo da posição da camada de virtualização em relação ao hardware e ao sistema operativo anfitrião, existem diferentes **tipos ou modelos de virtualização**:

- **Virtualização Completa** (_Full-Virtualization_) → _Guest_ não sabe que está virtualizado.
- **Paravirtualização** (_Para-virtualization_) → _Guest_ é modificado para cooperar com o _hypervisor_.
- **Emulação** (_Full System Emulation / Hardware Emulation_) → _Guest_ pode ser de uma arquitetura diferente da do _host_.
- **Virtualização ao Nível do SO** (a.k.a _Containerization_) → Existe a partilha do _kernel_ e isolamento dos ambientes.

# 2. Técnicas Genéricas de Virtualização

Existem diversas técnicas de virtualização que permitem a implementação da virtualização do CPU, nomeadamente:

- _**Trap-and-Emulate**_
- _**Binary Translation**_
- _**Dynamic Binary Translation**_

A virtualização começou nos anos 70 com a técnica **Trap-and-Emulate**, usada em _mainframes_ IBM, onde as CPUs estavam preparadas para que todas as instruções sensíveis gerassem trap. No entanto, a arquitetura x86 não cumpria este requisito, o que levou, nos anos 90, ao uso de _Binary Translation_, em que o _hypervisor_ traduzia instruções críticas do guest. Para reduzir o custo da tradução estática, surgiu depois a _Dynamic Binary Translation_ (DBT), que traduz apenas blocos de código à medida que são executados. Finalmente, a introdução do Intel VT-x e AMD-V em meados dos anos 2000 permitiu aplicar novamente o modelo puro de Trap-and-Emulate em x86, com desempenho quase nativo, tornando-se hoje a base da virtualização moderna, enquanto a DBT continua a ser utilizada em cenários de emulação e compatibilidade entre arquiteturas.

**Resumo da evolução histórica da virtualização**  
_Trap-and-Emulate_ _→_ _Binary Translation_ → _Dynamic Binary Translation_ → _Trap-and-Emulate_ (com Intel VT-x ou AMD-V).

## 2.1. _Trap-and-Emulate_ (TE)

Técnica clássica em que instruções sensíveis executadas pelo sistema convidado são intercetadas (_trap_) e emuladas pelo _hypervisor_. No entanto, na arquitetura x86 **nem todas as instruções geravam _traps_ corretamente**, o que **obrigou ao uso de binary translation.** Extensões modernas de CPU (Intel VT-x, AMD-V) resolveram esta limitação, permitindo aplicar de forma eficiente o modelo _trap-and-emulate_ e alcançar desempenho muito próximo ao nativo sem a utilização da técnica de tradução binária.

==**Processo de _Trap-and-Emulate_**==

Quando o sistema operativo convidado executa uma instrução privilegiada, esta não é executada diretamente no hardware. Em vez disso, ocorre um _trap_. Isto é, a instrução é intercetada pelo _hypervisor_ ou VMM.

- O VMM emula a ação correspondente a essa instrução e atualiza o estado do CPU virtual (vCPU) para refletir o mesmo efeito que teria acontecido se a instrução tivesse sido executada no processador físico.
- Depois da emulação, o controlo é devolvido ao kernel do sistema convidado, que continua a sua execução normalmente. Quando a execução regressa ao modo utilizador da máquina virtual, o sistema operativo convidado mantém a ilusão de estar a correr diretamente sobre o hardware, embora o VMM tenha controlado e emulado todas as instruções privilegiadas.

**Este processo, ou princípio de funcionamento, pode ser observado na Figura 1.**

![](https://i.imgur.com/hL1NqZY.png)  
Fig 1. Ilustração do processo _Trap-and-Emulate_.

  

É importante destacar que o **código em _user mode_ no sistema convidado é executado diretamente e à velocidade normal**, sem alterações significativas em relação a um ambiente não virtualizado. **No entanto**, o **código privilegiado em modo kernel é executado mais lentamente**, pois cada instrução privilegiada obriga a uma transição para o hypervisor (_trap_) e à sua posterior emulação.

- Este processo de ida e volta gera um _overhead_ considerável. O problema agrava-se quando existem vários sistemas convidados em execução, já que todos eles dependem do _hypervisor_ para emular as suas instruções privilegiadas, aumentando o custo de desempenho global.

**Requer a intercetação e emulação de instruções em tempo real. E, nem todas as instruções podem ser intercetadas e emuladas com sucesso.**

==**Limitações nas Arquiteturas x86 Originais**==

Segundo os princípios de virtualização de Popek & Goldberg (1974), para que o _trap-and-emulate_ funcione de forma eficiente, todas as instruções sensíveis devem:

- Gerar um _trap_ quando executadas em modo não privilegiado. No entanto, no **x86 original** (antes de VT-x / AMD-V), **isto não acontecia**:
- Algumas instruções sensíveis eram executadas em _Ring_ 1 ou _Ring_ 3 sem gerar trap.
    - **Exemplo**: instruções que acedem ou manipulam o estado da CPU (como `POPF`, que manipula flags do processador).
    - **Resultado**: o _guest_ OS conseguia executar instruções críticas sem o _hypervisor_ intervir, “partindo” assim o isolamento.

**Resumo**  
O _Trap-and-Emulate_ é uma técnica de virtualização em que o _guest_ executa diretamente no hardware, mas sempre que surge uma instrução privilegiada, esta gera um _trap_ para o _hypervisor_, que a emula em software, garantindo isolamento e segurança com desempenho quase nativo.

Antes do VT-x / AMD-V:

- O x86 tinha instruções privilegiadas que não geravam _trap_ (chamadas _sensitive but unprivileged instructions_).
- Isso impedia aplicar o modelo puro de _trap-and-emulate_, e o VMware, por exemplo, resolveu isso com _binary translation_.

Com VT-x (Intel) e AMD-V:

- Foram introduzidos modos especiais de execução (VMX root e non-root mode).
- Todas as instruções privilegiadas/sensíveis que antes “escapavam” passaram a ser corretamente intercetadas pelo hardware.
- Isso permitiu aplicar _trap-and-emulate_ de forma eficiente, sem necessitar de binary translation.

## 2.2. _Binary Translation_ (BT)

Esta técnica traduz instruções privilegiadas do sistema convidado em instruções seguras para o processador hospedeiro, oferecendo maior eficiência que a emulação, embora com um _overhead_ maior.

- Por outras palavras, o _hypervisor_ pode utilizar uma técnica chamada **_binary translation_ para gerir a execução de instruções dos sistemas operativos convidados** (_guest_ OSs) e das suas aplicações.

> “Performance of this method is worse than trap-and-emulate since all codes of the guest kernel is inspected by the VMM.”

Esta abordagem **distingue** **dois tipos de instruções**:

1. **Instruções críticas**
2. **Instruções não críticas**

As **instruções não críticas** não controlam diretamente o hardware nem comprometem a segurança do sistema. Estas podem ser executadas diretamente sobre o hardware, promovendo assim uma maior eficiência e desempenho.

Por outro lado, as **instruções críticas**, com potencial para afetar o controlo ou a segurança do sistema, são identificadas durante a tradução e substituídas por sequências de instruções seguras ou chamadas controladas ao hypervisor. Desta forma, não chegam a ser executadas diretamente pelo processador hospedeiro, evitando falhas de isolamento ou instabilidade.

**Esta captura seletiva de instruções críticas é necessária porque a tradução binária pode introduzir uma sobrecarga/_overhead_ significativa de desempenho se todas as instruções fossem traduzidas indiscriminadamente.**

==**Processo de Tradução Binária (Figura 2)**==

No processo de tradução binária devemos considerar as seguintes etapas principais:

1. **Posicionamento do Hypervisor**: O _hypervisor_ executa em _Ring_ 0, enquanto o sistema operativo convidado é colocado num nível menos privilegiado (e.g., _Ring_ 1, em implementações clássicas de x86 sem VT-x/AMD-V), perdendo acesso direto ao hardware.
2. **Análise do Fluxo de Instruções**: O _hypervisor_ analisa blocos de código do sistema operativo convidado, antes da sua execução, à procura de instruções potencialmente perigosas.
3. **Identificação das Instruções Críticas**: São detetadas instruções privilegiadas, de controlo ou sensíveis, que não podem ser executadas diretamente sobre o hardware.
4. **Substituição das Instruções Críticas**: Durante a tradução, estas instruções são substituídas por sequências seguras ou chamadas controladas ao _hypervisor_.
5. **Execução Direta das Instruções Não Críticas**: As instruções não críticas são executadas diretamente no hardware, maximizando a eficiência e o desempenho.

![](https://i.imgur.com/pKqiANG.png)  
Fig 2. Ilustração do processo de tradução binária.

  

==**Otimizações da _Binary Translation_**==

Uma das **otimizações** disponíveis para melhorar o desempenho da _Binary Translation_ é a técnica de _**caching**_.

- Otimizações podem ser necessárias quando existe um _overhead_ introduzido pela ação referente à identificação e substituição de instruções críticas.

Nesta técnica, um bloco de instruções é traduzido uma única vez e armazenado em _cache_. Quando o sistema convidado executa código que contém instruções sensíveis, verifica-se primeiro se a tradução já se encontra em _cache_. Se a instrução estiver disponível na _cache_ como instrução já traduzida, não é necessário realizar nova tradução.

==**Exemplos de Utilização da Tradução Binária**==

No código original abaixo escrito em _assembly language,_ podemos observar a execução normal de um bloco de instruções originária do _guest_:

```
push %ebx        ; callee saved
mov eax, edx     ; edx = eax
cli              ; disable interrupts
mov $1, %ecx     ; %ecx = 1
xor %ebx, %ebx   ; %ebx = 0
```

A instrução `cli` (_Clear Interrupt Flag_) pertence à arquitetura x86 e é utilizada para desativar interrupções no processador. O seu funcionamento baseia-se na manipulação do bit `IF` (_Interrupt Flag_), presente no registo `EFLAGS`. Quando o bit `IF` está ativo (`1`), o processador pode responder a interrupções de hardware; quando está limpo (`0`), as interrupções ficam desativadas.

**Esta capacidade é essencial para o sistema operativo, mas se executada diretamente por um sistema convidado (_guest_ OS) em ambiente virtualizado, poderia comprometer a segurança e o controlo do hospedeiro (_host_).**

No exemplo anterior, o comando `cli` tentaria modificar diretamente o registo `EFLAGS` do processador físico, limpando o bit `IF` e desativando as interrupções do hardware. **Se permitido**, o sistema operativo convidado poderia **interferir no funcionamento do host**, algo que o _hypervisor_ não pode permitir.

- **É neste ponto que a _binary translation_ entra em ação**:
- O hypervisor analisa o código e substitui a instrução crítica `cli` por uma sequência de instruções seguras que simulam o mesmo efeito, mas apenas no estado virtual do CPU do _guest_, sem alterar o hardware real, veja-se o exemplo seguinte:

```
(...)
and $0xfd, %gs:vcpu.flags
(...)
```

A operação `and` é uma instrução lógica bit a bit. O valor `$0xfd` corresponde ao binário 1111110111111101, que serve como máscara para limpar o bit responsável pelo _Interrupt Flag_ (`IF`). Já o endereço `%gs:vcpu.flags` corresponde a uma estrutura de dados mantida pelo _hypervisor_ que guarda o estado virtual do processador do _guest_ – A modificação ocorre apenas no contexto virtual do sistema convidado, sem qualquer impacto no registo `EFLAGS` do processador físico.

**Resumo**  
A _binary translation_ é uma técnica em que o _hypervisor_ traduz apenas as instruções críticas do sistema convidado, substituindo-as por sequências seguras ou chamadas controladas, enquanto as instruções não críticas são executadas diretamente no hardware. Esta abordagem evita falhas de isolamento, garante segurança e oferece maior eficiência do que a emulação completa, **introduzindo _overhead_ apenas quando necessário**, já que traduzir todas as instruções seria demasiado custoso em termos de desempenho.

O exemplo anterior ilustra de forma clara o funcionamento do mecanismo de virtualização _binary translation_, onde:

- As **instruções não críticas**, como `push`, `mov` ou `xor`, são executadas diretamente no hardware para garantir eficiência;
- As **instruções críticas**, como `cli`, são detetadas, traduzidas e substituídas por sequências seguras que garantem isolamento entre o _guest_ e o _host_.
    - O sistema convidado acredita que está a interagir diretamente com o hardware, quando na realidade o _hypervisor_ mantém o controlo absoluto, assegurando estabilidade e segurança.

## 2.3. _Dynamic Binary Translation_ (DBT)

A **tradução de todo o código do kernel** de um sistema operativo convidado pode ser **dispendiosa** em termos de desempenho como é, especialmente se for feita de forma estática e antes da execução, a técnica de virtualização _Binary Translation_ discutida anteriormente.

**Este problema** é resolvido pela técnica de _**Dynamic Binary Translation**_ (DBT).

Em termos de ==**princípios de funcionamento**==, o DBT analisa e traduz dinamicamente blocos de código (_basic blocks_). Estes blocos podem conter instruções críticas ou não. Quando encontra instruções críticas, a DBT não as deixa executar diretamente, mas sim substitui-as imediatamente por instruções seguras ou chamadas controladas ao _hypervisor_ que simulam o seu efeito – As instruções não críticas são mantidas e executadas diretamente no hardware.

Em termos de ==**otimizações**==, o _hypervisor_ pode reorganizar e otimizar as sequências de instruções traduzidas para melhorar o desempenho. Além disso, blocos de instruções frequentemente utilizadas podem ser guardadas em _cache_, evitando retradução e acelerando execuções futuras.

Por fim, e em termos de ==**limitações**==, e apesar de oferecer melhor desempenho que a emulação pura, a DBT tem limitações, especificamente:

- O desempenho degrada-se em sistemas que usam código auto modificável ou auto referenciado, já que isso obriga a invalidar e retraduzir blocos frequentemente.
- Sistemas em tempo real não podem ser virtualizados com esta técnica, devido à imprevisibilidade dos atrasos introduzidos pelo processo de tradução.

A **principal diferença entre _Binary Translation_ (BT) e _Dynamic Binary Translation_ (DBT)** está no momento e na forma como o código é traduzido.

- A BT consiste em converter instruções do sistema convidado em instruções equivalentes do anfitrião, podendo essa tradução ocorrer antes ou durante a execução, geralmente para substituir instruções privilegiadas e garantir segurança;
- Por outro lado, a. DBT realiza esta tradução em tempo real, à medida que o código é executado, armazenando e otimizando blocos traduzidos para melhorar o desempenho. Em suma, a DBT é uma forma mais avançada e adaptativa de BT, focada em eficiência e velocidade de execução.

# 2. Overview das Abordagens de Virtualização

A **tabela** seguinte **resume** os principais **modelos de virtualização**, o respetivo **nível de atuação** (hardware, sistema operativo ou processo) e a utilização das diferentes **técnicas de implementação** da virtualização (_Trap-and-Emulate, Binary Translation_ e _Dynamic Binary Translation_).

| Modelo de Virtualização         | Nível de Virtualização                    | TE  | BT  | DBT | Observações                                                                                                                       |
| ------------------------------- | ----------------------------------------- | --- | --- | --- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Full Virtualization**         | Hardware-level                            | ✅   | ✅   | ✅   | Guest OS corre sem modificações; pode usar DBT para _cross-architecture_. Por exemplo, VMware, VirtualBox, KVM, QEMU full system. |
| **Paravirtualization**          | Hardware-level                            | ❌   | ❌   | ❌   | _Guest_ OS modificado; chama _hypervisor_ diretamente; menos _overhead_. Por exemplo, Xen paravirtualized, VMware PV drivers.     |
| **Emulação**                    | Hardware-level / Process-level            | ❌   | ❌   | ✅   | _Guest_ OS ou aplicativo em arquitetura diferente do _host_. Ex: QEMU ARM→x86, Rosetta 2, emuladores de consolas.                 |
| **OS-level / Containerization** | OS-level / Process-level / Software-level | ❌   | ❌   | ❌   | Apenas isolamento de processos via kernel; não existe virtualização de CPU. Por exemplo, Docker, LXC, OpenVZ.                     |

_Trap-and-Emulation_ (TE), _Binary Translation_ (BT) e _Dynamic Binary Translation_ (DBT).

# 3. Mecanismos de Tolerância a Falhas – _Failover Clusters
Um _failover cluster_ é um grupo de servidores - físicos, virtuais ou mistos - configurados para garantir alta disponibilidade dos serviços. O objetivo é assegurar que aplicações críticas continuam acessíveis, com tempo de inatividade nulo ou mínimo.

No seu **funcionamento**, se um servidor (ou nó) falhar ou necessitar de ser reiniciado, outro nó do cluster assume automaticamente as cargas de trabalho, continuando a processar pedidos sem interrupções relevantes.

- Este mecanismo garante que os serviços permanecem disponíveis mesmo perante falhas de hardware ou software.

Os utilizadores experienciam **interrupções mínimas** nos serviços graças ao funcionamento automático dos _failover clusters_. O failover ocorre quando:

- O nó que aloja atualmente a instância fica inativo por qualquer motivo.
- Um dos recursos dentro da instância falha.
- O administrador força manualmente a ocorrência de um failover. Após o nó que ficou offline voltar a estar ativo, o serviço pode realizar um _failback_, ou seja, retornar ao nó primário.

O **principal objetivo** dos _failover clusters_ é o de oferecer alta disponibilidade e, em configurações mais avançadas, disponibilidade contínua.

- HA (High Availability) e CA (Continuous Availability). São largamente utilizados em ambientes empresariais para proteger o acesso a dados e recursos críticos, como bases de dados, sistemas de ficheiros e aplicações essenciais ao negócio.

==**O Conceito de Disponibilidade**==

A disponibilidade é um nível de serviço expresso como a percentagem de tempo em que um sistema ou serviço se encontra operacional.

A **alta disponibilidade** ou _High Availability_ (HA) compreende sistemas ou serviços que estão disponíveis em mais de 99% do tempo:

- Os requisitos de alta disponibilidade podem variar consoante a forma como a disponibilidade é medida.
- As interrupções planeadas (como manutenções programadas) normalmente não são incluídas no cálculo da disponibilidade.

==**Componentes em _Failover Clusters_**==

A Figura 3 ilustra um _failover cluster_ com dois nós, ligados entre si por uma rede dedicada (para _heartbeat_ e sincronização), a um armazenamento partilhado (via barramento ou iSCSI), e expostos a clientes externos através de uma rede de acesso.

> Os serviços/aplicações são executados de forma redundante: se um nó falhar, o outro assume automaticamente, garantindo alta disponibilidade.

![](https://i.imgur.com/2BqXA7G.png)  
Fig 3. Componentes de um _Failover Cluster_.

**Relembrar**  

- Barramento (_Shared Bus_) → Conexão física partilhada entre servidores e discos (modelo mais antigo, limitado em escalabilidade).  
    
- iSCSI → Protocolo moderno que transporta comandos _Small Computer System Interface_ (SCSI) através de redes IP, permitindo que qualquer servidor na rede aceda ao mesmo _storage_ como se fosse local.

==**Funções ou _Roles_ dos Servidores num _Failover Cluster_**==

Os servidores num _failover cluster_ podem ter uma variedade de funções:

- Servidores de Ficheiros e Armazenamento.
- Servidores de Virtualização (e.g., Hyper-V).
- Servidores de Base de dados.
- Servidores Web (e.g., IIS).
- (…) e, podem disponibilizar alta disponibilidade para uma variedade de outros serviços e aplicações.

==**Os Conceitos de _Failover_ e _Failback_**==

O _**failover**_ ocorre quando uma instância clusterizada (como máquinas virtuais, serviços ou processos) e todos os recursos associados são automaticamente transferidos de um nó para outro dentro do cluster. Isto pode acontecer em três situações:

1. quando o nó que aloja a instância falha,
2. quando um dos recursos associados à instância deixa de funcionar,
3. ou quando o administrador força manualmente o failover.

Já o _**failback**_ corresponde ao processo inverso: após o nó que esteve inativo voltar a estar operacional, os serviços e recursos podem regressar automaticamente ou manualmente ao nó primário.

==**Redes num _Failover cluster_**==

As redes desempenham um papel fundamental no funcionamento de um _failover cluster_. Estas suportam tanto a comunicação entre clientes e serviços como a comunicação interna entre os nós do cluster. Para garantir desempenho e redundância, recomenda-se a utilização de múltiplas interfaces de rede, nomeadamente:

- **Rede Pública**: utilizada pelos clientes para aceder ao serviço clusterizado.
- **Rede Privada**: dedicada à comunicação entre os nós do cluster (e.g., _heartbeat_).
- **Rede Pública** e **Privada**: utilizada para acesso ao armazenamento partilhado.

==**Opções de Armazenamento num _Failover Cluster_**==

Um _failover cluster_ exige armazenamento partilhado para garantir que os dados permanecem consistentes e acessíveis após a migração de serviços entre nós. Esse armazenamento é essencial para que qualquer nó possa assumir as _workloads_ em caso de falha. Entre as principais opções de armazenamento partilhado estão:

- **_Server Message Block_ (SMB)**: protocolo de partilha de ficheiros, muito utilizado em ambientes Windows.
- **_Network File System_ (NFS)**: é um protocolo de partilha de ficheiros pela rede, originalmente desenvolvido pela Sun Microsystems (anos 80). Este protocolo permite o acesso remoto a ficheiros a partir de diferentes máquinas (servidores e clientes) como se estivessem num sistema de ficheiros local.
- **_Internet Small Computer System Interface_ (iSCSI)**: envia comandos SCSI através de redes IP, permitindo que servidores acedam a discos remotos como se fossem locais.
- _**Fibre Channel**_: tecnologia de alta velocidade dedicada a redes de armazenamento (SAN), comum em ambientes empresariais críticos.

## 3.1. Exemplos de Implementação

A seguir, apresentam-se exemplos práticos de implementação de _failover clusters_ em múltiplos ambientes.

==**Exemplo 1 (_Failover Cluster_ no Windows Server 2008 R2)**==

A Figura 4 ilustra a arquitetura de _failover cluster_ com dois nós (i.e., _Two-Node Failover Clustering_) no Windows Server 2008 R2, utilizando o Hyper-V para virtualização.

![](https://i.imgur.com/AnU9imd.jpeg)  
Fig 4. Exemplo de implementação de um _Two-Node Failover Cluster_ no Windows Server 2008 R2, utilizando Hyper-V e armazenamento partilhado.

  

A arquitetura apresentada na figura anterior representa um _faillover cluster_ de dois nós sobre o Windows Server 2008 R2, onde dois hosts Hyper-V partilham armazenamento comum e monitorizam-se mutuamente. Em caso de falha de um nó, o outro assume as máquinas virtuais, assegurando alta disponibilidade. O ambiente é integrado com Active Directory (Domain Controller) e administrado centralmente através do Virtual Machine Manager.

==**Exemplo 2 (_Failover Cluster_ no Xen _Hypervisor_)**==

A Figura 5 representa um **_cluster_ de alta disponibilidade com XenServer** onde dois nós (`xen0` e `xen1`) partilham armazenamento via NFS.

O nó master coordena o ambiente e ambos trocam _heartbeats_ para monitorização. Em caso de falha de um nó, o outro assume automaticamente as máquinas virtuais, garantindo alta disponibilidade.

![](https://i.imgur.com/gFIvzpP.png)  
Fig 5. Exemplo de _failover_ em XenServer: dois nós com heartbeat e armazenamento externo em NFS.

## 3.2. O Conceito de Quórum

**De que forma se avalia se o _cluster_ pode continuar a fornecer serviços após uma falha?** Através da utilização do conceito de Quórum!

O quórum **representa o consenso mínimo necessário para que o _cluster_ continue a prestar serviços de forma segura**. Em termos práticos, define-se um número de votos que tem de ser atingido para o _cluster_ permanecer online e evitar situações de _split-brain_.

> Split-brain is a state of a server _cluster_ where nodes diverge from each other and have conflicts when handling incoming I/O operations.

Em ambientes como o Windows Server, o quórum é obtido por votação:

- Cada nó pode ter um voto e, consoante a configuração, também uma testemunha (_witness_). Por exemplo, uma partilha de ficheiros ou um disco partilhado podem contribuir com um voto adicional.
- O _cluster_ mantém-se operacional enquanto existir maioria de votos disponíveis e comunicantes.
- **O quórum define o consenso de que existe um número suficiente de membros do _cluster_ para fornecer serviços.**

==**“The Magic Number”**==

Quando o **número total de nós no _cluster_ é ímpar**, o conceito da maioria aplica-se de forma simples (por exemplo, três nós num cluster de cinco). No entanto, **em clusters com um número par de nós** (e.g., quatro), existem duas formas de tornar o total de votos ímpar:

- **Aumentar para mais um voto**: adicionar uma testemunha (_witness_) que irá significar um voto adicional. Esta irá requerer configuração manual do sistema
- **Reduzir um voto**, retirando de forma automatizada o direito de voto a um dos nós do cluster, conforme necessário.

==**Modos de Quórum**==

A **tabela seguinte** apresenta os **diferentes modos de quórum** utilizados em clusters de failover. O quórum define o número mínimo de elementos (nós e testemunhas) que devem estar disponíveis para que o cluster continue operacional.

Dependendo da configuração, o quórum pode basear-se apenas nos nós, ou incluir também uma testemunha em disco ou numa partilha de ficheiros. O modo Disk Only é o mais simples, mas também o menos resiliente, uma vez que depende exclusivamente de um único disco partilhado.

| **Modo de Quórum**               | **Quem tem direito de Voto?**                                                                | **Quando existe Quórum?**                                        |
| -------------------------------- | -------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Node Majority**                | Apenas os nós do cluster têm voto.                                                           | O quórum é mantido quando mais de metade dos nós estão online.   |
| **Node and Disk Majority**       | Os nós do cluster e um disco de testemunho (_disk witness_) têm voto.                        | O quórum é mantido quando mais de metade dos votos estão online. |
| **Node and File Share Majority** | Os nós do cluster e uma partilha de ficheiros de testemunho (_file share witness_) têm voto. | O quórum é mantido quando mais de metade dos votos estão online. |
| **No Majority: Disk Only**       | Apenas o disco partilhado do quórum tem voto.                                                | O quórum é mantido enquanto o disco partilhado estiver online.   |

==**Configurações Possíveis**==

A tabela seguinte ilustra como a **configuração do número de nós** e a **presença de uma testemunha** (_witness_) influenciam a capacidade do cluster sobreviver a falhas.

Com a**penas dois nós**, o sistema fica dividido em 50/50, o que significa que nenhuma das partes pode assumir o controlo em caso de falha de um nó. No entanto, ao adicionar uma testemunha, o número de votos torna-se ímpar, permitindo ao cluster resistir à falha de um nó.

À medida que o número de nós aumenta (3, 4, 5 ou mais), a capacidade de tolerar falhas simultâneas também cresce, especialmente quando existe uma testemunha configurada, garantindo maior resiliência e continuidade do serviço.

|**Server nodes**|**Can survive one server node failure**|**Can survive one server node failure, then another**|**Can survive two simultaneous server node failures**|
|---|---|---|---|
|2|50/50|No|No|
|2 + Witness|Yes|No|No|
|3|Yes|50/50|No|
|3 + Witness|Yes|Yes|No|
|4|Yes|Yes|50/50|
|4 + Witness|Yes|Yes|Yes|
|5 and above|Yes|Yes|Yes|

==**Recomendações para Atingir Quórum**==

As recomendações para a configuração de quórum num cluster variam conforme o número de nós e o tipo de infraestrutura disponível:

- **Dois nós**: é obrigatório utilizar uma testemunha (_witness_).
- **Três ou quatro nós**: a utilização de uma testemunha é fortemente recomendada.
- **Cinco nós ou mais**: a utilização de uma testemunha não é necessária, pois não acrescenta resiliência adicional.
- **Com acesso à Internet**: deve ser utilizada uma testemunha na cloud (_cloud witness_).
- **Em ambientes de TI** com outras máquinas e partilhas de ficheiros: recomenda-se o uso de uma testemunha baseada em partilha de ficheiros (_file share witness_).

==**Exemplos de Configuração de Quórum**==

**a) Dois nós sem uma Testemunha**

```
+--------+  +--------+
| Server |  | Server |
| Node 1 |  | Node 2 |
|  Vote  |  |    -   |
+--------+  +--------+
```

Neste exemplo, podemos observar que o _failover_ cluster:

- Pode sobreviver a uma falha de servidor: **50% de probabilidade**.
- Pode sobreviver a uma falha de servidor e, em seguida, a outra: **Não**.
- Pode sobreviver a duas falhas de servidor em simultâneo: Não

**b) Dois nós com uma Testemunha**

```
+--------+  +--------+  +--------+
| Server |  | Server |  | Witness|
| Node 1 |  | Node 2 |  | (Vote) |
| (Vote) |  | (Vote) |  +--------+
+--------+  +--------+ 
```

Neste exemplo, podemos observar que o _failover_ cluster:

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, a outra: **Não**.
- Pode sobreviver a duas falhas de servidor em simultâneo: **Não**.

**Análise (Dois nós sem e com Testemunha)**  
Num cluster de dois nós, sem uma testemunha (_witness_), existem apenas paridade de votos (50/50), o que impede o sistema de determinar automaticamente qual dos nós deve manter o controlo em caso de falha – Por isso, o cluster pode ficar indisponível.

Ao adicionar uma testemunha, o número total de votos **passa a ser ímpar** (3 votos), permitindo ao cluster atingir quórum mesmo que um dos nós falhe. Assim, a testemunha garante a continuidade do serviço quando ocorre uma única falha.

**c) Três Nós sem uma Testemunha**

```
+--------+  +--------+  +--------+
| Server |  | Server |  | Server |
| Node 1 |  | Node 2 |  | Node 3 |
| (Vote) |  | (Vote) |  | (Vote) | 
+--------+  +--------+  +--------+
```

Neste exemplo, podemos observar que o _failover_ cluster:

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, a outra: **50% de probabilidade**.
- Pode sobreviver a duas falhas de servidor em simultâneo: **Não**.

**d) Três Nós com uma Testemunha**

```
+--------+  +--------+  +--------+  +--------+
| Server |  | Server |  | Server |  | Witness|
| Node 1 |  | Node 2 |  | Node 3 |  |   (-)  |
| (Vote) |  | (Vote) |  | (Vote) |  +--------+
+--------+  +--------+  +--------+
```

Neste exemplo, podemos observar que o _failover_ cluster:

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor em simultâneo: **Não**.

**Análise (Três nós sem e com Testemunha)**  
Num cluster de três nós sem testemunha, existe um total de três votos. O cluster mantém-se operacional com dois nós ativos, mas se dois falharem em sequência, existe apenas 50% de probabilidade de recuperar o controlo, dependendo da ordem das falhas.

Ao adicionar uma testemunha (_witness_), o total de votos passa a ser quatro, permitindo uma melhor distribuição e maior tolerância a falhas consecutivas, garantindo que o cluster mantém o quórum mesmo após duas falhas sucessivas (embora não simultâneas).

**e) Quatro Nós sem uma Testemunha**

```
+--------+  +--------+  +--------+  +--------+
| Server |  | Server |  | Server |  | Server |
| Node 1 |  | Node 2 |  | Node 3 |  | Node 4 |
| (Vote) |  | (Vote) |  | (Vote) |  |   (-)  |
+--------+  +--------+  +--------+  +--------+
```

Neste exemplo, podemos observar que o _failover_ cluster:

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor em simultâneo: **50% de probabilidade**.

**f) Quatro Nós com uma Testemunha**

```
+--------+  +--------+  +--------+  +--------+  +--------+
| Server |  | Server |  | Server |  | Server |  | Witness|
| Node 1 |  | Node 2 |  | Node 3 |  | Node 4 |  |   (-)  |
| (Vote) |  | (Vote) |  | (Vote) |  | (Vote) |  +--------+
+--------+  +--------+  +--------+  +--------+
```

Neste exemplo, podemos observar que o _failover_ cluster:

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor em simultâneo: **Sim**.

**Análise (Quatro nós sem e com Testemunha)**  
Num cluster de quatro nós sem testemunha, o número total de votos é par (4), o que torna o sistema vulnerável a empates. Assim, embora o cluster suporte falhas isoladas e sucessivas, em caso de duas falhas simultâneas existe apenas 50% de probabilidade de se manter operacional.

Ao adicionar uma testemunha (_witness_), o total de votos torna-se ímpar (5), eliminando empates e garantindo que o cluster mantém o quórum mesmo com duas falhas simultâneas, aumentando significativamente a resiliência.

**g) Cinco nós (ou mais)**

```
+--------+  +--------+  +--------+  +--------+  +--------+
| Server |  | Server |  | Server |  | Server |  | Server |
| Node 1 |  | Node 2 |  | Node 3 |  | Node 4 |  | Node 5 |
| (Vote) |  | (Vote) |  | (Vote) |  | (Vote) |  | (Vote) |
+--------+  +--------+  +--------+  +--------+  +--------+
```

Neste exemplo, podemos observar que o _failover_ cluster:

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor em simultâneo: **Sim**.

**Análise – Cinco nós (ou mais)**  
Num **cluster com cinco ou mais nós**, o número total de votos é ímpar, o que elimina empates e torna o sistema naturalmente mais resiliente.

- Este tipo de configuração permite que o cluster permaneça operacional mesmo após a **falha de dois nós simultaneamente**, uma vez que continua a existir uma maioria de votos disponíveis (por exemplo, 3 de 5).

A partir de cinco nós, **não é necessário adicionar uma testemunha** (_witness_), pois esta não aumenta a resiliência — O cluster já possui uma tolerância adequada a falhas e mantém o quórum de forma estável.

==**Dynamic Quorum**==

O _Dynamic Quorum_ é um mecanismo que permite ao cluster ajustar de forma dinâmica o número de votos necessários para manter o quórum ou, consoante o número de nós ainda disponíveis.

Podemos observa na Figura 6:

- Quando o Server Node 2 falha, o cluster mantém-se ativo.
- Quando o Server Node 3 falha, o cluster continua operacional.
- Mesmo após a falha do Server Node 4, o cluster permanece em funcionamento.

O número total de votos necessários para atingir o quórum passa agora a ser determinado com base no número de nós disponíveis.

![](https://i.imgur.com/iAohNPq.png)  
Fig 6. Exemplo do funcionamento do _Dynamic Quorum_ : o cluster mantém o quórum mesmo após falhas sucessivas de nós.

  

**Sem este mecanismo dinâmico**, o **cluster deixaria de funcionar** após a perda de dois nós, pois não conseguiria atingir a maioria de votos.

- Com o _Dynamic Quorum_, o sistema recalcula automaticamente a maioria necessária, garantindo a resiliência e continuidade de serviço.

**Resumo**  

- O **quórum** define define quantos votos (nós/testemunhas) são necessários para o cluster continuar activo (é a “regra da maioria”).
- O **dynamic quorum** ajusta automaticamente esses votos conforme os nós entram ou saem, permitindo que o cluster continue a funcionar mesmo com menos nós disponíveis.

# 4. Princípios e Implementação Prática de um Failover Cluster em Hyper-V

Antes de se proceder à implementação de um Failover Cluster em ambiente Hyper-V, é essencial compreender os princípios e requisitos de configuração que asseguram o correto funcionamento e a estabilidade da infraestrutura. Estes requisitos definem a base técnica sobre a qual o cluster será construído, abrangendo aspetos de hardware, rede, sistema operativo e permissões administrativas.

O cumprimento destas condições é indispensável para garantir que o cluster atinge o nível de fiabilidade, compatibilidade e desempenho exigido pelo Windows Server.

==**Requisitos Gerais**==  
No caso da Microsoft, todo o hardware utilizado num cluster de failover deve estar certificado para a versão do Windows Server em execução. A solução completa de cluster deve ser validada através do Assistente para Validar Configuração (_Validate Configuration Wizard_), garantindo a compatibilidade e a fiabilidade de todos os componentes.

==**Nós Servidores e Componentes de Rede**==  
Os nós servidores que compõem o cluster devem apresentar configurações idênticas ou muito semelhantes, assegurando uniformidade e estabilidade. Além disso, todos os nós devem passar com sucesso nos testes de validação da configuração, de forma a confirmar a sua conformidade com os requisitos do Windows Server.

Caso seja utilizada a tecnologia iSCSI, cada adaptador de rede deve ser dedicado exclusivamente a uma função — ou para comunicação de rede, ou para iSCSI, mas nunca para ambas. Esta separação é fundamental para evitar congestionamentos e reduzir o risco de falhas de comunicação.

==**Outros Serviços**==  
Todos os nós do cluster devem utilizar resolução de nomes através de **DNS**, garantindo uma identificação fiável e uma comunicação consistente entre servidores.

Para além disso, Os servidores devem pertencer ao mesmo _**Active Directory Domain**_, o que assegura uma gestão centralizada e coerente do cluster.

- Nas versões mais recentes do Windows Server, é também possível que os nós pertençam apenas ao mesmo grupo de trabalho (Workgroup), embora esta opção seja menos comum em ambientes empresariais.

==**As Contas dos Administradores, Sistemas Operativos e Hardware**==  
A **conta de utilizador** responsável pela criação do cluster deve possuir:

- Direitos e permissões de administrador em todos os servidores envolvidos;
- Permissão para criar objetos de computador no domínio.

Estas permissões são essenciais para o correto registo e funcionamento do cluster no ambiente de rede.

Todos os nós devem executar a mesma versão do **Windows Server** (e.g., Windows Server 2016 Enterprise). A uniformidade na versão do sistema operativo evita incompatibilidades e simplifica a manutenção do cluster.

Por fim, e em termos de **hardware**, os processadores utilizados nos nós do cluster devem:

- Pertencer à família x64;
- Suportar virtualização;
- Ser devidamente selecionados para minimizar o risco de falhas.

Estas características garantem o desempenho e a fiabilidade do ambiente de virtualização baseado em Hyper-V.

**Resumo**  
Para ativar a alta disponibilidade num ambiente Hyper-V, é necessário:

1. Instalar a função Hyper-V.
2. Configurar as definições de rede.
3. Configurar o armazenamento partilhado (por exemplo, iSCSI).
4. Instalar a funcionalidade de Failover Clustering nos nós Hyper-V.
5. Validar a configuração utilizando o Assistente de Validação da Configuração (_Cluster Validation Wizard_).
6. Criar um cluster de failover através do Assistente de Criação de Cluster (_Create Cluster Wizard_).
7. Criar funções em cluster (_clustered roles_) para alojar as cargas de trabalho do cluster.
8. Criar uma máquina virtual altamente disponível no cluster de failover Hyper-V.

**Ao seguir estes passos, será possível tornar a infraestrutura altamente disponível e garantir a sua resiliência face a ataques inesperados ou falhas do sistema.**

## 4.1. Configuração dos Recursos Hyper-V

O Recurso [“Exemplo Hyper-V Clustering”](http://di.ubi.pt/~tsimoes/resources/17912/Exemplo-Hyper-V-Clustering.pdf) apresenta, de forma prática e ilustrada, todas as etapas necessárias para a configuração de um cluster Hyper-V com alta disponibilidade, nomeadamente:

- **Configuração do armazenamento partilhado (iSCSI)**: criação do NAS no lado do _target_ e ligação dos _initiators_ ou iniciadores nos nós Hyper-V.
    - O _**initiator**_ é o componente (software ou hardware) presente no nó do servidor Hyper-V que se liga ao servidor de armazenamento iSCSI.
    - O _**target**_ é o dispositivo de armazenamento (e.g., um NAS ou SAN) que fornece os discos partilhados aos quais o initiator ao qual este se liga.
- **Configuração de rede**: associação correta dos endereços IP dos _initiators_ e verificação de acessos.
- **Instalação das funcionalidades de Failover Clustering** nos nós Hyper-V.
- **Validação da configuração** dos nós utilizando o _Cluster Validation Wizard_.
- **Criação do cluster de failover** através do _Create Cluster Wizard_.
- **Configuração do modo de quórum** no cluster, de acordo com as boas práticas discutidas.
- **Adição e configuração das máquinas virtuais no cluster**, incluindo a partilha de ficheiros e a integração no ambiente de alta disponibilidade.
- **Testes de funcionamento** e **Live Migration** para demonstrar a resiliência e continuidade de serviço em caso de falha.