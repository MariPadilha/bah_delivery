# Etapa 2 — Avaliação e Tratamento de Riscos

Esta etapa dá continuidade à análise iniciada na [Etapa 1 — Casos de Abuso e Modelagem de Ameaças com STRIDE](./etapa-1-ameacas-stride.md), transformando as ameaças e os casos de abuso já identificados em riscos que possam ser avaliados, comparados, priorizados e tratados.

O sistema, os ativos, os usuários, as ameaças STRIDE e os casos de abuso são os mesmos da etapa anterior.

---

## Sumário

1. [Metodologia de Avaliação de Riscos](#1-metodologia-de-avaliação-de-riscos)
2. [Registro Inicial de Riscos](#2-registro-inicial-de-riscos)
3. [Rastreabilidade entre STRIDE e Riscos](#3-rastreabilidade-entre-stride-e-riscos)
4. [Próximas Etapas da Avaliação](#4-próximas-etapas-da-avaliação)
5. [Organização do Projeto](#5-organização-do-projeto)

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

## 3. Rastreabilidade entre STRIDE e Riscos

A rastreabilidade permite relacionar os riscos da Etapa 2 às ameaças identificadas anteriormente na Etapa 1.

| Risco | Ameaças STRIDE relacionadas | Relação |
|---|---|---|
| R01 | S01, S05 | As ameaças de obtenção de credenciais podem resultar no comprometimento da conta de um cliente. |
| R02 | S02 | O comprometimento de uma sessão pode permitir que um atacante utilize a API como o usuário legítimo. |
| R03 | S03 | A falsificação da identidade de um restaurante pode permitir o cadastro de um estabelecimento inexistente. |
| R04 | T01 | A confiança em valores calculados pelo cliente pode permitir a manipulação do valor do pedido. |
| R05 | T02, T04 | Alterações indevidas em informações relacionadas à entrega podem gerar fraude operacional. |
| R06 | RP01, RP02, RP03, RP04 | A ausência de registros íntegros pode dificultar ou impedir a atribuição de responsabilidade pelas ações realizadas. |
| R07 | I01, I02, I03 | Falhas de proteção e verificação de propriedade podem resultar na exposição indevida de dados de clientes. |
| R08 | I05, I07 | O armazenamento ou tratamento inadequado de informações sensíveis pode resultar em comprometimento de credenciais e dados. |
| R09 | D01, D02, D03 | As ameaças de indisponibilidade podem impedir o funcionamento normal da plataforma. |
| R10 | D04, D06 | A exploração das ameaças de disponibilidade pode interromper operações de restaurantes e entregas. |
| R11 | E01, E02, E03 | Falhas na autorização podem permitir a obtenção indevida de privilégios administrativos. |
| R12 | E05, E06 | O abuso de privilégios administrativos e a ausência de controles adequados podem permitir ações privilegiadas sem rastreabilidade suficiente. |
| R13 | T05, I06 | A concatenação direta da entrada do usuário nas consultas permite a injeção de comandos, e as mensagens de erro detalhadas fornecem ao atacante a estrutura interna necessária para explorá-la. |
| R14 | I04, I08 | A distinção entre "e-mail não cadastrado" e "senha incorreta" permite construir a lista de contas válidas, e o tráfego sem proteção adequada permite capturar as credenciais correspondentes. |
| R15 | T03, T06 | O pedido referencia o cardápio em vez de registrar o preço praticado, e as avaliações não verificam a realização do pedido, permitindo distorcer valores e reputação após a compra. |
| R16 | S04, E04 | O compartilhamento de contas de entregador e a verificação de perfil sem confirmação de escopo permitem o acesso a dados de clientes e de estabelecimentos concorrentes. |
| R17 | D05, D07 | A ausência de limites no envio de mensagens e no upload de imagens permite consumir cota, armazenamento e banda até a degradação do serviço. |

---

## 4. Próximas Etapas da Avaliação

O registro apresentado nesta seção será utilizado como base para as próximas atividades da Etapa 2.

A avaliação deverá complementar cada risco com:

1. probabilidade de ocorrência;
2. impacto;
3. justificativa para os valores atribuídos;
4. pontuação;
5. nível de risco;
6. prioridade de tratamento;
7. estratégia de tratamento;
8. controles de segurança;
9. mapeamento para o NIST CSF 2.0;
10. estimativa de risco residual.

Além dos itens acima, permanecem pendentes de inclusão neste documento:

- a tabela de classificação do nível do risco a partir da pontuação (Baixo, Médio, Alto e Crítico);
- a coluna "Vulnerabilidade ou condição" no registro de riscos.

---

## 5. Organização do Projeto

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | |
| Emanuel Ferreira | |
| Guilherme Mundt | |
| Lívia Barbosa | |
| Mariana Padilha | |
| Matheus Ciocca | |
