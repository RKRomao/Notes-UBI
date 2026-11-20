# 1. Introdução
Os _containers_ têm vindo a tornar-se cada vez mais populares no desenvolvimento de sistemas e aplicações, uma vez que:

- Eliminam a necessidade de configurar e gerir hardware.
- Eliminam a necessidade de configurar e gerir sistemas operativos.
- Permitem uma utilização mais eficiente dos recursos computacionais.
- Contribuem para a redução de tempo e de custos.

No entanto, as **plataformas de gestão de _containers_** concentram-se sobretudo na administração de _containers_ individuais. Assim, em projetos de grande dimensão, alguns aspetos tornam-se mais complexos de gerir, tais como:

- A configuração e gestão do _load balancing_.
- A conectividade à Internet.
- A orquestração do processo de distribuição/_deployment_ dos _containers_.

# 2. Plataformas de Gestão de _Containers_

Para simplificar e automatizar a gestão de _containers_ em projetos de grande escala, surgiram diversas plataformas especializadas que permitem orquestrar, monitorizar e escalar aplicações de forma eficiente.

Estas plataformas são responsáveis por coordenar múltiplos **_containers_ distribuídos por diferentes máquinas**, garantindo que as aplicações mantêm o desempenho, disponibilidade e resiliência desejados.

