# 1. Introdução à Computação na Nuvem

O termo _Cloud Computing_, ou Cloud, é utilizado para definir um conjunto de objetos que estão de alguma forma agrupados. A computação na nuvem é um tema vasto que engloba diferentes assuntos e tecnologias.

O conceito de computação na nuvem não é o mesmo que virtualização. A virtualização é a tecnologia que permite dividir os recursos de uma máquina física, computador ou servidor, em porções mais pequenas, intituladas de máquinas virtuais.

Em termos de **definição**, e apesar de **não existir um consenso**, podemos definir _Cloud Computing_ de duas formas:

1. _John McCarthy_ que apelidou nos 60 o termo que conhecemos hoje como c_loud computing_, como _utility computing_:

> Consiste num serviço básico, como o fornecimento de eletricidade ou água.

2. _National Institute of Standards and Technology_ (NIST) definiu em 2011:

> _Cloud computing_ é um modelo que permite o acesso ubíquo, conveniente e a pedido através da rede, a um conjunto de recursos de computação partilhados (redes, servidores, armazenamento, aplicações, serviços, etc.) que podem ser rapidamente aprovisionados ou libertos, com um mínimo de esforço e sem interação com o fornecedor.

**O modelo Cloud é composto por cinco características essenciais**, três modelos de serviço e quatro modelos de implementação. Podemos utilizar a fórmula seguinte para verificar se algo se enquadra no conceito _cloud_:

|                                                                        |
| ---------------------------------------------------------------------- |
| 5x3x4                                                                  |
| 5 Características, 3 Modelos de Serviços e 4 Modelos de Implementação. |

A computação em nuvem foi proposto sobre conceitos muito antigos de tecnologias de computação. Pode ser difícil entender a mudança de paradigma na computação tendo em conta estas características. Assim, considera-se que a evolução para o conceito cloud é uma revolução na abordagem de como os serviços são produzidos e consumidos. Quando um fornecedor cloud é escolhido, aluga-se parte de uma enorme estrutura concentrada em _data centers_. Por exemplo, o _Google Cloud_ é composto por inúmeros _data centers_ espalhados por todo o mundo (veja-se Figura 1).

Tendo em conta o crescimento exponencial dos _data centers_ estes têm sido construídos tendo em conta alguns dos seguintes critérios:

- Custos energéticos reduzidos.
- Melhor aproveitamento de fontes energéticas.
- Perto de abundantes fontes de água.
- Localização com um acesso de rede privilegiada (_high-speed networks_).
- Custos de terreno baixos e ocupação discreta.

O impacto desta mudança de paradigma incide essencialmente na economia e na forma como é feita a entrega de software, da mesma forma que o _download_ e o _streaming_ de músicas mudou o mercado musical. Evidentemente, o custo de software produzido para a cloud é inferior do que o software antigamente produzido e entregue no “formato tradicional”.

