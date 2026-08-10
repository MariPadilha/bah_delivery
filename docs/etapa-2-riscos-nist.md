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

Nesta etapa inicial, os campos de probabilidade, impacto, pontuação e nível permanecem pendentes de avaliação.

| ID | Origem STRIDE | Evento de risco | Probabilidade | Impacto | Pontuação | Nível |
|---|---|---|---:|---:|---:|---|
| R01 | S01, S05 | Comprometimento da conta de um cliente por obtenção indevida de suas credenciais | — | — | — | — |
| R02 | S02 | Utilização indevida de uma sessão legítima por meio do comprometimento de seu token | — | — | — | — |
| R03 | S03 | Cadastro de restaurante inexistente para obtenção fraudulenta de pagamentos e dados | — | — | — | — |
| R04 | T01 | Manipulação do valor de um pedido antes da realização do pagamento | — | — | — | — |
| R05 | T02, T04 | Alteração indevida de informações relacionadas ao processo de entrega | — | — | — | — |
| R06 | RP01, RP02, RP03, RP04 | Impossibilidade de atribuir responsabilidade a ações realizadas no sistema | — | — | — | — |
| R07 | I01, I02, I03 | Exposição indevida de dados pertencentes a clientes | — | — | — | — |
| R08 | I05, I07 | Comprometimento de credenciais e informações sensíveis armazenadas pela plataforma | — | — | — | — |
| R09 | D01, D02, D03 | Indisponibilidade da plataforma de delivery | — | — | — | — |
| R10 | D04, D06 | Interrupção ou sabotagem de operações relacionadas aos restaurantes e entregas | — | — | — | — |
| R11 | E01, E02, E03 | Obtenção indevida de privilégios administrativos | — | — | — | — |
| R12 | E05, E06 | Abuso de privilégios administrativos com possibilidade de ocultação das ações realizadas | — | — | — | — |
| R13 | T05, I06 | Comprometimento da integridade do banco de dados por injeção de comandos nas consultas da aplicação | — | — | — | — |
| R14 | I04, I08 | Captura de credenciais e de dados em trânsito, precedida da identificação das contas existentes na plataforma | — | — | — | — |
| R15 | T03, T06 | Distorção de preços e de avaliações após a confirmação do pedido pelo cliente | — | — | — | — |
| R16 | S04, E04 | Acesso a dados de clientes e de estabelecimentos por identidade não verificada ou por perfil sem delimitação de escopo | — | — | — | — |
| R17 | D05, D07 | Esgotamento de recursos da plataforma pelo uso abusivo das funções de envio de mensagens e de upload de imagens | — | — | — | — |

Os riscos **R13** a **R17** foram acrescentados após a revisão da cobertura do registro. A verificação constatou que dez ameaças da Etapa 1 (S04, T03, T05, T06, I04, I06, I08, D05, D07 e E04) ainda não haviam originado nenhum risco, entre elas a injeção de comandos SQL (T05) e o tráfego sem proteção adequada (I04). Com esses cinco riscos, as 36 ameaças identificadas na Etapa 1 passam a estar integralmente cobertas.

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
