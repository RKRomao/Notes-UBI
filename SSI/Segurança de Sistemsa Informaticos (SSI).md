Cadeira de cibersegurança (2.º Ciclo, UBI).

O programa cobre os fundamentos da criptografia moderna, cifras simétricas, funções de hash, MACs, criptografia de chave pública e curvas elípticas, e avança para tópicos aplicados como computação segura multipartes, mecanismos de segurança em sistemas operativos, vulnerabilidades de aplicações Web, exploração de buffer overflows, proteção dinâmica e auditoria de software. Cada aula é acompanhada por um guia laboratorial que aplica os conceitos com ferramentas como OpenSSL, SSH, Java e Python.

## Índice

- [Aulas Teóricas](#aulas-teóricas)
- [Aulas Práticas](#aulas-práticas)
- [Trabalhos](#trabalhos)
- [[SSI/Notas]]

## Aulas Teóricas

| #     | Tema                                                                                                                                                                                                                                   | Material                                    |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| 1     | Apresentação da UC; revisão de criptografia moderna: cifras de chave simétrica, funções de resumo (hash) e códigos de autenticação de mensagens (MAC)                                                                                  | [[SSI/Teóricas/lecture-1.pdf\|Lição 1]]     |
| 2     | Criptografia de chave pública: Diffie-Hellman, RSA e ElGamal, assinaturas e certificados digitais; autenticação e protocolos de conhecimento zero                                                                                      | [[SSI/Teóricas/lecture-2.pdf\|Lição 2]]     |
| 3     | Criptografia de curvas elípticas: grupos cíclicos, problema do logaritmo discreto, operações em curvas e esquema criptográfico integrado                                                                                               | [[SSI/Teóricas/lecture-3.pdf\|Lição 3]]     |
| 4     | Computação segura multipartes: transferência inconsciente 1-de-2 e partilha de segredos de Shamir                                                                                                                                      | [[SSI/Teóricas/lecture-4.pdf\|Lição 4]]     |
| 5     | Segurança em sistemas operativos: separação e mediação de recursos, proteção de memória e controlo de acesso                                                                                                                           | [[SSI/Teóricas/lecture-5.pdf\|Lição 5]]     |
| 6     | Vulnerabilidades em aplicações Web: injeção de código, XSS, autenticação/sessões, referência direta a objetos, CSRF, configuração e armazenamento inseguros, restrição de URLs, comunicação insegura e redirecionamentos não validados | [[SSI/Teóricas/lecture-6.pdf\|Lição 6]]     |
| 7     | Transbordamento de memória (buffer overflow) na heap e na pilha; exploração de problemas de contenção de memória em C                                                                                                                  | [[SSI/Teóricas/lecture-7.pdf\|Lição 7]]     |
| 8     | Proteção dinâmica: deteção com canários, aleatorização e ofuscação, verificação de integridade; vulnerabilidades de corrida e pressupostos de atomicidade                                                                              | [[SSI/Teóricas/lecture-8.pdf\|Lição 8]]     |
| 9     | Auditoria de software: modelação do sistema e de ataques, análise arquitetural de risco e identificação de requisitos de segurança                                                                                                     | [[SSI/Teóricas/lecture-9.pdf\|Lição 9]]     |
| Extra | Material extra (PDF sem texto extraível)                                                                                                                                                                                               | [[SSI/Teóricas/lecture-x.pdf\|Lição Extra]] |

## Aulas Práticas

| Aula | Tema                                                                                                                               | Material                                                            |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| 1    | Funções de hash, criptografia de chave simétrica e MACs; manuseamento da ferramenta OpenSSL                                        | [[Aula 1]] · [[SSI/Práticas/lab-1.pdf\|Lab 1]]                      |
| 2    | Funções de hash para construção de provas de trabalho; revisão de criptografia de chave pública e assinaturas digitais             | [[SSI/Práticas/lab-2.pdf\|Lab 2]]                                   |
| 3    | OpenSSL avançado: geração de números pseudo-aleatórios, derivação de chaves a partir de palavras-passe e teste de comunicações TLS | [[SSI/Práticas/lab-3.pdf\|Lab 3]]                                   |
| 4    | Autenticação de entidades: autenticação fraca, razoavelmente forte e forte                                                         | [[SSI/Práticas/lab-4.pdf\|Lab 4]]                                   |
| 5    | Instalação e utilização do Secure Shell (SSH)                                                                                      | [[SSI/Práticas/Lab-5\|Lab 5]] · [[SSI/Práticas/lab-5-SSH.pdf\|SSH]] |
| 6    | Implementação de ElGamal em Java e estudo da Java Cryptography Architecture (JCA)                                                  | [[SSI/Práticas/lab-6.pdf\|Lab 6]]                                   |
| 7    | Implementação de ElGamal em curvas elípticas em Python                                                                             | [[SSI/Práticas/lab-7.pdf\|Lab 7]]                                   |
| 8    | Controlo de acesso discricionário num sistema operativo Unix-like                                                                  | [[SSI/Práticas/lab-8.pdf\|Lab 8]]                                   |
| 9    | Instalação de servidor HTTP/PHP e MySQL; aplicação web para teste de um vetor de ataque                                            | [[SSI/Práticas/lab-9.pdf\|Lab 9]]                                   |
| 10   | Vulnerabilidades de buffer overflow e condições de corrida: implementação, compilação e execução de programas suscetíveis          | [[SSI/Práticas/lab-10.pdf\|Lab 10]]                                 |

## Trabalhos

- [[Bat (couf couf) Evidence A Wayne]]
problema da fragmentacao da memeoria:
quando alguns programas entram e saem da memoria, deixa espacos obscuros na memoria
