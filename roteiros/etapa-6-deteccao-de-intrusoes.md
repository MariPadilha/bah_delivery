# Etapa 6 - Monitoramento e Detecção de Intrusões

Este roteiro descreve como o Bah Delivery perceberia comportamentos suspeitos depois de entrar em operação. Ele não propõe a instalação de um sistema de detecção: parte dos controles de função *Detect* já definidos no [plano de tratamento da Etapa 2](../docs/etapa-2-riscos-nist.md#352-planos-por-risco) e os organiza em eventos, regras e resposta.

---

## Sumário

1. [O que é detecção de intrusões](#1-o-que-é-detecção-de-intrusões)
2. [A diferença entre prevenir e detectar](#2-a-diferença-entre-prevenir-e-detectar)
3. [Eventos que devem ser registrados](#3-eventos-que-devem-ser-registrados)
4. [Regras de detecção](#4-regras-de-detecção)
5. [O que acontece depois de um alerta](#5-o-que-acontece-depois-de-um-alerta)
6. [Distribuição Integrante X Responsabilidades](#6-distribuição-integrante-x-responsabilidades)

---

## 1. O que é detecção de intrusões

> **Em preenchimento.** Integrante 2.

---

## 2. A diferença entre prevenir e detectar

> **Em preenchimento.** Integrante 2.

---

## 3. Eventos que devem ser registrados

Um evento só é útil à detecção se for possível responder três perguntas a partir dele: quem agiu, sobre o que agiu e se aquilo destoa do comportamento habitual. Por isso a escolha dos eventos não é feita pela facilidade de registrá-los, e sim pelas regras que eles precisam alimentar e pelos riscos que o registro precisa tornar visíveis.

### 3.1. Conteúdo mínimo de cada evento

Todo evento registrado segue o conteúdo mínimo definido em **C31**: autor, papel, data e hora, origem da requisição, recurso afetado e valores anterior e posterior, com mascaramento dos campos sensíveis. Sem esses campos, o registro descreve que algo aconteceu, mas não permite decidir se foi indevido, e nenhuma regra da seção 4 poderia ser escrita sobre ele.

Três condições acompanham o conteúdo e vêm do mesmo plano:

- **Onde o registro fica.** Armazenamento segregado do banco da aplicação, em modo apenas de acréscimo e com encadeamento por resumo criptográfico (**C30**), na zona de auditoria do [diagrama da Etapa 3](../docs/etapa-3-arquitetura-segura.md#2-diagrama-da-arquitetura-segura). Registro que o administrador pode apagar não sustenta detecção de abuso de privilégio.
- **Quem lê.** Apenas o papel de auditoria (**C47**), e não todo administrador.
- **Por quanto tempo.** Doze meses, conforme a política de auditoria (**C28**), que também define a relação de eventos de registro obrigatório.

### 3.2. O que não deve ser registrado

O registro não pode se tornar uma segunda cópia dos dados que a plataforma protege. Ficam de fora a senha, o token de sessão, os dados de pagamento e o corpo integral das requisições, com mascaramento aplicado aos campos sensíveis (**C31**, no tratamento de R08). Um registro completo demais desloca o risco em vez de reduzi-lo: passa a ser o alvo mais barato, e sob acesso mais amplo do que o do próprio banco.

### 3.3. Relação dos eventos

| # | Evento | Onde se origina | Risco relacionado | Por que é registrado |
|---|---|---|---|---|
| E1 | Falha de autenticação, com identificação da conta e da origem | Serviço de autenticação | R01 | É a matéria-prima da percepção de tentativas automatizadas, e só faz sentido em série: uma falha isolada é ruído, e a repetição é o sinal |
| E2 | Autenticação bem-sucedida a partir de dispositivo ou origem não reconhecidos | Serviço de autenticação | R01 | Distingue o acesso legítimo do acesso com credencial válida obtida indevidamente, que é o desfecho que a contagem de falhas não enxerga |
| E3 | Exigência, sucesso e recusa do segundo fator | Serviço de autenticação | R01 | Permite verificar se a decisão de DA01 está sendo aplicada nos perfis em que o segundo fator é obrigatório |
| E4 | Emissão, renovação, rotação e revogação de token de sessão, com dispositivo e origem | Serviço de autenticação | R02 | Sem esses eventos não é possível observar o mesmo token em duas origens na mesma janela, nem a reapresentação de token já rotacionado |
| E5 | Recusa de autorização, com a rota, o perfil resolvido no servidor e o recurso solicitado | Regras de autorização | R11, R16 | Uma recusa é o comportamento correto, mas a sequência de recusas contra rotas administrativas é tentativa de elevação de privilégio |
| E6 | Concessão ou alteração de papel administrativo, e o primeiro acesso a rota administrativa por conta recém-elevada | Regras de autorização e gestão de contas | R11 | É o par de eventos exigido por C70: a elevação legítima é rara e conhecida, então toda ocorrência merece verificação |
| E7 | Operação administrativa sensível: exclusão de registro, alteração de dados de repasse e acesso a dados de pagamento, com as aprovações envolvidas | API REST | R12 | Sustenta a dupla aprovação de C75 e permite reconstituir o que foi feito quando a apuração começa |
| E8 | Leitura do repositório de auditoria e tentativa de escrita ou exclusão nele | Zona de auditoria | R12 | Tentar apagar o registro é, por si só, o evento mais significativo do conjunto, e é o que C76 observa |
| E9 | Entrada recusada pela validação, com o motivo e a origem, e erro de execução de consulta | Validação e acesso a dados | R13, I06 | São os eventos `busca_recusada` e `falha_na_busca` já emitidos pelo código da [Etapa 4](../docs/etapa-4-codigo-seguro-e-testes-seguranca.md#142-consulta-parametrizada), e o erro de sintaxe é o rastro típico de uma tentativa de injeção em curso |
| E10 | Transição de estado do pedido ou da entrega fora da sequência esperada, e divergência entre o valor recebido e o recalculado | API REST | R04, R05 | Fraude operacional não produz erro técnico: ela aparece como sequência improvável, e não como falha |
| E11 | Acesso a registros pertencentes a outros titulares, contabilizado por conta | API REST | R07, R16 | Cada acesso pode ser legítimo; o volume em série é o que caracteriza a coleta indevida observada por C39 |
| E12 | Bloqueio ou limitação aplicada na borda, e saturação de recurso da plataforma | CDN com WAF e infraestrutura | R09, R17 | Separa a indisponibilidade causada por demanda legítima da causada por abuso, distinção que a resposta ao incidente exige |
| E13 | Falha na geração de registro e lacuna na cadeia de resumos | Zona de auditoria | R06 | É o evento que vigia o próprio mecanismo: a ausência de registro precisa gerar alerta, porque um registro que não foi gerado não pode ser reconstituído depois |

E13 tem natureza distinta dos demais e é a razão de C33 existir. Todas as regras da seção 4 pressupõem que os eventos cheguem ao repositório; se essa suposição falhar em silêncio, a plataforma passa a parecer segura exatamente quando deixou de observar a si mesma.

### 3.4. Cobertura das regras pelos eventos

Os treze eventos foram escolhidos de modo que as três regras da seção seguinte se apoiem em dados já previstos no plano de tratamento, sem exigir instrumentação nova. Os eventos que não alimentam nenhuma das três permanecem na relação porque são exigidos pela política de auditoria (C28) e sustentam a rastreabilidade de R06 e R12, que existe independentemente de haver alerta associado.

---

## 4. Regras de detecção

> **Em preenchimento.** Regra 1, integrante 1. Regra 2, integrante 3. Regra 3, integrante 4. Cada regra indica risco observado, fonte de dados, condição de alerta e resposta inicial, conforme o formato do enunciado.

| Risco observado | Fonte de dados | Condição de alerta | Resposta inicial |
|---|---|---|---|
| | | | |
| | | | |
| | | | |

---

## 5. O que acontece depois de um alerta

> **Em preenchimento.** Integrante 6.

---

## 6. Distribuição Integrante X Responsabilidades

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | Definir a regra de detecção 1. |
| Emanuel Ferreira | Descrever o que acontece depois de um alerta. |
| Guilherme Mundt | Definir a regra de detecção 3. |
| Lívia Barbosa | Definir a regra de detecção 2. |
| Mariana Padilha | Apresentar o conceito de detecção de intrusões e a diferença entre prevenir e detectar. |
| Matheus Ciocca | Definir os eventos do sistema que devem ser registrados. |
