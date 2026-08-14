# Etapa 2 — Avaliação e Tratamento de Riscos

Esta etapa dá continuidade à análise iniciada na [Etapa 1 — Casos de Abuso e Modelagem de Ameaças com STRIDE](./etapa-1-ameacas-stride.md), transformando as ameaças e os casos de abuso já identificados em riscos que possam ser avaliados, comparados, priorizados e tratados.

O sistema, os ativos, os usuários, as ameaças STRIDE e os casos de abuso são os mesmos da etapa anterior.

---

## Sumário

1. [Metodologia de Avaliação de Riscos](#1-metodologia-de-avaliação-de-riscos)
2. [Registro Inicial de Riscos](#2-registro-inicial-de-riscos)
3. [Tratamento de Riscos](#3-tratamento-de-riscos)
4. [Considerações Finais](#4-considerações-finais)
5. [Distribuição Integrante X Responsabilidades](#5-distribuição-integrante-x-responsabilidades)

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

---

### 2.1 Tabela do Registro de Riscos

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

### 2.2 Justificativas da Avaliação dos Riscos

A atribuição dos valores de probabilidade e impacto considera as condições atuais identificadas para o Bah Delivery. A probabilidade representa a possibilidade de ocorrência do evento nas condições atuais do sistema, enquanto o impacto representa a gravidade das consequências caso o evento se concretize.

A avaliação utiliza a escala definida anteriormente no documento, na qual a probabilidade varia de 1 (Baixa) a 4 (Alta) e o impacto varia de 1 (Baixo) a 4 (Muito alto).

---
### R01 - Comprometimento da conta de um cliente por obtenção indevida de suas credenciais

Probabilidade: 3 (Média-alta). As ameaças S01 e S05 indicam condições que podem resultar na obtenção indevida das credenciais de um cliente. Uma vez obtidas credenciais válidas, o atacante pode utilizá-las para se autenticar como o usuário legítimo. Dessa forma, o evento é considerado plausível nas condições identificadas.

Impacto: 3 (Alto). O comprometimento permite acesso às informações e funcionalidades disponíveis para a conta afetada, podendo também possibilitar ações em nome do cliente. Entretanto, o evento descrito considera inicialmente o comprometimento de uma conta individual, não implicando necessariamente o comprometimento simultâneo de diversos usuários ou de uma operação essencial da plataforma.

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

### 2.3 Priorização dos Riscos

A priorização considera principalmente a pontuação de risco (Probabilidade × Impacto) atribuída na seção 2.1 e justificada na seção 2.2. Entretanto, como diversos riscos apresentam a mesma pontuação, o desempate leva em conta os seguintes fatores:
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

### 3.4. Mapeamento dos Riscos para as Funções do NIST CSF 2.0

O mapeamento indica quais funções são relevantes para o tratamento de cada risco. Para que a marcação não fosse automática, cada função foi associada a um critério explícito de aplicação, e uma função só foi marcada quando o risco satisfaz o critério correspondente.

| Função | Marcada quando |
|---|---|
| Govern | O tratamento depende de uma decisão, política ou atribuição de responsabilidade anterior ao controle técnico, inclusive a contratação de terceiros. |
| Identify | O controle só pode ser corretamente dimensionado após um levantamento que ainda não existe, como inventário de ativos, de fluxos, de permissões ou de trechos de código. |
| Protect | Existe salvaguarda preventiva capaz de reduzir a probabilidade ou o impacto do evento. |
| Detect | O evento, ou algum de seus precursores, produz sinal observável na telemetria da própria plataforma. |
| Respond | Existe ação concreta de contenção a ser executada após a detecção. |
| Recover | Há estado do sistema, dos dados ou dos usuários que precise ser restaurado após o evento. |

#### Matriz de mapeamento

| Risco | Nível | Govern | Identify | Protect | Detect | Respond | Recover |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|
| R01 | Alto | X | — | X | X | X | X |
| R02 | Médio | — | — | X | X | X | — |
| R03 | Médio | X | — | X | X | X | X |
| R04 | Alto | — | X | X | X | X | X |
| R05 | Médio | — | — | X | X | X | X |
| R06 | Médio | X | X | X | X | — | — |
| R07 | Crítico | X | X | X | X | X | X |
| R08 | Crítico | X | X | X | X | X | X |
| R09 | Crítico | X | X | X | X | X | X |
| R10 | Alto | — | — | X | X | X | X |
| R11 | Crítico | X | X | X | X | X | X |
| R12 | Alto | X | — | X | X | X | X |
| R13 | Crítico | X | X | X | X | X | X |
| R14 | Alto | — | X | X | X | X | X |
| R15 | Médio | — | — | X | X | X | X |
| R16 | Alto | — | X | X | X | X | — |
| R17 | Alto | X | X | X | X | X | X |

#### Justificativa das marcações

**R01 — Comprometimento da conta de um cliente.** *Govern* é marcada porque exigir autenticação multifator de clientes é uma decisão de negócio, que pondera o atrito no acesso contra a proteção da conta, e precisa de um responsável. *Identify* não é marcada: o ativo e o fluxo de autenticação já foram descritos na Etapa 1, não restando levantamento pendente. *Detect* corresponde à percepção de tentativas repetidas e de acesso a partir de dispositivo não reconhecido; *Respond*, ao bloqueio da conta e à revogação das sessões; *Recover*, à devolução do acesso ao titular e ao estorno dos pedidos realizados indevidamente.

**R02 — Uso indevido de sessão por token comprometido.** É a linha mais restrita do mapeamento. *Govern* não é marcada porque a política de autenticação que rege a sessão é a mesma decidida em R01, e repeti-la aqui seria duplicação. *Identify* não é marcada pela ausência de levantamento pendente. *Recover* não é marcada porque revogar o token é ação de contenção, e não de restauração: encerrada a sessão, não resta estado a recompor além do que já é tratado em R01.

**R03 — Cadastro de restaurante inexistente.** *Govern* é marcada porque os requisitos de verificação do estabelecimento — quais documentos são exigidos, quem aprova o cadastro e o que ocorre em caso de recusa — constituem política, e não configuração. *Identify* não é marcada porque o fluxo de cadastro já é conhecido. *Detect* não se refere ao momento do cadastro, e sim aos padrões operacionais anômalos que o estabelecimento fraudulento produz depois, como cancelamentos sucessivos e pedidos não entregues.

**R04 — Manipulação do valor do pedido.** *Identify* é marcada porque é necessário levantar quais operações hoje aceitam valores calculados no cliente, e esse conjunto ainda não está determinado. *Govern* não é marcada porque validar o valor no servidor não envolve decisão que preceda a implementação. *Detect* corresponde ao registro da divergência entre o valor recalculado pelo servidor e o valor recebido.

**R05 — Alteração indevida de informações de entrega.** Nem *Govern* nem *Identify* são marcadas: o fluxo de entrega já está descrito na Etapa 1 e o tratamento consiste em autorização e validação, sem decisão prévia pendente. *Detect* corresponde à percepção de mudanças de estado fora da sequência esperada da entrega, e *Recover*, à restauração das informações corretas.

**R06 — Impossibilidade de atribuir responsabilidade.** É o único risco sem *Respond* e um dos poucos sem *Recover*. *Govern* trata da política de auditoria: quais ações devem ser registradas, por quanto tempo e quem responde pela integridade dos registros. *Identify* corresponde ao levantamento das ações que hoje não são registradas. *Detect* refere-se ao monitoramento da própria geração dos registros, isto é, à percepção de lacunas e falhas na cadeia. *Respond* não é marcada porque R06 não produz um incidente a ser contido: ele degrada a capacidade de resposta de todos os demais riscos. *Recover* não é marcada porque um registro que não foi gerado não pode ser reconstituído posteriormente, o que torna o tratamento deste risco necessariamente preventivo.

**R07 — Exposição indevida de dados de clientes.** Reúne as seis funções. *Govern* corresponde à classificação e à minimização dos dados pessoais e às obrigações decorrentes da legislação de proteção de dados. *Identify* exige o mapeamento dos pontos da aplicação que retornam dados de clientes. *Recover* é marcada com ressalva: a confidencialidade perdida não é restaurável, de modo que a função se limita ao encerramento dos acessos indevidos e à comunicação aos titulares afetados e à autoridade competente.

**R08 — Comprometimento de credenciais e dados sensíveis armazenados.** Reúne as seis funções. *Govern* corresponde à política de criptografia e de custódia de segredos, incluindo quem detém as chaves. *Identify* exige o inventário dos locais em que credenciais e informações sensíveis são armazenadas. *Recover* é marcada porque a rotação forçada das credenciais e a reemissão das chaves restabelecem um estado seguro, ainda que não desfaçam a exposição já ocorrida.

**R09 — Indisponibilidade da plataforma.** Reúne as seis funções. *Govern* é marcada de forma especialmente clara porque a estratégia definida na seção 3.2 inclui compartilhar, o que exige decidir, contratar e acompanhar um serviço especializado de proteção, além de estabelecer metas de disponibilidade. *Identify* corresponde ao levantamento dos componentes críticos e dos pontos únicos de falha.

**R10 — Interrupção de operações de restaurantes e entregas.** *Govern* e *Identify* não são marcadas porque as operações afetadas já foram identificadas na Etapa 1 e o tratamento não depende de decisão anterior. As demais funções aplicam-se integralmente, uma vez que o evento é observável, contornável e reversível.

**R11 — Obtenção indevida de privilégios administrativos.** Reúne as seis funções. *Govern* é o núcleo do tratamento: definir quem pode ser administrador, mediante qual aprovação e com que periodicidade a concessão é revista. *Identify* corresponde ao inventário das permissões vigentes e das contas que hoje as detêm. *Recover* abrange a revogação dos privilégios obtidos e a reversão das alterações administrativas realizadas.

**R12 — Abuso de privilégios administrativos com ocultação.** *Govern* trata da segregação de funções e da definição de quem audita o administrador, que é exatamente o ponto do risco: sem essa separação, o agente audita a si mesmo. *Identify* não é marcada porque o levantamento das ações sensíveis é o mesmo já realizado em R06 e não se repete aqui.

**R13 — Injeção de comandos nas consultas da aplicação.** Reúne as seis funções, apesar de a estratégia ser evitar. *Govern* é marcada porque eliminar a condição exige um padrão de codificação que torne obrigatória a consulta parametrizada em toda a aplicação, e não apenas nos trechos já conhecidos. *Identify* é indispensável: sem o inventário completo das consultas dinâmicas, não é possível afirmar que a condição foi eliminada. *Recover* corresponde à restauração da integridade do banco de dados a partir de cópia de segurança.

**R14 — Captura de credenciais e dados em trânsito.** *Identify* corresponde ao levantamento dos pontos de comunicação sem proteção adequada e das respostas de autenticação que diferenciam conta existente de inexistente. *Govern* não é marcada porque o tratamento é de configuração e padronização, sem decisão que o preceda. *Detect* é marcada com ressalva relevante: a interceptação ocorre fora da plataforma e não é observável por ela, de modo que o que se detecta é a enumeração de contas que costuma precedê-la.

**R15 — Distorção de preços e avaliações após a confirmação do pedido.** *Govern* não é marcada porque a regra de alteração de preços é comercial e se implementa na própria aplicação, sem constituir decisão de segurança prévia. *Identify* também não é marcada, pois os pontos afetados já decorrem das ameaças T03 e T06. *Detect* corresponde à percepção de alteração de preço de item com pedido em aberto e de avaliação sem pedido correspondente.

**R16 — Acesso a dados por identidade não verificada ou perfil sem escopo delimitado.** *Identify* é marcada porque é necessário mapear qual perfil acessa qual escopo de informação, levantamento que ainda não existe. *Govern* não é marcada porque a política de privilégios aplicável é a mesma tratada em R11. *Recover* não é marcada porque, diferentemente de R07, o alcance é pontual e nenhum estado do sistema é alterado: encerrar o acesso e revisar o escopo do perfil esgota o tratamento.

**R17 — Esgotamento de recursos por mensagens e uploads.** Reúne as seis funções. *Govern* é marcada porque a estratégia inclui compartilhar, exigindo a contratação do armazenamento externo, e porque a definição das cotas por usuário é decisão de produto. *Identify* corresponde ao levantamento das funções que hoje não possuem limite de uso. *Recover* abrange a liberação do armazenamento indevidamente consumido e o retorno da capacidade normal.

#### Observações sobre a distribuição das funções

| Função | Riscos marcados | Total |
|---|---|---:|
| Govern | R01, R03, R06, R07, R08, R09, R11, R12, R13, R17 | 10 |
| Identify | R04, R06, R07, R08, R09, R11, R13, R14, R16, R17 | 10 |
| Protect | Todos os riscos | 17 |
| Detect | Todos os riscos | 17 |
| Respond | Todos, exceto R06 | 16 |
| Recover | Todos, exceto R02, R06 e R16 | 14 |

*Protect* aparece em todos os riscos como consequência direta da seção 3.2: todas as estratégias escolhidas foram reduzir ou evitar, e ambas dependem de salvaguarda preventiva. Caso algum risco tivesse sido aceito, a função não seria marcada naquela linha. *Detect* também aparece em todos os riscos porque cada evento produz algum sinal observável na plataforma, ainda que em R14 esse sinal seja apenas o precursor do evento, e não o evento em si.

A diferenciação efetiva do mapeamento ocorre, portanto, em *Govern*, *Identify*, *Respond* e *Recover*, que somam 50 marcações em 68 possíveis. Dos 102 cruzamentos entre riscos e funções, 84 foram marcados e 18 foram descartados com justificativa própria.

Os riscos que reúnem as seis funções são R07, R08, R09, R11, R13 e R17. Cinco deles são exatamente os riscos de nível crítico do registro, o que é coerente com a priorização estabelecida na seção 2.3: quanto mais central o ativo atingido, mais amplo o conjunto de resultados de segurança necessários. A linha mais restrita é a de R02, com três funções, em razão do alcance limitado e da reversibilidade da sessão comprometida.

As funções indicadas nesta seção descrevem apenas os resultados esperados. Os controles concretos, os responsáveis e as evidências de verificação correspondentes a cada risco são definidos no plano de tratamento.

---

### 3.5. Plano de Tratamento

Esta seção apresenta, para cada risco do registro, os controles propostos, os responsáveis por sua implementação e as evidências que permitirão verificar se o controle existe e funciona. É o ponto em que o plano deixa de descrever resultados esperados e passa a indicar medidas concretas.

O plano observa três regras de coerência com as seções anteriores:

1. A estratégia de cada risco é a mesma definida na seção 3.2 e não é reaberta aqui.
2. As funções do NIST são exatamente as marcadas na matriz da seção 3.4. Toda função marcada possui ao menos um controle correspondente, e nenhum controle invoca função que não tenha sido marcada para aquele risco.
3. Cada controle indica onde será aplicado e qual condição identificada na Etapa 1 pretende remover ou limitar. Controles genéricos, como "usar criptografia" ou "monitorar o sistema", não são utilizados: quando a medida deriva de uma dessas ideias, o texto especifica o componente, o parâmetro e a forma de verificação.

Os controles recebem identificadores próprios (`C01` em diante) para que possam ser referenciados sem ambiguidade. Alguns controles atendem a mais de um risco e reaparecem com o mesmo identificador, o que é consolidado na seção 3.5.4.

Nenhum controle está implementado. Conforme o enunciado, a redução efetiva do risco somente poderá ser afirmada após implementação, teste e obtenção de evidências, o que é tratado na estimativa de risco residual da seção 3.7.

---

#### 3.5.1. Responsáveis

Como o Bah Delivery não está implementado, os responsáveis são definidos por papel funcional, e não por pessoa. Os papéis abaixo são os utilizados nas colunas *Responsável* de todo o plano. Eles descrevem a organização hipotética que manteria a plataforma, e não se confundem com a distribuição de tarefas entre os integrantes do grupo, registrada na seção 4.

| Papel | Escopo de atuação |
|---|---|
| Backend | API REST, regras de negócio, consultas ao banco de dados, autenticação e autorização no servidor. |
| Frontend | Interface web dos quatro perfis, formulários e apresentação de mensagens ao usuário. |
| Infraestrutura | Servidor da aplicação, banco de dados, TLS, backups, borda (CDN e WAF) e serviços externos contratados. |
| Segurança | Políticas, revisão de código sob a ótica de segurança, definição das regras de alerta e condução da resposta a incidentes. |
| Produto | Decisões de negócio que precedem o controle técnico: atrito aceitável na autenticação, cotas por usuário e requisitos de cadastro. |
| Suporte | Atendimento aos quatro perfis, aprovação de cadastros, contenção manual, estornos e comunicação aos usuários. |
| Encarregado de Dados | Obrigações decorrentes da legislação de proteção de dados: classificação, minimização, retenção e comunicação aos titulares e à autoridade. |

---

#### 3.5.2. Planos por Risco

Os riscos são apresentados na ordem do registro. A ordem de implementação dos controles é definida na seção 3.6 e não corresponde a esta sequência.

---

##### R01 - Comprometimento da conta de um cliente por obtenção indevida de suas credenciais

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Credenciais de autenticação, dados pessoais dos clientes, histórico de pedidos e informações de pagamento.
- **Funções do NIST:** Govern, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C01 | Política de autenticação aprovada pelo Produto, definindo que o segundo fator é obrigatório para os perfis administrador e restaurante e exigido do cliente em acesso a partir de dispositivo não reconhecido, com responsável designado pela revisão anual. | Govern | Produto e Segurança | Documento de política versionado no repositório, com data de aprovação e responsável nomeado. |
| C02 | Segundo fator de autenticação implementado conforme a política, por aplicativo de código temporário para administrador e restaurante e por código enviado ao cliente em dispositivo novo. | Protect | Backend | Teste automatizado que confirma a recusa do acesso administrativo somente com usuário e senha; roteiro de teste manual do fluxo do cliente. |
| C03 | Limitação progressiva das tentativas de autenticação, aplicada por conta e por origem da requisição, com atraso incremental e desafio após o limiar, sem bloqueio permanente acionado apenas pela contagem por conta. | Protect | Backend | Teste automatizado que envia tentativas sucessivas e verifica o atraso aplicado; verificação de que a conta permanece acessível à origem legítima, conforme exigido em R10. |
| C04 | Recusa, no cadastro e na troca de senha, de senhas presentes em listas públicas de credenciais vazadas, por consulta que não transmite a senha em texto claro. | Protect | Backend | Teste automatizado com senha conhecidamente vazada esperando recusa; registro da versão da lista consultada. |
| C05 | Publicação dos registros SPF e DKIM e política DMARC em modo de rejeição para o domínio da plataforma, com as notificações oficiais deixando de conter links que levem à tela de autenticação. | Protect | Infraestrutura | Consulta pública aos registros do domínio; revisão dos modelos de mensagem confirmando a ausência de links de autenticação. |
| C06 | Regra de alerta para cinco falhas de autenticação na mesma conta em dez minutos e para autenticação bem-sucedida a partir de dispositivo ou origem não reconhecidos. | Detect | Segurança | Simulação das duas condições em ambiente de teste com registro do alerta gerado e do tempo até a notificação. |
| C07 | Bloqueio da conta identificada como comprometida e revogação de todas as suas sessões e tokens de renovação. | Respond | Backend e Suporte | Exercício de conta comprometida, medindo o tempo entre a decisão e a perda de acesso do atacante simulado. |
| C08 | Devolução do acesso ao titular por fluxo de recuperação com verificação de identidade, seguida do estorno dos pedidos realizados durante o período de comprometimento. | Recover | Suporte | Procedimento documentado; registro dos casos tratados com data de restabelecimento e valor estornado. |

*Observação:* C03 é deliberadamente formulado para não introduzir o problema descrito em D06, no qual o bloqueio por tentativas se converte em meio de indisponibilizar contas alheias. O mesmo controle é reaproveitado no tratamento de R10.

---

##### R02 - Utilização indevida de uma sessão legítima por meio do comprometimento de seu token

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Credenciais de autenticação, sessões de usuário e API REST.
- **Funções do NIST:** Protect, Detect e Respond.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C09 | Redução da validade do token de acesso para quinze minutos, com renovação por token de uso único e rotativo, transportados em cookie com os atributos HttpOnly, Secure e SameSite. | Protect | Backend | Teste automatizado que confirma a expiração no prazo e a recusa do token de renovação já utilizado; inspeção dos atributos do cookie na resposta. |
| C10 | Vinculação do token de renovação ao dispositivo e à faixa de origem em que foi emitido, com exigência de nova autenticação quando qualquer dos dois muda. | Protect | Backend | Teste que reapresenta o token a partir de outra origem e espera recusa com nova autenticação. |
| C11 | Regra de alerta para uso do mesmo token a partir de dois dispositivos ou origens distintos na mesma janela e para reapresentação de token de renovação já rotacionado. | Detect | Segurança | Simulação de reuso de token com registro do alerta e da sessão encerrada. |
| C07 | Revogação de todas as sessões e tokens de renovação da conta afetada. | Respond | Backend e Suporte | Mesma evidência registrada em R01. |

*Observação:* a ausência de *Recover* segue a justificativa da seção 3.4. Revogar o token é contenção, e o estado eventualmente alterado durante a sessão comprometida é recomposto pelo controle C08, já previsto em R01.

---

##### R03 - Cadastro de restaurante inexistente para obtenção fraudulenta de pagamentos e dados

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Dados dos restaurantes, informações de pagamento e dados pessoais dos clientes.
- **Funções do NIST:** Govern, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C12 | Política de credenciamento de estabelecimentos, definindo os documentos exigidos, quem aprova o cadastro, o prazo de análise e os efeitos da recusa. | Govern | Produto e Suporte | Política versionada; amostra de cadastros analisados com a decisão registrada e o aprovador identificado. |
| C13 | Validação do CNPJ informado na base pública da Receita Federal, confirmação do endereço do estabelecimento e verificação de que a conta de repasse pertence ao mesmo CNPJ. | Protect | Backend e Suporte | Teste com CNPJ inexistente e com CNPJ divergente da conta bancária, ambos esperando recusa; registro da consulta realizada por cadastro aprovado. |
| C14 | Manutenção do estabelecimento em estado pendente até a aprovação, sem receber pedidos nem repasses nesse período. | Protect | Backend | Teste automatizado que tenta enviar pedido a estabelecimento pendente e espera recusa. |
| C15 | Painel de indicadores por estabelecimento com alerta para cancelamentos sucessivos, pedidos marcados como entregues sem confirmação do cliente e taxa de contestação acima do limiar definido. | Detect | Segurança e Suporte | Simulação com dados de teste reproduzindo o padrão fraudulento e registro do alerta gerado. |
| C16 | Suspensão do estabelecimento e retenção do repasse pendente enquanto a apuração ocorre. | Respond | Suporte | Registro dos casos suspensos com data, motivo e valor retido. |
| C17 | Estorno aos clientes atingidos e comunicação individual informando o ocorrido. | Recover | Suporte | Procedimento documentado; relação dos estornos efetuados com o prazo de conclusão. |

---

##### R04 - Manipulação do valor de um pedido antes da realização do pagamento

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Pedidos, carrinho de compras e informações de pagamento.
- **Funções do NIST:** Identify, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C18 | Levantamento das operações que hoje aceitam valores calculados no navegador, produzindo a relação dos endpoints e dos campos envolvidos. | Identify | Backend | Relação dos endpoints anexada ao repositório, com o responsável pela verificação de cada um. |
| C19 | Recálculo obrigatório do total no servidor a partir do preço vigente registrado no cardápio, descartando integralmente o valor recebido do cliente. | Protect | Backend | Teste automatizado que envia total divergente e verifica que o pagamento é iniciado pelo valor recalculado. |
| C20 | Registro de cada divergência entre o valor recebido e o recalculado, com alerta quando a mesma conta produzir divergências repetidas. | Detect | Backend e Segurança | Consulta ao registro após a execução do teste de divergência; simulação de reincidência com o alerta gerado. |
| C21 | Rejeição do pedido cujo valor divirja do recalculado, com resposta de erro específica e sinalização da conta para análise. | Respond | Backend | Teste automatizado esperando a rejeição; verificação da sinalização na conta usada no teste. |
| C22 | Procedimento de estorno e recomposição dos pedidos concluídos por valor incorreto antes da implementação do recálculo. | Recover | Suporte | Procedimento documentado; relação dos pedidos recompostos com o valor corrigido. |

---

##### R05 - Alteração indevida de informações relacionadas ao processo de entrega

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Pedidos, histórico de entregas e dados pessoais dos clientes.
- **Funções do NIST:** Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C23 | Restrição da atualização do status da entrega ao entregador atribuído ao pedido e ao restaurante que o produziu, verificada no servidor a cada requisição. | Protect | Backend | Teste automatizado em que outro entregador tenta atualizar o pedido e recebe recusa. |
| C24 | Máquina de estados da entrega com as transições permitidas declaradas explicitamente, e endereço de entrega imutável após a autorização do pagamento, admitindo alteração apenas por novo pedido. | Protect | Backend | Teste que tenta transição fora da sequência e alteração de endereço após o pagamento, ambos esperando recusa. |
| C25 | Regra de alerta para tentativa de transição fora da sequência esperada e para requisição de alteração de endereço após o pagamento. | Detect | Segurança | Simulação das duas tentativas com registro do alerta gerado. |
| C26 | Congelamento da entrega em estado sob apuração e escalonamento imediato ao Suporte. | Respond | Backend e Suporte | Roteiro de contenção executado em ambiente de teste, com o tempo até o congelamento. |
| C27 | Restauração das informações corretas da entrega a partir do histórico versionado do pedido. | Recover | Backend e Suporte | Exercício de restauração comparando o estado recomposto com o histórico registrado. |

---

##### R06 - Impossibilidade de atribuir responsabilidade a ações realizadas no sistema

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Logs do sistema, histórico de entregas, cardápios, pedidos e informações de pagamento.
- **Funções do NIST:** Govern, Identify, Protect e Detect.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C28 | Política de auditoria definindo os eventos de registro obrigatório, o prazo de retenção de doze meses e o responsável pela integridade dos registros. | Govern | Segurança | Política versionada com a relação dos eventos obrigatórios e o responsável nomeado. |
| C29 | Levantamento das ações sensíveis que hoje não produzem registro, abrangendo pedidos, alterações de cardápio, cancelamentos, conclusões de entrega e operações administrativas. | Identify | Backend e Segurança | Relação das lacunas encontradas, com o endpoint correspondente e a data do levantamento. |
| C30 | Registro de auditoria em modo apenas de acréscimo, mantido em armazenamento segregado do banco da aplicação, com encadeamento por resumo criptográfico e sem permissão de alteração ou exclusão para o perfil administrador. | Protect | Infraestrutura | Tentativa de alteração e de exclusão de registro pelo perfil administrador, ambas esperando recusa; verificação do encadeamento em amostra. |
| C31 | Conteúdo mínimo obrigatório por evento registrado: autor, papel, data e hora, origem da requisição, recurso afetado e valores anterior e posterior, com mascaramento dos campos sensíveis. | Protect | Backend | Inspeção de amostra dos registros confirmando a presença dos campos e a ausência de senha, token e dados de pagamento. |
| C32 | Comprovação da entrega por código de confirmação fornecido pelo cliente no ato do recebimento, com registro de geolocalização na retirada e na entrega. | Protect | Backend | Teste que conclui a entrega sem o código e espera recusa; inspeção dos dados registrados em entrega concluída. |
| C33 | Verificação diária automatizada da cadeia de resumos e alerta para lacuna na sequência de eventos ou falha na geração do registro. | Detect | Infraestrutura e Segurança | Remoção intencional de um registro em ambiente de teste, com o alerta correspondente. |

*Observação:* a ausência de *Respond* e de *Recover* segue a justificativa da seção 3.4. R06 não produz um incidente a ser contido, e um registro que não foi gerado não pode ser reconstituído depois, o que torna o tratamento necessariamente preventivo.

---

##### R07 - Exposição indevida de dados pertencentes a clientes

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Dados pessoais dos clientes, pedidos, histórico de pedidos e API REST.
- **Funções do NIST:** Govern, Identify, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C34 | Política de classificação, minimização e retenção dos dados pessoais, indicando quais campos cada perfil pode receber e por quanto tempo permanecem disponíveis. | Govern | Encarregado de Dados | Política versionada com a tabela de campos por perfil e os prazos de retenção. |
| C35 | Mapeamento dos endpoints que retornam dados de clientes e dos campos efetivamente devolvidos por cada um. | Identify | Backend | Relação dos endpoints e campos, comparada à tabela da política C34. |
| C36 | Verificação, em todo endpoint, de que o registro solicitado pertence ao usuário autenticado, e substituição dos identificadores sequenciais de pedidos, endereços e avaliações por identificadores não previsíveis. | Protect | Backend | Teste automatizado que solicita registro de outro cliente e espera recusa; verificação de que nenhuma resposta expõe identificador sequencial. |
| C37 | Redução do conteúdo devolvido por perfil, de modo que o restaurante receba apenas o nome do cliente e os itens do pedido, e o entregador receba endereço e telefone somente durante a janela da entrega. | Protect | Backend | Comparação das respostas dos três perfis com a tabela de campos da política C34. |
| C38 | Expiração do acesso do entregador aos dados do cliente após a conclusão da entrega, com o histórico de entregas deixando de apresentar endereço e telefone. | Protect | Backend | Teste que consulta o histórico após a conclusão e verifica a ausência dos campos. |
| C39 | Regra de alerta para volume anômalo de leituras de registros pertencentes a outros titulares a partir de uma mesma conta. | Detect | Segurança | Simulação de leitura sequencial de registros com registro do alerta e do limiar acionado. |
| C40 | Suspensão da conta envolvida e bloqueio do padrão de acesso identificado. | Respond | Segurança e Suporte | Exercício de contenção com o tempo entre o alerta e a suspensão. |
| C41 | Encerramento dos acessos indevidos, apuração da extensão da exposição e comunicação aos titulares atingidos e à autoridade de proteção de dados nos prazos legais. | Recover | Encarregado de Dados | Procedimento documentado com os prazos; registro das comunicações efetuadas em caso simulado. |

*Observação:* conforme a ressalva da seção 3.4, a confidencialidade perdida não é restaurável. A função *Recover* limita-se, neste risco, ao encerramento dos acessos e ao cumprimento das obrigações de comunicação.

---

##### R08 - Comprometimento de credenciais e informações sensíveis armazenadas pela plataforma

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Credenciais de autenticação, banco de dados, informações de pagamento e logs do sistema.
- **Funções do NIST:** Govern, Identify, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C42 | Política de criptografia e de custódia de chaves, definindo os algoritmos aceitos, quem detém as chaves e a periodicidade de rotação. | Govern | Segurança e Infraestrutura | Política versionada; registro das rotações realizadas com data e responsável. |
| C43 | Inventário dos locais em que credenciais e informações sensíveis são armazenadas, incluindo backups, ambientes de teste e registros de auditoria. | Identify | Infraestrutura | Inventário datado, com o responsável por cada local e a classificação do dado ali mantido. |
| C44 | Armazenamento das senhas com função de derivação Argon2id, com sal individual por usuário e parâmetros de custo revisados anualmente. | Protect | Backend | Inspeção do formato armazenado em ambiente de teste; registro da revisão anual dos parâmetros. |
| C45 | Eliminação do armazenamento local dos dados de pagamento, mantendo-se apenas a referência tokenizada devolvida pelo provedor. | Protect | Backend | Verificação de que nenhuma coluna do banco retém número de cartão; inspeção do esquema após a alteração. |
| C46 | Criptografia em repouso do banco de dados e dos backups, com os segredos da aplicação mantidos em gerenciador dedicado e ausentes do código-fonte. | Protect | Infraestrutura | Verificação da configuração de criptografia; busca por segredos no histórico do repositório com resultado registrado. |
| C31 | Registro de auditoria sem o corpo integral das requisições, com mascaramento de senha, token e dados de pagamento. | Protect | Backend | Mesma evidência registrada em R06, com atenção específica à ausência dos campos sensíveis. |
| C47 | Restrição do acesso aos registros de auditoria ao papel de auditoria, deixando de estar disponível a todos os administradores. | Protect | Infraestrutura | Tentativa de consulta por administrador sem o papel de auditoria, esperando recusa. |
| C48 | Regra de alerta para leitura em massa das tabelas de credenciais e para acesso ao banco de dados realizado fora da aplicação. | Detect | Infraestrutura e Segurança | Simulação de consulta direta ao banco com registro do alerta gerado. |
| C49 | Revogação dos segredos comprometidos e bloqueio imediato do acesso identificado. | Respond | Infraestrutura | Exercício de vazamento simulado, medindo o tempo até a revogação. |
| C50 | Rotação forçada das senhas dos usuários atingidos, reemissão das chaves de criptografia e invalidação de todas as sessões ativas da plataforma. | Recover | Infraestrutura e Backend | Procedimento documentado, executado em ambiente de teste com o tempo total registrado. |

*Observação:* C45 reduz o impacto ao eliminar o dado armazenado, e não transfere a estratégia para compartilhar. A plataforma permanece responsável pela operação de pagamento e pela referência mantida.

---

##### R09 - Indisponibilidade da plataforma de delivery

- **Estratégia:** Reduzir e compartilhar.
- **Ativos atingidos:** Servidor da aplicação, API REST, banco de dados e pedidos.
- **Funções do NIST:** Govern, Identify, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C51 | Definição da meta de disponibilidade da plataforma e contratação de serviço especializado de proteção contra indisponibilidade, com responsável designado pelo acompanhamento do contrato. | Govern | Produto e Infraestrutura | Meta documentada; contrato vigente e relatório periódico do serviço contratado. |
| C52 | Levantamento dos componentes críticos e dos pontos únicos de falha da plataforma, inclusive dependências externas. | Identify | Infraestrutura | Relação dos componentes com a indicação dos que não possuem redundância. |
| C53 | Mitigação volumétrica na borda por meio de CDN e WAF, absorvendo o tráfego antes que ele alcance o servidor da aplicação. | Protect | Infraestrutura | Relatório de tráfego bloqueado na borda; teste de carga comparando o volume recebido pela origem. |
| C54 | Limitação de requisições por conta e por origem nos endpoints públicos de consulta de restaurantes e cardápios. | Protect | Backend | Teste automatizado que excede o limite e espera resposta de recusa por excesso de requisições. |
| C55 | Paginação obrigatória, limite máximo de registros por resposta e tempo máximo de execução nas consultas de busca e de histórico. | Protect | Backend | Teste que solicita a totalidade dos registros e verifica a aplicação do limite; medição do tempo de consulta. |
| C56 | Limite de pedidos simultâneos em aberto por cliente e envio do pedido ao restaurante somente após a autorização do pagamento. | Protect | Backend | Teste que abre pedidos além do limite esperando recusa; verificação de que o restaurante não recebe pedido sem pagamento autorizado. |
| C57 | Monitoramento de latência, taxa de erro e saturação de recursos, com alerta por limiar e verificação externa de disponibilidade. | Detect | Infraestrutura | Painel de monitoramento em operação; registro de alerta gerado em teste de carga. |
| C58 | Procedimento de contenção documentado, prevendo a ativação do modo de proteção na borda, o bloqueio das origens abusivas e a degradação controlada das funções não essenciais. | Respond | Infraestrutura | Procedimento versionado, exercitado ao menos uma vez com o tempo de execução registrado. |
| C59 | Backup diário com teste de restauração trimestral, mantendo registrado o tempo de recuperação efetivamente obtido. | Recover | Infraestrutura | Relatório de cada teste de restauração, com data, duração e resultado. |

---

##### R10 - Interrupção ou sabotagem de operações relacionadas aos restaurantes e entregas

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Pedidos, dados dos restaurantes, dados dos entregadores e credenciais de autenticação.
- **Funções do NIST:** Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C60 | Limite de entregas aceitas simultaneamente por entregador e expiração automática da reserva não iniciada dentro do prazo definido, devolvendo o pedido à fila. | Protect | Backend | Teste que aceita entregas além do limite esperando recusa; teste de expiração verificando o retorno do pedido à fila. |
| C03 | Limitação das tentativas de autenticação aplicada por origem, e não apenas por conta, de modo que a repetição deliberada de tentativas não indisponibilize a conta de um usuário legítimo. | Protect | Backend | Teste que simula tentativas contra conta alheia e verifica que o titular continua conseguindo autenticar-se. |
| C61 | Regra de alerta para acúmulo de reservas de entrega sem progresso por um mesmo entregador e para tentativas de autenticação em série contra contas distintas. | Detect | Segurança | Simulação dos dois padrões com registro dos alertas gerados. |
| C62 | Liberação das entregas retidas, devolução à fila da região e suspensão da conta responsável pelo abuso. | Respond | Backend e Suporte | Exercício de contenção com o tempo entre o alerta e a liberação dos pedidos. |
| C63 | Reprocessamento dos pedidos afetados e recomposição da fila de entregas da região atingida. | Recover | Suporte | Registro dos pedidos reprocessados e do tempo de normalização da região. |

---

##### R11 - Obtenção indevida de privilégios administrativos

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Controle de permissões, API REST, sessões de usuário e banco de dados.
- **Funções do NIST:** Govern, Identify, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C64 | Política de concessão e revisão periódica dos privilégios administrativos, com aprovação nominal registrada e revisão trimestral das contas que os detêm. | Govern | Segurança e Produto | Política versionada; ata de cada revisão trimestral com as contas mantidas e revogadas. |
| C65 | Inventário das permissões vigentes e das contas que atualmente as detêm, incluindo acessos concedidos em caráter temporário. | Identify | Backend e Segurança | Inventário datado, comparado à relação de aprovações da política C64. |
| C66 | Verificação de autorização no servidor em toda rota administrativa, com negação por padrão, deixando a interface de ser o único ponto de restrição. | Protect | Backend | Teste automatizado que invoca cada rota administrativa com conta de cliente e espera recusa. |
| C67 | Recusa do perfil do usuário como campo da requisição de cadastro ou de atualização, com a atribuição de papel ocorrendo apenas por fluxo administrativo aprovado. | Protect | Backend | Teste que envia o campo de perfil no cadastro e verifica que ele é ignorado. |
| C68 | Assinatura do token de sessão com segredo forte mantido em gerenciador de segredos, algoritmo fixado no servidor e papel do usuário resolvido no servidor a cada requisição, em vez de lido do token. | Protect | Backend | Teste com token forjado e com algoritmo alterado, ambos esperando recusa; verificação de que a alteração do papel no token não produz efeito. |
| C69 | Exigência de nova autenticação e geração de registro de auditoria em toda alteração de permissões de conta. | Protect | Backend | Teste que altera permissão sem reautenticar esperando recusa; inspeção do registro gerado. |
| C70 | Regra de alerta para toda concessão de privilégio administrativo e para o primeiro acesso a rota administrativa por conta recém-elevada. | Detect | Segurança | Simulação de elevação de privilégio com registro dos dois alertas. |
| C07 | Bloqueio da conta envolvida e revogação de todas as suas sessões e tokens de renovação. | Respond | Backend e Segurança | Mesma evidência registrada em R01. |
| C71 | Revogação dos privilégios obtidos indevidamente e das concessões derivadas realizadas pela conta. | Respond | Backend e Segurança | Exercício de elevação simulada, verificando a revogação em cadeia das permissões concedidas. |
| C72 | Reversão das alterações administrativas praticadas pela conta, reconstituídas a partir do registro de auditoria. | Recover | Backend e Suporte | Exercício de reversão comparando o estado final com o registro de auditoria do período. |

---

##### R12 - Abuso de privilégios administrativos com possibilidade de ocultação das ações realizadas

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Controle de permissões, logs do sistema, informações de pagamento e banco de dados.
- **Funções do NIST:** Govern, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C73 | Segregação do perfil administrador em papéis distintos para a gestão de usuários, a consulta de auditoria e o acesso a dados de pagamento, eliminando a concentração de todas as capacidades em um único perfil. | Govern | Segurança e Produto | Matriz de papéis versionada; teste que confirma a recusa de operação fora do papel atribuído. |
| C74 | Definição do papel de auditoria como independente e sem poder de alteração, com indicação nominal de quem audita as ações dos administradores e com que periodicidade. | Govern | Segurança | Designação documentada; relatórios de auditoria periódicos assinados pelo responsável. |
| C75 | Exigência de dupla aprovação para operações administrativas sensíveis, como exclusão de registros, alteração de dados de repasse e acesso a informações de pagamento. | Protect | Backend | Teste que executa a operação com uma única aprovação e espera recusa; inspeção do registro das duas aprovações. |
| C30 | Registro de auditoria em modo apenas de acréscimo, em armazenamento segregado e sem permissão de alteração para o perfil administrador. | Protect | Infraestrutura | Mesma evidência registrada em R06, com ênfase na tentativa de supressão descrita em RP04. |
| C76 | Regra de alerta para ação administrativa sobre dados de pagamento e para tentativa de escrita ou exclusão no repositório de auditoria. | Detect | Infraestrutura e Segurança | Simulação das duas tentativas com registro dos alertas gerados. |
| C77 | Suspensão do administrador envolvido e revisão de todas as ações por ele praticadas no período sob apuração. | Respond | Segurança | Procedimento documentado; relatório da revisão em caso simulado. |
| C78 | Reversão das alterações praticadas e recomposição dos registros a partir da cópia segregada de auditoria. | Recover | Infraestrutura e Backend | Exercício de recomposição comparando o resultado com a cópia segregada. |

*Observação:* a ausência de *Identify* segue a justificativa da seção 3.4. O levantamento das ações sensíveis é o mesmo realizado em C29, no tratamento de R06, e não se repete aqui.

---

##### R13 - Comprometimento da integridade do banco de dados por injeção de comandos nas consultas da aplicação

- **Estratégia:** Evitar.
- **Ativos atingidos:** Banco de dados, pedidos, cardápios, avaliações e API REST.
- **Funções do NIST:** Govern, Identify, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C79 | Padrão de codificação que torna obrigatório o uso de consultas parametrizadas em toda a aplicação, com regra de análise estática impedindo a integração de código que concatene entrada do usuário em consulta. | Govern | Segurança e Backend | Padrão versionado; execução da regra no pipeline, com registro de integração bloqueada em teste. |
| C80 | Inventário completo das consultas dinâmicas existentes, obtido por análise estática do código-fonte, condição sem a qual não é possível afirmar que a exposição foi eliminada. | Identify | Backend | Relatório da análise estática datado, com a relação das ocorrências e o responsável pela correção de cada uma. |
| C81 | Reescrita de todas as consultas identificadas com vinculação de parâmetros, eliminando a concatenação da entrada do usuário, especialmente nos campos de busca de restaurantes e produtos. | Protect | Backend | Nova execução da análise estática sem ocorrências; teste automatizado com carga de injeção esperando resultado sem efeito. |
| C82 | Validação da entrada dos campos de busca por lista de caracteres permitidos e limite de tamanho, mantida como defesa adicional e não como proteção principal. | Protect | Backend | Teste com entrada fora da lista permitida esperando recusa. |
| C83 | Uso de conta de banco de dados com privilégio mínimo pela aplicação, sem permissão de alteração de estrutura nem de acesso a tabelas fora do seu escopo. | Protect | Infraestrutura | Tentativa de alteração de estrutura pela conta da aplicação, esperando recusa. |
| C84 | Substituição das mensagens de erro detalhadas por mensagem genérica ao cliente, com o rastreamento da exceção e a consulta executada mantidos apenas no registro interno. | Protect | Backend | Provocação de erro em ambiente de teste, verificando a resposta ao cliente e a presença do detalhe apenas no registro. |
| C85 | Regra de alerta para erro de sintaxe de consulta na aplicação e para assinatura de injeção identificada na borda. | Detect | Segurança | Simulação de tentativa de injeção com registro dos alertas gerados. |
| C86 | Bloqueio da origem identificada e desativação temporária do endpoint afetado até a correção. | Respond | Infraestrutura | Procedimento documentado, exercitado com o tempo entre o alerta e a desativação. |
| C59 | Backup diário com teste de restauração trimestral, mantendo registrado o tempo de recuperação efetivamente obtido. | Recover | Infraestrutura | Mesma evidência registrada em R09. |
| C87 | Verificação da integridade dos registros restaurados e apuração das alterações praticadas entre o backup e a detecção. | Recover | Infraestrutura e Backend | Exercício de restauração comparando os registros recompostos com o registro de auditoria do período. |

*Observação:* a estratégia é evitar porque C79, C80 e C81, em conjunto, removem a condição que dá origem ao risco. Os demais controles permanecem no plano porque a eliminação só pode ser afirmada após a conclusão do inventário, e porque a injeção pode reaparecer em código futuro caso o padrão não seja imposto pelo pipeline.

---

##### R14 - Captura de credenciais e de dados em trânsito, precedida da identificação das contas existentes na plataforma

- **Estratégia:** Reduzir.
- **Ativos atingidos:** API REST, credenciais de autenticação, informações de pagamento e sistema de autenticação.
- **Funções do NIST:** Identify, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C88 | Levantamento dos pontos de comunicação que não utilizam TLS ou que apresentam certificado mal configurado, e das respostas que diferenciam conta existente de inexistente. | Identify | Infraestrutura e Backend | Relatório de varredura dos endpoints, com a relação das ocorrências e o responsável por cada correção. |
| C89 | Exigência de TLS na versão 1.2 ou superior em toda a comunicação, com redirecionamento das requisições em texto claro, cabeçalho de transporte estrito e monitoramento da validade do certificado. | Protect | Infraestrutura | Verificação externa da configuração de TLS; alerta configurado para a expiração do certificado. |
| C90 | Padronização da resposta e do tempo de resposta no login e na recuperação de senha, de modo que conta existente e inexistente sejam indistinguíveis. | Protect | Backend | Teste automatizado comparando corpo, código de resposta e tempo entre os dois casos. |
| C91 | Regra de alerta para tentativas de autenticação ou de recuperação com muitos endereços distintos a partir de uma mesma origem. | Detect | Segurança | Simulação de enumeração com registro do alerta e do limiar acionado. |
| C92 | Bloqueio da origem identificada e exigência de desafio adicional nas requisições subsequentes. | Respond | Backend e Infraestrutura | Teste que confirma o bloqueio após o limiar e a apresentação do desafio. |
| C93 | Rotação das credenciais e revogação das sessões das contas identificadas como expostas, com comunicação aos titulares. | Recover | Backend e Suporte | Procedimento documentado; registro das contas tratadas e das comunicações enviadas. |

*Observação:* conforme a ressalva da seção 3.4, a interceptação ocorre fora da plataforma e não é observável por ela. O que C91 detecta é a enumeração de contas que costuma precedê-la, e não a captura em si.

---

##### R15 - Distorção de preços e de avaliações após a confirmação do pedido pelo cliente

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Cardápios, pedidos e avaliações.
- **Funções do NIST:** Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C94 | Gravação do preço praticado no próprio pedido no momento da confirmação, de modo que o pedido deixe de referenciar o preço vigente no cardápio. | Protect | Backend | Teste que altera o preço no cardápio após a confirmação e verifica que o valor do pedido permanece inalterado. |
| C95 | Versionamento do cardápio, registrando autoria, data e valor anterior a cada alteração de preço ou de disponibilidade. | Protect | Backend | Inspeção do histórico após sequência de alterações, confirmando autoria e valores anteriores. |
| C96 | Vinculação da avaliação a um pedido efetivamente entregue e ao cliente que o realizou, admitindo uma única avaliação por pedido. | Protect | Backend | Teste que tenta avaliar sem pedido correspondente e avaliar duas vezes o mesmo pedido, ambos esperando recusa. |
| C97 | Regra de alerta para alteração de preço de item com pedido em aberto e para avaliação sem pedido correspondente. | Detect | Segurança | Simulação das duas situações com registro dos alertas gerados. |
| C98 | Bloqueio da alteração de preço enquanto houver pedido em aberto e remoção das avaliações irregulares identificadas. | Respond | Backend e Suporte | Teste do bloqueio; registro das avaliações removidas com o motivo. |
| C99 | Recomposição do valor acordado com o cliente e recálculo da nota do estabelecimento após a remoção das avaliações irregulares. | Recover | Suporte | Registro dos casos recompostos e da nota anterior e posterior ao recálculo. |

---

##### R16 - Acesso a dados de clientes e de estabelecimentos por identidade não verificada ou por perfil sem delimitação de escopo

- **Estratégia:** Reduzir.
- **Ativos atingidos:** Dados dos entregadores, dados dos restaurantes, cardápios e dados pessoais dos clientes.
- **Funções do NIST:** Identify, Protect, Detect e Respond.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C100 | Matriz de perfil por escopo de informação, declarando quais registros cada papel pode acessar e sob qual condição, levantamento que ainda não existe. | Identify | Backend e Segurança | Matriz versionada, comparada às rotas efetivamente expostas pela API. |
| C101 | Verificação, nas operações de gerenciamento, de que o cardápio, o produto ou o pedido manipulado pertence ao estabelecimento autenticado, e não apenas de que o usuário é um restaurante. | Protect | Backend | Teste em que um estabelecimento tenta alterar produto de outro e recebe recusa. |
| C102 | Verificação da identidade do entregador na ativação da conta e reverificação periódica, com a sessão vinculada ao dispositivo utilizado. | Protect | Suporte e Backend | Registro das verificações realizadas por conta ativa; teste de uso da sessão em outro dispositivo esperando nova autenticação. |
| C103 | Regra de alerta para acesso a registros fora do escopo declarado na matriz C100 e para uso simultâneo da conta de entregador em dispositivos distintos. | Detect | Segurança | Simulação dos dois padrões com registro dos alertas gerados. |
| C104 | Encerramento da sessão envolvida, revisão do escopo atribuído ao perfil e nova verificação de identidade do entregador antes do restabelecimento do acesso. | Respond | Suporte e Backend | Procedimento documentado; registro dos casos tratados com a data do restabelecimento. |

*Observação:* a ausência de *Recover* segue a justificativa da seção 3.4. O alcance é pontual e nenhum estado do sistema é alterado, de modo que encerrar o acesso e revisar o escopo do perfil esgota o tratamento.

---

##### R17 - Esgotamento de recursos da plataforma pelo uso abusivo das funções de envio de mensagens e de upload de imagens

- **Estratégia:** Reduzir e compartilhar.
- **Ativos atingidos:** Servidor da aplicação, cardápios e sistema de autenticação.
- **Funções do NIST:** Govern, Identify, Protect, Detect, Respond e Recover.

| ID | Controle proposto | Função | Responsável | Evidência e verificação |
|---|---|---|---|---|
| C105 | Definição das cotas de envio de mensagens e de armazenamento por usuário e contratação do serviço externo de armazenamento, com responsável designado pelo acompanhamento do custo e da cota. | Govern | Produto e Infraestrutura | Cotas documentadas por perfil; contrato vigente e relatório mensal de consumo. |
| C106 | Levantamento das funções que hoje não possuem limite de uso, abrangendo recuperação de senha, confirmação de cadastro e envio de imagens. | Identify | Backend | Relação das funções sem limite, com o limite proposto para cada uma. |
| C107 | Limite de mensagens de recuperação de senha e de confirmação de cadastro por endereço de destino e por origem da requisição, dentro de janela de tempo definida. | Protect | Backend | Teste que excede o limite e verifica a recusa do envio adicional. |
| C108 | Validação de formato, tamanho e quantidade das imagens enviadas por estabelecimento, com normalização no servidor antes do armazenamento. | Protect | Backend | Teste com arquivo acima do tamanho permitido, com formato não aceito e com quantidade acima da cota, todos esperando recusa. |
| C109 | Armazenamento das imagens em serviço externo com cota contratada, entregues por CDN, de modo que o consumo deixe de recair sobre o servidor da aplicação. | Protect | Infraestrutura | Verificação de que as imagens são servidas pelo domínio da CDN; relatório de consumo do serviço contratado. |
| C110 | Regra de alerta para consumo anômalo de armazenamento por estabelecimento e para volume de mensagens acima do previsto. | Detect | Infraestrutura e Segurança | Simulação de consumo acima do limiar com registro dos alertas gerados. |
| C111 | Suspensão da função abusada para a conta envolvida, preservando o acesso às demais operações. | Respond | Backend e Suporte | Teste da suspensão seletiva, confirmando que as demais funções permanecem disponíveis. |
| C112 | Remoção do conteúdo indevidamente enviado e liberação da cota de armazenamento consumida. | Recover | Infraestrutura e Suporte | Registro do volume liberado e da capacidade restabelecida após cada ocorrência. |

---

#### 3.5.3. Cobertura das Funções pelos Controles

A tabela confirma que cada função marcada na matriz da seção 3.4 possui controle correspondente no plano, e que nenhum controle foi proposto para função não marcada.

| Risco | Funções marcadas na seção 3.4 | Controles propostos |
|---|---|---|
| R01 | Govern, Protect, Detect, Respond, Recover | C01 a C08 |
| R02 | Protect, Detect, Respond | C09, C10, C11, C07 |
| R03 | Govern, Protect, Detect, Respond, Recover | C12 a C17 |
| R04 | Identify, Protect, Detect, Respond, Recover | C18 a C22 |
| R05 | Protect, Detect, Respond, Recover | C23 a C27 |
| R06 | Govern, Identify, Protect, Detect | C28 a C33 |
| R07 | Todas as seis | C34 a C41 |
| R08 | Todas as seis | C42 a C50 e C31 |
| R09 | Todas as seis | C51 a C59 |
| R10 | Protect, Detect, Respond, Recover | C60 a C63 e C03 |
| R11 | Todas as seis | C64 a C72 e C07 |
| R12 | Govern, Protect, Detect, Respond, Recover | C73 a C78 e C30 |
| R13 | Todas as seis | C79 a C87 e C59 |
| R14 | Identify, Protect, Detect, Respond, Recover | C88 a C93 |
| R15 | Protect, Detect, Respond, Recover | C94 a C99 |
| R16 | Identify, Protect, Detect, Respond | C100 a C104 |
| R17 | Todas as seis | C105 a C112 |

---

#### 3.5.4. Controles que Atendem a Mais de um Risco

Cinco controles são reaproveitados integralmente por mais de um risco, e três decisões de projeto se repetem em controles distintos aplicados a pontos diferentes da aplicação. O levantamento não estabelece ordem de execução: ele apenas identifica os controles cujo efeito se distribui por vários riscos, servindo de insumo para a definição da ordem de implementação da seção 3.6.

| Controle | Riscos atendidos | Observação |
|---|---|---|
| C03 | R01, R10 | A limitação de tentativas por conta e por origem trata a obtenção de credenciais e, ao mesmo tempo, impede que o próprio controle se torne meio de indisponibilizar contas alheias. |
| C07 | R01, R02, R11 | A revogação de sessões é a ação de contenção comum ao comprometimento de conta, de sessão e de privilégio. |
| C30 | R06, R12 | O registro em modo apenas de acréscimo sustenta a rastreabilidade geral e é a única barreira contra a supressão de evidências pelo administrador. |
| C31 | R06, R08 | O conteúdo mínimo do registro atende à necessidade de auditoria e, pelo mascaramento, evita que o log se torne cópia dos dados sensíveis. |
| C59 | R09, R13 | O backup com teste de restauração é a base da recuperação tanto da indisponibilidade quanto da perda de integridade do banco. |

| Decisão de projeto repetida | Controles | Riscos atendidos |
|---|---|---|
| Verificação de propriedade do recurso no servidor | C23, C36, C101 | R05, R07, R16 |
| Decisão de autorização tomada no servidor, e não na interface nem em campo controlado pelo usuário | C66, C67, C68 | R11 |
| Limitação de uso por conta e por origem | C03, C54, C60, C107 | R01, R09, R10, R17 |

Os controles com maior alcance são, portanto, os relacionados à verificação de propriedade do recurso, ao registro de auditoria protegido e à limitação de uso, o que é coerente com a concentração de marcações observada na seção 3.4.

---

### 3.6. Ordem de Implementação dos Controles

A priorização da seção 2.3 responde a qual risco é mais grave. Esta seção responde a uma pergunta diferente: em que sequência os controles do plano devem ser executados. As duas ordens não coincidem, porque a execução também depende de pré-requisitos técnicos e do alcance de cada controle.

A ordem é definida a partir dos seguintes critérios, aplicados nesta sequência:

1. **Prioridade do risco.** A posição na seção 2.3 é o ponto de partida da ordenação.
2. **Dependência entre controles.** Um controle que condiciona outro é implementado antes. É o caso dos controles de *Identify*, cujo levantamento dimensiona os controles de *Protect* do mesmo risco: sem C80, não é possível concluir C81.
3. **Alcance do controle.** Os cinco controles reaproveitados por mais de um risco, relacionados na seção 3.5.4, antecipam-se aos controles de risco único, porque uma única implementação produz efeito em várias linhas do registro.
4. **Custo de reversão.** Controles que alteram o esquema do banco ou o formato de dados armazenados precedem os que apenas leem esses dados, para evitar retrabalho.
5. **Esforço e responsável.** Controles atribuídos ao mesmo responsável são agrupados quando os critérios anteriores não os separam.

A coluna *Ordem* indica a posição de execução, e não a prioridade do risco, que permanece registrada na coluna própria para permitir a comparação entre as duas. A coluna *Controles envolvidos* reproduz a seção 3.5.3, que permanece sendo a fonte da correspondência entre riscos e controles.

A unidade da ordenação é o risco, e não o controle isolado: cada posição corresponde ao conjunto de controles de um risco. Os cinco controles reaproveitados, relacionados na seção 3.5.4, são implementados na primeira posição em que aparecem, e as posições seguintes que os reutilizam não os implementam de novo. Quando um controle necessário a uma posição só é entregue mais adiante, a coluna *Pré-requisito* registra isso explicitamente.

| Ordem | Risco | Prioridade (2.3) | Nível inicial | Controles envolvidos | Pré-requisito | Justificativa da posição |
|:---:|---|:---:|---|---|---|---|
| 1 | R06 | 13 | Médio | C28 a C33 | — | O registro de auditoria é o que torna verificável todo o restante do plano. C30 e C31 são reaproveitados por R08 e R12, e a evidência exigida por todos os controles de função *Detect* é o alerta registrado. Sem essa base, nenhuma regra das posições seguintes pode ser escrita nem comprovada. Dentro do risco, C29 precede C30 e C31. |
| 2 | R13 | 1 | Crítico | C79 a C87 e C59 | C30 e C31, da posição 1, para o alerta de C85 | Risco de maior pontuação do registro e único com estratégia *Evitar*. C79 vem antes de C81 porque a regra de análise estática precisa estar no pipeline antes da reescrita, sob pena de o código novo reintroduzir a concatenação, e C80 é pré-requisito declarado de C81. A parte *Recover* do risco depende de C59, entregue na posição 9. |
| 3 | R01 | 6 | Alto | C01 a C08 | — | Antecipado em relação à prioridade por dois motivos: C03 e C07 são reaproveitados por R10, R02 e R11, e o serviço de autenticação definido aqui é pressuposto de C68, na posição 5, que resolve o papel do usuário a cada requisição. C01 precede C02, porque a política define de quais perfis o segundo fator é exigido. |
| 4 | R02 | 14 | Médio | C09, C10, C11 e C07 | C07, da posição 3 | O risco de menor prioridade entre os antecipados. C09 e C10 alteram o formato e o ciclo de vida da sessão emitida na posição 3; fazê-lo depois da posição 5 obrigaria a refazer a emissão, a renovação e os testes de C68. É o critério de custo de reversão aplicado à sessão. |
| 5 | R11 | 2 | Crítico | C64 a C72 e C07 | Sessão de R02 e C07, das posições 3 e 4 | Segundo risco em prioridade. Estabelece o ponto único de decisão de autorização no servidor (C66) e a resolução do papel fora do token (C68), reaproveitados como decisão de projeto por C23, C36 e C101, nas posições 8, 10 e 13. C65 precede C66, porque não é possível negar por padrão sem conhecer as permissões vigentes. |
| 6 | R12 | 10 | Alto | C73 a C78 e C30 | C30, da posição 1, e C65, da posição 5 | Antecipado porque C73 e C74 segregam os papéis administrativos inventariados em C65 e criam o papel de auditoria de que C47 depende, na posição 7. Manter R12 na posição correspondente à sua prioridade deixaria C47 sem o papel a que ele restringe o acesso. |
| 7 | R08 | 3 | Crítico | C42 a C50 e C31 | C31, da posição 1, e C74, da posição 6 | C44, C45 e C46 alteram o formato do dado armazenado, e o critério de custo de reversão os posiciona antes dos controles que apenas leem esses dados. C44 depois da posição 3 evita refazer o fluxo de autenticação já entregue. C43 precede os três, por delimitar onde o dado sensível hoje existe. |
| 8 | R07 | 4 | Crítico | C34 a C41 | C66, da posição 5 | C36 substitui identificadores sequenciais por identificadores não previsíveis, alteração de dados persistidos e de contratos da API cuja reversão é cara, e aplica a verificação de propriedade sobre a função de autorização da posição 5. C34 e C35 precedem C37, porque a redução do conteúdo devolvido depende da tabela de campos por perfil. |
| 9 | R09 | 5 | Crítico | C51 a C59 | — | Conjunto autônomo, de responsabilidade predominante da Infraestrutura, que pode correr em paralelo às posições anteriores. Fecha a parte *Recover* de R13 ao entregar C59, e a borda contratada em C51 e C53 é pré-requisito de C109, na posição 12. C52 precede C53, por identificar o que precisa ser absorvido. |
| 10 | R16 | 7 | Alto | C100 a C104 | C66, da posição 5, e C36, da posição 8 | C100 é levantamento e precede C101, que é a mesma verificação de propriedade de C36 aplicada às operações de gerenciamento. Com a função de autorização e a verificação de propriedade já existentes, a posição consome trabalho menor do que sua prioridade sugere. |
| 11 | R04 | 8 | Alto | C18 a C22 | C30 e C31, da posição 1, para o alerta de C20 | Conjunto independente das posições anteriores. C18 precede C19: o recálculo obrigatório no servidor só pode ser afirmado como completo depois que a relação dos endpoints que aceitam valores do navegador estiver fechada. |
| 12 | R17 | 9 | Alto | C105 a C112 | C53 e C54, da posição 9, e C03, da posição 3 | C107 reaproveita a limitação por origem já implementada em C03 e C54, e C109 depende da borda contratada na posição 9. C105 e C106 precedem os limites, porque a cota é decisão de Produto e o levantamento define a que funções ela se aplica. |
| 13 | R14 | 11 | Alto | C88 a C93 | Fluxo de autenticação da posição 3 | C90 padroniza as respostas do login e da recuperação de senha entregues na posição 3, e C92 reaproveita o bloqueio de origem já disponível na borda. C89 não depende de nenhum controle e é configuração isolada de Infraestrutura, podendo ser antecipado em paralelo; a posição do risco é determinada pelos demais controles. |
| 14 | R10 | 12 | Alto | C60 a C63 e C03 | C03, da posição 3 | Restam apenas os controles próprios do abuso da fila de entregas, já que C03 foi implementado na posição 3 justamente na forma que não indisponibiliza contas alheias, que é a exigência registrada neste risco. |
| 15 | R03 | 15 | Médio | C12 a C17 | — | Nenhum pré-requisito técnico interno, mas depende de integração com base pública externa e de decisão de Produto sobre os documentos exigidos, o que o torna o conjunto de menor acoplamento e maior prazo externo. Pode correr em paralelo desde o início, sem alterar as demais posições. |
| 16 | R05 | 16 | Médio | C23 a C27 | C36, da posição 8, e C66, da posição 5 | C23 é a verificação de propriedade já implementada em C36 e C101, aplicada à atualização do status da entrega. Dentro do risco, C24 precede C25, porque a regra de alerta observa exatamente as transições que a máquina de estados declara como inválidas. Posição próxima à prioridade, com esforço reduzido pelo que já existe. |
| 17 | R15 | 17 | Médio | C94 a C99 | — | Menor prioridade do registro e nenhum controle reaproveitado por outro risco. C94 é o controle determinante e não depende de nenhum outro, de modo que a posição final não cria bloqueio para nenhuma outra. Dentro do risco, C94 e C95 precedem C97, pela mesma razão da posição anterior: o alerta observa a alteração que o congelamento do preço no pedido torna inócua. |

Quatro riscos ocupam posição melhor do que sua prioridade: **R06** e **R02**, porque entregam a base sobre a qual os demais são verificados ou testados, e **R01** e **R12**, porque contêm controles de que posições seguintes dependem. As demais posições apenas acomodam esse deslocamento e preservam a ordem relativa da seção 2.3, o que é verificável pela coluna *Prioridade*: lidas de cima para baixo, ignorando os quatro riscos antecipados, elas permanecem em ordem crescente.

A consequência a registrar é que nenhum risco crítico é adiado por conveniência. R13 e R11, primeiro e segundo em prioridade, ocupam as posições 2 e 5, e as três posições que os antecedem existem porque, sem elas, a implementação dos dois seria refeita ou não poderia ser comprovada.

---

### 3.7. Risco Residual Esperado

O risco residual é o risco que permanece após a implementação dos controles propostos. Esta seção o apresenta como **estimativa**, e não como resultado obtido.

A distinção é necessária porque nenhum controle do plano está implementado. A reavaliação registrada aqui descreve a probabilidade e o impacto que se espera obter **caso** os controles do risco sejam implementados, testados e comprovados pelas evidências indicadas na seção 3.5.2. Enquanto essas evidências não existirem, o risco vigente continua sendo o da seção 2.1, e nenhuma redução pode ser afirmada.

A estimativa observa três regras:

1. A probabilidade e o impacto residuais utilizam a mesma escala de 1 a 4 da seção 1, e a pontuação residual é obtida pela mesma multiplicação.
2. O impacto residual raramente se altera. Os controles do plano atuam predominantemente sobre a probabilidade do evento; quando o impacto é reduzido, isso decorre de um controle que elimina ou limita o ativo atingido, e a linha indica qual controle produz esse efeito.
3. Nenhum risco chega a zero. O residual é o risco remanescente que a plataforma decide manter, e por isso cada linha recebe uma condição de aceitação: o que precisa ser verdadeiro para que o residual seja considerado aceitável e sob qual acompanhamento ele permanece.

A coluna *Nível residual* utiliza as mesmas faixas já aplicadas na seção 2.1, uma vez que a tabela de classificação permanece pendente na seção 1: pontuação de 1 a 3 é Baixo, de 4 a 6 é Médio, de 8 a 9 é Alto e de 12 a 16 é Crítico. São as faixas que reproduzem, sem exceção, os níveis atribuídos no registro inicial. Caso a seção 1 registre limiares diferentes, apenas os rótulos desta coluna mudam, e não as pontuações.

| Risco | Inicial (P × I) | Nível inicial | P residual | I residual | Pontuação residual | Nível residual | Condição de aceitação |
|---|:---:|---|:---:|:---:|:---:|---|---|
| R01 | 3 × 3 = 9 | Alto | 2 | 3 | 6 | Médio | Segundo fator ativo nos perfis definidos por C01 e limitação progressiva comprovada por C03, com o alerta de C06 exercitado. Aceito porque a obtenção da credencial ocorre fora da plataforma e não é eliminável por ela; permanece sob acompanhamento do volume de falhas de autenticação. |
| R02 | 2 × 3 = 6 | Médio | 1 | 3 | 3 | Baixo | Expiração em quinze minutos e recusa do token de renovação já utilizado comprovadas por C09, com vinculação de C10 testada. Aceita-se a janela de até quinze minutos em que um token capturado ainda é válido, sob o alerta de reuso de C11. |
| R03 | 2 × 3 = 6 | Médio | 1 | 3 | 3 | Baixo | Nenhum estabelecimento recebe pedido ou repasse antes da aprovação (C14) e a validação de CNPJ e conta de repasse de C13 é executada em todo cadastro. Aceita-se a fraude praticada com documentação legítima, que o painel de C15 acompanha por indicadores de comportamento. |
| R04 | 3 × 3 = 9 | Alto | 1 | 3 | 3 | Baixo | Recálculo no servidor comprovado por C19 em todos os endpoints relacionados por C18. Aceita-se apenas o intervalo entre a divergência e a rejeição de C21, com o registro de C20 permitindo identificar reincidência. |
| R05 | 2 × 3 = 6 | Médio | 1 | 3 | 3 | Baixo | Restrição ao entregador atribuído (C23) e transições declaradas em máquina de estados (C24) comprovadas por teste. Aceita-se a alteração praticada pelo próprio entregador legítimo do pedido, que permanece rastreável pelo registro de auditoria. |
| R06 | 2 × 3 = 6 | Médio | 1 | 3 | 3 | Baixo | Registro em modo apenas de acréscimo com encadeamento verificado (C30) e conteúdo mínimo presente em amostra (C31), com a verificação diária de C33 sem lacuna. Aceita-se a contestação sobre ação praticada por credencial legítima comprometida, que é tratada por R01 e R02. |
| R07 | 3 × 4 = 12 | Crítico | 2 | 4 | 8 | Alto | Nenhuma resposta da API devolve campo fora da tabela de C34, verificação de propriedade de C36 comprovada e alerta de volume anômalo de C39 exercitado. O impacto não se reduz: a confidencialidade perdida não é restaurável, conforme a observação da seção 3.5.2. Permanece o maior residual do registro, ao lado de R09, e exige revisão semestral do escopo de campos por perfil. |
| R08 | 3 × 4 = 12 | Crítico | 2 | 3 | 6 | Médio | Senhas armazenadas com Argon2id (C44), nenhuma coluna do banco retendo dado de pagamento (C45) e criptografia em repouso e custódia de segredos verificadas (C46). É o caso previsto na regra 2: **C45 reduz o impacto** ao eliminar o dado armazenado, de modo que um acesso indevido ao banco não alcança mais informação de cartão. Aceito sob rotação de chaves na periodicidade de C42. |
| R09 | 3 × 4 = 12 | Crítico | 2 | 4 | 8 | Alto | Absorção do tráfego na borda comprovada em teste de carga (C53), limites de C54 a C56 ativos e tempo de recuperação registrado no teste trimestral de restauração (C59). O impacto permanece 4 porque a indisponibilidade atinge a plataforma inteira; parte do risco é compartilhada com o serviço contratado em C51, o que não a elimina. Aceito enquanto a meta de disponibilidade de C51 for cumprida. |
| R10 | 2 × 4 = 8 | Alto | 1 | 4 | 4 | Médio | Limite de entregas simultâneas e expiração de reserva comprovados por C60, e verificação de que C03 não bloqueia permanentemente a conta legítima. Aceita-se o abuso praticado dentro do limite por conta legítima, observado pelo alerta de acúmulo sem progresso de C61. |
| R11 | 3 × 4 = 12 | Crítico | 1 | 4 | 4 | Médio | Toda rota administrativa coberta por teste de negação por padrão (C66), papel resolvido no servidor e token forjado recusado (C68), e alerta de concessão de privilégio ativo (C70). Aceita-se o uso indevido de privilégio concedido legitimamente, que deixa de ser R11 e passa a ser tratado por R12. |
| R12 | 2 × 4 = 8 | Alto | 1 | 4 | 4 | Médio | Segregação de papéis em vigor (C73), papel de auditoria independente designado (C74), dupla aprovação comprovada em teste (C75) e alerta de tentativa de escrita no repositório de auditoria exercitado (C76). Aceita-se o abuso praticado dentro do papel atribuído, que a auditoria periódica de C74 detecta depois do fato, e não durante. |
| R13 | 4 × 4 = 16 | Crítico | 1 | 4 | 4 | Médio | Inventário de consultas dinâmicas concluído (C80), nova execução da análise estática sem ocorrências (C81) e regra do pipeline bloqueando a integração de código que concatene entrada (C79). Enquanto o inventário não estiver fechado, o residual vale apenas para os pontos já convertidos, conforme registrado na [Etapa 4](./etapa-4-codigo-seguro-e-testes-seguranca.md#15-resultado-esperado-da-prática). O impacto permanece 4 porque uma ocorrência remanescente ainda alcança o banco, limitada pelo privilégio mínimo de C83. |
| R14 | 2 × 4 = 8 | Alto | 1 | 4 | 4 | Médio | Verificação externa de TLS sem apontamento (C89) e respostas de login e recuperação indistinguíveis entre conta existente e inexistente (C90). Aceito porque a captura ocorre fora do alcance da plataforma: o que ela observa é a enumeração que costuma precedê-la, pelo alerta de C91. |
| R15 | 2 × 3 = 6 | Médio | 1 | 3 | 3 | Baixo | Preço gravado no pedido no momento da confirmação (C94), cardápio versionado com autoria e valor anterior (C95) e avaliação vinculada a pedido entregue (C96). Aceita-se a alteração de preço para pedidos futuros, que é operação legítima do estabelecimento. |
| R16 | 3 × 3 = 9 | Alto | 2 | 3 | 6 | Médio | Matriz de perfil por escopo publicada e comparada às rotas expostas (C100) e verificação de propriedade nas operações de gerenciamento comprovada (C101). A probabilidade não chega a 1 porque C102 depende de verificação de identidade conduzida pelo Suporte, procedimento manual e sujeito a falha; permanece sob o alerta de acesso fora do escopo de C103. |
| R17 | 3 × 3 = 9 | Alto | 2 | 2 | 4 | Médio | Cotas definidas por perfil (C105), limites de envio e de upload comprovados por teste (C107 e C108) e imagens servidas pelo domínio da CDN (C109). É o segundo caso previsto na regra 2: **C109 reduz o impacto** ao deslocar o consumo do servidor da aplicação para o serviço contratado. Aceito sob acompanhamento mensal do consumo previsto em C105. |

A distribuição esperada, caso o plano seja integralmente implementado, é de seis riscos em nível Baixo, nove em Médio e dois em Alto, sem nenhum risco Crítico remanescente. Os dois que permanecem em Alto são **R07** e **R09**, e a razão é a mesma nos dois casos: o impacto não se reduz. A exposição de dados pessoais é irreversível depois de ocorrida, e a indisponibilidade atinge a plataforma inteira independentemente da causa. Nos dois, o plano atua sobre a probabilidade, e é isso que a estimativa registra.

O impacto é reduzido em apenas dois riscos, **R08** e **R17**, e em ambos por um controle que elimina ou desloca o ativo atingido — C45, que remove o dado de pagamento do banco, e C109, que transfere o armazenamento das imagens para serviço externo. Nos quinze restantes, o impacto residual é igual ao inicial, o que é coerente com a regra 2 e com a natureza dos controles propostos.

Nenhuma linha desta seção é evidência de redução obtida. Todas as condições de aceitação são redigidas como verificações a executar, e cada uma remete à evidência já definida na seção 3.5.2 para o controle correspondente. Enquanto essas evidências não existirem, o risco vigente é o da seção 2.1, e a reavaliação registrada aqui permanece sendo o que a seção declara desde a abertura: uma estimativa.

---

## 4. Considerações Finais

A etapa parte das 36 ameaças e dos casos de abuso da Etapa 1 e chega a 112 controles com responsável e evidência definidos. Entre um ponto e outro, quatro decisões determinam o resultado e merecem registro.

**A cobertura foi verificada, e não presumida.** A revisão da seção 2.1 constatou que dez ameaças da Etapa 1 não haviam originado risco algum, entre elas a injeção de comandos SQL. Os cinco riscos acrescentados a partir dessa verificação incluem R13, que veio a ser o de maior pontuação do registro e o único tratado com a estratégia *Evitar*. Se a cobertura não tivesse sido conferida, o risco mais grave da plataforma teria ficado de fora do plano.

**As funções do NIST descrevem resultados, e não medidas.** A matriz da seção 3.4 marca apenas as funções que o tratamento de cada risco efetivamente alcança, e a seção 3.5.3 confirma que toda função marcada tem controle correspondente e que nenhum controle invoca função não marcada. As ausências são justificadas caso a caso: R06 não tem *Respond* porque um registro que não foi gerado não pode ser reconstituído, e R07 tem *Recover* limitado porque confidencialidade perdida não se restaura. Marcar as seis funções em todos os riscos produziria uma matriz completa e falsa.

**Prioridade e ordem de execução respondem a perguntas diferentes.** A seção 2.3 ordena por gravidade; a seção 3.6, por sequência de implementação. As duas não coincidem, e a divergência é o próprio resultado: o registro de auditoria, décimo terceiro em prioridade, ocupa a primeira posição porque é o que torna verificável todo o restante, e a sessão, décima quarta, é antecipada porque alterá-la depois obrigaria a refazer a autorização. Nenhum risco crítico foi adiado por conveniência — os dois primeiros em prioridade ocupam as posições 2 e 5, e o que os antecede existe para que a implementação deles não seja refeita.

**Nada nesta etapa é evidência de risco reduzido.** Nenhum controle está implementado. A seção 3.7 é estimativa declarada, cada linha traz a condição que precisaria ser verdadeira para que o residual fosse aceitável, e o risco vigente continua sendo o da seção 2.1. A estimativa aponta para dois riscos permanecendo em nível Alto mesmo com o plano inteiro executado — R07 e R09 —, porque neles o impacto não se reduz, apenas a probabilidade.

O que a etapa entrega às seguintes é a cadeia rastreável entre ameaça, risco, estratégia, função e controle. A [Etapa 3](./etapa-3-arquitetura-segura.md) posiciona os controles em componentes e zonas de confiança, a [Etapa 4](./etapa-4-codigo-seguro-e-testes-seguranca.md) implementa e testa dois deles em código, e a [Etapa 6](../roteiros/etapa-6-deteccao-de-intrusoes.md) organiza os controles de função *Detect* em eventos e regras. Em todas elas, o identificador do controle é o que mantém a ligação com este documento.

---

## 5. Distribuição Integrante X Responsabilidades

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | Definir critérios de avaliação de riscos e registrar riscos iniciais. |
| Emanuel Ferreira | Definir ordem de priorização e estimar o risco residual esperado. |
| Guilherme Mundt | Relacionar os riscos às funções do NIST CSF 2.0. |
| Lívia Barbosa | Priorizar os riscos e definir estratégias de tratamento. |
| Mariana Padilha | Avaliar e justificar os riscos inicialmente mapeados. |
| Matheus Ciocca | Definir plano de tratamento de riscos e controles concretos. |
