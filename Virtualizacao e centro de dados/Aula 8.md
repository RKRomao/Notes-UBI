## 1. Introdução aos Microsserviços
### **O que são microsserviços?**

De uma forma muito resumida, os microsserviços são pequenos **serviços** **autónomos** que **trabalham em conjunto**.

À medida que escrevemos novas funcionalidades para uma determinada aplicação ou ambiente, o **código base cresce** (abordagem monolítica). Como o tempo, pode ser difícil saber onde uma alteração precisa deve ser feita, isto porque o código base da aplicação torna-se **complexo e extenso**.

O código determina com uma função e/ou implementação começa a dispersar-se por todo o lado, tornando cada vez mais difícil corrigir erros ou implementar novas funcionalidades.

Num **sistema monolítico**, lutamos para manter o código coeso, criando abstrações e módulos que permitem a simplificação na implementação de novas funcionalidades.

**Coesão** → O impulso para agrupar código que se encontra de alguma forma relacionado, este é um conceito importante quando pensamos em microsserviços.

Numa abordagem baseada em microsserviços concentramos as fronteiras dos nossos serviços nas fronteiras do negócio, tornando óbvio a localização de um determinado código para a funcionalidade XX ou YY. Ao assegurar estas fronteiras, evitamos a tentação de tornar a implementação de uma determinada funcionalidade demasiado extensa, evitando assim todas as dificuldades associadas.

### **Qual o tamanho ideal para um microsserviço?**

Não existe um número mágico para o número máximo de linhas num microsserviço.  

⚠️ **A atribuição de um valor é problemática!**

Existem linguagens de programação que são mais expressivas que outras, onde existe a necessidade de mais ou menos linhas de código para realizar a implementação de algo. Existe também a questão relacionada com a utilização de bibliotecas externas, dependências, que por si só contêm muitas linhas de código.

Uma pista importante que pode ajudar a responder a esta questão é se o código base atual de um microsserviço pode ou não, ser gerido pela atual equipa responsável. Caso não seja possível, este é um claro indício que o código base do microsserviço deve ser reestruturado e/ou dividido.

## 1.2. Microsserviços Autónomos
Os microsserviços são **entidades separadas e autónomas**. Estes podem ser isoladamente implementados numa plataforma como serviço (PaaS), ou até mesmo como um processo num sistema operativo.

Devemos sempre tentar evitar agrupar múltiplos serviços numa mesma máquina.

Embora a utilização deste conceito de isolamento possa adicionar alguma sobrecarga, a simplicidade resultante torna o nosso sistema distribuído muito mais **fácil de entender e manter**. As tecnologias atuais conseguem mitigar muitos dos desafios associados a estes tipos de implementações.

Todas as comunicações entre os serviços são realizadas por meio da rede, o que possibilita impor a separação entre os serviços e previne riscos de acoplamento rígido.

As **alterações aos microsserviços** devem acontecer de forma independente, a sua distribuição ou _deployment_ também deve acontecer de forma que não exista a necessidade de alterar os consumidores dos serviços.

Devemos pensar sobre o que os nossos serviços devem expor e o que devem esconder. Se existir demasiada partilha, os nossos serviços consumidores ficam acoplados às nossas representações internas — Isto **diminui** a nossa **autonomia**, uma vez que existe a necessidade de uma maior coordenação adicional com os consumidores ao fazer alterações aos microsserviços que os alimentam.

**O conceito chave** para resolver esta problemática → Implementar _decoupling services_ (_i.e.,_ microsserviços desacopolados).

Para desacoplar adequadamente os serviços, necessitamos de os modelar e implementar APIs de forma correta e adequada.

## 1.3. Benefícios-Chave
Os **benefícios dos microsserviços são vastos e variados**. Muitos destes podem ser atribuídos a um qualquer sistema distribuído. No entanto, os microsserviços tendem a atingir estes benefícios numa maior escala, principalmente devido à forma como existe uma implementação ou adoção, a um nível mais avançado, dos conceitos dos sistemas distribuídos e das arquiteturas orientadas a serviços.

**Ao longo das próximas secções iremos discutir os diferentes benefícios-chave inerentes à utilização ou adoção dos microsserviços:**

1. Heterogeniedade de Tecnologias (do inglês, _technology heterogeneity_);
2. Resiliência (do inglês, _resilience_);
3. Dimensionamento (do inglês, _scaling_);
4. Facilidade de Distribuição/_Deployment_ (do inglês, _ease of deployment_);
5. Alinhamento Organizacional (do inglês, _organizational alignment_);
6. Composição (do inglês, _composability_);
7. Substituível (do inglês, _replaceability_).

### 1.3.1. Heterogeneidade das Tecnologias

Num sistema composto por **múltiplos serviços** que colaboram entre si, podemos optar por utilizar **diferentes tecnologias para cada um deles**. Assim, existe a possibilidade de escolher a ferramenta ou tecnologia correta para cada tarefa/serviço, em vez de optar por uma abordagem mais padronizada.

**Exemplo (Figura 1)**  
Se uma determinada parte do nosso sistema necessitar de um melhoramento em termos de desempenho, podemos optar por utilizar um conjunto de tecnologias diferentes que sejam mais capazes de atingir os níveis de desempenho desejados. Também podemos optar por uma forma diferente de armazenamento — Numa aplicação para uma rede social, podemos armazenar as interações dos utilizadores numa **base de dados orientada a grafos** e as publicações dos utilizadores podem ser armazenadas numa **base de dados orientadas a documentos** (_i.e.,_ NoSQL).

