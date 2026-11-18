---
disciplina: Redes
tags:
  - redes
  - routing
  - ospf
---
# OSPF (Open Shortest Path First)

Protocolo de **roteamento dinâmico interno** (*Interior Gateway Protocol* — IGP) que atua na **camada 3** (Internet) do modelo TCP/IP. O seu trabalho principal é fazer com que os routers de uma rede conversem entre si para descobrir caminhos, partilhar topologias e preencher as tabelas de roteamento automaticamente, sem intervenção humana constante.

## Funcionalidade

O OSPF funciona com base no conceito de **estado de link** (*link-state*) e executa as seguintes etapas:

1. **Descoberta de vizinhos** — envia pacotes *Hello* para detetar outros routers OSPF ligados diretamente.
2. **Mapeamento da rede** — cada router cria um **LSA** (*Link-State Advertisement*) que detalha as suas conexões e o estado delas. Os LSAs são propagados (*flooding*) por toda a rede.
3. **Cálculo da rota** — com todos os LSAs reunidos numa base de dados idêntica para todos os routers (**LSDB**), cada equipamento executa o **algoritmo de Dijkstra**, calculando a árvore de caminhos mais curtos com ele próprio como raiz.

## Problema que resolve

O OSPF foi criado para resolver as limitações severas do seu antecessor, o **RIP** (*Routing Information Protocol*):

- **Limitação de tamanho:** só suportava redes com no máximo **15 saltos** (routers).
- **Métrica cega:** escolhia caminhos apenas pelo número de routers, ignorando completamente a velocidade dos links.
- **Convergência lenta:** demorava minutos a reagir à queda de um link, gerando *loops* de rede devastadores.

## Contexto

O OSPF é utilizado no contexto de **Sistemas Autónomos (AS)** internos, ou seja, redes controladas por uma única organização, tais como:

- Redes de grandes empresas e multinacionais.
- Redes de campus universitários.
- Infraestruturas internas de ISPs, para interligar as suas centrais.

## Finalidade

Garantir a entrega eficiente de pacotes através da rota mais rápida disponível, maximizando a utilização da largura de banda e garantindo alta disponibilidade: se um caminho falhar, a rede recupera em segundos.

## História

O desenvolvimento do OSPF começou no final dos anos 1980 pela **IETF**, motivado pelo crescimento acelerado da Internet, que já não conseguia ser suportado pelo RIP.

| Ano      | Marco                                                    |
| -------- | -------------------------------------------------------- |
| **1989** | OSPFv1 na RFC 1131 — base experimental                   |
| **1991** | OSPFv2 na RFC 1247 (John Moy) — versão robusta para IPv4 |
| **1998** | OSPFv2 consolidado na RFC 2328 (versão atual para IPv4)  |
| **1999** | OSPFv3 (RFC 2740) — suporte a IPv6                       |

### Curiosidade documentada

Ao contrário de muitos protocolos que usam TCP ou UDP na camada 4, o OSPF funciona **diretamente sobre IP**. Tem o seu próprio número de protocolo IP reservado: o **Protocolo 89**. Qualquer pacote IP com «Protocol 89» no cabeçalho é identificado de imediato pelo router como tráfego de controlo do OSPF.
	
## Exemplo de utilização

Rede empresarial com três escritórios em triângulo:

```
        [Porto]
         /     \
   10 Gbps     10 Mbps (backup)
       /         \
[Lisboa] ------- [Coimbra]
            1 Gbps
```

Se o Porto quiser enviar dados para Coimbra, o OSPF analisa as métricas — **custo = 10⁸ / largura de banda (bps)**:

1. **Caminho direto:** Porto → Coimbra (10 Mbps) → custo alto (lento).
2. **Caminho indireto:** Porto → Lisboa (10 Gbps) → Coimbra (1 Gbps) → custo muito baixo (rápido).

**O OSPF em ação:** decide enviar os dados do Porto para Coimbra **passando por Lisboa**. Embora faça mais saltos, a velocidade da fibra compensa. Se o link de Lisboa falhar, o OSPF recalcula a rota em milissegundos e ativa temporariamente o link de backup direto de 10 Mbps.

## Ligações

- [[Pilha TCP-IP]]
- [[Trabalhos e Avaliação]]
