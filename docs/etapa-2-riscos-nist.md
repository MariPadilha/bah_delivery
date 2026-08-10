# Etapa 2 — Avaliação e Tratamento de Riscos

Esta etapa dá continuidade à análise iniciada na [Etapa 1 — Casos de Abuso e Modelagem de Ameaças com STRIDE](./etapa-1-ameacas-stride.md), transformando as ameaças e os casos de abuso já identificados em riscos que possam ser avaliados, comparados, priorizados e tratados.

O sistema, os ativos, os usuários, as ameaças STRIDE e os casos de abuso são os mesmos da etapa anterior.

---

## Sumário

1. [Metodologia de Avaliação de Riscos](#1-metodologia-de-avaliação-de-riscos)
2. [Registro Inicial de Riscos](#2-registro-inicial-de-riscos)
3. [Tratamento de Riscos](#3-tratamento-de-riscos)
4. [Distribuição Integrante X Responsabilidades](#4-distribuição-integrante-x-responsabilidades)

---

## Nota sobre a Revisão dos Identificadores da Etapa 1

Durante a elaboração desta etapa, o grupo identificou uma colisão de identificadores entre as duas etapas: as ameaças da categoria *Repudiation* haviam sido numeradas de `R01` a `R04`, e o registro de riscos passou a utilizar o mesmo prefixo `R` a partir de `R01`, conforme o modelo apresentado no enunciado. A tabela de rastreabilidade tornava-se ambígua, pois uma linha como "R06 originado de R01, R02, R03 e R04" não permitia distinguir ameaças de riscos.

Para preservar a coerência entre as etapas, as ameaças de *Repudiation* foram renumeradas de `R01`–`R04` para `RP01`–`RP04` no documento da Etapa 1, mantendo inalterados o texto, o conteúdo e a ordem das ameaças. As referências cruzadas nos casos de abuso CA04 e CA08 e nas considerações finais foram atualizadas na mesma medida. Nenhuma ameaça foi acrescentada, removida ou reescrita: a alteração é exclusivamente de identificação.

O prefixo `R` fica assim reservado aos riscos desta etapa.

---

## 1. Metodologia de Avaliação de Riscos

A avaliação de riscos do Bah Delivery foi estruturada a partir das ameaças identificadas na Etapa 1 por meio do modelo STRIDE.

Para esta etapa, as ameaças identificadas anteriormente foram utilizadas como origem para a definição de eventos de risco. O objetivo é estabelecer uma relação rastreável entre as ameaças da Etapa 1 e os riscos que serão posteriormente avaliados, priorizados e tratados.

A avaliação dos riscos será realizada considerando dois fatores:

- **Probabilidade:** representa a possibilidade de ocorrência do evento de risco nas condições atuais do sistema.
- **Impacto:** representa a gravidade das consequências caso o evento de risco ocorra.

A pontuação de cada risco será obtida pela multiplicação da probabilidade pelo impacto.

> **Risco = Probabilidade × Impacto**

A probabilidade e o impacto serão avaliados utilizando uma escala de 1 a 4.

---

### 1.1 Critérios de Probabilidade

| Valor | Classificação | Critério |
|---:|---|---|
| 1 | Baixa | O evento de risco é pouco provável de ocorrer nas condições atuais do sistema. |
| 2 | Média-baixa | O evento de risco pode ocorrer, mas depende de condições específicas. |
| 3 | Média-alta | O evento de risco é plausível e existem condições favoráveis para sua ocorrência. |
| 4 | Alta | O evento de risco é provável de ocorrer nas condições atuais do sistema. |

---

### 1.2 Critérios de Impacto

| Valor | Classificação | Critério |
|---:|---|---|
| 1 | Baixo | O evento causa consequências limitadas para o sistema ou para seus usuários. |
| 2 | Médio | O evento pode afetar parcialmente uma operação, ativo ou grupo limitado de usuários. |
| 3 | Alto | O evento pode causar prejuízos relevantes ou comprometer ativos importantes do sistema. |
| 4 | Muito alto | O evento pode comprometer ativos críticos, diversos usuários ou operações essenciais da plataforma. |

---

### 1.3 Cálculo do Risco

A pontuação do risco será calculada pela multiplicação entre a probabilidade e o impacto:

> **Risco = Probabilidade × Impacto**

Dessa forma, a pontuação mínima possível é 1 e a máxima é 16.

| Probabilidade | Impacto | Pontuação |
|---:|---:|---:|
| 1 | 1 | 1 |
| 1 | 2 | 2 |
| 1 | 3 | 3 |
| 1 | 4 | 4 |
| 2 | 1 | 2 |
| 2 | 2 | 4 |
| 2 | 3 | 6 |
| 2 | 4 | 8 |
| 3 | 1 | 3 |
| 3 | 2 | 6 |
| 3 | 3 | 9 |
| 3 | 4 | 12 |
| 4 | 1 | 4 |
| 4 | 2 | 8 |
| 4 | 3 | 12 |
| 4 | 4 | 16 |

---

### 1.4 Matriz de Risco

| Probabilidade \ Impacto | 1 | 2 | 3 | 4 |
|---|---:|---:|---:|---:|
| **1 — Baixa** | 1 | 2 | 3 | 4 |
| **2 — Média-baixa** | 2 | 4 | 6 | 8 |
| **3 — Média-alta** | 3 | 6 | 9 | 12 |
| **4 — Alta** | 4 | 8 | 12 | 16 |

A classificação final de cada risco será definida após a atribuição de probabilidade e impacto.

---

## 2. Registro Inicial de Riscos

Os riscos foram derivados das ameaças identificadas na Etapa 1. O registro inicial mantém a rastreabilidade por meio dos identificadores STRIDE utilizados anteriormente.

| ID | Origem STRIDE | Evento de risco | Probabilidade | Impacto | Pontuação | Nível |
|---|---|---|---:|---:|---:|---|
| R01 | S01, S05 | Comprometimento da conta de um cliente por obtenção indevida de suas credenciais | 3 | 3 | 9 | Alto |
| R02 | S02 | Utilização indevida de uma sessão legítima por meio do comprometimento de seu token | 2 | 3 | 6 | Médio |
| R03 | S03 | Cadastro de restaurante inexistente para obtenção fraudulenta de pagamentos e dados | 2 | 3 | 6 | Médio |
| R04 | T01 | Manipulação do valor de um pedido antes da realização do pagamento | 3 | 3 | 9 | Alto |
| R05 | T02, T04 | Alteração indevida de informações relacionadas ao processo de entrega | 2 | 3 | 6 | Médio |
| R06 | RP01, RP02, RP03, RP04 | Impossibilidade de atribuir responsabilidade a ações realizadas no sistema | 2 | 3 | 6 | Médio |
| R07 | I01, I02, I03 | Exposição indevida de dados pertencentes a clientes | 3 | 4 | 12 | Crítico |
| R08 | I05, I07 | Comprometimento de credenciais e informações sensíveis armazenadas pela plataforma | 3 | 4 | 12 | Crítico |
| R09 | D01, D02, D03 | Indisponibilidade da plataforma de delivery | 3 | 4 | 12 | Crítico |
| R10 | D04, D06 | Interrupção ou sabotagem de operações relacionadas aos restaurantes e entregas | 2 | 4 | 8 | Alto |
| R11 | E01, E02, E03 | Obtenção indevida de privilégios administrativos | 3 | 4 | 12 | Crítico |
| R12 | E05, E06 | Abuso de privilégios administrativos com possibilidade de ocultação das ações realizadas | 2 | 4 | 8 | Alto |
| R13 | T05, I06 | Comprometimento da integridade do banco de dados por injeção de comandos nas consultas da aplicação | 4 | 4 | 16 | Crítico |
| R14 | I04, I08 | Captura de credenciais e de dados em trânsito, precedida da identificação das contas existentes na plataforma | 2 | 4 | 8 | Alto |
| R15 | T03, T06 | Distorção de preços e de avaliações após a confirmação do pedido pelo cliente | 2 | 3 | 6 | Médio |
| R16 | S04, E04 | Acesso a dados de clientes e de estabelecimentos por identidade não verificada ou por perfil sem delimitação de escopo | 3 | 3 | 9 | Alto |
| R17 | D05, D07 | Esgotamento de recursos da plataforma pelo uso abusivo das funções de envio de mensagens e de upload de imagens | 3 | 3 | 9 | Alto |

Os riscos **R13** a **R17** foram acrescentados após a revisão da cobertura do registro. A verificação constatou que dez ameaças da Etapa 1 (S04, T03, T05, T06, I04, I06, I08, D05, D07 e E04) ainda não haviam originado nenhum risco, entre elas a injeção de comandos SQL (T05) e o tráfego sem proteção adequada (I04). Com esses cinco riscos, as 36 ameaças identificadas na Etapa 1 passam a estar integralmente cobertas.

---
## 2.1 Justificativas da Avaliação dos Riscos

A atribuição dos valores de probabilidade e impacto considera as condições atuais identificadas para o Bah Delivery. A probabilidade representa a possibilidade de ocorrência do evento nas condições atuais do sistema, enquanto o impacto representa a gravidade das consequências caso o evento se concretize.

A avaliação utiliza a escala definida anteriormente no documento, na qual a probabilidade varia de 1 (Baixa) a 4 (Alta) e o impacto varia de 1 (Baixo) a 4 (Muito alto).

---
### *R01 - Comprometimento da conta de um cliente por obtenção indevida de suas credenciais*

  Probabilidade = 3 (Média-Alta). As ameaças S01 e S05 indicam condições que podem resultar na obtenção indevida das credenciais de um cliente. Uma vez obtidas credenciais válidas, o atacante pode utilizá-las para se autenticar como o usuário legítimo. Dessa forma, o evento é considerado plausível nas condições identificadas.
  
  Impacto = 3 (Alto). O comprometimento permite acesso às informações e funcionalidades disponíveis para a conta afetada, podendo também possibilitar ações em nome do cliente. Entretanto, o evento descrito considera inicialmente o comprometimento de uma conta individual, não implicando necessariamente o comprometimento simultâneo de diversos usuários ou de uma operação essencial da plataforma.

Pontuação: 3 × 3 = 9 (Alto).

---
### R02 - Utilização indevida de uma sessão legítima por meio do comprometimento de seu token

Probabilidade: 2 (Média-baixa). A concretização do evento depende do comprometimento prévio de um token de sessão válido. Portanto, o atacante precisa satisfazer uma condição específica antes de conseguir utilizar a sessão como o usuário legítimo.

Impacto: 3 (Alto). Um token comprometido pode permitir acesso aos dados e operações disponíveis naquela sessão. Entretanto, o alcance do ataque tende a permanecer limitado aos privilégios do usuário e à validade da sessão comprometida, justificando impacto alto, mas não muito alto.

Pontuação: 2 × 3 = 6 (Médio).

---
### R03 - Cadastro de restaurante inexistente para obtenção fraudulenta de pagamentos e dados

Probabilidade: 2 (Média-baixa). A ocorrência depende da possibilidade de cadastrar um estabelecimento sem verificação suficiente de sua identidade. Além disso, a criação de um restaurante fraudulento exige preparação e interação com o processo de cadastro, caracterizando condições específicas para a concretização do evento.

Impacto: 3 (Alto). Um estabelecimento falso pode enganar clientes e gerar operações fraudulentas, causando prejuízos financeiros e comprometendo dados associados aos pedidos. Entretanto, o impacto tende inicialmente a se concentrar nos usuários que interagirem com o estabelecimento fraudulento.

Pontuação: 2 × 3 = 6 (Médio).

---
### R04 - Manipulação do valor de um pedido antes da realização do pagamento

Probabilidade: 3 (Média-alta). A ameaça T01 está associada à confiança em valores calculados ou fornecidos pelo cliente. Caso esses valores sejam aceitos sem validação independente no servidor, existe uma condição favorável para que um usuário mal-intencionado altere o valor enviado antes da realização do pagamento.

Impacto: 3 (Alto). A exploração compromete a integridade financeira da operação e pode permitir a realização de pedidos por valores diferentes dos efetivamente devidos, causando prejuízos ao estabelecimento ou à plataforma. Entretanto, o evento está relacionado inicialmente à manipulação de pedidos específicos e não necessariamente ao comprometimento de todo o sistema de pagamentos.

Pontuação: 3 × 3 = 9 (Alto).

---
### R05 - Alteração indevida de informações relacionadas ao processo de entrega

Probabilidade: 2 (Média-baixa). A ocorrência depende da existência e exploração de falhas específicas que permitam a alteração de informações de entrega por um usuário que não deveria possuir essa permissão.

Impacto: 3 (Alto). A manipulação pode causar divergências no estado das entregas, fraudes operacionais, atrasos e prejuízos aos usuários envolvidos. Entretanto, seus efeitos tendem a permanecer concentrados nas entregas afetadas, sem necessariamente comprometer a plataforma como um todo.

Pontuação: 2 × 3 = 6 (Médio).

---
### R06 - Impossibilidade de atribuir responsabilidade a ações realizadas no sistema

Probabilidade: 2 (Média-baixa). Para que o risco produza consequências relevantes, é necessário que uma ação contestada ou maliciosa seja realizada e que os registros disponíveis sejam insuficientes para determinar sua autoria. Dessa forma, sua concretização depende de condições específicas.

Impacto: 3 (Alto). A ausência de rastreabilidade pode prejudicar investigações de incidentes, auditorias e resolução de disputas entre clientes, restaurantes, entregadores e administradores. Apesar disso, as funcionalidades principais da plataforma podem continuar operando normalmente.

Pontuação: 2 × 3 = 6 (Médio).

---
### R07 - Exposição indevida de dados pertencentes a clientes

Probabilidade: 3 (Média-alta). As ameaças I01, I02 e I03 estão relacionadas a falhas de proteção e de verificação de propriedade dos recursos. Como informações de clientes são acessadas por diferentes funcionalidades, essas condições tornam plausível o acesso indevido a dados pertencentes a outros usuários.

Impacto: 4 (Muito alto). Dependendo da abrangência da falha, informações de diversos clientes podem ser expostas. O evento compromete diretamente a confidencialidade de dados dos usuários e pode atingir um conjunto significativo de contas, justificando a classificação de impacto muito alto.

Pontuação: 3 × 4 = 12 (Crítico).

---
### R08 - Comprometimento de credenciais e informações sensíveis armazenadas pela plataforma

Probabilidade: 3 (Média-alta). As ameaças I05 e I07 indicam condições relacionadas ao armazenamento ou tratamento inadequado de informações sensíveis. Caso o componente responsável pelo armazenamento seja comprometido, esses dados podem ser obtidos indevidamente.

Impacto: 4 (Muito alto). Credenciais e informações sensíveis representam ativos importantes para a segurança da plataforma. Seu comprometimento pode afetar diversos usuários e ainda possibilitar ataques posteriores utilizando os dados obtidos.

Pontuação: 3 × 4 = 12 (Crítico).

---
### R09 - Indisponibilidade da plataforma de delivery

Probabilidade: 3 (Média-alta). As ameaças D01, D02 e D03 representam diferentes condições capazes de prejudicar a disponibilidade da plataforma. Como o serviço depende de seus recursos computacionais e componentes permanecerem acessíveis, a interrupção é considerada plausível diante das ameaças identificadas.

Impacto: 4 (Muito alto). A indisponibilidade da plataforma compromete diretamente uma operação essencial do sistema. Clientes podem ficar impossibilitados de realizar pedidos, restaurantes de recebê-los e entregadores de executar suas atividades, atingindo simultaneamente diferentes grupos de usuários.

Pontuação: 3 × 4 = 12 (Crítico).

---
### R10 - Interrupção ou sabotagem de operações relacionadas aos restaurantes e entregas

Probabilidade: 2 (Média-baixa). Diferentemente da indisponibilidade geral da plataforma, este evento exige a exploração de funcionalidades ou operações específicas relacionadas aos restaurantes ou entregadores. Sua concretização, portanto, depende de condições mais específicas.

Impacto: 4 (Muito alto). O evento pode interromper pedidos ou entregas e provocar prejuízos operacionais aos envolvidos. Afetando diversos usuários.

Pontuação: 2 × 4 = 8 (Alto).

---
### R11 - Obtenção indevida de privilégios administrativos

Probabilidade: 3 (Média-alta). As ameaças E01, E02 e E03 estão relacionadas a falhas de autorização capazes de permitir que um usuário execute operações além dos privilégios correspondentes ao seu perfil. A existência dessas condições torna plausível a obtenção indevida de privilégios elevados.

Impacto: 4 (Muito alto). Uma conta com privilégios administrativos pode possuir acesso a funcionalidades e informações de grande alcance. A obtenção indevida desse nível de privilégio pode afetar diversos usuários, modificar informações relevantes e comprometer operações importantes da plataforma.

Pontuação: 3 × 4 = 12 (Crítico).

---
### R12 - Abuso de privilégios administrativos com possibilidade de ocultação das ações realizadas

Probabilidade: 2 (Média-baixa). O evento exige que o agente possua ou obtenha previamente privilégios administrativos e que, adicionalmente, existam condições que permitam realizar ações sem rastreabilidade suficiente. A necessidade dessas condições reduz a probabilidade de ocorrência.

Impacto: 4 (Muito alto). Caso o evento ocorra, ações administrativas de grande alcance podem ser executadas contra dados ou funcionalidades importantes. A possibilidade de ocultar ou dificultar a atribuição dessas ações também prejudica a investigação e recuperação após o incidente.

Pontuação: 2 × 4 = 8 (Alto).

---
### R13 - Comprometimento da integridade do banco de dados por injeção de comandos nas consultas da aplicação

Probabilidade: 4 (Alta). As ameaças T05 e I06 indicam condições diretamente favoráveis à exploração: entradas fornecidas pelo usuário são concatenadas às consultas da aplicação e mensagens de erro detalhadas podem revelar informações sobre sua estrutura interna. Portanto, não se trata apenas de uma possibilidade abstrata, mas da presença de condições concretas que facilitam a exploração.

Impacto: 4 (Muito alto). Uma injeção de comandos bem-sucedida pode comprometer diretamente o banco de dados, possibilitando leitura ou alteração indevida das informações armazenadas. Como o banco constitui um ativo central da plataforma e armazena informações utilizadas por diferentes funcionalidades e usuários, o impacto potencial é muito alto.

Pontuação: 4 × 4 = 16 (Crítico).

---
### R14 - Captura de credenciais e de dados em trânsito, precedida da identificação das contas existentes na plataforma

Probabilidade: 2 (Média-baixa). As ameaças I04 e I08 indicam que respostas diferentes durante a autenticação podem permitir a identificação de contas existentes e que tráfego sem proteção adequada pode possibilitar a captura das informações transmitidas. Entretanto, a concretização do evento depende da combinação dessas condições e da capacidade do atacante de observar o tráfego, justificando probabilidade média-baixa.

Impacto: 4 (Muito alto). Caso credenciais e dados sejam capturados, contas podem ser posteriormente comprometidas e informações transmitidas podem ter sua confidencialidade violada. Dependendo do alcance da condição de comunicação insegura, diferentes usuários podem ser afetados.

Pontuação: 2 × 4 = 8 (Alto).

---
### R15 - Distorção de preços e de avaliações após a confirmação do pedido pelo cliente

Probabilidade: 2 (Média-baixa). As ameaças T03 e T06 estão relacionadas a condições específicas do tratamento de preços e avaliações. A ocorrência do evento depende da exploração desses comportamentos durante ou após determinadas operações da plataforma.

Impacto: 3 (Alto). A manipulação pode gerar divergências entre o preço apresentado no momento da compra e aquele posteriormente associado ao produto, além de permitir avaliações que não correspondam a pedidos legítimos. Isso compromete a integridade e a confiança nas informações da plataforma, mas seus efeitos tendem a ser mais localizados do que aqueles associados ao comprometimento de ativos centrais.

Pontuação: 2 × 3 = 6 (Médio).

---
### R16 - Acesso a dados de clientes e de estabelecimentos por identidade não verificada ou por perfil sem delimitação de escopo

Probabilidade: 3 (Média-alta). As ameaças S04 e E04 indicam problemas relacionados ao compartilhamento de contas de entregador e à verificação de perfis sem confirmação adequada do escopo de acesso. Essas condições tornam plausível que um usuário consiga visualizar informações que não deveriam estar disponíveis ao seu perfil ou contexto.

Impacto: 3 (Alto). O acesso indevido pode comprometer a confidencialidade de informações pertencentes a clientes ou estabelecimentos. Entretanto, considerando o evento atualmente definido, não há indicação suficiente de que a falha permita necessariamente a exposição massiva de dados ou o comprometimento de uma operação essencial da plataforma.

Pontuação: 3 × 3 = 9 (Alto).

---
### R17 - Esgotamento de recursos da plataforma pelo uso abusivo das funções de envio de mensagens e de upload de imagens

Probabilidade: 3 (Média-alta). As ameaças D05 e D07 indicam ausência de limites adequados no envio de mensagens e no upload de imagens. Essas condições permitem que um usuário realize repetidamente essas operações e consuma recursos da plataforma. Entretanto, o esgotamento efetivo depende de volume suficiente de requisições ou dados enviados.

Impacto: 3 (Alto). O consumo excessivo pode utilizar armazenamento, largura de banda ou capacidade de processamento e provocar degradação das funcionalidades para usuários legítimos. Apesar de relevante, o impacto pode ocorrer gradualmente ou permanecer concentrado nos componentes afetados, diferentemente de uma indisponibilidade completa da plataforma.

Pontuação: 3 × 3 = 9 (Alto).

---

## 2.3 Priorização dos Riscos
A priorização considera principalmente a pontuação de risco (Probabilidade × Impacto) atribuída na seção 2.2. Entretanto, como diversos riscos apresentam a mesma pontuação, o desempate leva em conta os seguintes fatores:
- A extensão do dano financeiro, operacional, reputacional ou legal decorrente da concretização do risco;
- Se o evento tende a atingir um único usuário ou transação, ou múltiplos perfis simultaneamente;
- A criticidade do ativo comprometido para o funcionamento da plataforma;
- A facilidade de reverter ou remediar o dano após o incidente;
- Se a concretização do risco cria ou agrava condições para a ocorrência de outros riscos do registro;
- A velocidade com que o risco pode ser explorado e a necessidade de ação imediata, considerando também a probabilidade já atribuída ao risco.

| Prioridade | ID do Risco | Nível | Justificativa |
|---|---|---|---|
| 1 | R13 | Crítico | Possui a maior pontuação do registro e compromete diretamente o banco de dados, ativo central da plataforma. |
| 2 | R11 | Crítico | Pode habilitar outros riscos críticos, como exposição de dados e indisponibilidade, ao conceder controle administrativo indevido. |
| 3 | R08 | Crítico | Credenciais comprometidas podem ser reutilizadas para tomada de contas, ampliando o alcance do dano. |
| 4 | R07 | Crítico | Compromete de forma irreversível a privacidade de um grande número de clientes. |
| 5 | R09 | Crítico | Afeta a disponibilidade de toda a plataforma, mas tende a ser revertido assim que a causa é contornada. |
| 6 | R01 | Alto | Pode resultar em pedidos fraudulentos e alteração de dados pessoais do cliente afetado. |
| 7 | R16 | Alto | Expõe dados de clientes e de estabelecimentos, atingindo mais de um perfil de usuário. |
| 8 | R04 | Alto | Gera prejuízo financeiro concentrado em pedidos específicos, com recuperação viável por estorno. |
| 9 | R17 | Alto | Tem efeito gradual e mitigável por limites de uso, reduzindo a urgência do tratamento. |
| 10 | R12 | Alto | A ocultação das ações dificulta identificar e reverter o dano causado. |
| 11 | R14 | Alto | Credenciais capturadas podem ser reaproveitadas em outros ataques, mesmo após a troca. |
| 12 | R10 | Alto | Impacta operações pontuais de restaurantes e entregas, com retomada simples após o incidente. |
| 13 | R06 | Médio | Compromete a apuração de qualquer outro incidente do registro por falta de rastreabilidade. |
| 14 | R02 | Médio | Tem efeito limitado à duração da sessão, revertido facilmente pela revogação do token. |
| 15 | R03 | Médio | Prejudica apenas os usuários que interagirem com o estabelecimento fraudulento. |
| 16 | R05 | Médio | Afeta entregas específicas, com correção manual simples das informações. |
| 17 | R15 | Médio | Tem os efeitos mais localizados do grupo e a menor urgência de tratamento. |

---

## 3. Tratamento de Riscos
Após a identificação e a priorização dos riscos, foram definidas estratégias e medidas para reduzir a probabilidade de concretização ou impacto estimado de cada um dos riscos mapeados.

---

### 3.1. Estratégias de Tratamento de Riscos
O tratamento dos riscos será realizado considerando as seguintes estratégias:

| Estratégia | Descrição |
|---|---|
| Evitar | Eliminar a atividade ou condição que dá origem ao risco. |
| Reduzir | Implementar medidas para diminuir a probabilidade ou o impacto. |
| Compartilhar | Atribuir parte da operação ou das consequências a um terceiro. |
| Aceitar | Reconhecer e manter conscientemente o risco, com justificativa e acompanhamento. |

---

### 3.2. Risco X Estratégia de Tratamento
| ID do Risco | Nível Inicial | Estratégia Principal | Justificativa |
|---|---|---|---|
| R01 | Alto | Reduzir | O login de clientes é necessário ao sistema, mas pode receber proteções adicionais, como autenticação multifator. |
| R02 | Médio | Reduzir | A manutenção de sessões é necessária ao uso da plataforma, mas os tokens podem ter validade menor e vinculação ao dispositivo. |
| R03 | Médio | Reduzir | O cadastro de estabelecimentos precisa continuar aberto, mas pode exigir verificação mais rigorosa da identidade do restaurante. |
| R04 | Alto | Reduzir | O cálculo do valor do pedido pode continuar no cliente por usabilidade, mas o valor final deve ser validado no servidor antes do pagamento. |
| R05 | Médio | Reduzir | A atualização de informações de entrega é necessária à operação, mas deve ter controle de acesso e validação mais rígidos. |
| R06 | Médio | Reduzir | Registros de auditoria confiáveis reduzem o risco, mas não eliminam toda possibilidade de contestação sobre a autoria das ações. |
| R07 | Crítico | Reduzir | Os dados de clientes são necessários ao funcionamento do sistema, mas seu acesso deve ser limitado e monitorado. |
| R08 | Crítico | Reduzir | O armazenamento de credenciais e dados sensíveis é necessário, mas deve ser protegido por criptografia forte e controles de acesso rigorosos. |
| R09 | Crítico | Reduzir e compartilhar | A plataforma pode reforçar sua própria infraestrutura e também contar com serviços especializados de proteção contra indisponibilidade. |
| R10 | Alto | Reduzir | As operações de restaurantes e entregadores precisam continuar disponíveis, com maior proteção contra o abuso de suas funcionalidades específicas. |
| R11 | Crítico | Reduzir | As funções administrativas são necessárias, mas devem possuir autorização rigorosa e verificação constante de permissões. |
| R12 | Alto | Reduzir | Auditoria e segregação de funções reduzem o risco de abuso administrativo, mas não eliminam totalmente a possibilidade de ocultação por usuários com privilégios legítimos. |
| R13 | Crítico | Evitar | As consultas ao banco de dados podem ser reescritas de forma parametrizada, eliminando a condição que possibilita a injeção de comandos. |
| R14 | Alto | Reduzir | A comunicação entre cliente e servidor é necessária, mas deve ser protegida por criptografia de transporte e por respostas padronizadas de autenticação. |
| R15 | Médio | Reduzir | Preços e avaliações precisam permanecer editáveis para manutenção do catálogo, mas alterações após a confirmação do pedido devem ter controle e versionamento. |
| R16 | Alto | Reduzir | O acesso a informações entre perfis é necessário ao funcionamento da plataforma, mas o escopo de acesso deve ser verificado e delimitado. |
| R17 | Alto | Reduzir e compartilhar | Os envios de mensagens e uploads devem continuar disponíveis, mas podem ter limites de taxa e compartilhar armazenamento com serviços especializados em nuvem. |

---

### 3.3. Funções do NIST CSF 2.0

O NIST Cybersecurity Framework 2.0 organiza os resultados esperados de segurança em seis funções. As funções descrevem **o que precisa ser alcançado**, e não como isso será feito: elas não são controles nem se confundem com as medidas técnicas adotadas pela plataforma.

| Função | Finalidade |
|---|---|
| **Govern** | Definir políticas, responsabilidades, prioridades e critérios de decisão sobre o risco. |
| **Identify** | Conhecer os ativos, as dependências, as vulnerabilidades e os riscos existentes. |
| **Protect** | Implementar salvaguardas que reduzam a probabilidade ou o impacto do evento. |
| **Detect** | Perceber eventos suspeitos, falhas e possíveis incidentes. |
| **Respond** | Conter, analisar, comunicar e tratar o incidente ocorrido. |
| **Recover** | Restaurar serviços e dados e reduzir os prejuízos causados. |

A distinção entre função, resultado esperado e controle é necessária para evitar que o framework seja utilizado apenas como rótulo. A função indica a categoria do resultado; o resultado esperado descreve a situação que se pretende alcançar no sistema; e o controle é a medida concreta e verificável que produz esse resultado.

| Função | Resultado esperado no Bah Delivery | Controle que produz o resultado |
|---|---|---|
| Govern | A plataforma define quem pode possuir privilégios administrativos e com que periodicidade essa concessão é revista. | Política de concessão e revisão periódica de privilégios administrativos, com responsável designado. |
| Identify | A plataforma conhece todos os pontos do código em que a entrada do usuário compõe uma consulta ao banco de dados. | Inventário das consultas dinâmicas obtido por análise estática do código-fonte. |
| Protect | O acesso às contas administrativas não pode ser obtido apenas com usuário e senha. | Autenticação multifator obrigatória para o perfil administrador. |
| Detect | Tentativas repetidas de autenticação malsucedida contra uma mesma conta são percebidas pela plataforma. | Regra de alerta para cinco falhas de autenticação na mesma conta em dez minutos. |
| Respond | Uma conta identificada como comprometida deixa de operar antes que o dano aumente. | Bloqueio automático da conta e revogação de todas as suas sessões ativas. |
| Recover | Os pedidos afetados por um incidente voltam a refletir os valores corretos. | Procedimento documentado de estorno e recomposição dos pedidos atingidos. |

Os controles apresentados acima são exemplos destinados a ilustrar a distinção. Os controles efetivamente propostos para cada risco são definidos no plano de tratamento.

---

## 4. Distribuição Integrante X Responsabilidades

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | Definir critérios de avaliação de riscos e registrar riscos iniciais. |
| Emanuel Ferreira | Definir ordem de priorização e estimar o risco residual esperado. |
| Guilherme Mundt | Relacionar os riscos às funções do NIST CSF 2.0. |
| Lívia Barbosa | Priorizar os riscos e definir estratégias de tratamento. |
| Mariana Padilha | Avaliar e justificar os riscos inicialmente mapeados. |
| Matheus Ciocca | Definir plano de tratamento de riscos e controles concretos. |