A plataforma mais popular e amplamente adotada é o [**Kubernetes**](https://kubernetes.io/) (também conhecido por **K8s**), uma plataforma de orquestração de _containers_ de código aberto que se tornou o padrão de referência para a gestão automatizada de aplicações em ambientes baseados em _containers_.

# 3. Kubernetes
O [**Kubernetes**](https://kubernetes.io/) é um sistema de código aberto concebido para automatizar a distribuiar, o dimensionamento e a gestão de aplicações containerizadas. É atualmente a tecnologia de referência para orquestração de _containers_, permitindo gerir ambientes complexos com múltiplos serviços de forma eficiente e escalável.

O nome Kubernetes provém de uma expressão grega que significa “piloto de um navio”. Esta origem simbólica reflete o papel do Kubernetes como o “piloto” que conduz e coordena um “navio de _containers_”, assegurando que todos os componentes de uma aplicação navegam em conjunto e de forma ordenada.

Frequentemente abreviado como K8s, onde o 88 representa os oito caracteres entre “k” e “s”, o Kubernetes é altamente inspirado no sistema [Google Borg](https://research.google/pubs/large-scale-cluster-management-at-google-with-borg/), um orquestrador interno utilizado pela Google para gerir milhões de _containers_ nas suas operações globais.

> O Kubernetes foi inicialmente desenvolvido pela Google e, após o lançamento da versão 1.0 em julho de 2015, foi doado à _Cloud Native Computing Foundation_ (CNCF), uma das principais subfundações da Linux Foundation.

==**Ambientes do Kubernetes**==

O Kubernetes é uma plataforma portátil, extensível e de código aberto concebida para gerir e orquestrar aplicações baseadas em _containers_. A sua portabilidade permite que seja **executado em praticamente qualquer ambiente**, seja em nuvens públicas, infraestruturas privadas ou servidores locais, oferecendo grande flexibilidade às equipas de desenvolvimento e operações.

==**Vantagens**==  
Uma das principais vantagens do Kubernetes é a sua capacidade de criar uma camada de abstração sobre as tarefas mais complexas da gestão de _containers_, simplificando processos como a distribuição de cargas de trabalho, a criação e configuração de redes, a execução e monitorização de aplicações, bem como a realização de atualizações automáticas e controladas.

Além disso, o Kubernetes oferece um vasto conjunto de funcionalidades que aumentam a eficiência e a fiabilidade da infraestrutura:

- Realiza cura automática de _containers_ (reiniciando ou substituindo-os automaticamente em caso de falha).
- Efetua escalonamento automático de acordo com a procura.
- Automatiza as atualizações dos _containers_.
- Gere de forma integrada o armazenamento (_storage_).
- Gere o tráfego de rede entre os diferentes serviços e componentes.

Desta forma, o Kubernetes liberta os programadores da complexidade operacional subjacente, **permitindo-lhes concentrar-se no desenvolvimento** e na **escalabilidade das aplicações**, enquanto o sistema assegura a orquestração, resiliência e eficiência da infraestrutura subjacente.

==**Princípios de Funcionamento**==  

O Kubernetes é uma plataforma de orquestração de _containers_ concebida para automatizar a gestão de aplicações complexas compostas por múltiplos microsserviços.

Num **cenário típic**o, um sistema pode necessitar, por exemplo, de 10 instâncias do container do Microsserviço A, 15 instâncias do Microsserviço B, e assim sucessivamente.

- O Kubernetes é responsável por criar, distribuir e manter todas estas instâncias de forma automática, eficiente e equilibrada.

Para além de gerir o número de _containers_ ativos, o Kubernetes integra um conjunto de funcionalidades avançadas que garantem resiliência, desempenho e continuidade de serviço, nomeadamente:

- _**Auto Scaling**_: Permite escalar automaticamente o número de _containers_ com base na procura ou carga do sistema, aumentando ou reduzindo recursos de forma dinâmica.
- _**Load Balancing**_: Gere o equilíbrio de carga entre várias instâncias de um mesmo microserviço, assegurando que o tráfego é distribuído de forma uniforme e que não há sobrecarga em nenhum container.
- _**Self-Healing**_: Implementa mecanismos de autocorreção, monitorizando o estado dos _containers_ e substituindo automaticamente quaisquer instâncias que falhem ou fiquem inativas.
- _**Zero Downtime Deployments**_: Permite realizar atualizações de versões de aplicações sem interrupção do serviço, garantindo uma experiência contínua para os utilizadores finais.

![](https://i.imgur.com/SgTTuZH.png)  
Fig 1. Kubernetes - Orquestração de Containers.

Em conjunto, estas capacidades tornam o Kubernetes uma plataforma robusta e inteligente, capaz de gerir automaticamente o ciclo de vida completo das aplicações containerizadas (desde a sua criação até à sua manutenção e atualização em produção).

==**Kubernetes Clusters**==

Um **cluster** é um **conjunto de computadores interligados** que trabalham em conjunto e são percebidos como um único sistema lógico. Esta abordagem permite distribuir tarefas, aumentar a capacidade de processamento e garantir maior disponibilidade e fiabilidade dos serviços.

Cada computador dentro do cluster é designado por nó (node) – Figura 2. Os nós são geralmente configurados para executar o mesmo tipo de tarefas, cooperando entre si para cumprir objetivos comuns.

![](https://i.imgur.com/D6qNumE.png)  
Fig 2. Kubernetes Clusters.

Por exemplo, alojar site, processar grandes volumes de dados ou, no contexto do Kubernetes, executar _containers_ que contêm aplicações distribuídas.

Um cluster utiliza um software centralizado responsável por agendar, distribuir e controlar as tarefas que são executadas pelos diferentes nós.

> Este software atua como o “cérebro” do cluster, garantindo que os recursos são utilizados de forma eficiente, que as aplicações são corretamente distribuídas e que o estado global do sistema é constantemente monitorizado.

Este componente central é executado em computadores designados por control planes (Figura 2).

- Os control planes são responsáveis por gerir e coordenar o funcionamento de todo o cluster, tomando decisões como a alocação de cargas de trabalho, o monitoramento do estado dos nós e dos _containers_, e a recuperação automática em caso de falhas.

**Resumo**  
Enquanto os nós de trabalho executam as aplicações, os _control planes_ garantem o controlo, a orquestração e a estabilidade do _cluster_.

## 2.2. Arquitetura

A Figura 3 ilustra de forma clara a arquitetura de um cluster Kubernetes, destacando a separação entre o _Control Plane_ e os nós de trabalho (Nodes), que constituem os **dois principais componentes do sistema**.

![](https://i.imgur.com/WF2ut2j.png)  
Fig 3. Arquitetura de um Cluster Kubernetes composta pelo Control Plane e os Nodes.

==**Control Plane**==  
O _**Control Plane**_ é o responsável pela gestão e orquestração global do _cluster_. Ele contém vários componentes essenciais:

- O **`kube-apiserver`** ou _**API Server**_, que atua como o ponto de entrada para todas as comunicações com o cluster, processando pedidos REST e interligando os diferentes módulos.
    - > (…) É o frontend de comunicação entre todos os componentes do cluster, através de uma API RESTful.
        
- O _**controller manager**_, que monitoriza o estado do cluster e assegura que este corresponde ao estado desejado (por exemplo, recriação de _Pods_ em caso de falhas).
    - > “A Pod is a Kubernetes abstraction that represents a group of one or more application containers (such as Docker), and some shared resources for those containers. Those resources include: Shared storage, as Volumes. Networking, as a unique cluster IP address.”
        

Opcionalmente, podemos adicionar ou integrar no kubernetes um _**cloud controller manager**_ que incorpora no cluster tecnologias cloud, como por exemplo, _load balancers_, filas ou _storage_.

- O _**scheduler**_, que decide em que nó cada novo _pod_ será executado, com base na disponibilidade de recursos e nas políticas definidas.
    - > (…) componente responsável pela atribuição de tarefas (_Pods_) aos nós.
        
- O **`etcd`** ou _**Backing Store**_, uma base de dados distribuída que armazena o estado atual e a configuração do cluster.
    - Tudo o que podemos consultar com o comando `kubectl get` é armazenado no `etcd`.
    - O comando `kubectl create` cria uma nova entrada no `etcd`.

==**Nodes**==  
Por outro lado, os _**Nodes**_ são responsáveis por executar as aplicações containerizadas. Cada nó contém três componentes principais:

- O **`kubelet`**, agente que corre em cada nó – comunica com o Control Plane e garante que os _containers_ definidos nos _pods_ estão a correr corretamente.
- O **`kube-proxy`**, é responsável pela configuração de rede do nó, assim, este gere a comunicação de rede entre os _Pods_ e o exterior.
    - (…) Também implementa regras para encaminhamento e _load balacing_, ou seja, implementa parte do conceito de _Service_.
- O _**Container Runtime**_, é o motor responsável por criar e executar os _containers_ (por exemplo, Docker ou containerd).
    - As suas funções incluem obter, iniciar e parar as imagens.
    - O Kubernetes é compatível com vários **_engines_ de execução de _containers_**, como Docker, rkt, CRI-O e containerd, permitindo flexibilidade na escolha do runtime utilizado para criar e gerir _containers_ dentro dos nós do cluster.

Em conjunto, o _Control Plane_ e os _Nodes_ formam uma infraestrutura coordenada e eficiente, onde o primeiro gere e supervisiona o segundo, assegurando que as aplicações distribuídas correm de forma automática, escalável e resiliente.

## 2.3. The Command Line Interface (CLI) – `kubectl`

A interação com um cluster Kubernetes é realizada através da ferramenta de linha de comando kubectl (Figura 4). Esta ferramenta permite ao utilizador **efetuar o deploy de aplicações**, **inspecionar e gerir recursos** do cluster (como _Pods_, serviços e deployments), bem como visualizar logs e o estado das aplicações em execução.

**O `kubectl` é, assim, a interface principal entre o utilizador e o control plane do Kubernetes.**

![](https://i.imgur.com/hez3iXS.png)  
Fig 4. Kubernetes' Command Line Interface (CLI).

==**Comandos Básicos com `kubectl`**==

O `kubectl` é a principal ferramenta de linha de comandos utilizada para interagir com um cluster Kubernetes, permitindo criar, inspecionar, atualizar e eliminar recursos.

A estrutura geral de um comando segue a sintaxe `kubectl <action> <resource>`, onde:

- A _**action**_ define o que se pretende fazer, como _create_, _describe_, _delete_, _apply_ ou _expose_.
- A _**resource**_ indica o alvo da ação, como _node_, _pod_, _deployment_, _service_, entre outros.

**Por exemplo**:

- Para visualizar os nós existentes no cluster, utiliza-se: `kubectl get nodes`. O resultado apresenta a lista de nós disponíveis, a partir dos quais o Kubernetes escolherá automaticamente onde executar as aplicações, com base nos recursos disponíveis em cada nó.
    
    É também possível utilizar o parâmetro `--help` após qualquer subcomando para obter informações adicionais sobre os parâmetros disponíveis.
    
    ```
    tiagomiguelcs@mini ~ % kubectl get nodesNAME       STATUS   ROLES           AGE     VERSIONminikube   Ready    control-plane   4h25m   v1.34.0
    ```
    
- `kubectl version`, lista a versão atual do Kubernetes. Este comando mostra as versões do cliente e do servidor, confirmando que ambos estão acessíveis.
    
    ```
    tiagomiguelcs@mini ~ % kubectl versionClient Version: v1.32.2Kustomize Version: v5.5.0Server Version: v1.34.0WARNING: version difference between client (1.32) and server (1.34) exceeds the supported minor version skew of +/-1
    ```
    

Para uma referência completa dos comandos e opções do `kubectl`, consultar a documentação oficial disponível na seguinte hiperligação: [https://kubernetes.io/docs/reference/kubectl/](https://kubernetes.io/docs/reference/kubectl/)

## 2.4. Kubernetes _Pods_

Num cluster Kubernetes, os **_containers_ não são executados diretamente**. Em vez disso, são **empacotados dentro de um objeto chamado _Pod_** (Figura 5). O _Pod_ representa uma instância em execução de uma aplicação no cluster e pode conter um ou mais _containers_ que partilham o mesmo contexto de execução.

![](https://i.imgur.com/kNA0aLS.png)  
Fig 5. Kubernetes Pods e Nodes.

O _Pod_ representa uma instância em execução de uma aplicação no cluster e pode conter um ou mais _containers_ que partilham o mesmo espaço de rede e armazenamento. É considerado a unidade mais pequena de execução em Kubernetes, ou seja, é o menor objeto que pode ser criado, gerido e escalado dentro do sistema.

**Então** → Um Pod é, portanto, um grupo de um ou mais _containers_ que partilham determinados recursos e configurações, e que inclui:

- **Armazenamento Partilhado** (_shared storage_), que permite aos _containers_ dentro do mesmo pod acederem a volumes comuns.
- **Configurações de Rede**, assegurando que todos os _containers_ do pod partilham o mesmo endereço IP e as mesmas portas de comunicação.
- **Especificações** (_specifications_) que definem como os _containers_ devem ser executados, incluindo comandos, variáveis de ambiente e políticas de reinício.

De forma simplificada, um Pod é semelhante a um conjunto de _containers_ com namespaces e volumes de ficheiros partilhados, funcionando como uma unidade coesa dentro do cluster (Figura 5).

No entanto, se for necessário **escalar** uma aplicação, **não é possível simplesmente adicionar novos _containers_ a um Pod existente**.

- O processo de escalabilidade em Kubernetes baseia-se na criação de novos _Pods_, que podem ser executados no mesmo nó ou noutros nós do cluster, garantindo assim uma distribuição equilibrada e maior resiliência da aplicação.

### 2.4.1. Ciclo de Vida / _Pod Lifecyle_
O ciclo de vida de um Pod (_Pod lifecycle_) em Kubernetes pode ser descrito em várias etapas principais (veja-se Figura 6), nomeadamente:

![](https://i.imgur.com/douTeSM.png)  
Fig 6. Pod lifecycle.

- **_Scheduled_ → _Pending_**: O pod é agendado para execução. O container _runtime_ começa a fazer o download das imagens necessárias e a preparar os _containers_.
- **_Pending_ → _Running_**: Quando todos os recursos do pod estão prontos e os _containers_ são iniciados com sucesso, o pod entra no estado Running.
- **_Running_ → _Succeeded_**: O pod passa para o estado Succeeded quando conclui a sua tarefa com sucesso e termina de forma normal.
- **_Running_/_Pending_ → _Failed_**: Se ocorrerem falhas, como erros nos _containers_, falha ao carregar uma imagem ou terminação inesperada, o pod entra no estado Failed.
- _**Unknown**_: Se o Kubernetes não conseguir determinar o estado atual do pod (por exemplo, devido a falhas de comunicação com o nó), o pod é classificado como Unknown.

Estas etapas permitem ao Kubernetes acompanhar o progresso e o estado dos _Pods_, assegurando a gestão automática da execução e recuperação de falhas.

### 2.4.2. _Deployment Options_

> _Pod Deployment Options_ with `kubectl`.

No Kubernetes, existem várias formas de implementar e gerir _Pods_ através da linha de comandos com o kubectl, cada uma com diferentes níveis de abstração e controlo:

- _**POD Templates**_: Permitem definir a configuração de um pod através de um ficheiro de configuração (geralmente em YAML). Este ficheiro especifica detalhes como o nome das imagens dos _containers_, o registry de onde são descarregadas, as portas utilizadas e outros parâmetros de execução (runtime details).
- _**Replication Controllers**_: Baseiam-se em pod templates e definem um número fixo de _Pods_ que devem estar sempre a correr. O _replication controller_ monitoriza o cluster e substitui automaticamente os _Pods_ que falham, garantindo que o número desejado é mantido.
    - _**Replica Sets**_: São uma versão moderna e mais eficiente dos _replication controllers_. Além de manterem o número de _Pods_ ativos, utilizam um sistema de identificação através de labels (_selector values_) para gerir e selecionar grupos de _Pods_ de forma mais flexível.
- _**Deployments**_: Introduzem uma camada de gestão adicional sobre os _replica sets_, facilitando tarefas de atualização e manutenção.
    - _**Rolling Updates**_: Permitem lançar gradualmente uma nova versão de _Pods_ sem interromper os existentes, garantindo zero _downtime_ durante o processo.
    - _**Rollback**_: Possibilitam reverter para uma versão anterior caso ocorra algum problema com a atualização, também assegurando zero _downtime_.

![](https://i.imgur.com/QQ3Wkgj.png)(a) High Availability (HA) — Replicação para assegurar tolerância a falhas. Mesmo existindo um único Pod em execução, se este falhar o replication controller recupera- o..

![](https://i.imgur.com/R2K7zuS.png)(b) Load-Balancing and Scaling — Processo de balanceamento de carga e escalabilidade (scaling) que permite distribuir os pedidos entre várias instâncias de uma aplicação.

Fig 7. Replication Controller.

Estas opções tornam o Kubernetes uma plataforma altamente resiliente e automatizada para gerir a execução e atualização contínua de aplicações baseadas em _containers_.

### 2.4.3. Tipos de Serviços

> Service Types

Embora cada Pod possua um endereço IP único, esses endereços não são expostos para fora do cluster sem recurso a um _Service_. Os _Services_ permitem que as aplicações recebam tráfego e podem ser expostos de diferentes formas, definidas através do campo _type_ em _ServiceSpec_.

==_**ClusterIP**_==  
_ClusterIP_, é o tipo predefinido e expõe o _Service_ através de um IP interno do cluster, ficando acessível apenas a partir do interior do próprio cluster (Figura 8).

![](https://i.imgur.com/I83B3dl.png)  
Fig 8. ClusterIP

==_**NodePort**_==  
O tipo _NodePort_ expõe o Service na mesma porta de cada Node selecionado, usando NAT, permitindo o acesso externo através de `<NodeIP>:<NodePort>`. É um superconjunto de _ClusterIP_.

A Figura 9 ilustra a forma como tráfego externo consegue alcançar um Pod que se encontra dentro de um cluster através da especificação de 3 portas:

![](https://i.imgur.com/UBFAyZa.png)  
Fig 9. NodePort

- `targetPort` (80): corresponde à porta 80 dentro do container, onde a aplicação está efetivamente a correr. É para esta porta que o Service encaminha o tráfego quando chega ao Pod.
    
- `port` (80): é a porta exposta pelo Service dentro do cluster. Outros _Pods_ ou componentes internos comunicam com o serviço através desta porta, que neste caso também é a 80.
    
- `nodePort` (30008): é a porta aberta em cada Node do cluster — dentro do intervalo permitido, que vai de 30000 a 32767 — permitindo o acesso externo ao serviço. O tráfego que chega ao Node pela porta 30008 é encaminhado para o port do serviço e daí para o targetPort dentro do container.
    
    **Resumo do Fluxo dos Pedidos**  
    30008 (`nodePort`) → 80 (`port`) → 80 (`targetPort`).
    

==_**LoadBalancer**_==  
O tipo _LoadBalancer_ cria um _load balancer_ externo na cloud (quando suportado) e atribui um IP externo fixo ao Service. É um superconjunto de NodePort.

![](https://i.imgur.com/AlTwVmt.png)  
![](https://i.imgur.com/aorSenQ.png)  
Fig 10. LoadBalancer

A Figura 10 representa o funcionamento de um _Service_ do tipo _LoadBalancer_ no Kubernetes, e mostra como o tráfego externo proveniente da Internet ou de um utilizador chega aos _Pods_ dentro do cluster.

- O diagrama ilustra todos os elementos envolvidos no percurso do tráfego: o Load Balancer, os Nodes, os serviços (_NodePort_ e _ClusterIP_) e os _Pods_ finais.

No topo da figura, um utilizador realiza um pedido (e.g., `curl http://myapp.com`). Este pedido chega primeiro ao Load Balancer nativo da infraestrutura (normalmente fornecido pelo ambiente de cloud). O _LoadBalancer_ recebe um endereço IP público próprio e distribui o tráfego entre os Nodes disponíveis.

Cada Node tem exposto o mesmo _nodePort_, neste caso na porta 30008, o que permite que qualquer Node possa receber o tráfego encaminhado pelo _LoadBalancer_. Assim, o pedido curl `http://192.168.1.X:30008`, onde X=2X=2, 33 ou 44) chega ao Service do tipo _NodePort_, representado na imagem pela camada azul logo abaixo do _LoadBalancer_.

O Service _NodePort_, por sua vez, encaminha o tráfego para o Service do tipo _ClusterIP_, mostrado a meio da figura. O _ClusterIP_ atua como ponto de entrada interno para os _Pods_ que fornecem a aplicação. Este serviço interno faz o balanceamento dentro do cluster, enviando o tráfego para um dos _Pods_ disponíveis — por exemplo, 10.244.0.4, 10.244.1.4 ou 10.244.2.4 — representado a vermelho.

**Resumo do Fluxo dos Pedidos**  
Utilizador → `LoadBalancer` (30008) → `NodePort` → `ClusterIP` → `Pod`

O diagrama ilustrado na Figura 10 demostra que um Service _LoadBalancer_ é essencialmente composto por dois níveis:

- Um balanceamento externo (_LoadBalancer_ da cloud),
- Um serviço NodePort interno que distribui o tráfego para o serviço ClusterIP e, finalmente, para os _Pods_. Este mecanismo garante alta disponibilidade, permitindo que o tráfego seja distribuído entre vários Nodes e _Pods_, mesmo que alguns fiquem indisponíveis.

==_**ExternalName**_==  
O tipo _ExternalName_ mapeia o Service para o valor definido em _externalName_ (e.g., [foo.bar.example.com](http://foo.bar.example.com/)), devolvendo um registo CNAME e sem configurar qualquer proxy. - Requer kube-dns ≥ 1.7 ou CoreDNS ≥ 0.0.8.

**_NodePort_ vs _LoadBalancer_**  
O tipo _NodePort_ é a forma mais básica de permitir que tráfego externo chegue diretamente a um serviço. Tal como o nome indica, este tipo abre uma porta específica no Node, e todo o tráfego enviado para essa porta é encaminhado para o serviço. Já o tipo _LoadBalancer_ é o método padrão para expor um serviço à Internet; cada serviço recebe o seu próprio endereço IP, facilitando o acesso externo de forma mais direta.

No caso do minikube, os serviços do tipo _LoadBalancer_ podem ser expostos através do comando minikube tunnel. Este comando deve ser executado numa janela de terminal separada para manter o _LoadBalancer_ ativo. Quando se termina o processo com Ctrl-C, as rotas de rede criadas são automaticamente removidas.

### 2.4.4. Comandos Básicos para Gestão de _Pods_

É possível criar um Pod de duas formas: através de um ficheiro YAML, utilizando um _**Pod Template**_ que define todas as suas configurações (imagem, portas, labels, etc.), ou de forma **manual**, diretamente pela linha de comandos com o `kubectl`.

Veja o exemplo seguinte:

```
apiVersion: v1kind: Podmetadata:     name: myapp-pod     labels:       app: myapp       type: front-end spec:     containers:      - name: nginx-container        image: nginx:1.7.0        ports:         - containerPort: 80      - name: postgres        image: postgres        env:         - name: POSTGRES_PASSWORD           value: mysecretpassword
```

> - **kind**: can be a pod, service, replicaSet or deployment.
> - **metadata**: is a dictionary with the data about the object.
> - **labels**: is a dictionary with several data defined freely.
> - **spec**: is a dictionary with thespecification of the pod.
> - **containers**: is a List/Array of the pod containers, that is, the list of containers that comprises the Pod.

Após a criação deste ficheiro, é possível criar o Pod `hello-world-rest-api-pod` com base na imagem `adfonte/hello-server` e expor a aplicação na porta 8080 das seguintes formas:

- **Criar um Pod Utilizando um Ficheiro YAML**:
    
    ```
    kubectl create -f pod-definition.yaml
    ```
    
    Este método utiliza o ficheiro de configuração para definir todas as propriedades do Pod (imagem, portas, labels, etc.).
- Ou **alternativamente**, Criar um Pod **Manualmente** (sem ficheiro YAML):
    
    ```
    kubectl run hello-world-rest-api-pod --image=adfonte/hello-server --port=8080
    ```
    
    Este comando cria diretamente o Pod a partir da linha de comandos, definindo a imagem e a porta exposta.

Depois de criado o Pod, é importante **saber como inspecioná-lo e geri-lo dentro do cluster Kubernetes**.

Os seguintes comandos permitem listar, obter detalhes e remover _Pods_ de forma simples através do `kubectl`:

- Listar os _Pods_ ativos: `kubectl get pods`.
- Obter informações detalhadas sobre _Pods_: `kubectl describe pods`.
- Remover um pod específico: `kubectl delete pod hello-world-rest-api-pod`.

### 2.4.5. _Replication Controllers (RC)_

Os Replication Controllers (RC) baseiam-se em _pod templates_ e definem um número fixo de _Pods_ que devem estar sempre a correr.

Um _Replication Controller_ monitoriza o cluster e substitui automaticamente os _Pods_ que falham, garantindo que o número desejado é mantido.

Veja o exemplo seguinte:

```
apiVersion: v1kind: ReplicationControllermetadata:  name: myapp-rc  labels:    app: myapp    type: front-endspec:  template:    metadata:      name: myapp-pod      labels:        app: myapp        type: front-end     spec:      containers:       - name: nginx-container         image: nginx:1.7.0         ports:          - containerPort: 80       - name: postgres         image: postgres         env:          - name: POSTGRES_PASSWORD            value: mysecretpassword  replicas: 3
```

> - **specs**: includes the specification of pods.
> - **template**: the template section includes the Pod template.
> - **replicas**: declares the number of replicas of pods.

Após a criação do ficheiro YAML, é possível implementar e verificar um _Replication Controller_ num cluster Kubernetes utilizando os seguintes comandos:

```
kubectl create -f rc-definition.yaml
kubectl get replicationcontroller
Kubectl get pods 
```

- `kubectl create -f rc-definition.yaml`: Cria um Replication Controller com base no ficheiro `rc-definition.yaml`.
- `kubectl get replicationcontroller`: Lista todos os _Replication Controllers_ ativos no cluster, mostrando informações como o nome, número de réplicas desejadas, disponíveis e em execução.
    - É útil para verificar o estado do RC criado e confirmar que está a funcionar corretamente.
- `Kubectl get pods` Mostra todos os _Pods_ atualmente em execução no cluster, incluindo os criados automaticamente pelo Replication Controller.
    - Permite confirmar se o RC criou o número correto de _Pods_ e se estes estão em estado “Running”.

**Resumo**  
Na realidade, um Replication Controller (RC) define um Pod template que serve de base para criar várias réplicas idênticas desse Pod.

- O que o RC faz é garantir que um número específico de _Pods_ baseados nesse template estão sempre em execução.
    - Se algum Pod falhar, o RC cria automaticamente outro com as mesmas especificações.

### 2.4.6. _Replica Sets_
Replica Sets são uma versão moderna e mais eficiente dos _replication controllers_ (veja secção 2.4.4). Além de manterem o número de _Pods_ ativos, utilizam um sistema de identificação através de _labels_ (selector values) para gerir e selecionar grupos de _Pods_ de forma mais flexível.

Veja o exemplo seguinte:

```
apiVersion: apps/v1kind: ReplicaSetmetadata:  name: myapp-replicaset  labels:    app: myapp    tier: front-endspec:  template:    metadata:      name: myapp-pod      labels:        app: myapp        tier: front-end     spec:      containers:      - name: nginx-container        image: nginx:1.7.0        ports:         - containerPort: 80      - name: postgres        image: postgres        env:         - name: POSTGRES_PASSWORD           value: mysecretpassword  replicas: 3  selector:    matchLabels:      tier: front-end
```

> - **apiVersion**: `apps/v1` this is the api version that supports ReplicaSet.
> - **selector**: enable replicaset to identify the pods to monitor by filtering them by their labels.
>     - Example: Filtering by tier label.

Após a criação do ficheiro YAML, é possível implementar e verificar um _Replica Set_ num cluster Kubernetes utilizando os seguintes comandos:

```
kubectl create -f replicaset-definition.yamlkubectl get replicasetKubectl get podsKubectl delete replicaset myapp-replicaset 
```

- `kubectl create -f replicaset-definition.yaml`: Cria um _ReplicaSet_ com base no ficheiro YAML especificado (e.g., replicaset-definition.yaml).
    - Este ficheiro define o _template_ do Pod, o número de réplicas desejadas e as _labels_ utilizadas para identificar e gerir esses _Pods_.
- `kubectl get replicaset`: Lista todos os _ReplicaSets_ atualmente ativos no cluster, mostrando o nome, o número de réplicas desejadas, disponíveis e em execução.
    - É útil para verificar se o _ReplicaSet_ foi criado corretamente e se está a funcionar como esperado.
- `kubectl get pods`: Mostra todos os _Pods_ geridos pelo ReplicaSet.
- `Kubectl delete replicaset myapp-replicaset`: Elimina o _ReplicaSet_ com o nome `myapp-replicaset` e, por defeito, remove também todos os _Pods_ geridos por ele.
    - Este comando é útil para limpar o ambiente ou testar a recriação automática de _Pods_.

### 2.4.7. _Kubernetes Deployments_
Os _Pods_ representam instâncias individuais da aplicação (por exemplo, uma aplicação Web), e cada container é encapsulado dentro de um Pod.

Para garantir disponibilidade e escalabilidade, múltiplos _Pods_ podem ser geridos por Replication Controllers ou ReplicaSets.

No entanto, o _**Deployment**_ **é um objeto Kubernetes** de nível superior que atua como uma camada de gestão avançada sobre os _ReplicaSets_. Com ele, é possível:

- Atualizar as instâncias da aplicação de forma contínua e sem interrupções (_rolling updates_).
- Reverter atualizações em caso de erros inesperados (_rollback_).
- Pausar e retomar atualizações para um controlo mais granular das mudanças.

Em suma, o Deployment oferece uma forma mais eficiente, controlada e automatizada de gerir o ciclo de vida das aplicações em Kubernetes.

Veja o exemplo seguinte:

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: myapp-deployment  labels:    app: myapp    tier: front-endspec:  template:    metadata:      name: mysql:5.7      labels:        app: myapp        tier: front-end     spec:      containers:      - name: nginx-container        image: nginx:1.7.0        ports:         - containerPort: 80      - name: postgres        image: postgres        env:         - name: POSTGRES_PASSWORD           value: mysecretpassword  replicas: 3  selector:    matchLabels:      tier: frontend
```

> - **kind**: Kubernetes object type (deployment) (…) the rest is identical to a replicaset.

Após a criação do ficheiro YAML, é possível implementar e verificar um _deployment_ num cluster Kubernetes utilizando os seguintes comandos:

```
kubectl create -f deployment-definition.yamlkubectl get deploymentskubectl get replicasets Kubectl get pods
```

- `kubectl create -f deployment-definition.yaml`: Cria um _Deployment_ com base no ficheiro YAML especificado (`deployment-definition.yaml`).
    - Este ficheiro define a imagem da aplicação, o número de réplicas desejadas, as labels, e o template dos _Pods_ a serem geridos.
    - O _Deployment_ cria automaticamente um ReplicaSet, que por sua vez cria e gere os _Pods_ definidos.
- `kubectl get deployments`: Lista todos os Deployments existentes no cluster, mostrando informações como o nome, número de réplicas desejadas, em execução e atualizadas.
    - Este comando é usado para verificar o estado e progresso do Deployment criado.
- `kubectl get replicasets`: Cada _Deployment_ cria e gere um ou mais ReplicaSets; por exemplo, quando ocorre uma atualização de versão, um novo ReplicaSet é criado automaticamente.
    - Este comando permite ver qual ReplicaSet está ativo e quantos _Pods_ estão a ser geridos por ele.
- `kubectl pods`: Lista todos os _Pods_ em execução no cluster, incluindo os criados pelo _Deployment_ (via ReplicaSet).
    - Permite confirmar que o número correto de _Pods_ foi criado e que estão no estado `Running`, validando assim que o _Deployment_ está a funcionar conforme esperado.

Estes comandos seguem a hierarquia natural do Kubernetes: _Deployment_ → _ReplicaSet_ → _Pods_, permitindo criar, inspecionar e validar cada nível de abstração da aplicação.

**Resumo**  
**No Kubernetes, o _Deployment_ é um objeto de mais alto nível que orquestra automaticamente todos os componentes necessários para colocar uma aplicação em execução e gerir o seu ciclo de vida.**

De forma simplificada:

- O Deployment é o controlador principal, este define como a aplicação deve ser executada (imagem, número de réplicas, estratégia de atualização, labels, etc.).
- A partir desta definição, o Kubernetes cria e gere ReplicaSets, que por sua vez criam e mantêm os _Pods_.

## 2.5. Locais de Implementação

==**Onde deve ser feita a instalação ou implementação do Kubernetes?**==

A implementação ou instalação do Kubernetes pode ser feita de duas formas principais:

- numa infraestrutura própria (_on-premises_);
- ou numa plataforma Cloud.

**Infraestrutura própria**: requer a criação manual do cluster, a instalação de um ambiente de _runtime_ (como Docker ou outro compatível) e a configuração de componentes essenciais, como _load balancing_, _logging_, monitorização, redes virtuais, sistema operativo, memória e armazenamento (_storage_). Esta abordagem oferece maior controlo, mas também aumenta a complexidade da gestão.

**Plataformas Cloud**: fornecedores como a Microsoft, através do Azure _Kubernetes Service_ (AKS), disponibilizam um ambiente Kubernetes totalmente integrado e gerido. Esta opção reduz significativamente a complexidade da implementação e manutenção, permitindo ao utilizador focar-se no desenvolvimento e gestão das aplicações, em vez da infraestrutura subjacente.

# 4. Kubernetes via Minikube

O [**Minikube**](https://minikube.sigs.k8s.io/docs/start/) é uma ferramenta que permite executar um cluster Kubernetes localmente, ideal para aprender, testar e desenvolver aplicações sem necessidade de uma infraestrutura complexa.

O objetivo desta ferramenta é o de simplificar a utilização do Kubernetes em ambientes de desenvolvimento, disponibilizando todas as funcionalidades essenciais num único nó local (_on-premises_).

Para o utilizar, basta ter instalado o Docker (ou outro ambiente compatível de _containers_ ou máquinas virtuais), e com apenas um comando, é possível iniciar um cluster local: `minikube start`.

Após a execução deste comando, o Minikube cria automaticamente um cluster Kubernetes funcional na máquina local.

## 4.1. Interagir com um Cluster

Depois de instalado, o **Minikube** pode ser facilmente controlado através de comandos simples na linha de comandos. **Estes comandos permitem iniciar, parar, pausar e configurar o cluster local Kubernetes. Por exemplo:**

- `minikube start`: inicia o cluster Kubernetes local.
- `minikube start --driver=docker`: inicia o cluster utilizando o Docker como driver, em vez de uma máquina virtual.
- `minikube stop`: para o cluster e liberta os recursos do sistema.
- `minikube pause`: pausa o Kubernetes sem afetar as aplicações que estão implementadas.
- `minikube unpause`: retoma o funcionamento do cluster após uma pausa.
- `minikube dashboard`: abre o painel de controlo gráfico (dashboard) do Kubernetes no navegador, permitindo visualizar e gerir os recursos do cluster.
- `minikube dashboard —url`: mostra apenas o endereço (URL) do dashboard, sem o abrir automaticamente.
- `minikube config set memory 16384`: aumenta o limite de memória do cluster para 16 GB (a alteração requer reiniciar o Minikube).

Estes comandos permitem gerir de forma fácil o ciclo de vida do cluster local, tornando o Minikube uma ferramenta prática e eficiente para aprendizagem, testes e desenvolvimento com Kubernetes.

## 4.2. _Deployment_ de uma Aplicação num Cluster

O processo de _deployment_ de uma aplicação num cluster Kubernetes é realizado através da ferramenta de linha de comando **`kubectl`**, que permite criar, expor e gerir aplicações de forma controlada e escalável.

No ambiente minikube o comando `kubectl` também pode ser utilizando para gerir um cluster local

Podemos considerar as seguintes etapas no processo de _deployment_ e gestão de uma aplicação num cluster Kubernetes:

1. **Criar um _Deployment_**: Utiliza-se o `kubectl` para criar um deployment, que define como a aplicação deve ser executada (imagem, número de réplicas, configurações, etc.).
2. **Expor a aplicação com o _NodePort_**: Permite tornar a aplicação acessível externamente, mapeando uma porta do nó (máquina física ou virtual) para uma porta do serviço dentro do cluster.
3. **Configurar o serviço _LoadBalancer_**: Cria automaticamente um balanceador de carga que distribui o tráfego entre múltiplas instâncias da aplicação, garantindo maior desempenho e disponibilidade.

Adicionalmente, e se necessário:

4. **Configurar o _AutoScaling_**: Além do balanceamento de carga, é possível configurar o escalonamento automático (AutoScaling), ajustando dinamicamente o número de _Pods_ em função da carga de trabalho.
5. **Obter informações sobre os recursos do cluster**: Comandos como `kubectl get pods`, `kubectl get services`, `kubectl get replicasets` e `kubectl get deployments` permitem monitorizar o estado dos diferentes componentes e recursos em execução.
6. **Eliminar _Pods_, Serviços ou _Deployments_**: Recursos podem ser removidos com comandos como `kubectl delete pod`, `kubectl delete service` ou `kubectl delete deployment`, conforme a necessidade.

Este conjunto de operações e comandos permite gerir o ciclo de vida completo de uma aplicação no Kubernetes, desde a sua criação até à sua exposição, escalonamento e eventual remoção.

## 4.3. Rollout and Versioning

Um _rollout_ é o processo de distribuição gradual ou atualização dos _containers_ de uma aplicação no Kubernetes.

Quando um _Deployment_ é criado pela primeira vez, ele inicia automaticamente um _rollout_, gerando uma nova revisão (_revision_), entitulado por Revisão 1.

Mais tarde, quando a aplicação é atualizada (por exemplo, ao alterar a versão da imagem do _container_), o Kubernetes executa um novo _rollout_, criando uma nova revisão, chamada Revisão 2.

Cada rollout corresponde, portanto, a uma nova versão controlada do Deployment, o que permite:

- **Acompanhar facilmente as alterações** feitas ao longo do tempo.
- **Efetuar _rollback_** (reverter) para uma versão anterior caso ocorra algum problema durante a atualização.

**Resumo**  
O sistema de _rollouts_ e revisões no Kubernetes oferece um controlo de versões integrado para deployments, garantindo _atualizações seguras, reversíveis e sem interrupções de serviço_.

## 4.4. Tipos de Estratégias de _Deployment_

Quando uma aplicação necessita de ser atualizada no Kubernetes, o _Deployment_ pode aplicar diferentes estratégias de _rollout_ para gerir a substituição das instâncias existentes pela nova versão (Figura X). As duas estratégias mais comuns são as seguintes:

1. _**Recreate**_ - Nesta estratégia, todas as instâncias atuais são primeiro destruídas. Apenas depois são criadas todas as novas instâncias da aplicação atualizada.
    - Este método é simples, mas provoca downtime, já que não ficam instâncias disponíveis durante a transição.
2. _**Rolling Update**_ (padrão) — Nesta abordagem, as instâncias são substituídas de forma gradual, atualizando uma a uma.
    - Novas réplicas são criadas enquanto as antigas são removidas, permitindo que a aplicação permaneça disponível ao longo de todo o processo, garantindo zero _downtime_ na maioria dos cenários.

![](https://i.imgur.com/gvyyHPr.png)  
Fig 14.Tipos de Estratégias de Deployment.

==Exemplo – Atualização de um _Deployment_==

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: myapp-deployment  labels:    app: myapp    type: front-endspec:  template:    metadata:      name: mysql:5.7      labels:        app: myapp        type: front-end     spec:      containers:      - name: nginx-container        image: nginx:latest        ports:         - containerPort: 80      - name: postgres        image: postgres        env:         - name: POSTGRES_PASSWORD           value: mysecretpassword  replicas: 3  selector:    matchLabels:      Tier: front-end
```

Depois de criar um Deployment, o Kubernetes oferece um conjunto de comandos para atualizar a aplicação, verificar o estado das alterações, consultar o histórico de versões e, se necessário, reverter para uma versão anterior.

**Atualizar a Aplicação**  

- `kubectl apply -f deployment-definition.yml`: Aplica alterações ao Deployment com base no ficheiro YAML atualizado.
    - Este comando é geralmente usado quando se edita o YAML (por exemplo, para alterar a imagem, número de réplicas, estratégia, labels, etc.). É a forma recomendada para fazer atualizações declarativas.
- `kubectl set image deployment/myapp-deployment nginx=nginx:latest`: Atualiza a imagem do container diretamente pela linha de comando, sem editar o ficheiro YAML.
    - Aqui, o container chamado nginx recebe a nova imagem nginx:latest. Esta abordagem é prática para atualizações rápidas ou testes.

**Verificar o Estado e Histórico do _Rollout_**  

- `kubectl describe deployment deployment/myapp-deployment`: Mostra informações detalhadas sobre o _Deployment_: número de réplicas, eventos recentes, expansão dos _ReplicaSets_, estratégia usada e mais.
    - É útil para diagnosticar problemas de atualização.
- `kubectl rollout status deployment deployment/myapp-deployment`: Apresenta o estado atual do _rollout_, indicando se as novas instâncias já foram criadas e se a atualização foi concluída com sucesso.
- `kubectl rollout history deployment deployment/myapp-deployment`: Mostra o histórico de revisões do _Deployment_.
    - Permite consultar alterações anteriores e identificar as versões disponíveis para _rollback_.

## 4.5. Kubernetes Networking

Na arquitetura de rede do Kubernetes, ==**cada nó do cluster possui um endereço IP próprio**==, atribuído pela rede física onde está ligado. Este IP permite o acesso direto ao nó, por exemplo através de REST ou SSH.

Contudo, os pods (as unidades básicas onde correm containers) recebem endereços IP internos e dinâmicos, atribuídos por uma rede virtual criada especificamente pelo Kubernetes. Esta rede é separada da rede física dos nós e permite que todos os pods comuniquem entre si de forma transparente dentro do cluster (veja Figura 15).

![](https://i.imgur.com/QuQ7cu0.png)  
Fig 15. Kubernetes Networking.

**Na Figura 15**:

- Os nós mantêm os seus IPs físicos (e.g., `192.168.1.10` e `192.168.1.11`).
- Os pods criados nesses nós recebem IP internos virtuais (e.g., `10.32.0.94` e `10.32.0.14`), que não são acessíveis diretamente de fora do _cluster_.

Apesar de cada _pod_ possuir um IP único, estes IPs não são expostos externamente. **Sempre que uma aplicação precisa de receber tráfego** a partir do exterior, **é necessário criar um Service**.

Os Services permitem:

- Expor a aplicação ao exterior;
- Garantir IP estável mesmo que os _pods_ mudem;
- Distribuir tráfego entre vários pods (load balancing interno).

A forma como um Service é exposto depende do campo type no ServiceSpec, podendo ser, por exemplo, ClusterIP, NodePort ou LoadBalancer.

==**Kubernetes Networking with Minikube**== A Figura 16 ilustra o funcionamento a rede do Kubernetes quando utilizamos o Minikube, ilustrando a separação entre o host físico, o nó Kubernetes virtualizado criado pelo Minikube e os pods que correm dentro desse nó.

![](https://i.imgur.com/kmgJ4n8.png)  
Fig 16. Kubernetes Networking with Minikube.

A figura ilustra a hierarquia de redes quando se utiliza Minikube:

- **Host físico**: `192.168.1.10`
    - A máquina real onde o Minikube executa.
- **Nó kubernetes** (via Minikube): `192.168.1.2`
    - Máquina Virtual ou _container_ que executa o _cluster_.
- **Pods**: `10.244.0.x`
    - Rede virtual interna utilizada pelo Kubernetes.

## 4.6. Exemplo Prático 1 (Minikube + Docker)

1. **Instalar o Minikube**  
    Deve seguir o guia de instalação ou “_Get Started_” existente na seguinte hiperligação [https://shorturl.at/NzaCX](https://shorturl.at/NzaCX).
    
2. **Iniciar um Cluster Minikube**  
    `minikube start`
    
3. **Criar um _Deployment_ de um Serviço**  
    `kubectl create deployment [deployment_name] --image=[image_src]`
    
    Exemplo de utilização deste comando:
    
    ```
    tiagomiguelcs@mini ~ % kubectl create deployment hello-world-rest-api --image=adfonte/hello-serverdeployment.apps/hello-world-rest-api created
    ```
    
    Este comando cria um Deployment chamado `hello-world-rest-api`, utilizando a imagem `adfonte/hello-server` – O Kubernetes cria automaticamente um Pod com um único container baseado nesta imagem.
    
4. **Expor a Aplicação**  
    `kubectl expose deployment hello-world-rest-api --port=[port_number] --type=NodePort --name=[service_name]`
    
    Exemplo de utilização deste comando:
    
    ```
    tiagomiguelcs@mini ~ % kubectl expose deployment hello-world-rest-api --port=8080 --type=NodePort --name=hello-world-rest-api-serviceservice/hello-world-rest-api-service exposed
    ```
    
    Este comando cria um Service do tipo NodePort com o nome `hello-world-rest-api-service` – O Service atua como uma interface entre os utilizadores externos e a aplicação dentro do cluster, expondo a porta 8080 para permitir o acesso ao serviço a partir do exterior.
    
5. (Opcional) **Verificar os Serviços Ativos**  
    `kubectl get services`
    
    Exemplo de utilização deste comando:
    
    ```
    tiagomiguelcs@mini ~ % kubectl get servicesNAME                           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGEhello-world-rest-api-service   NodePort    10.101.240.43   <none>        8080:31743/TCP   23skubernetes                     ClusterIP   10.96.0.1       <none>        443/TCP          97s
    ```
    
    Lista todos os serviços atualmente ativos no cluster, incluindo o recém-criado `hello-world-rest-api-service`.
    
6. **_Forward_ de Portas**  
    `kubectl port-forward <pod_name> [local_port_number:internal_port_number]:[internal_port_number]` ou `minikube service hello-world-rest-api-service`
    
    Exemplo de utilização destes comandos:
    
    ```
    tiagomiguelcs@mini edumark % kubectl port-forward hello-world-rest-api-f4f95cc-btql6 7080:8080Forwarding from 127.0.0.1:7080 -> 8080Forwarding from [::1]:7080 -> 8080(...)tiagomiguelcs@mini ~ % minikube service hello-world-rest-api-service✅  Tunnel successfully started(...)📌  NOTE: Please do not close this terminal as this process must stay alive for the tunnel to be accessible ...(...)🏃  Starting tunnel for service hello-world-rest-service-lb2.✋  Stopped tunnel for service hello-world-rest-service-lb2.Opening service default/hello-world-rest-api-service in defaultbrowser...
    ```
    
    > The easiest way to access this service is to let minikube launch a web browser for you (may not work when using minikube on Docker), that is, using `minikube service hello-world-rest-api-service` command. Alternatively, use `kubectl` to forward the port
    
    Se necessitar, o nome do pod pode ser encontrado através do comando: `kubectl get pods`.
    
7. **Acesso à Aplicação/Serviço Web**  
    Podemos agora aceder ao serviço através do seguinte endereço [http://localhost:7080/messages/estudantes/TVCD](http://localhost:7080/messages/estudantes/TVCD) (Figura 11).
    
    ![](https://i.imgur.com/EmkaoPe.png)  
    Fig 11. Exemplo 1.
    
    Neste primeiro exemplo, a aplicação `hello-world-rest-api` é desacopolada, exposta e acessível externamente num cluster Kubernetes, utilizando apenas alguns comandos do `kubectl` num ambiente Minikube.
    
8. (Opcional) **Obter Detalhes do Serviço**  
    `kubectl describe services/hello-world-rest-api-service`
    
    ```
    tiagomiguelcs@mini ~ % kubectl describe services/hello-world-rest-api-serviceName:                     hello-world-rest-api-serviceNamespace:                defaultLabels:                   app=hello-world-rest-apiAnnotations:              <none>Selector:                 app=hello-world-rest-apiType:                     NodePortIP Family Policy:         SingleStackIP Families:              IPv4IP:                       10.101.240.43IPs:                      10.101.240.43Port:                     <unset>  8080/TCPTargetPort:               8080/TCPNodePort:                 <unset>  31743/TCPEndpoints:                10.244.0.3:8080Session Affinity:         NoneExternal Traffic Policy:  ClusterInternal Traffic Policy:  ClusterEvents:                   <none>
    ```
    
    Este comando mostra informações detalhadas sobre o serviço, incluindo a porta externo atribuído automaticamente pelo Kubernetes (no intervalo típico entre 30000 e 32767 para o tipo _NodePort_).
    
    Este é a porta que pode ser utilizado para aceder à aplicação externamente; por exemplo, através de um _browser_.
    

## 4.7. Exemplo Prático 2 (_Load Balancer_ Sem _Scaling_)
Quando se cria um Service do tipo _LoadBalancer_, o objetivo é expor a aplicação para fora do cluster e distribuir o tráfego de rede entre os _Pods_ existentes.

Neste exemplo prático é realizada a configuração do tipo LoadBalanecer sem Scaling, onde:

- O _LoadBalancer_ atua como um balanceador de carga estático: envia pedidos para os _Pods_ disponíveis.
- O número de _Pods_ (instâncias da aplicação) é fixo: só muda se for ajustado manualmente.
- É ideal para cenários de baixa carga ou testes locais, onde a quantidade de recursos é previsível.

1. **Instalar e Iniciar um Cluster Minikube** (Ver Exemplo 1)
    
2. **Criar o _Deployment_ de um Serviço**  
    `kubectl create deployment [deployment_name] --image=[image_src]`
    
    Exemplo de utilização deste comando:
    
    ```
    tiagomiguelcs@mini ~ % kubectl create deployment hello-world-rest-api-2 --image=adfonte/hello-serverdeployment.apps/hello-world-rest-api created
    ```
    
3. **Expor a Aplicação com _LoadBalancer_**  
    `kubectl expose deployment [deployment_name] --port=[port] --type=LoadBalancer --name=[service_name]`
    
    Exemplo de utilização deste comando:
    
    ```
    tiagomiguelcs@mini ~ % kubectl expose deployment hello-world-rest-api-2 --port=8080 --type=LoadBalancer --name=hello-world-rest-service-lb-2service/hello-world-rest-service-lb-2 exposed
    ```
    
    Este cria um Service chamado `hello-world-rest-service-lb-2`, do tipo _LoadBalancer_, que distribui o tráfego externo entre as instâncias do deployment.
    
    A aplicação é exposta na porta 8080, permitindo acesso externo ao cluster.
    
4. **Ativar o _LoadBalancer_ no Minikube**  
    `minikube tunnel`
    
    Exemplo de utilização deste comando:
    
    ```
    tiagomiguelcs@mini ~ % minikube tunnel✅  Tunnel successfully started📌  NOTE: Please do not close this terminal as this process must stay alive for the tunnel to be accessible ...🏃  Starting tunnel for service hello-world-rest-service-lb-2.\
    ```
    
    O comando `minikube tunnel` deve ser executado numa janela separada, pois ele cria um túnel de rede que permite ligar serviços do tipo _LoadBalancer_ ao ambiente local, simulando o comportamento de um balanceador de carga em nuvem.
    
5. (Opcional) **Verificar os Serviços Ativos**  
    `kubectl get services`
    
    Exemplo de utilização deste comando:
    
    ```
    tiagomiguelcs@mini ~ % kubectl get servicesNAME                           TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGEhello-world-rest-api-service   NodePort       10.101.240.43    <none>        8080:31743/TCP   19mhello-world-rest-service-lb-2   LoadBalancer   10.103.126.143   127.0.0.1     8080:30341/TCP   114skubernetes                     ClusterIP      10.96.0.1        <none>        443/TCP          20m
    ```
    
    Este comando lista todos os serviços disponíveis no cluster. Neste caso, o comando permite confirmar o endereço IP externo atribuído ao serviço do tipo _LoadBalancer_, o qual pode ser usado para aceder à aplicação.
    
6. **Escalar o _Deployment_ para 3 Réplicas**  
    `kubectl kubectl scale deployment [deployment_name] --replicas=[number_of_replicas]`
    
    Exemplo de utilização deste comando:
    
    ```
    tiagomiguelcs@mini ~ % kubectl scale deployment hello-world-rest-api-2 --replicas=3deployment.apps/hello-world-rest-api-2 scaled
    ```
    
    Este comando aumenta o número de _Pods_ ativos do deployment para três, garantindo maior capacidade de processamento e tolerância a falhas. O _LoadBalancer_ irá automaticamente distribuir o tráfego entre as três instâncias da aplicação.
    
7. **Acesso à Aplicação/Serviço Web**  
    Podemos agora aceder ao serviço através do seguinte endereço [http://localhost:8080/messages/estudantes/TVCD](http://localhost:8080/messages/estudantes/TVCD) (Figura 12).
    
    ![](https://i.imgur.com/Boy55Tu.png)  
    Fig 12. Exemplo 2.
    
    Neste segundo exemplo, a aplicação `hello-world-rest-api-2` é implementada, exposta e escalada de forma equilibrada, tirando partido do _LoadBalancer_ para assegurar alta disponibilidade e desempenho consistente.
    

## 4.8. Exemplo Prático 3 (_Load Balancer_ Com _Scaling_)

No exemplo anterior (Exemplo 2), foi explorado o scaling manual, onde o número de _Pods_ era ajustado explicitamente pelo utilizador.

**Neste Exemplo 3**, iremos explorar o **scaling automático**, em que o sistema passa a ajustar dinamicamente o número de _Pods_ em função da carga de trabalho.

O _LoadBalancer_ continua a distribuir o tráfego entre as instâncias, mas agora o Kubernetes pode aumentar ou reduzir automaticamente o número de _Pods_ conforme a procura, garantindo melhor desempenho, maior disponibilidade e eficiência no uso dos recursos.

O _LoadBalancer_ por si só apenas distribui tráfego; já o **_LoadBalancer_ com scaling** combina distribuição e adaptação automática, garantindo que a aplicação responde eficientemente a variações na procura, mantendo alto desempenho e disponibilidade.

1. Instalar e Iniciar um Cluster Minikube (Ver Exemplo 1)
    
2. Criar o Deployment de um Serviço (Ver Exemplo 2)
    
3. Expor a Aplicação com _LoadBalancer_ (Ver Exemplo 2)
    
4. **Configurar o _Scaling_ Automático (_Horizontal Pod Autoscaler_)**  
    `kubectl autoscale deployment [deployment_name] --min=[min_value] --max=[max_value] --cpu-percent=[percent]`
    
    Exemplo de utilização deste comando:
    
    ```
       tiagomiguelcs@mini ~ % kubectl autoscale deployment hello-world-rest-api-2 --min=2 --max=10 --cpu-percent=70
    ```
    
    No comando anterior, o Kubernetes ajusta o número de _Pods_ entre 2 e 10, conforme a utilização de CPU.
    

**Resumo**  
O scaling pode ser feito de duas formas:

- Manual – Exemplo 2: `kubectl scale deployment hello-world-rest-api --replicas=3`, onde o utilizador define explicitamente o número de réplicas (neste caso, 3).
- ou Automático (_Horizontal Pod Autoscaler_) – Exemplo 3: `kubectl autoscale deployment hello-world-rest-api --min=2 --max=10 --cpu-percent=70`, onde o Kubernetes ajusta o número de _Pods_ entre 2 e 10, conforme a utilização de CPU.

> ==**Horizontal scaling**== means that the response to increased load is to deploy more Pods. This is different from ==**vertical scaling**==, which for Kubernetes would mean assigning more resources (e.g., memory or CPU) to the Pods.

## 4.9. Exemplo Prático 4 (Metrics-Server)

O _metrics-server_ é um **agente de monitorização** interna do Kubernetes que recolhe métricas em tempo real a partir do _Kubelet_ (o agente que corre em cada nó do cluster).

Estas métricas são depois disponibilizadas à API do Kubernetes, sendo utilizadas por componentes como o _Horizontal Pod Autoscaler_ (HPA) ou o _Vertical Pod Autoscaler_ (VPA) para decidir quando escalar recursos.

Sem o _metrics-server_, o Kubernetes não iria conseguir medir a utilização de recursos dos _Pods_, e, por consequência, não poderia escalar automaticamente com base em métricas de carga.

==**Ativar o _metrics-server_ no Minikube**==

No **Minikube**, o **_metrics-server_ não está ativo por defeito**, sendo necessário ativá-lo manualmente. Este componente é um _addon_ oficial do Kubernetes responsável por recolher métricas de desempenho (CPU, memória, etc.) necessárias para o _scaling automático_.

Para ativar o addon, basta executar o seguinte comando:

```
tiagomiguelcs@mini ~ % minikube addons enable metrics-server💡  metrics-server is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub (...)🌟  The 'metrics-server' addon is enabled
```

Pode ainda confirmar se o _addon_ já se encontra ativo com: `minikube addons list`.

Uma vez instalado e configurado, o metrics-server pode ser utilizado para recolher métricas de desempenho do cluster.

Os comandos apresentados nas secções seguintes permitem confirmar o seu funcionamento e consultar as métricas disponíveis para suportar o scaling automático dos _Pods_.

==**Consultar métricas diretamente através da Metrics API**==

O metrics-server disponibiliza uma API própria, chamada [metrics.k8s.io](http://metrics.k8s.io/), que pode ser consultada diretamente:

- Obter métricas de todos os nós do cluster: `kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes` – Estas métricas incluem informações como CPU _usage_, _memory_ _usage_ e _timestamp_ da recolha.
- Alternativamente, é possível consultar métricas de uma forma mais simples e legível através do comando: `kubectl top pods` – Este comando mostra uma tabela com o uso atual de CPU e memória de cada pod, permitindo monitorizar a performance dos recursos do cluster em tempo real.
    
    ```
    tiagomiguelcs@mini ~ % kubectl top podsNAME                                    CPU(cores)   MEMORY(bytes)hello-world-rest-api-f4f95cc-btql6      2m           424Mihello-world-rest-api-f4f95cc-g4stf      2m           347Mihello-world-rest-api-f4f95cc-j6gnv      2m           347Mihello-world-rest-api2-9ff98cdc7-2c25x   2m           398Mihello-world-rest-api2-9ff98cdc7-dnfvj   2m           354Mihello-world-rest-api2-9ff98cdc7-kbqqg   3m           390Mi
    ```
    

**Resumo**  
O _metrics-server_ permite recolher e visualizar métricas em tempo real sobre os recursos utilizados por _Pods_ e nós, sendo uma ferramenta essencial para monitorização e escalabilidade automática no Kubernetes.

## 4.10. Exemplo Prático 5 (Replication Controller + _LoadBalancer_)

O objetivo deste exemplo é demonstrar a criação de um _Replication Controller_ chamado `hello-world-rest-api-rc`, baseado na imagem `adfonte/hello-server`, com 3 réplicas da aplicação, e expor esse controlador através de um serviço do tipo _LoadBalancer_ na porta 8080.

```
apiVersion: v1kind: ReplicationControllermetadata:     name: hello-world-rest-api-rc     labels:       app: hello-world-rest-api       type: front-end spec:    template:       metadata:            name: hello-world-rest-api-pod            labels:              app: hello-world-rest-api              type: front-end        spec:            containers:             - name: hello-world-rest-api               image: adfonte/hello-server               ports:                - containerPort: 8080    replicas: 3
```

```
tiagomiguelcs@mini ~ % kubectl create -f rc-definition.yamlreplicationcontroller/hello-world-rest-api-rc createdtiagomiguelcs@mini ~ % kubectl expose replicationcontroller hello-world-rest-api-rc --port=8080 --type=LoadBalancer --name=hello-world-rest-service-lbservice/hello-world-rest-service-lb exposed
```

```
tiagomiguelcs@mini ~ % kubectl get allNAME                               READY   STATUS  RESTARTS  AGE(...)pod/hello-world-rest-api-rc-bl8rk  1/1     Running   0       4m3spod/hello-world-rest-api-rc-w9bql  1/1     Running   1       4m3spod/hello-world-rest-api-rc-znlw2  1/1     Running   0       4m3s          NAME                                           DESIRED CURRENT READY AGEreplicationcontroller/hello-world-rest-api-rc  3       3       3     4m3sNAME                                   TYPE           CLUSTER-IP ... service/hello-world-rest-service-lb    LoadBalancer   10.109.52.251    (...)
```

Adicionalmente, podemos **escalar o serviço** de 3 para 6 replicas da seguinte forma:

```
tiagomiguelcs@mini ~ % kubectl get replicationcontrollerNAME                      DESIRED   CURRENT   READY   AGEhello-world-rest-api-rc   3         3         3       9m39stiagomiguelcs@mini ~ % kubectl scale --replicas=6 -f rc-definition.yamlreplicationcontroller/hello-world-rest-api-rc scaledtiagomiguelcs@mini ~ % kubectl get replicationcontrollerNAME                      DESIRED   CURRENT   READY   AGEhello-world-rest-api-rc   6         6         6       10m
```

## 4.11. Exemplo Prático 6 (Replica Set + _LoadBalancer_)

O objetivo deste exemplo é demonstrar a criação de um _ReplicaSet_ chamado `hello-world-rest-api-rs`, baseado na imagem `adfonte/hello-server`, com 3 réplicas da aplicação.

- Os _Pods_ serão identificados e filtrados através da label `tier=frontend`, e o _ReplicaSet_ será exposto externamente na porta 8080 utilizando um serviço do tipo _LoadBalancer_.

```
apiVersion: apps/v1kind: ReplicaSetmetadata:     name: hello-world-rest-api-rs     labels:       app: hello-world-rest-api       tier: front-end spec:    replicas: 3    selector:       matchLabels:        tier: front-end    template:       metadata:        name: hello-world-rest-api-pod        labels:              app: hello-world-rest-api              tier: frontend        spec:            containers:             - name: hello-world-rest-api               image: adfonte/hello-server               ports:                - containerPort: 8080
```

```
tiagomiguelcs@mini ~ % kubectl create -f replicaset-definition.yamlreplicaset.apps/hello-world-rest-api-rs createdtiagomiguelcs@mini ~ % kubectl expose replicaset hello-world-rest-api-rs --port=8080 --type=LoadBalancer --name=hello-world-rest-service-lb-3service/hello-world-rest-service-lb-3 exposed
```

```
tiagomiguelcs@mini ~ % kubectl get allNAME                                    READY   STATUS    RESTARTS   AGEpod/hello-world-rest-api-rs-5drvn       1/1     Running   0          4mpod/hello-world-rest-api-rs-bzqnk       1/1     Running   0          4mpod/hello-world-rest-api-rs-fgzmj       1/1     Running   0          4mNAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGEservice/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP    7dNAME                                    DESIRED   CURRENT   READY   AGEreplicaset.apps/hello-world-rest-api-rs     3         3        3     4m(...)
```

No ReplicaSet, o Kubernetes não cria nem gere _Pods_ de forma aleatória, este identifica e controla _Pods_ com base nas labels definidas no seletor (_selector_) do _ReplicaSet_.

==**Em termos práticos**==

O ficheiro YAML do ReplicaSet contém uma secção chamada selector, que define quais os _Pods_ que ele deve gerir.

Todos os _Pods_ que tiverem as mesmas labels especificadas no selector passam a ser geridos por esse ReplicaSet.

Por exemplo:

```
(...)selector:  matchLabels:    tier: frontend(...)
```

Isto significa que o ReplicaSet irá controlar todos os _Pods_ com a label _tier_ = _frontend_, tanto os que forem criados por ele como os que já existiam previamente no cluster (veja Figura 13).

![](https://i.imgur.com/CJbyCkJ.png)  
Fig 13. Create a Replicaset with Yaml.

Adicionalmente, e à semelhança do exemplo anterior, **podemos escalar o serviço** de 3 para 6 replicas mediante um dos seguintes métodos:

==**Método 1 – Editar o ficheiro YAML**==

A forma mais direta é alterar o número de réplicas no ficheiro de definição e aplicar novamente a configuração:

1. Editar o ficheiro replicaset-definition.yaml e atualizar o campo: `replicas: 6`
    
2. Aplicar as alterações ao cluster: `kubectl replace -f replicaset-definition.yaml`
    

Este método atualiza também o ficheiro de configuração, mantendo a definição do ReplicaSet consistente com o estado atual no cluster.

==**Método 2 – Usar o comando `kubectl scale`**==

Outra opção é escalar diretamente pela linha de comandos, sem modificar o ficheiro YAML:

```
kubectl scale --replicas=6 -f replicaset-definition.yaml
```

ou

```
kubectl scale --replicas=6 replicaset.apps/hello-world-rest-api-rs
```

Esta abordagem atualiza o número de réplicas no cluster, mas não altera o ficheiro YAML original, ou seja, a configuração guardada permanece com o valor anterior.

==**Método 3 – Escalonamento Automático (_autoscale_)**==

Por fim, é possível configurar o escalonamento automático com base na carga usando o _Horizontal Pod Autoscaler_:

```
kubectl autoscale replicaset hello-world-rest-api-rs --min=3 --max=10 --cpu-percent=70
```

Neste modo, o Kubernetes ajusta automaticamente o número de _Pods_ entre 3 e 10, consoante a utilização média de CPU (neste caso, 70%).

## 4.12. Exemplo Prático 7 (Deployment + _LoadBalancer_)

O objetivo deste exemplo é a criação de um _Deployment_ chamado `hello-world-rest-api-deployment`, baseado no ReplicaSet anterior, mantendo a mesma imagem e configuração de _Pods_.

Neste novo _Deployment_, será definida uma estratégia de atualização do tipo _RollingUpdate_, garantindo atualizações contínuas e sem interrupções. Por fim, o _Deployment_ será exposto externamente na porta 8080 através de um serviço do tipo _LoadBalancer_.

```
apiVersion: apps/v1kind: Deploymentmetadata:     name: hello-world-rest-api-deployment     labels:       app: hello-world-rest-api       tier: front-end spec:    replicas: 3    strategy:       type: RollingUpdate    selector:       matchLabels:        tier: front-end    template:       metadata:        name: hello-world-rest-api-pod        labels:              app: hello-world-rest-api              tier: front-end        spec:            containers:             - name: hello-world-rest-api               image: adfonte/hello-server               ports:                - containerPort: 8080
```

```
tiagomiguelcs@mini ~ % kubectl create -f deployment-definition.yamldeployment.apps/hello-world-rest-api-deployment createdtiagomiguelcs@mini ~ % kubectl get allNAME                                                   READY   ...            pod/hello-world-rest-api-deployment-7d645f687f-9fr2q   0/1     pod/hello-world-rest-api-deployment-7d645f687f-jb7tz   0/1     pod/hello-world-rest-api-deployment-7d645f687f-nrsxr   1/1                 NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGEservice/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP    7d6hNAME                                         READY   UP-TO-DATE   AVAILABLEdeployment.apps/hello-world-rest-api-deployment   1/3     3            1           NAME                                                      DESIRED CURRENT replicaset.apps/hello-world-rest-api-deployment-7d64f687f   3     3
```

# 5. _Azure Kubernetes Service_ (AKS)

O _Azure Kubernetes Service_ (AKS) oferece um ambiente Kubernetes totalmente integrado na cloud da Azure, eliminando a complexidade de instalar, configurar e manter um cluster Kubernetes em infraestrutura física (Figura 17).

No AKS, o **control plane** (máquina de gestão do cluster) é fornecido gratuitamente pela Azure; os custos recaem apenas sobre as máquinas virtuais usadas como worker nodes, bem como sobre o armazenamento e a componente de rede associada.

![](https://i.imgur.com/NDdzTJ4.png)  
Fig 17. Azure Kubernetes Service (AKS)

Encontra-se disponível em [K8s-AKS](https://di.ubi.pt/~tsimoes/resources/17912/K8s-AKS.pdf) um tutorial introdutório prático sobre K8s no AKS, que pode ser seguido para obter uma compreensão prática dos conceitos fundamentais na criação de _clusters_ Kubernetes no Azure.