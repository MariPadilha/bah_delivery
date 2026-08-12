# Etapa 3 — Arquitetura Segura

Esta etapa visa transformar os riscos e controles definidos na [Etapa 2 — Avaliação e Tratamento de Riscos](./etapa-2-riscos-nist.md) em requisitos de segurança e decisões de arquitetura.

---

## Sumário

1. [Requisitos de Segurança](#1-requisitos-de-segurança)
2. [Distribuição Integrante X Responsabilidades](#2-distribuição-integrante-x-responsabilidades)

---

## 1. Requisitos de Segurança

| ID | Ameaça de Origem | Risco de origem | Requisito de segurança | Critério de verificação
|---|---|---|---|---
| [RS01](#rs01--proteção-contra-tentativas-automatizadas-de-autenticação) | S01 | R01 | O Bah Delivery deve limitar tentativas repetidas de autenticação e aplicar mecanismos adicionais de verificação de identidade quando forem detectadas tentativas suspeitas | O requisito será atendido quando tentativas excessivas de autenticação forem limitadas e acessos considerados suspeitos exigirem verificação adicional antes de serem autorizados
| [RS02](#rs02--proteção-contra-injeção-de-comandos-sql) | T05 | R13 | Utilizar consultas parametrizadas e validar as entradas dos usuários conforme o formato esperado, impedindo que dados fornecidos alterem a estrutura das consultas SQL | O requisito será atendido quando entradas maliciosas não alterarem as consultas SQL e entradas inválidas forem rejeitadas antes do processamento
| [RS03](#rs03--controle-de-autorização-decidido-no-servidor) | E01, E02, E03 | R11 | Verificar a autorização no servidor em toda rota administrativa, negando o acesso por padrão, e resolver o perfil do usuário no próprio servidor, sem aceitá-lo como campo da requisição nem confiar em token com assinatura não verificada | O requisito será atendido quando rotas administrativas invocadas por conta sem privilégio forem recusadas, o campo de perfil enviado na requisição for ignorado e tokens com assinatura inválida ou algoritmo alterado forem rejeitados

---

### RS01 — Proteção contra tentativas automatizadas de autenticação
**Risco relacionado:** R01 — Comprometimento da conta de um cliente por obtenção indevida de suas credenciais.

**Justificativa:** A ameaça S01 considera que um atacante pode utilizar credenciais obtidas
em vazamentos de outros serviços para realizar tentativas de autenticação
em massa contra contas de clientes (*credential stuffing*). Sem mecanismos que dificultem tentativas automatizadas de autenticação, credenciais reutilizadas podem resultar no comprometimento de contas legítimas. Isso pode permitir acesso ao histórico de pedidos, endereços residenciais e outras informações associadas ao cliente, além da realização de pedidos fraudulentos em nome da vítima.

### RS02 — Proteção contra injeção de comandos SQL
**Risco relacionado:** R13 — Comprometimento da integridade do banco de dados por injeção de comandos nas consultas da aplicação.

**Justificativa:** A ameaça T05 considera que campos de busca de restaurantes e produtos podem receber entradas controladas pelo usuário. Caso esses valores sejam concatenados diretamente às consultas SQL, um atacante poderá inserir elementos capazes de modificar a consulta originalmente definida pela aplicação. A exploração dessa vulnerabilidade pode permitir leitura, alteração ou exclusão indevida de informações armazenadas no banco de dados, comprometendo pedidos, cardápios, avaliações e outros dados utilizados pelo Bah Delivery. Como o banco de dados constitui um ativo crítico da plataforma, o risco R13 foi classificado como crítico e deve ser tratado por meio da separação entre os comandos SQL definidos pela aplicação e os valores fornecidos externamente.

### RS03 — Controle de autorização decidido no servidor
**Risco relacionado:** R11 — Obtenção indevida de privilégios administrativos.

**Justificativa:** As ameaças E01, E02 e E03 compartilham a mesma condição: a autorização é decidida fora do servidor. Em E01, a restrição existe apenas na interface, e os endpoints de gerenciamento de usuários e de restaurantes não verificam o papel do usuário autenticado. Em E02, o perfil é recebido como campo da requisição e gravado sem validação, permitindo que o atacante se declare administrador. Em E03, o token de sessão carrega o perfil e é aceito sem verificação adequada da assinatura, o que possibilita forjar um token administrativo. Por isso R11 foi classificado como crítico e ocupa a segunda prioridade do registro, já que habilita outros riscos críticos, como R07 e R12. O tratamento exige que o servidor seja a única autoridade sobre a autorização.

---

### 1.2. Vulnerabilidade Catalogada Correspondente

| Risco | Vulnerabilidade ou categoria | Referência utilizada | Relação com o sistema
|---|---|---|---|
| R01 | Improper Restriction of Excessive Authentication Attempts | CWE-307 | Essa fraqueza está relacionada ao cenário identificado em S01, pois a ausência de limitação permite que um atacante realize um grande número de tentativas de login utilizando credenciais obtidas anteriormente
| R13 | Neutralização inadequada de elementos especiais usados ​​em um comando SQL ('Injeção de SQL') | CWE-89 | Essa fraqueza corresponde diretamente ao cenário identificado em T05 e R13, pois uma entrada fornecida nos campos de busca poderia alterar a consulta caso fosse concatenada diretamente ao comando SQL
| R11 | Missing Authorization da categoria Broken Access Control | CWE-862 | Essa fraqueza corresponde ao cenário identificado em E01 e R11, pois as rotas administrativas executam a operação sem verificar o papel do usuário autenticado, confiando na ocultação feita pela interface. As demais variantes do requisito têm fraquezas próprias no catálogo: CWE-915 para o perfil gravado a partir da requisição (E02) e CWE-347 para o token aceito sem verificação da assinatura (E03)

---

## 2. Distribuição Integrante X Responsabilidades

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | Definir requisito de segurança e sua respectiva vulnerabilidade catalogada. |
| Emanuel Ferreira | Definir decisões de arquitetura. |
| Guilherme Mundt | Modelar diagrama de arquitetura segura. |
| Lívia Barbosa | Definir requisito de segurança e sua respectiva vulnerabilidade catalogada. |
| Mariana Padilha | Definir requisito de segurança e sua respectiva vulnerabilidade catalogada. |
| Matheus Ciocca | Definir decisões de arquitetura. |