![](https://i.imgur.com/DFR3FZh.png)  
Fig 1. _Google Data Center_

## 1.1. Evolução dos Paradigmas de Computação

![](https://i.imgur.com/pneyPJn.png)  
Fig 2. Evolução temporal dos paradigmas de computação.

Para além da computação na nuvem, discutido anteriormente, analisaremos dois dos paradigmas de computação ilustrados na Figura 2.

==_**Utility Computing**_== é um model de provisionamento de serviços no qual um provedor de serviços disponibiliza recursos de computação e gestão de infraestruturas ao cliente conforme as necessidades.

- Os custos de aquisição das “utilidades” são feitos a partir de uma taxa fixa de utilização.
- O _utility computing_ visa maximizar essencialmente a eficiência de utilização de recursos e a redução dos custos associados.

Com a utilização de dispositivos baseados na Internet das Coisas (IoT) e o 5G, a colocação da computação e a sua análise é feita perto de onde os dados são criados - Esta é a base do paradigma ==_**Edge Computing**_==.

- _**Edge Computing**_ é um paradigma focado em trazer a computação o mais próximo possível para a fonte onde os dados são criados, a fim de reduzir a latência e a utilização da largura de banda.
- Em termos mais simples, este modelo permite executar menos processos em ambientes baseados na nuvem movendo esses processos para um ambiente local, como um computador de um utilizador, um dispositivo IoT, ou até mesmo um _edge server._

Tal como tinha sido discutido anteriormente, _**Cloud Computing**_, e de acordo com o NIST é “(…) um modelo que permite o acesso ubíquo, conveniente e a pedido através da rede, a um conjunto de recursos de computação partilhados (redes, servidores, armazenamento, aplicações, serviços, etc.) que podem ser rapidamente aprovisionados ou libertos, com um mínimo de esforço e sem interação com o fornecedor.” na internet e torna-as como um serviço (i.e., _utility service_).
## 1.2. As Características Essenciais

As **cinco características** essencias do _**Cloud Computing**_ são:

1. _**On-demand self-service**_ (Autosserviço a pedido): Um utilizador tem a possibilidade de aprovisionar recursos de computação (e.g., tempo de acesso a um servidor ou espaço de armazenamento na rede) sem interagir com outra pessoa. Por outras palavras, um serviço cloud não existe a necessidade de uma interação com outra pessoa para alocar recursos.
2. _**Broad network access**_ (Acesso generalizado à rede): Os recursos estão disponíveis na rede (_e.g.,_ Internet) e são acedidos utilizando mecanismos comuns (_e.g.,_ _browser_) - Para que algo seja um serviço _Cloud_ o acesso tem que ser feito utilizando um acesso comum.
3. _**Resource pooling**_ (Acesso partilhado a recursos): Os recursos de computação do fornecedor de serviços _Cloud_ são partilhados por vários clientes/utilizadores, com recursos físicos ou virtualizados associados de forma dinâmica.
4. _**Rapid elasticity**_ (Elasticidade rápida): Os recursos são disponibilizados ou cancelados de uma forma dinâmica, elástica, ou até mesmo de forma automática para ser possível escalar serviços ou recursos (i.e., alocação de mais ou menos recursos computacionais) tendo em conta a procura.
5. _**Measured service**_ (Serviço medido): A utilização dos recursos é monitorada fornecendo uma transparência imediata entre os fornecedores de serviços _Cloud_ e os clientes. Por exemplo, espaço em disco utilizado em `GB`, processamento em `Ghz`, largura de banda em `MBps`, ou número de contas de utilizadores.

## 1.3. Os Modelos

No âmbito da computação na nuvem, é essencial compreender os diferentes modelos pelos quais os serviços são entregues aos utilizadores. Estes modelo definem a natureza da relação entre o fornecedor de serviços da nuvem e os seus clientes, determinado quem é responsável pelo quê, em termos de gestão, manutenção e implementação. Os modelos de serviços de _cloud_, que incluem _Software as a Service_ (SaaS), _Plataform as a Service_ (PaaS) e _Infrastructure as a Service_ (IaaS), oferecem abordagens distintas para atender às necessidades variadas das organizações e dos utilizadores finais. Iremos também discutir os modelos de implementação que definem como a infraestrutura da nuvem é disponibilizada e gerida.

### 1.3.1. Modelos de Serviços

Existem os seguintes modelos de serviço de _Cloud_:

- _**Software as a Service (SaaS)**_ (Software como um Serviço): A alocação de software, em instâncias, para diferentes utilizadores - _on-demand software_. Por outras palavras, fornecer ao cliente uma aplicação de software que corre numa infraestrutura de computação.
- _**Plataform as a Service (PaaS)**_ (Plataforma como um Serviço): Fornece máquinas virtuais, sistemas operativos, aplicações, frameworks de desenvolvimento, transações e controlo de estruturas. O cliente pode construir as suas aplicações na estrutura cloud fornecida. Isto é possível utilizando as linguagens e ferramentas de programação fornecidas pelo serviço PaaS.
- _**Infrastructure as a Service (IaaS)**_ (Infraestrutura como um Serviço): Fornece máquinas virtuais, armazenamento, e outro tipos de hardware que podem ser fornecidos ao cliente como recursos. Por outras palavras, é o de fornecer ao cliente a possibilidade de instalar recursos fundamentais de computação, especificamente, processamento, armazenamento, memória e rede. Sobre estes recursos o utilizador pode instalar e correr software (sejam sistemas operativos ou aplicações).

**A grande maioria dos serviços de _Cloud_ existentes na Internet são serviços do tipo _Software as a Service_.**

### 1.3.2. Modelos de Implementação

Apesar de existirem outros, os dois modelos principais são:

- _**Private Cloud**_ (_Cloud_ privada): Uma infraestrutura _Cloud_ disponibilizada para uso exclusivo de uma única organização - A estrutura pode ser organizada/gerida por essa organização ou por terceiros e pode ser utilizada dentro ou fora da organização.
- _**Public Cloud**_ (_Cloud_ pública): Uma infraestrutura _Cloud_ disponibilizada ao público e é normalmente disponibilizada por uma organização e vendida por um fornecedor de _Cloud_.

Temos ainda as seguintes **variantes**:

- _**Hybrid Cloud**_ (_Cloud_ híbrida): Uma infraestrutura _Cloud_ híbrida que combina múltiplos modelos de implementação que formam uma só unidade. Por outras palavras, infraestrutura _Cloud_ que é uma combinação de duas ou mais infraestruturas de _Cloud_ (privada, pública, comunidade).
- _**Community Cloud**_ (_Cloud_ de comunidade): Uma infraestrutura _Cloud_ disponibilizada para utilização exclusiva de uma comunidade de utilizadores ou função única. Pode ser utilizada por uma ou mais organizações, onde existe uma partilha de interesses, políticas, questões de segurança, regulamentos, etc. Esta pode ser gerida por uma das organizações ou por terceiros.

## 1.4. As Vantagens e Desvantagens da Computação na Nuvem

As principais **vantagens** da adoção da computação distribuída é um _matching_ quase perfeito das características essenciais do _Cloud Computing_.

Considere as seguintes vantagens principais:

- _**Auto serviço a pedido**_: Um cliente pode alocar recursos computacionais sem a necessidade de envolver os recursos humanos da organização.
- **Interoperabilidade de acessos**: O acesso a recursos da _Cloud_ está disponível via métodos _standards_ e independentes de plataformas.
- **Custos reduzidos**: As redes utilizadas são de tal forma eficientes que uma elevada utilização leva a uma redução de custos.
- **Acesso partilhado a recursos**: Recursos e sistemas virtuais são dinamicamente alocados/realocados tendo em conta as necessidades atuais dos utilizadores/clientes.
- **Elasticidade dos recursos**: O sistema pode adicionar recursos - Por exemplo, aumentar os recursos computacionais atualmente disponíveis para um determinado sistema virtual (i.e., computadores mais potentes e/ou mais computadores dentro da mesma gama requisitada). Estes recursos podem ser comprados a qualquer momento pelo utilizador/cliente.
- **Serviço medido**: A utilização de um serviço _Cloud_ é medido, auditado e reportado ao cliente tendo como base um sistema métrico.
- **Facilidade de utilização**: Não existe a necessidade de requisitar licenças para utilizar ou implementar serviços.
- **Confiabilidade**: Redes escaláveis e implementadas sobre mecanismos de balanceamento de carga e _failover_.
- **Gestão terceirizada**: Terceiros estão encarregues de gerir a estrutura _Cloud_ contratada, existindo assim uma redução de custos por parte das organizações ou utilizadores - Estes não têm custos na gestão das estruturas necessárias à disponibilização de serviços.
- **Manutenção simplificada**: Terceiros estão encarregues de manter e atualizar todos os recursos de _Cloud_ contratados.

**A Computação Clássica _versus_ a Computação na Nuvem**:  
Os especialistas da IBM estimaram uma redução de custos de 80% a 90% em comparação com o paradigma de computação convencional.

As principais **desvantagens** da adoção da computação na nuvem são as seguintes:

- **Soluções não personalizadas**: Muita das soluções _Cloud_, que seguem um modelo de serviço SaaS, nem sempre são feitas à medida do utilizador/cliente final.
- **Tráfego elevado**: Por vezes um elevado volume de tráfego pode levar a constrangimentos na implementação e utilização de aplicações integradas na _Cloud_.
- **Comunicação protocolar deficiente**: Todas as comunicações são feitas utilizando protocolos Web onde existe a herança dos seguintes problemas: estado atual da mensagem enviada, caminhos de envio, etc.
- **Segurança e privacidade**: Os dados estão numa estrutura desconhecida ao cliente - A segurança e transferência dos dados para a _Cloud_ pode comprometer questões de segurança e privacidade.

## 1.5. O Modelo Proposto pelo NIST

O _U.S. National Institute of Standards and Technology (NIST)_ trabalhou na definição do modelo de computação em nuvem, propondo um modelo que destaca a separação entre os modelos de serviços e os modelos de implantação.

**Relembrar**

- **Modelos de Implementação**: Refere-se à localização e gestão da estrutura _cloud_.
- **Modelos de Serviços**: Refere-se aos tipos de serviços que podem ser acedidos mediante uma plataforma _cloud_.

Estes modelos, e os seus relacionamentos, também definem as características essenciais da computação na nuvem - A Figura 3 define o modelo visual da definição da computação na nuvem.

![](https://i.imgur.com/k8ayPbp.png)  
Fig 3. _NIST Visual Model of Cloud Computing Definition._

## 1.6. _Open Standards_

Existe uma indústria a investigar e a criar diferente**_standards_ para a _cloud_**. Este é um trabalho contínuo de investigação que permite desenvolver diferentes padrões de integração e desenvolvimento, por exemplo:

- Plataforma de virtualização de recursos.
- Arquiteturas Orientadas a Serviços.
- Web Frameworks.
- _Deployment_ de software open-source.
- Padronizar os Web _Services_.
- Automação de Sistemas
- Computação em grelha.
- (…)

Estes padrões ajudam a criar um melhor suporte ao desenvolvimento e manutenção das diferentes aplicações existentes na nuvem - Desta forma é possível, dados portáveis e universalmente acessíveis.

# 2. Fornecedores de Nuvem Públicas ou Privadas

Existem atualmente no mercado diferentes provedores de nuvem ou _Cloud_ _Providers_. Estes fornecem aos utilizadores diferentes modelos de serviços e de implementação, veja-se, por exemplo os provedores ilustrados na Figura 4.

![](https://i.imgur.com/C2x2WWb.png)  
Fig 4. Exemplos de fornecedores de Nuvem Públicas ou Privadas.

As funções dos utilizadores individuais e/ou organizacionais e as suas interações com os diferentes provedores de _cloud_ podem ser observadas na Figura 5.

![](https://i.imgur.com/oqbZRO7.png)  
Fig 5. Visão geral das interações entre os _cloud players_ e os _providers_  
Adapted from: Distributed and Cloud Computing: From Parallel Processing to the Internet of Things, Kai Hwang, Jack Dongarra, Geoffrey C. Fox (Authors), Morgan Kaufmann, 1st edition, 2011.

Analisando a Figura 5 podemos concluir que:

- _**Organization Users**_ / **Individual Users** (Utilizadores das organizações ou individuais): Os utilizadores individuais ou das organizações podem requisitar diferentes serviços (e.g., SaaS, PaaS ou IaaS).
- _**Application Provider**_ (_i.e.,_ oferece SaaS): Os _providers_ servem tanto os utilizadores individuais como os utilizadores das organizações.
- _**Cloud Plataform Provider**_ (_i.e.,_ oferece PaaS): Estes fornecem serviços do tipo PaaS a utilizadores individuais e/ou organizacionais e também a outros providers (e.g., _providers_ que fornecem exclusivamente serviços do tipo SaaS).
- _**Cloud Service Provider**_ (_i.e.,_ oferece IaaS): Estes fornecem serviços do tipo IaaS a utilizadores individuais e/ou organizacionais e também a outros _providers_ (_e.g,_ _providers_ que fornecem exclusivamente serviços do tipo PaaS) - Fornecimento de recursos de hardware e software.

Consumidores compram ou alugam _cloud services_ na forma de IaaS, PaaS ou SaaS - Muitos dos cloud _entrepreneurs_ estão a a vender os seus _utility services_ a um elevado número de consumidores/utilizadores. A compra de _cloud services_ é feita na forma de IaaS, PaaS, ou SaaS.

Então → _Cloud providers_ aproveitam a crescente procura de muitas empresas e utilizadores em terceirizar os trabalhos de computação e armazenamento.

Os custos de aquisição de recursos virtualizados são geralmente mais baixos comparativamente aos custos associados à instalação, manutenção e atualização de _in-house servers_.

Todos os modelos de serviços (_i.e.,_ IaaS, PaaS e SaaS) permitem que os utilizadores acedam a serviços pela Internet, dependentes inteiramente das infraestruturas de cloud fornecidas pelos _cloud service providers_. Estes modelos são oferecidos sobre um conjunto de _**Service Level Agreements (SLA)**_ entre o _provider_ e o utilizador.

Os SLAs são mais comuns nos serviços de rede, pois representam as características de _Quality of Service_ (QoS) deste tipo de serviços. Por outro lado, para os serviços de computação em nuvem é mais difícil negociar um SLA - Neste contexto, e de uma forma global, os SLAs englobam questões relacionadas com _service availability_, _data integrity_, _privacy_ e _security protection_ onde a margem de negociação é reduzida.