![](https://i.imgur.com/Q7FO3Vy.png)  
Fig 1. Os microsserviços permitem a utilização de diferentes tecnologias.  
Source: "_Building Microservices, Designing Fine-Grained Systems_".

Com a adoção de uma abordagem baseada em microsserviços é possível ainda **adotar tecnologias** mais rapidamente e compreender melhor os novos avanços na sua utilização.

Uma das maiores barreiras para experimentar e adotar **novas tecnologias** são os riscos associados à sua utilização. Com uma **aplicação monolítica**, se existir a necessidade de utilizar uma nova linguagem de programação, base de dados ou _framework_, qualquer alteração irá impactar de forma massiva numa grande parte do sistema. Num sistema composto por múltiplos **microsserviços**, existem múltiplos locais onde uma nova tecnologia pode ser experimentada/adotada - Tipicamente, são escolhidos serviços de menor risco, onde é possível limitar qualquer impacto negativo. Muitas empresas consideram esta capacidade de absorver mais rapidamente novas tecnologias o verdadeiro benefício da adoção de uma abordagem baseada em microsserviços.

**A adoção de novas tecnologias pode resultar em custos adicionais.**

Algumas empresas optam por impor restrições nas escolhas das suas linguagens de programação, frameworks e/ou ambientes. Por exemplo, a _Netflix_ e o _Twitter_ (_X_) usam a _Java Virtual Machine_ (JVM), uma vez que têm uma grande compreensão da fiabilidade e desempenho deste sistema. No entanto, a utilização da JVM torna mais difícil a comunicaçãos com outros serviços ou clientes que não são baseados em tecnologias baseadas na linguagem de programação _Java_.

### 1.3.2. Resiliência

Um conceito-chave na engenharia de resiliência é o **padrão _bulkhead_**. Especificamente, se um componente de um sistema falha, mas essa falha não se propaga, é possível isolar o problema e o resto do sistema pode continuar a funcionar.

Num ambiente monolítico, se o serviço falhar, tudo irá parar de funcionar. Para mitigar este problema, podemos executar o ambiente em várias máquinas de forma distribuída numa tentativa de reduzir possíveis falhas. No caso dos microsserviços, podemos construir sistemas que lidam com a falha total dos serviços.

**Necessitamos de ter algum cuidado!**  
Para garantir que os nossos sistemas de microsserviços conseguem “abraçar” adequadamente esta resiliência, necessitamos de compreender as fontes da falha com as quais os sistemas distribuídos têm que lidar. As redes podem falhar, assim como as próprias máquinas. É necessário saber lidar com esses fatores e com o possível impacto (caso exista) nos utilizadores finais do sistema.

### 1.3.3. Dimensionamento

Num sistema **monolítico** de grande escala, e quando existe a necessidade de o dimensionar, **todo o sistema tem que ser redimensionado**. Com serviços menores, podemos apenas redimensionar os serviços que necessitam de ser escalados, permitindo assim executar outras partes do sistema em _hardware_ menos potente – Veja-se exemplo ilustrado na Figura 2.

![](https://i.imgur.com/uIyNqrt.png)  
Fig 2. É possível escalar, dimensionar, apenas aqueles microsserviços que necessitam de mais recursos. Source: "_Building Microservices, Designing Fine-Grained Systems_".

**Exemplo**  
Quando existe a adoção de serviços _on-demand_, como aqueles fornecidos pela Amazon AWS, podemos até mesmo aplicar esta escalabilidade _on-demand_ para partes específicas do nosso sistema que necessitam de mais ou menos recursos. Isto irá permitir um maior controlo de custos.

### 1.3.4. Facilidade de Distribuição/_Deployment_

Uma alteração numa linha numa aplicação monolítica, que, por exemplo, tem um milhão de linhas de código, requer um _redeploy_ de toda a aplicação.

**O _deployment_ de aplicações monolíticas resultam numa implementação ou _deployment_ de alto impacto e risco!**  
Na prática, a distribuição ou _deployment_ de alto impacto e risco são raras, existe sempre um medo racional e compreensível de que alguma coisa irá “partir” - As alterações feitas às aplicações monolíticas são incrementais entre lançamentos e versões, permitindo uma ronda maior de testes e verificações. As “novas” funcionalidades são colocadas em produção após n+n+ incrementações e verificações.

Com a adoção de uma abordagem baseada em microsserviços, as **alterações** podem ser feitas num único serviço e a sua distribuição é feita de forma independente do resto do sistema. Isto permite realizar o _deployment_ de código de forma mais rápida e segura. **Se existir um problema, um serviço pode ser isolado rapidamente, facilitando o _rollback_ rápido.**

**Exemplo**  
Podemos disponibilizar **novas funcionalidades** aos clientes de forma mais rápida. Estas é uma das principais razões pelas quais empresas como a _Amazon_ e a _Netflix_ utilizam estas arquiteturas - Garantir que removem o máximo de obstáculos possíveis para lançar novos produtos e/ou funcionalidades em produtos existentes.

### 1.3.5. Alinhamento Organizacional

Equipas extensas e bases de códigos extensos são evidentes, e gritantes problemas nas organizações atuais. Estes problemas podem ser exacerbados quando a própria equipa se encontra distribuída. Por outro lado, sabemos que **equipas mais pequenas** a trabalhar em bases de código tendem a ser muito **mais produtivas**.

Os microsserviços permitem alinhar melhor uma arquitetura com a organização, ajudando a minimizar o número de pessoas a trabalhar numa única implementação/código e/ou funcionalidade de forma a atingir o ponto ideal em termos de tamanho da equipa de programadores e níveis de produtividade.

### 1.3.6. Composição

Uma das principais promessas dos sistemas distribuídos e das arquiteturas orientadas a serviços é que abrimos **oportunidades para a reutilização de funcionalidades.**

A adoção de microsserviços possibilita consumir funcionalidades de diferentes formas (_e.g.,_ _mobile apps, cloud applications, IoT devices_).

Já não é possível pensar que uma aplicação Web ou móvel é restrita a um determinado ambiente ou fim. Atualmente é necessário considerar inúmeras formas como podemos integrar e interagir com aplicações Web, nativas e móveis.

Numa abordagem baseada em microsserviços a composição refere-se à integração e coordenação dos diferentes microsserviços que compõem uma aplicação, seja por meio de orquestração, API _Gateway_, eventos ou outras técnicas. Essa abordagem permite construir aplicações escaláveis, flexíveis e resilientes, divididas em componentes independentes e interconectados.

### 1.3.7. Substituível

O contacto com sistemas de _legacy_ software é inevitável - Estes são muitas vezes sistemas de larga escala, problemáticos, e difíceis de gerir. No entanto, estes sistemas continuam a ser vitais para o funcionamento de certas empresas.

**Legacy System = Aquele sistema que ninguém quer mexer/manter/alterar/implementar/reimplementar!**

Sistemas escritos durante décadas de uma forma estranha e variável e muitas vezes executados em _hardware_ considerado obsoleto.

**Por que razão estes _legacy systems_ não são substituídos?**

A resposta a esta questão é sempre **variável**, no entanto, e de uma forma genérica, podemos afirmar que estes **trabalhos de migração** são sempre considerados **extensos** e **arriscados**.

**Com a adoção de microsserviços, as problemáticas relacionadas com a substituição de grandes volumes de código é inexistente**. O custo de substituir pequenas porções de códigos, num ou mais microsserviços, é muito reduzido comparativamente à reescrita ou migração de um _software legacy_ escrito de uma forma monolítica.

**Com que frequência já excluiu mais de cem linhas de código num único dia e não se preocupou muito com isso?**  
Com os microsserviços, sendo frequentemente de tamanho semelhante, as nossas barreiras “emocionais” para reescrever ou remover serviços são baixas.

Quando necessário, as equipas que adotaram uma abordagem baseada em microsserviços estão completamente confortáveis com a reescrita ou reimplementação completa de serviços. Quando uma base de código tem apenas algumas centenas de linhas, é difícil uma ligação emocional, sendo que o custo de substituição é bastante pequeno.

**Microsserviços Vs. _Containers_**  
Os _containers_ (com Docker, Podman, etc.) são uma forma de empacotar e executar aplicações de modo isolado e portátil:

- Cada container contém a aplicação e suas dependências.
- É uma ferramenta de infraestrutura, não um padrão de arquitetura.

Os _containers_ são uma solução prática para distribuir e executar microsserviços de forma eficiente.

# 2. Introdução aos _Containers_

Em **2000**, foi introduzido no FreeBSD o conceito de _jails_, uma das primeiras implementações de uma tecnologia baseada em _containers_.

As FreeBSD _jails_ permitem dividir um sistema FreeBSD em vários subsistemas independentes, designados por _jails_.

Estas _jails_ foram desenvolvidas como ambientes seguros, que os administradores de sistema podiam partilhar com múltiplos utilizadores, tanto internos como externos a uma organização, garantindo isolamento e segurança entre cada instância.

Posteriormente, em **2001**, Jacques Gélinas criou o projeto VServer, que possibilitava executar vários servidores Linux de propósito geral num único sistema físico, assegurando um elevado nível de independência e segurança entre eles. A solução VServer foi o primeiro esforço no Linux para separar o ambiente de utilizador (_user-space_) em unidades distintas, conhecidas como Servidores Virtuais Privados ou _Virtual Private Servers_ (VPS). Cada VPS comportava-se como um servidor independente, oferecendo aos processos que nele corriam a mesma aparência e funcionalidade de um sistema real.

Em **2006**, Paul Menage adaptou o mecanismo de _cpusets_, já existente no kernel do Linux, introduzindo alterações mínimas e pouco intrusivas, com impacto reduzido no desempenho, na qualidade do código, na complexidade e na compatibilidade futura. O resultado deste trabalho foi a criação dos _process containers genéricos_, que mais tarde foram renomeados para _control groups_, ou simplesmente _cgroups_, **sendo este um dos pilares fundamentais das tecnologias modernas de _containers_ no Linux.**

Em **2008**, Eric W. Biederman introduziu o conceito de _user namespaces_ no Linux. Esta implementação permitiu que cada processo tivesse o seu próprio conjunto de utilizadores, possibilitando, em particular, que um processo tivesse privilégios de _root_ num _container_, mas sem qualquer privilégio fora dele.

Por volta do mesmo período, engenheiros da IBM criaram o projeto ==_Linux Containers_ (LXC)==, que adicionava ferramentas de _userspace_ sobre as funcionalidades de _cgroups_ e _namespaces_, já existentes e implementadas no kernel.

O projeto LXC representou um marco importante na evolução dos _containers_ Linux, ao proporcionar uma experiência de utilização mais simples e integrada, tornando a gestão de _containers_ mais acessível e prática para administradores e programadores.

Ainda em **2008**, surgiu a Docker, inicialmente desenvolvida pela empresa dotCloud, trazendo consigo a sua tecnologia de _containers_ que revolucionaria como as aplicações são distribuídas e executadas.

A tecnologia Docker introduziu novos conceitos e ferramentas que simplificaram drasticamente a criação, execução e partilha de containers, como, por exemplo:

- Uma _interface_ de linha de comandos (_i.e.,_ CLI) simples para criar e executar imagens em camadas (_layered images_).
- Um servidor _daemon_, responsável por gerir os containers e as imagens.
- Uma biblioteca de imagens pré-construídas, prontas a utilizar.
- O conceito de servidor de registo (_registry server_), onde as imagens podem ser armazenadas e partilhadas.

Combinadas, estas inovações permitiram aos utilizadores criar rapidamente novos _containers_ em camadas e partilhá-los facilmente com outros, estabelecendo as bases do ecossistema moderno de containers que hoje domina o desenvolvimento e a implementação de aplicações.

## 2.1. Definição e Arquitetura

Um _container_ Linux é um conjunto de um ou mais processos que são isolados do restante sistema, garantindo que a sua execução não interfere com outros processos ou aplicações.

Todos os ficheiros necessários para a sua execução são fornecidos a partir de uma imagem distinta, o que torna os _containers_ portáteis e consistentes em todas as fases do ciclo de desenvolvimento; desde o desenvolvimento, passando pelos testes, até à produção.

Esta abordagem permite acelerar significativamente os processos de desenvolvimento e integração contínua, eliminando a necessidade de replicar ambientes de teste tradicionais, que são normalmente mais pesados e demorados de configurar.

![](https://i.imgur.com/83VG7FX.png)  
Fig 3. Estrutura típica de um ecossistema de _containers_.

**A Figura 3 ilustra a arquitetura típica de um ecossistema de _containers_**, representando as diferentes camadas composta por uma _stack_; desde a infraestrutura física até às aplicações em execução dentro dos _containers_.

Na base encontra-se a **infraestrutura**, que pode ser composta por hardware físico (como servidores, armazenamento e rede) ou por uma infraestrutura virtualizada, ou cloud (como AWS, Azure, Google Cloud, etc.). Esta camada fornece os recursos físicos necessários para a execução do sistema operativo anfitrião.

Acima da infraestrutura está o **sistema operativo anfitrião**, normalmente uma distribuição Linux ou Windows Server. É neste sistema que os containers são criados e geridos. O kernel do sistema operativo é partilhado por todos os containers, garantindo isolamento entre eles através de mecanismos como _cgroups_ e _namespaces_.

O _**container runtime**_ é o motor responsável pela execução e gestão dos containers. Ele cria, inicia, para e elimina containers, gerindo o seu ciclo de vida e garantindo que cada um corre de forma isolada. Exemplos comuns incluem o Docker Engine, o containerd, o CRI-O ou o runc. Esta camada é essencial para permitir que as aplicações sejam executadas de forma consistente e previsível.

Cada container inclui as **bibliotecas** e **binários** necessários para a aplicação funcionar. Isto significa que cada container transporta as suas próprias dependências, evitando conflitos entre versões de bibliotecas partilhadas no host.

Esta característica é uma das razões pelas quais os containers são tão portáteis e consistentes entre diferentes ambientes.

Por fim, na camada superior encontram-se as **aplicações** (_App_ 1, _App_ 2 e _App_ 3). Cada uma corre dentro do seu próprio _container_, com o seu conjunto independente de bibliotecas e dependências – Este isolamento permite que várias aplicações coexistam no mesmo host sem interferirem entre si.

**Porque se utiliza a Virtualização ao Nível do Sistema Operativo (_OS-Level Virtualization_)?**

A virtualização ao nível do sistema operativo surge como uma resposta às limitações da virtualização ao nível do hardware. **Criar uma máquina virtual tradicional é um processo mais lento e pesado**, pois cada VM precisa de inicializar o seu próprio sistema operativo e imagem completa a partir do zero.

Em cenários com milhares de máquinas virtuais a arrancar simultaneamente, este processo torna-se altamente ineficiente.

Além disso, o **armazenamento das imagens** das máquinas virtuais representa outro desafio, uma vez que há uma abundância de conteúdo repetido entre elas (por exemplo, ficheiros de sistema e bibliotecas comuns).

A virtualização ao nível do sistema operativo (OS-level virtualization) resolve estes problemas ao introduzir uma camada de virtualização dentro do próprio sistema operativo, que particiona os recursos físicos da máquina: CPU, memória, rede e armazenamento em ambientes isolados.

Desta forma, é possível executar múltiplos ambientes virtuais independentes (_containers_) sob o mesmo kernel, garantindo isolamento, leveza e rapidez de inicialização, em comparação com as máquinas virtuais tradicionais.

Do ponto de vista do utilizador, os containers funcionam como servidores reais, pois cada um possui o seu próprio conjunto de processos, sistema de ficheiros, contas de utilizador, interfaces de rede com endereços IP, tabelas de encaminhamento, regras de firewall e outras configurações personalizadas.

Contudo, **todos os containers partilham o mesmo kernel do sistema operativo anfitrião**. Por esse motivo, a virtualização ao nível do sistema operativo é também conhecida como virtualização de imagem única do sistema operativo (_single-OS image virtualization_).

Nas Figuras 4 e 5, podemos observar, respetivamente, uma comparação visual entre os _containers_ e as máquinas virtuais; e as abordagens distintas de _deployment_ das aplicações mediante uma abordagem tradicional, virtualizada ou baseada em _containers_.

![](https://i.imgur.com/ZFjNqMK.png)  
Fig 4. Máquinas Virtuais Vs. Containers.

  

![](https://i.imgur.com/9B5HZay.png)  
Fig 5. Comparação entre implementações tradicionais, virtualizadas e baseadas em containers, mostrando a evolução da arquitetura desde a execução direta sobre o hardware até à virtualização leve proporcionada pelos containers.

A Figura 5 demonstra a evolução tecnológica desde a execução direta sobre o hardware até à virtualização ao nível do sistema operativo:

- Implementação tradicional: simples, mas pouco isolada.
- Implementação virtualizada: mais segura, mas pesada.
- Implementação com containers: leve, rápida, eficiente e ideal para ambientes de cloud e _DevOps_.

## 2.2. Vantagens e Desvantagens

As máquinas virtuais ao nível do sistema operativo, como os containers, apresentam diversas vantagens em relação à virtualização tradicional baseada em hardware.

Entre as principais **vantagens**, destacam-se:

- Arranque e encerramento muito rápidos, devido à ausência da sobrecarga de inicializar um sistema operativo completo em cada instância.
- Baixo consumo de recursos, uma vez que todos os containers partilham o mesmo kernel e bibliotecas de base.
- Elevada escalabilidade, permitindo a execução de inúmeros containers em simultâneo.
- Possibilidade de sincronizar estados entre o container e o ambiente anfitrião quando necessário, facilitando a gestão e a integração de aplicações.

Contudo, existem também **desvantagens** importantes:

- Todos os containers partilham o mesmo sistema operativo, o que limita a flexibilidade. Não é possível, por exemplo, executar containers com sistemas operativos diferentes sobre o mesmo host.
- Existem questões de segurança e privacidade, uma vez que uma falha no kernel ou uma vulnerabilidade de isolamento pode comprometer vários containers em simultâneo.

Assim, a virtualização ao nível do sistema operativo oferece eficiência e desempenho, mas requer maior cuidado com a segurança e uniformidade no ambiente de execução.

## 2.3. Crescente Popularidade

A utilização de containers tem aumentado significativamente, e a adoção de ferramentas de orquestração, como o Kubernetes, tem crescido de forma ainda mais acentuada.

Conforme o relatório _RightScale State of the Cloud_ de 2019, a utilização de _containers_ Docker continuou a expandir-se, passando de 49% em 2018 para 57% em 2019. Já o Kubernetes, utilizado para orquestrar e gerir containers Docker, registou um crescimento ainda mais expressivo, com a sua adoção a subir de 27% para 48% no mesmo período.

Entre as empresas (_enterprise users_), os números são ainda mais elevados: 66% utilizam Docker e 60% recorrem ao Kubernetes nas suas infraestruturas.

No que diz respeito aos serviços de containers na cloud, a AWS manteve uma taxa de adoção de 44% (sem variação face a 2018), enquanto o _Azure Container_ Service cresceu de 20% para 28% e o _Google Container Engine_ aumentou ligeiramente, atingindo 15% de adoção.

**Caso de Estudo – Netflix**

Em 2008, a Netflix iniciou uma migração total para a cloud, transferindo gradualmente toda a sua infraestrutura interna para a AWS (_Amazon Web Services_). Este processo culminou em 2017, altura em que quase toda a operação da Netflix já corria em máquinas virtuais na AWS. Funcionalidades críticas, como a navegação no catálogo, os sistemas de recomendação de conteúdos e os pagamentos, passaram a ser totalmente suportadas pela infraestrutura da Amazon.

Ao longo dos anos, a Netflix tornou-se uma referência em arquitetura _cloud-native_, contribuindo para o desenvolvimento de padrões e práticas amplamente adotados pela indústria, como os microsserviços desacoplados e a infraestrutura imutável, ambos fundamentais para escalabilidade e resiliência em ambientes de cloud. **A migração total para a cloud revelou-se um sucesso notável, consolidando a Netflix como um caso exemplar de transformação digital.**

Mesmo possuindo já uma arquitetura _cloud-native_ madura, a Netflix tem vindo a investir em tecnologias de _containers_, reconhecendo o seu potencial para simplificar operações, aumentar a flexibilidade e melhorar a eficiência. A adoção de _containers_ permitiu à Netflix manter os padrões de isolamento e escalabilidade utilizados nas máquinas virtuais, mas de uma forma mais leve, ágil e otimizada, reforçando a sua posição como líder na adoção de tecnologias inovadoras em _cloud computing_.

## 2.4. Software

Abaixo encontra-se uma lista dos **10 melhores softwares de _containers_ de 2020**, que se destacaram pela sua popularidade, desempenho e funcionalidades na gestão e orquestração de _containers_ em diferentes ambientes de cloud e infraestrutura local.

| Posição | Software                  |
| ------- | ------------------------- |
| #1      | Docker                    |
| #2      | AWS Fargate               |
| #3      | Google Kubernetes Engine  |
| #4      | Amazon ECS                |
| #5      | LXC (Linux Containers)    |
| #6      | Container Linux by CoreOS |
| #7      | Microsoft Azure           |
| #8      | Google Cloud Platform     |
| #9      | Portainer                 |
| #10     | Apache Mesos              |

# 3. Docker

A Docker Inc. foi fundada por Solomon Hykes e Sebastien Pahl durante o programa de incubação de startups da Y Combinator, no verão de 2010, sendo oficialmente lançada em **2011**.

A tecnologia Docker foi apresentada publicamente pela primeira vez em 2013, na conferência PyCon, em Santa Clara, e no mesmo ano, em março de 2013, foi lançada como software open-source.

O Docker baseia-se na virtualização ao nível do sistema operativo (_OS-level virtualization_) para distribuir software em pacotes chamados _containers_. Estes _containers_ permitem executar aplicações de forma isolada e segura, com todas as suas dependências e bibliotecas necessárias, garantindo consistência em qualquer ambiente.

Todos os _containers_ partilham o mesmo kernel do sistema operativo anfitrião, o que torna o Docker mais leve e eficiente do que as soluções de virtualização tradicionais baseadas em máquinas virtuais.

Os **containers Docker**, **executados sobre o Docker Engine**, apresentam **três características fundamentais** que definem o seu sucesso e adoção generalizada:

1. _**Standard**_ (Normalizados): o Docker criou o padrão industrial para _containers_, garantindo que estes são portáteis e compatíveis em qualquer ambiente, desde o desenvolvimento local até à cloud.
2. _**Lightweight**_ (Leves): os _containers_ partilham o kernel do sistema operativo anfitrião, o que elimina a necessidade de executar um sistema operativo completo por aplicação. Esta abordagem resulta em maior eficiência de utilização dos servidores, bem como em redução dos custos de hardware e de licenciamento.
3. _**Secure**_ (Seguros): as aplicações executadas em _containers_ encontram-se isoladas entre si, e o Docker fornece mecanismos de isolamento robustos por defeito, considerados dos mais seguros da indústria.

Em conjunto, estas características tornam os _containers_ Docker uma solução eficiente, portátil e segura para a execução e distribuição de aplicações modernas.

> Docker _containers_ are everywhere: Linux, Windows, Mac, Data center, Cloud, Serverless

## 3.1. Componentes

O Docker assenta numa arquitetura cliente-servidor, onde cada componente desempenha um papel específico na construção e gestão de _containers_.

O **cliente Docker** é o ponto de interação do utilizador, é através dele que são executados comandos como `docker build`, `docker run` ou `docker push`. Estes comandos são enviados para o _daemon_ Docker – o serviço responsável por realizar as operações mais complexas, como criar, executar e distribuir _containers_.

O **cliente** e o _**daemon**_ podem funcionar no mesmo sistema ou em máquinas diferentes, sendo possível ligar um cliente Docker local a um _daemon_ remoto.

A **comunicação** entre o cliente e o _daemon_ é feita através de uma API REST, que utiliza sockets UNIX (em sistemas locais) ou uma interface de rede (em ligações remotas). Esta separação entre cliente e servidor torna o Docker flexível, modular e facilmente integrável em ambientes distribuídos ou de cloud.

A arquitetura do Docker é composta por **três elementos principais**:

1. Docker _Daemon_,
2. Docker _Client_,
3. e Docker _Registries_ (Registos Docker).

Cada um desempenha uma função essencial na criação, execução e distribuição de _containers_.

**_The_ Docker _Daemon_** 👿

O _daemon_ do Docker é o serviço responsável por ouvir pedidos da API Docker e gerir todos os objetos Docker, como imagens, _containers_, redes e volumes. Além disso, um _daemon_ pode comunicar com outros _daemons_ (👿↔👿) de forma a gerir serviços distribuídos em ambientes de cluster, como acontece no Docker Swarm.

**Docker _Client_**

O cliente Docker é a principal interface de interação com o sistema. É através dele que o utilizador executa comandos como `docker run`, `docker build` ou `docker ps`.

- Quando o utilizador executa um destes comandos, o cliente envia a instrução ao _daemon_ (_dockerd_), que realiza a operação correspondente.

O cliente comunica através da API Docker e pode interagir com múltiplos _daemons_ simultaneamente, seja localmente ou remotamente, o que o torna flexível e adaptável a vários ambientes.

**_Docker Registries_ (Registos Docker)**

Os registos armazenam as imagens Docker, que servem de base para criar _containers_.

> containers ↔ images

O **Docker _Hub_** é o registo público padrão, utilizado por milhões de utilizadores para partilhar e descarregar imagens. Contudo, é também possível configurar registos privados, por exemplo, através do _Docker Trusted Registry_ (DTR) incluído no _Docker Datacenter_ (DDC).

Quando se executam comandos como `docker pull` ou `docker run`, as imagens necessárias são transferidas automaticamente do registo configurado (tipicamente o Docker _Hub_). Já o comando `docker push` é utilizado para enviar imagens criadas localmente para o registo (container → image).

Em **resumo**, o _daemon_ executa as operações, o cliente serve de interface de controlo e os registos fornecem e armazenam as imagens, juntos, estes componentes tornam o ecossistema Docker uma plataforma poderosa, modular e escalável para a virtualização de aplicações mediante _containers_.

**Overview dos Componentes do Docker**

A Figura 6 ilustra de forma clara a arquitetura do Docker e o modo como os seus principais componentes – cliente, _daemon_ e registo (registry) – interagem entre si para construir, executar e distribuir _containers_.

![](https://i.imgur.com/COPKKM8.png)  
Fig 6. Componentes que compõem a arquitetura do Docker.

## 3.2. Docker _Images_, DockerFiles e o Docker _Compose_

> Images vs Containers Image - a static template with instructions for creating a container

**Docker _Images_**
Uma **imagem Docker** é um pacote portátil que contém o software e todas as dependências necessárias para que esse software seja executado corretamente, incluindo bibliotecas, ficheiros de configuração e o ambiente de _runtime_, tipicamente construída a partir de um _Dockerfile_ (Figura 7).

![](https://i.imgur.com/hOdlUlM.png)  
Fig 7. DockerFile to Docker Image = Docker Container

Quando uma imagem é carregada para a memória e executada, ela torna-se num container, ou seja, numa instância em funcionamento dessa imagem, isolada do restante sistema.

As **imagens Docker são imutáveis**, o que significa que não podem ser modificadas diretamente. Caso seja necessário realizar alterações, como atualizar uma dependência, adicionar ficheiros ou mudar configurações, a única forma de o fazer é criar uma nova imagem a partir da anterior, aplicando as modificações desejadas e gerando uma nova versão.

**Uma imagem pode ser feita a partir de:**

- Uma _**Base Image**_: é uma imagem que usa a imagem docker “scratch”, uma imagem vazia que não cria uma camada com um sistema de ficheiros. Esta imagem pode ser usada para aplicações que usam diretamente o Kernel do SO onde está o Docker Engine
- Uma _**Parent Image**_: é qualquer imagem que seja usada para criar outra imagem. A Parent Image pode conter já um SO, e software instalado, por exemplo, o NGINX

**DockerFiles**
O Dockerfile é um ficheiro de texto que contém uma sequência de instruções que o Docker executa automaticamente para construir uma imagem. Cada instrução (como `FROM`, `COPY`, `RUN` ou `CMD`) gera uma nova camada (layer) sobre a imagem base, resultando numa imagem final personalizada e reutilizável.

**Exemplo**  

> A forma mais simples de criar imagens é partir de uma imagem existente e a partir de um DockerFile

Considere o seguinte exemplo que ilustra um dockerfile que permite criar uma imagem a partir do Ubuntu 18 e com o serviço MariaDB:

```
FROM ubuntu:18.04RUN apt-get update && \    apt-get install -y mariadb-server && \    apt-get cleanEXPOSE 3306
```

ou um exemplo de um DockerFile que permite criar uma imagem Docker capaz de compilar e executar uma aplicação .NET Core:

```
FROM mcr.microsoft.com/dotnet/core/sdk:2.2WORKDIR /appCOPY myapp_code .RUN dotnet build -c Release -o /relEXPOSE 80WORKDIR /relENTRYPOINT ["dotnet", "myapp.dll"]
```

**Neste último exemplo:**

- `FROM`: Esta instrução indica a imagem base que será utilizada para construir a nova imagem.
    
- `WORKDIR`: Esta instrução define o diretório de trabalho dentro do container, ou seja, a pasta onde os comandos seguintes serão executados.
    
- `COPY`: Este comando copia ficheiros do host (máquina local) para o container.
    
    - Neste exemplo, copia a pasta `myapp_code` (presente no sistema anfitrião) para o diretório de trabalho atual dentro do container (`/app`).
    - O ponto (`.`) indica que os ficheiros serão copiados para o diretório definido anteriormente pelo `WORKDIR`.
- `RUN`: Esta instrução executa comandos dentro do container durante a construção da imagem.
    
    - No exemplo, comando `dotnet build -c Release -o /rel`compila a aplicação .NET em modo Release e guarda os ficheiros compilados no diretório `/rel`.
    - Esta operação ocorre durante a construção da imagem, não durante a execução do container.
- `EXPOSE`: Este comando indica que a imagem está configurada para utilizar a porta 80 – a porta padrão para aplicações web.
    
- `ENTRYPOINT`: Esta instrução define o comando que será executado automaticamente quando o container for iniciado.
    
    - Neste caso, o container irá lançar o _runtime_ do .NET para executar o ficheiro `myapp.dll`, ou seja, iniciar a aplicação compilada.
    
    **Resumo**  
    Este Dockerfile automatiza todo o processo de construção e execução de uma aplicação .NET Core, nomeadamente:
    
    - Utiliza uma imagem base com o SDK-
    - Copia o código-fonte.
    - Compila o projeto.
    - Expõe a porta 80 para acesso externo.
    - Define o ponto de entrada da aplicação. Dessa forma, o resultado é uma imagem autossuficiente e pronta para ser executada em qualquer ambiente com Docker.
    

**Docker _Compose_**

O Docker _Compose_ é uma ferramenta concebida para definir e executar aplicações Docker compostas por **múltiplos containers**, ou serviços, permitindo gerir facilmente aplicações complexas que dependem de vários serviços interligados, como bases de dados, servidores web e aplicações _backend_.

O _Compose_ simplifica o controlo de toda a _stack_ da aplicação, centralizando a configuração de serviços, redes e volumes num único ficheiro YAML, denominado `docker-compose.yml`. Este ficheiro descreve como os diferentes _containers_ devem ser criados e interligados, oferecendo uma visão clara e organizada da aplicação completa.

Com apenas um comando (`docker compose up`), é possível criar e iniciar todos os serviços definidos no ficheiro de configuração, facilitando a automatização do processo de _deployment_ e a replicação do ambiente em diferentes sistemas.

O uso do Docker _Compose_ segue essencialmente três etapas principais:

1. Definir o ambiente da aplicação num Dockerfile, garantindo que pode ser reproduzido em qualquer sistema.
2. Descrever os serviços que compõem a aplicação no ficheiro `docker-compose.yml`, especificando como devem ser executados em conjunto num ambiente isolado.
3. Executar o comando `docker compose up`, que cria e inicia automaticamente todos os _containers_ e dependências configuradas, colocando a aplicação em funcionamento.
    1. É ainda possível iniciar apenas um ou mais serviços especificados no ficheiro `docker-compose.yaml`

Esta abordagem torna o Docker _Compose_ uma ferramenta essencial para o desenvolvimento, testes e orquestração de aplicações multi-_container_, mantendo a consistência e simplificando a gestão em ambientes de equipa e produção.

**Estrutura dos Ficheiros YAML no Docker _Compose_**

O Docker _Compose_ permite que os programadores gerem e controlem vários _containers_ em simultâneo através de um único ficheiro de configuração, normalmente denominado `compose.yml` ou `docker-compose.yml`. Este ficheiro é escrito em formato YAML, o que o torna fácil de ler e editar, uma vez que usa indentação com espaços ou tabulações em vez de chavetas ou parênteses, ao contrário do que acontece em outros formatos.

A estrutura do ficheiro é organizada de forma hierárquica, onde cada nível define um conjunto de regras e propriedades relacionadas com a configuração e execução dos serviços (i.e., _containers_).

> serviços == containers

Atualmente, e de uma forma geral, quase todos os ficheiros incluem 3 elementos principais:

1. _**Services**_ (serviços) — especifica os _containers_ que serão criados e executados, juntamente com as respetivas configurações, como a imagem utilizada, portas expostas, variáveis de ambiente e dependências entre serviços.
2. _**Volumes**_ (volumes utilizados) — define os volumes de armazenamento que serão partilhados entre _containers_, permitindo a persistência de dados mesmo após a paragem ou eliminação de _containers_.
3. _**Networks**_ (redes) — descreve as redes internas que ligam os diferentes serviços entre si, garantindo uma comunicação segura e isolada dentro do ambiente Docker _Compose_.

Um exemplo deste ficheiro pode ser observado no seguinte excerto de código:

```
(...)services:  mongodb:    image: mongodb/mongodb-community-server:latest    restart: always    container_name: mongodb    user: "${UID}:${GID}"    ports:      - 27018:27017    volumes:      - "${DATA}/mongodb/db:/data/db"      - "${DATA}/mongodb/configdb:/data/configdb"    networks:      - ForbiddenForest(...)
```

> - **Services**: The service tag is the parent tag and contains all the containers or services to be deployed.
> - **Image**: the base image of a container can be defined by either using a preexisting image that is available on DockerHub or by images built by a DockerFile.
> - **Ports**: Exposing the port to the host system, where the syntax is `host:container`, meaning that the forwarding is accomplished from host to container.
> - **Volunes**: The `${DATA}` is an environment variable that specifies a folder, this `${DATA}` folder of the host’s file system is mapped to the `/data/db` folder of the container.
>     - This portion of the file system is writable, which means that the container can not only read but also write (and delete) files in the host machine.

A estrutura modular e declarativa do ficheiro `yaml` permite gerir ambientes complexos de forma simples e reprodutível, ideal para equipas de desenvolvimento e ambientes de produção que exigem consistência e escalabilidade.

## 3.3. Sistema de Ficheiros

As imagens Docker utilizam um sistema de ficheiros denominado _Union File System_ (UnionFS), que permite empilhar vários sistemas de ficheiros — designados por branches. Isto irá permitir criar uma estrutura unificada e hierárquica.

O conteúdo de cada branch é fundido (_merged_), permitindo que o utilizador visualize todos os ficheiros como se fizessem parte de um único sistema de ficheiros coerente.

Durante a construção de uma imagem Docker, são empilhados vários branches, cada um representando uma camada (layer). É importante notar que:

- Só é possível adicionar novas camadas — não se podem apagar nem modificar as inferiores;
- O último branch, também conhecido como camada de escrita, é a única parte mutável;
- Essa camada de escrita não é persistente, ou seja, as alterações desaparecem quando o container é desligado.

Esta abordagem baseada em camadas ou _layers_ torna as imagens eficientes, reutilizáveis e leves, uma vez que diferentes _containers_ podem partilhar camadas comuns sem duplicar dados, reduzindo o consumo de espaço e tempo de carregamento.

**Exemplo**
![](https://i.imgur.com/WCKswJk.png)  
Fig 8. Exemplo do funcionamento do sistema de ficheiros UnionFS no Docker.

**A Figura 5 ilustra o funcionamento do sistema de ficheiros UnionFS** utilizado nas imagens Docker, complementando o que foi explicado anteriormente sobre o modo como estas imagens são estruturadas em _layers_.

Cada camada representa uma modificação incremental à camada anterior — por exemplo, a instalação de um pacote, a adição de ficheiros ou a execução de um comando num Dockerfile. Estas camadas são combinadas pelo UnionFS, que as une num único sistema de ficheiros unificado visível ao container em execução.

No exemplo apresentado na figura, o `Dockerfile` mostra três instruções principais:

```
FROM busyboxADD x.txt /dataCMD ls /data
```

Neste exemplo, cada linha do `Dockerfile` gera uma camada distinta:

- **_Layer_ 1** — `FROM busybox`: Define a imagem base, neste caso o BusyBox, que contém um conjunto mínimo de utilitários Linux, como /bin/sh, /bin/cp e /bin/cd.
    
    Esta é a primeira camada e serve de fundação para as restantes.
    
- **_Layer_ 2** — `ADD x.txt /data`: Cria uma nova camada que adiciona o ficheiro x.txt ao diretório /data dentro da imagem.
    
    O UnionFS empilha esta camada sobre a anterior, combinando os ficheiros novos com os existentes.
    
- **_Layer_ 3** — `CMD ls /data`: Define o comando por defeito que será executado quando o container for iniciado.
    
    Embora esta camada não adicione novos ficheiros, ela representa uma alteração à configuração da imagem e, portanto, é registada como uma camada adicional.
    

Esta fusão mostra como o Docker constrói imagens de forma incremental e eficiente, reutilizando camadas anteriores sempre que possível. Assim, as imagens tornam-se modulares, permitindo reduzir o tempo de construção e poupar espaço, uma vez que múltiplas imagens e _containers_ podem partilhar as mesmas camadas de base.

## 3.4. Swarm Mode

As versões mais recentes do Docker incluem o _Swarm Mode_, uma funcionalidade integrada que permite gerir nativamente um cluster de Docker _Engines_, designado por swarm.

Um swarm é, essencialmente, um conjunto de máquinas (físicas ou virtuais) que executam o Docker Engine e que são agrupadas para formar um ambiente distribuído e orquestrado.

Através da linha de comandos do Docker (CLI), é possível:

- Criar um swarm, adicionando diferentes nós (máquinas) ao cluster.
- Deployment de serviços e aplicações no _swarm_.
- Gerir o comportamento e o estado dos nós, serviços e tarefas dentro do cluster.

Uma das principais vantagens do _Swarm Mode_ é que não requer software de orquestração adicional (_e.g.,_ Kubernetes), uma vez que a gestão do cluster encontra-se integrada diretamente no Docker _Engine_.

> Dessa forma, o Swarm Mode oferece uma solução simples e nativa para distribuição, escalabilidade e alta disponibilidade de _containers_ em ambientes de produção.

## 3.5. Learn Docker

Encontra-se disponível na página [101-Docker](https://ttools.ubi.pt/edumark/view?lecture=7&filename=02-2024.md&branch=workshops) um mini tutorial introdutório prático sobre Docker, que pode ser seguido para obter uma compreensão prática dos conceitos fundamentais aqui discutidos e desenvolver um domínio progressivo desta tecnologia.

Adicionalmente, devem também explorar os recursos disponíveis nas apresentações [Learn Docker](https://di.ubi.pt/~tsimoes/resources/17912/Learn-Docker.pdf) e [Azure Containers](https://di.ubi.pt/~tsimoes/resources/17912/Exemplo-Azure-Containers.pdf).