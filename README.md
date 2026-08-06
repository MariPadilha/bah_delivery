# Bah Delivery

O Bah Delivery é uma plataforma web voltada para o gerenciamento integrado de pedidos e entregas de refeições, conectando diferentes perfis de usuário em um único ambiente digital. O sistema permite que clientes consultem cardápios, realizem pedidos e acompanhem o status das entregas, enquanto restaurantes podem administrar seus produtos, receber e organizar pedidos. Os entregadores possuem acesso às solicitações de entrega e informações necessárias para a execução do serviço, e os administradores são responsáveis pelo gerenciamento e controle geral da plataforma.

---

## Sumário

1. [Identificação do Sistema](#1-identificação-do-sistema)
2. [Descrição do Sistema](#2-descrição-do-sistema)
3. [Usuários, Ativos e Pontos de Interação](#3-usuários-ativos-e-pontos-de-interação)
4. [Visão Geral da Arquitetura](#4-visão-geral-da-arquitetura)
5. [Modelagem de Ameaças (STRIDE)](#5-modelagem-de-ameaças-stride)
6. [Casos de Abuso](#6-casos-de-abuso)
7. [Considerações Finais](#7-considerações-finais)
8. [Organização do Projeto](#8-organização-do-projeto)

---

## 1. Identificação do Sistema

| Item | Descrição |
|------|-----------|
| **Nome do sistema** | Bah Delivery |
| **Disciplina** | Engenharia de Software Seguro |
| **Integrantes** | Arthur Medeiros, Emanuel Ferreira, Guilherme Mundt, Lívia Barbosa, Mariana Padilha e Matheus Ciocca |
| **Repositório** | https://github.com/MariPadilha/bah_delivery |

### Justificativa da Escolha do Sistema

O sistema de delivery foi escolhido por representar uma aplicação real e amplamente utilizada, que envolve múltiplos usuários, processamento de pagamentos e armazenamento de dados sensíveis. Essas características possibilitam explorar diversos aspectos relacionados à segurança da informação e à modelagem de ameaças, atendendo aos objetivos da disciplina.

---

## 2. Descrição do Sistema

### Problema que o Sistema Resolve

O processo tradicional de pedidos de refeições frequentemente envolve comunicação fragmentada entre clientes, restaurantes e entregadores, podendo causar atrasos, erros nas solicitações e dificuldades no acompanhamento das entregas. O Bah Delivery busca solucionar esses problemas ao automatizar e organizar o fluxo de pedidos, centralizando as informações em uma única plataforma, reduzindo falhas de comunicação e proporcionando maior agilidade e transparência durante todo o processo de compra e entrega.

### Perfis de Usuário

#### Cliente

Usuário responsável pela realização de pedidos de refeições através da plataforma.

**Principais funcionalidades:**

- Cadastro e autenticação no sistema;
- Gerenciamento dos dados pessoais;
- Consulta de restaurantes disponíveis;
- Visualização de cardápios e produtos;
- Adição de produtos ao carrinho;
- Realização de pedidos;
- Seleção da forma de pagamento;
- Processamento e confirmação do pagamento;
- Acompanhamento do status dos pedidos;
- Consulta do histórico de pedidos;
- Avaliação de restaurantes e entregas.

#### Restaurante

Usuário responsável pelo gerenciamento do estabelecimento, produtos e atendimento dos pedidos recebidos.

**Principais funcionalidades:**

- Cadastro e gerenciamento do restaurante;
- Gerenciamento do cardápio;
- Cadastro, edição e remoção de produtos;
- Visualização dos pedidos recebidos;
- Atualização do status dos pedidos;
- Consulta do histórico de pedidos recebidos.

#### Entregador

Usuário responsável pela retirada e entrega dos pedidos aos clientes.

**Principais funcionalidades:**

- Cadastro e autenticação no sistema;
- Visualização de entregas disponíveis;
- Aceitação de solicitações de entrega;
- Consulta das informações necessárias para entrega;
- Atualização do status da entrega;
- Consulta do histórico de entregas realizadas.

#### Administrador

Usuário responsável pelo gerenciamento e controle geral da plataforma.

**Principais funcionalidades:**

- Gerenciamento de usuários;
- Controle de permissões de acesso;
- Gerenciamento de restaurantes cadastrados;
- Monitoramento das atividades do sistema;
- Consulta de logs e registros da aplicação;
- Administração geral da plataforma.

### Principais Funcionalidades do Sistema

- Cadastro e autenticação de usuários;
- Controle de acesso baseado em perfis;
- Gerenciamento de usuários e permissões;
- Cadastro e gerenciamento de restaurantes;
- Gerenciamento de cardápios e produtos;
- Carrinho de compras;
- Realização e gerenciamento de pedidos;
- Processamento de pagamentos;
- Registro e confirmação de transações;
- Acompanhamento do fluxo de entrega;
- Atualização de status dos pedidos;
- Histórico de pedidos e entregas;
- Avaliação de restaurantes;
- Armazenamento e consulta de informações do sistema;
- Registro de logs para auditoria e segurança.

### Informações Armazenadas ou Transmitidas

O sistema manipula diferentes tipos de informações durante sua operação, incluindo:

- Dados pessoais dos usuários;
- Endereços de entrega;
- Credenciais de autenticação;
- Dados dos restaurantes;
- Cardápios;
- Pedidos;
- Histórico de pedidos;
- Avaliações;
- Informações sobre pagamentos;
- Logs do sistema.

### Recursos que Precisam ser Protegidos

Os principais recursos que necessitam de proteção no **Bah Delivery** são:

- Dados pessoais dos clientes, restaurantes, entregadores e administradores;
- Credenciais de autenticação (login, senha e sessões de usuários);
- Banco de dados da aplicação;
- Informações de pagamento e transações financeiras;
- Histórico de pedidos e entregas;
- Cardápios e produtos cadastrados pelos restaurantes;
- Dados cadastrais dos restaurantes;
- Informações dos entregadores;
- APIs responsáveis pela comunicação entre cliente e servidor;
- Sistema de autenticação e controle de permissões;
- Servidores da aplicação;
- Logs de auditoria e monitoramento.

A proteção desses recursos é essencial para garantir a confidencialidade, integridade e disponibilidade das informações processadas pelo sistema, assegurando que apenas usuários autorizados tenham acesso aos recursos da plataforma.

---

## 3. Usuários, Ativos e Pontos de Interação

### Usuários

- Cliente
- Restaurante
- Entregador
- Administrador

### Ativos

| Ativo | Descrição | Criticidade |
|--------|-----------|-------------|
| Dados pessoais dos clientes | Nome, CPF, e-mail, telefone e endereço de entrega. | Alta |
| Credenciais de autenticação | Login, senha criptografada e sessões dos usuários. | Crítica |
| Banco de dados | Armazena todas as informações do sistema. | Crítica |
| Pedidos | Informações referentes aos pedidos realizados pelos clientes. | Alta |
| Informações de pagamento | Dados utilizados durante o processamento dos pagamentos. | Crítica |
| Histórico de pedidos | Registros de pedidos realizados e concluídos. | Alta |
| Cardápios | Produtos, preços e categorias cadastrados pelos restaurantes. | Média |
| Dados dos restaurantes | Informações cadastrais e operacionais dos estabelecimentos. | Alta |
| Dados dos entregadores | Informações pessoais e histórico das entregas realizadas. | Alta |
| Logs do sistema | Registro de autenticações, operações e eventos do sistema. | Alta |
| API REST | Comunicação entre frontend, backend e demais serviços. | Alta |
| Servidor da aplicação | Infraestrutura responsável pela execução do sistema. | Crítica |
| Controle de permissões | Responsável pela autorização de acesso conforme o perfil do usuário. | Crítica |

### Pontos de Interação

| Usuário | Pontos de interação |
|----------|---------------------|
| Cliente | Cadastro, login, consulta de restaurantes, visualização do cardápio, carrinho de compras, realização de pedidos, pagamento, acompanhamento da entrega, histórico de pedidos e avaliações. |
| Restaurante | Login, gerenciamento do restaurante, gerenciamento do cardápio, cadastro de produtos, visualização dos pedidos, atualização do status e consulta do histórico. |
| Entregador | Login, visualização das entregas disponíveis, aceitação de pedidos, consulta dos dados da entrega, atualização do status e histórico de entregas. |
| Administrador | Gerenciamento de usuários, controle de permissões, gerenciamento de restaurantes, monitoramento do sistema, consulta de logs e administração geral da plataforma. |

### Justificativa da Classificação

A identificação dos ativos permite reconhecer quais recursos possuem maior valor para o funcionamento da plataforma e quais necessitam de maior nível de proteção. Ativos classificados como **Críticos** podem comprometer diretamente a segurança do sistema caso sejam violados, enquanto ativos classificados como **Alta** ou **Média** representam recursos importantes para a continuidade das operações e para a proteção dos dados dos usuários.

Esses ativos servirão como base para a etapa de Modelagem de Ameaças (STRIDE), permitindo identificar possíveis vulnerabilidades relacionadas à autenticação, integridade dos dados, disponibilidade do sistema, confidencialidade das informações e controle de acesso.

---

## 4. Visão Geral da Arquitetura

Esta seção apresenta os diagramas desenvolvidos a fim de oferecer uma visão simplificada de como os usuários e componentes interagem dentro do sistema de delivery.

- [Diagrama de Casos de Uso](./diagramas/diagrama-caso-de-uso.png)

---

## 5. Modelagem de Ameaças (STRIDE)

Esta seção aplica o modelo STRIDE ao **Bah Delivery**, relacionando cada ameaça identificada aos ativos descritos na seção *Usuários, Ativos e Pontos de Interação*. As ameaças foram organizadas em uma tabela por categoria, e os identificadores utilizam um prefixo correspondente à letra da categoria (**S**poofing, **T**ampering, **R**epudiation, **I**nformation Disclosure, **D**enial of Service e **E**levation of Privilege).

### Spoofing (Falsificação de identidade)

O sistema possui quatro perfis distintos de usuário, e todas as operações relevantes dependem da correta identificação de quem as executa. Como o cadastro de restaurantes e entregadores concede acesso a dados pessoais de clientes, falhas na verificação de identidade permitem que um atacante assuma o papel de um usuário legítimo.

| ID | Componente ou ativo | Ameaça identificada | Possível impacto |
|----|---------------------|---------------------|------------------|
| S01 | Credenciais de autenticação | Um atacante utiliza senhas vazadas de outros serviços para tentar acessar contas de clientes em massa (*credential stuffing*), pois o sistema não limita tentativas de login nem exige múltiplo fator de autenticação | Acesso ao histórico de pedidos, endereços residenciais e meios de pagamento salvos; realização de pedidos fraudulentos em nome da vítima |
| S02 | API REST e sessões de usuário | O token de sessão é interceptado ou obtido pelo atacante e reutilizado para consumir a API como se fosse o usuário legítimo | O atacante executa qualquer operação permitida ao perfil da vítima sem conhecer a senha da conta |
| S03 | Dados dos restaurantes | Um usuário mal-intencionado cadastra um restaurante inexistente, pois o sistema não valida CNPJ, alvará ou endereço do estabelecimento | Recebimento de pagamentos por pedidos que nunca serão entregues, coleta de dados pessoais de clientes e perda de confiança na plataforma |
| S04 | Dados dos entregadores | Uma conta de entregador é compartilhada ou vendida e passa a ser utilizada por uma pessoa que nunca foi verificada pela plataforma | Uma pessoa não identificada recebe o endereço residencial dos clientes e retira pedidos nos estabelecimentos em nome de outro entregador |
| S05 | Credenciais de autenticação | Envio de mensagens de *phishing* que imitam as notificações oficiais da plataforma, direcionando o cliente a uma página falsa de login | Captura das credenciais do cliente, viabilizando o acesso indevido descrito em S01 |

### Tampering (Alteração indevida de dados)

O fluxo de compra do Bah Delivery envolve dados que circulam entre o navegador do cliente, a API e o banco de dados, passando ainda pelo restaurante e pelo entregador. Cada etapa em que um valor, um endereço ou um status é informado por uma das partes representa uma oportunidade de alteração indevida, especialmente quando o servidor confia em informações enviadas pelo cliente sem validá-las novamente.

| ID | Componente ou ativo | Ameaça identificada | Possível impacto |
|----|---------------------|---------------------|------------------|
| T01 | Pedidos e carrinho de compras | O valor total do pedido é calculado no navegador e aceito pelo servidor sem recálculo, permitindo que o cliente altere o preço antes de enviar o pedido ao pagamento | Pagamento de valor inferior ao devido, gerando prejuízo financeiro ao restaurante e à plataforma |
| T02 | Pedidos | O endereço de entrega é alterado após a confirmação do pagamento, sem que o pedido seja revalidado ou o restaurante notificado | Desvio da mercadoria para um endereço controlado por terceiro |
| T03 | Cardápios | O restaurante altera o preço de um produto depois que o pedido já foi confirmado pelo cliente, pois o pedido referencia o cardápio em vez de armazenar o preço praticado no momento da compra | Cobrança divergente do valor acordado e disputa entre cliente e estabelecimento |
| T04 | Pedidos e histórico de entregas | O entregador marca o pedido como entregue sem ter realizado a entrega, uma vez que a atualização de status não exige nenhuma evidência ou confirmação do cliente | Cliente permanece sem o produto e o pagamento é liberado indevidamente ao entregador e ao restaurante |
| T05 | Banco de dados | Injeção de comandos SQL nos campos de busca de restaurantes e produtos, decorrente da concatenação direta da entrada do usuário nas consultas | Alteração ou destruição de pedidos, cardápios e avaliações, comprometendo a integridade de toda a base |
| T06 | Avaliações | Inserção ou edição fraudulenta de avaliações, seja por meio de contas descartáveis, seja por requisições que não verificam se o autor realmente realizou o pedido | Distorção da reputação de restaurantes e entregadores, levando clientes a decidirem com base em informação falsa |

### Repudiation (Possibilidade de negar uma ação realizada)

O Bah Delivery intermedeia relações entre partes que não se conhecem e que podem ter interesses conflitantes diante de um problema na entrega. Sem registros confiáveis e protegidos contra alteração, a plataforma não consegue determinar o que de fato ocorreu, e qualquer um dos envolvidos pode negar a ação que executou. O ativo **Logs do sistema**, já identificado como de criticidade alta, é o principal elemento de proteção contra essa categoria.

| ID | Componente ou ativo | Ameaça identificada | Possível impacto |
|----|---------------------|---------------------|------------------|
| R01 | Logs do sistema e informações de pagamento | O cliente nega ter realizado um pedido e solicita o estorno da cobrança, e o sistema não mantém registro da operação com data, hora, endereço de origem e dispositivo utilizado | Perda financeira com estornos indevidos, sem que a plataforma disponha de evidência para contestar a alegação |
| R02 | Histórico de entregas | O entregador nega ter retirado o pedido no restaurante ou tê-lo entregue ao cliente, pois a conclusão da entrega não exige comprovante, registro fotográfico ou geolocalização | Disputa entre cliente, restaurante e entregador sem qualquer elemento objetivo que permita atribuir responsabilidade |
| R03 | Cardápios e pedidos | O restaurante nega ter alterado o preço de um produto ou ter cancelado um pedido, uma vez que o sistema não registra o histórico de alterações do cardápio nem a autoria dos cancelamentos | Conflito com o cliente, impossibilidade de apurar a conduta do estabelecimento e perda de confiança na plataforma |
| R04 | Logs do sistema | Um administrador com acesso direto ao banco de dados apaga ou edita registros de log para ocultar operações que realizou | Eliminação das evidências de uso indevido, inviabilizando a auditoria e a responsabilização de quem detém maior privilégio no sistema |

### Information Disclosure (Exposição indevida de informações)

Para que uma entrega seja concluída, o Bah Delivery precisa expor o endereço residencial e o telefone do cliente a duas partes externas à plataforma: o restaurante e o entregador. A isso somam-se os dados armazenados no banco — CPF, credenciais e informações de pagamento — e o tráfego que circula entre o navegador e a API. A exposição indevida nasce tanto da ausência de controles técnicos quanto da entrega de mais informação do que cada perfil precisa para executar sua tarefa.

| ID | Componente ou ativo | Ameaça identificada | Possível impacto |
|----|---------------------|---------------------|------------------|
| I01 | API REST e pedidos | Os endpoints identificam pedidos, endereços e avaliações por um identificador sequencial e não verificam se o registro solicitado pertence ao usuário autenticado, permitindo que o cliente altere o identificador na requisição e leia dados de terceiros | Leitura do histórico de pedidos, endereços de entrega e dados pessoais de qualquer cliente da plataforma a partir de uma conta comum |
| I02 | Dados pessoais dos clientes | O entregador mantém acesso ao endereço, telefone e nome do cliente mesmo após a conclusão da entrega, pois a informação permanece disponível no histórico de entregas | Acúmulo de endereços residenciais em poder de um perfil pouco verificado (ver S04), possibilitando uso posterior para fins não relacionados ao serviço |
| I03 | Dados pessoais dos clientes | O restaurante recebe o cadastro completo do cliente, incluindo CPF e demais pedidos realizados, quando apenas o nome e o item solicitado são necessários para o preparo | Exposição desnecessária de dados pessoais a dezenas de estabelecimentos, ampliando a superfície de vazamento sem qualquer benefício operacional |
| I04 | API REST e credenciais de autenticação | A comunicação entre o navegador e a API trafega sem TLS ou com certificado mal configurado, permitindo a leitura do conteúdo das requisições em redes compartilhadas | Captura de credenciais, tokens de sessão e dados de pagamento em trânsito, viabilizando o sequestro de sessão descrito em S02 |
| I05 | Banco de dados e informações de pagamento | Senhas armazenadas com algoritmo de hash fraco ou sem *salt* e dados de pagamento persistidos em texto claro no banco de dados | Um único acesso indevido à base compromete as credenciais de todos os usuários e os meios de pagamento associados |
| I06 | API REST | Mensagens de erro da aplicação retornam o rastreamento da exceção, a consulta SQL executada ou a versão dos componentes utilizados | O atacante obtém a estrutura interna do banco e as tecnologias em uso, facilitando a exploração da injeção descrita em T05 |
| I07 | Logs do sistema | Os registros de auditoria armazenam o corpo integral das requisições, incluindo senhas, tokens de sessão e dados de pagamento, e ficam acessíveis a todos os administradores | O log, criado como mecanismo de proteção, passa a ser uma cópia adicional dos dados mais sensíveis do sistema |
| I08 | Sistema de autenticação | O login e a recuperação de senha distinguem "e-mail não cadastrado" de "senha incorreta", permitindo confirmar quais endereços possuem conta na plataforma | Construção de uma lista de contas válidas que torna mais eficientes os ataques descritos em S01 e S05 |

### Denial of Service (Indisponibilidade ou degradação do serviço)

O Bah Delivery opera com forte concentração de demanda em horários de refeição, e sua operação depende de recursos que não são apenas computacionais: a capacidade de produção dos restaurantes e a disponibilidade dos entregadores também são finitas. Por isso, a indisponibilidade pode ser provocada tanto pela saturação da infraestrutura quanto pelo uso abusivo das próprias funcionalidades do sistema, sem que nenhuma falha técnica seja explorada.

| ID | Componente ou ativo | Ameaça identificada | Possível impacto |
|----|---------------------|---------------------|------------------|
| D01 | Pedidos e dados dos restaurantes | Contas automatizadas realizam um grande volume de pedidos que nunca serão pagos ou retirados, pois o sistema não limita pedidos simultâneos por cliente nem exige confirmação do pagamento antes de enviá-los ao estabelecimento | Ocupação da capacidade de produção do restaurante com pedidos falsos, desperdício de insumos e impedimento do atendimento a clientes legítimos |
| D02 | API REST e servidor da aplicação | Ausência de limitação de requisições nos endpoints públicos de consulta de restaurantes e cardápios, permitindo o envio de um volume massivo de chamadas | Lentidão ou indisponibilidade da plataforma justamente no horário de pico, com perda de vendas para restaurantes e para a plataforma |
| D03 | Banco de dados | Consultas de busca e de histórico executadas sem paginação ou sem limite de intervalo, permitindo que uma única requisição solicite a totalidade dos registros | Esgotamento dos recursos do banco de dados a partir de poucas requisições, afetando todas as operações do sistema simultaneamente |
| D04 | Processamento de solicitações de entrega | Um entregador aceita simultaneamente diversas entregas disponíveis sem qualquer limite e não as executa, mantendo os pedidos reservados em seu nome | Pedidos ficam retidos sem entregador efetivo, e outros entregadores são impedidos de aceitá-los, paralisando o fluxo de entregas de uma região |
| D05 | Sistema de autenticação | O envio de mensagens de recuperação de senha e de confirmação de cadastro não possui limite por endereço ou por origem da requisição | Esgotamento da cota do serviço de mensagens, impedindo que usuários legítimos recuperem o acesso, além de uso da plataforma para incomodar terceiros |
| D06 | Credenciais de autenticação | O bloqueio de conta adotado como resposta às tentativas repetidas de login (ver S01) é acionado sem distinguir a origem das tentativas, permitindo que um atacante bloqueie deliberadamente contas alheias | Impedimento do acesso de clientes, restaurantes e entregadores legítimos, transformando um controle de segurança em vetor de indisponibilidade |
| D07 | Cardápios e servidor da aplicação | O envio de imagens de produtos não valida tamanho, formato nem quantidade por estabelecimento | Consumo do armazenamento e da banda do servidor até a interrupção do serviço, a partir de uma conta de restaurante comum |

### Elevation of Privilege (Obtenção indevida de permissões)

O sistema define quatro perfis com poderes bastante distintos, e o ativo **Controle de permissões** foi classificado como crítico justamente porque é ele que separa um cliente de um administrador capaz de gerenciar toda a plataforma. As ameaças desta categoria decorrem de decisões de autorização tomadas no lugar errado — no navegador em vez do servidor, ou com base em informação que o próprio usuário controla.

| ID | Componente ou ativo | Ameaça identificada | Possível impacto |
|----|---------------------|---------------------|------------------|
| E01 | Controle de permissões e API REST | A restrição de acesso é aplicada apenas na interface, que oculta as opções indisponíveis ao perfil, enquanto os endpoints correspondentes não verificam o papel do usuário autenticado | Um cliente autenticado invoca diretamente as rotas de gerenciamento de usuários e de restaurantes, obtendo poderes de administrador |
| E02 | Cadastro de usuários e controle de permissões | O perfil do usuário é recebido como um campo da requisição de cadastro ou de atualização de dados e gravado sem validação no servidor | Um atacante cria ou converte a própria conta em administrador ou restaurante durante o cadastro, sem qualquer aprovação da plataforma |
| E03 | Sessões de usuário e API REST | O token de sessão carrega o perfil do usuário e é aceito sem verificação adequada da assinatura, ou é assinado com um segredo fraco e previsível | Forja de um token válido com perfil de administrador, concedendo acesso irrestrito ao sistema sem passar pela autenticação |
| E04 | Dados dos restaurantes e cardápios | As operações de gerenciamento de cardápio e de pedidos verificam apenas que o usuário é um restaurante, sem confirmar que o registro manipulado pertence ao estabelecimento autenticado | Um restaurante altera preços, remove produtos ou consulta os pedidos de um concorrente, obtendo vantagem competitiva indevida |
| E05 | Controle de permissões | O perfil de administrador concentra todas as capacidades do sistema, sem segregação entre a gestão de usuários, a consulta de logs e o acesso a dados de pagamento | O comprometimento de uma única conta administrativa entrega o controle total da plataforma, incluindo a supressão de evidências descrita em R04 |
| E06 | Controle de permissões e logs do sistema | A alteração de permissões de uma conta não exige reautenticação nem gera registro de auditoria da mudança | Um acesso administrativo obtido por meio de S01 ou S02 concede privilégios permanentes a contas controladas pelo atacante, sem que a elevação seja detectada |

---

## 6. Casos de Abuso

Enquanto a modelagem STRIDE identifica ameaças de forma isolada, os casos de abuso descrevem como essas ameaças se combinam em cenários concretos de ataque. Cada caso apresenta o percurso completo de um agente mal-intencionado — que pode ser um atacante externo, um usuário indevido ou até mesmo um usuário legítimo da plataforma — desde as condições que tornam o abuso possível até o impacto produzido.

Os casos foram construídos a partir das ameaças descritas na seção anterior, e cada um indica explicitamente os identificadores STRIDE que o sustentam, permitindo rastrear a origem de cada cenário. Não se pretendeu cobrir individualmente as 36 ameaças levantadas: foram selecionadas as combinações consideradas mais representativas em termos de probabilidade e impacto, assegurando que as seis categorias do STRIDE e os quatro perfis de usuário estivessem contemplados.

### Visão Geral dos Casos de Abuso

| ID | Título | Ator | Categorias STRIDE |
|----|--------|------|-------------------|
| CA01 | Sequestro de conta de cliente para fraude em pedidos | Atacante externo | Spoofing e Information Disclosure |
| CA02 | Cadastro de restaurante fantasma para captura de pagamentos | Usuário mal-intencionado | Spoofing e Information Disclosure |
| CA03 | Manipulação do valor do pedido antes do pagamento | Cliente autenticado | Tampering |
| CA04 | Confirmação fraudulenta de entrega não realizada | Entregador cadastrado | Tampering e Repudiation |
| CA05 | Coleta em massa de dados de clientes por manipulação de identificadores | Cliente autenticado | Information Disclosure |
| CA06 | Sabotagem operacional de restaurante em horário de pico | Concorrente ou atacante externo | Denial of Service |
| CA07 | Elevação de privilégio para obtenção de acesso administrativo | Cliente autenticado | Elevation of Privilege |
| CA08 | Abuso de privilégio administrativo com supressão de evidências | Administrador da plataforma | Elevation of Privilege e Repudiation |

### CA01 — Sequestro de conta de cliente para fraude em pedidos

| Item | Descrição |
|------|-----------|
| **Ator** | Atacante externo, sem vínculo com a plataforma |
| **Objetivo** | Assumir a identidade de um cliente legítimo para realizar pedidos fraudulentos e acessar seus dados pessoais e meios de pagamento |
| **Condições necessárias** | O sistema não limita tentativas de login nem exige múltiplo fator de autenticação; as telas de login e de recuperação de senha distinguem "e-mail não cadastrado" de "senha incorreta"; a comunicação entre o navegador e a API não é protegida adequadamente por TLS |
| **Impacto esperado** | Prejuízo financeiro direto ao cliente, exposição de dados pessoais e de endereço residencial, e uso da conta comprometida como base para novas fraudes na plataforma |
| **Ameaças relacionadas** | S01, S02, I04 e I08 |
| **Categorias STRIDE** | Spoofing e Information Disclosure |

**Sequência de ações**

1. O atacante explora a diferenciação das mensagens de erro para confirmar quais endereços de e-mail possuem conta na plataforma, construindo uma lista de alvos válidos.
2. Sobre essa lista, testa de forma automatizada credenciais vazadas de outros serviços, sem que o sistema limite as tentativas ou exija um segundo fator de autenticação.
3. Ao obter acesso a uma conta, consulta o histórico de pedidos e obtém o endereço residencial, o telefone e os meios de pagamento salvos da vítima.
4. Alternativamente, em uma rede compartilhada, intercepta o token de sessão de um cliente já autenticado e o reutiliza, dispensando o conhecimento da senha.
5. Com a sessão ativa, realiza pedidos custeados pelos meios de pagamento salvos, direcionando as entregas para endereços de sua escolha.

### CA02 — Cadastro de restaurante fantasma para captura de pagamentos

| Item | Descrição |
|------|-----------|
| **Ator** | Usuário mal-intencionado, sem estabelecimento real |
| **Objetivo** | Receber pagamentos por pedidos que nunca serão preparados nem entregues e coletar dados pessoais dos clientes atraídos |
| **Condições necessárias** | O cadastro de restaurante é aceito sem validação de CNPJ, alvará ou endereço do estabelecimento; o restaurante recebe o cadastro completo do cliente a cada pedido |
| **Impacto esperado** | Prejuízo financeiro aos clientes, formação de uma base de dados pessoais obtida de forma fraudulenta e perda de confiança na plataforma como intermediária das transações |
| **Ameaças relacionadas** | S03 e I03 |
| **Categorias STRIDE** | Spoofing e Information Disclosure |

**Sequência de ações**

1. O atacante cria uma conta de restaurante com dados fictícios ou pertencentes a terceiros, e o cadastro é aprovado sem qualquer verificação.
2. Publica um cardápio com preços abaixo do praticado no mercado, de modo a atrair pedidos rapidamente.
3. Clientes realizam pedidos e efetuam o pagamento, que é processado e confirmado normalmente pela plataforma.
4. A cada pedido recebido, o atacante coleta o cadastro completo do cliente, incluindo CPF, telefone e endereço de entrega.
5. Os pedidos nunca são preparados, ou são marcados como concluídos sem qualquer participação de um entregador real.
6. O atacante abandona a conta antes que o volume de reclamações resulte em bloqueio, e repete o processo sob um novo cadastro.

### CA03 — Manipulação do valor do pedido antes do pagamento

| Item | Descrição |
|------|-----------|
| **Ator** | Cliente autenticado com intenção maliciosa |
| **Objetivo** | Pagar por um pedido valor inferior ao efetivamente devido |
| **Condições necessárias** | O valor total do pedido é calculado no navegador e aceito pelo servidor sem recálculo; o pedido referencia o cardápio em vez de registrar o preço praticado no momento da compra |
| **Impacto esperado** | Prejuízo financeiro direto ao restaurante e à plataforma, que entregam o produto por valor inferior ao devido, com possibilidade de repetição em escala enquanto a falha não for corrigida |
| **Ameaças relacionadas** | T01 e T03 |
| **Categorias STRIDE** | Tampering |

**Sequência de ações**

1. O atacante monta um carrinho normalmente e observa a requisição enviada ao servidor no momento de fechar o pedido.
2. Antes do envio, altera o valor total da compra ou o preço unitário dos itens para uma quantia significativamente menor.
3. O servidor aceita o valor recebido sem recalculá-lo a partir do cardápio e encaminha o pedido para o pagamento.
4. O pagamento é processado e confirmado, e o pedido segue para preparo e entrega pelo valor adulterado.
5. Como o pedido apenas referencia o cardápio, sem preservar o preço praticado, a divergência não fica evidente no registro da transação.

### CA04 — Confirmação fraudulenta de entrega não realizada

| Item | Descrição |
|------|-----------|
| **Ator** | Entregador cadastrado na plataforma |
| **Objetivo** | Receber o valor referente à entrega sem efetivamente entregar o pedido ao cliente |
| **Condições necessárias** | A atualização do status da entrega não exige comprovante, registro fotográfico, código de confirmação ou geolocalização; a conclusão da entrega libera o pagamento automaticamente |
| **Impacto esperado** | Cliente lesado sem receber o produto pago, disputa entre as três partes sem elementos objetivos que permitam atribuir responsabilidade e desgaste da reputação da plataforma |
| **Ameaças relacionadas** | T04 e R02 |
| **Categorias STRIDE** | Tampering e Repudiation |

**Sequência de ações**

1. O entregador aceita a solicitação e retira o pedido no restaurante normalmente.
2. Em vez de realizar a entrega, atualiza o status do pedido para "entregue" diretamente pelo aplicativo.
3. Como o sistema não exige nenhuma evidência da entrega, a atualização é aceita sem contestação.
4. O pagamento é liberado ao entregador e ao restaurante, e o pedido é encerrado como concluído.
5. Ao ser questionado pela reclamação do cliente, o entregador nega a irregularidade, e a ausência de registros objetivos impede apurar o que de fato ocorreu.

### CA05 — Coleta em massa de dados de clientes por manipulação de identificadores

| Item | Descrição |
|------|-----------|
| **Ator** | Cliente autenticado com intenção maliciosa |
| **Objetivo** | Obter pedidos, endereços e dados pessoais de outros clientes da plataforma |
| **Condições necessárias** | Os endpoints identificam pedidos, endereços e avaliações por identificadores sequenciais; as requisições não verificam se o registro solicitado pertence ao usuário autenticado |
| **Impacto esperado** | Vazamento em larga escala de dados pessoais, endereços residenciais e histórico de consumo, com violação de privacidade dos clientes e exposição da plataforma a sanções legais |
| **Ameaças relacionadas** | I01 e I02 |
| **Categorias STRIDE** | Information Disclosure |

**Sequência de ações**

1. O atacante realiza um pedido legítimo e observa o identificador numérico atribuído a ele nas requisições da API.
2. Altera manualmente esse identificador para valores próximos e reenvia a requisição utilizando a sua própria sessão válida.
3. Como o servidor confirma apenas que existe um usuário autenticado, sem verificar a quem pertence o registro, os dados de outros clientes são retornados normalmente.
4. O atacante automatiza a variação do identificador e percorre toda a faixa de registros existentes na base.
5. Os dados coletados são armazenados fora da plataforma e podem ser revendidos ou utilizados para viabilizar os ataques descritos em CA01.

### CA06 — Sabotagem operacional de restaurante em horário de pico

| Item | Descrição |
|------|-----------|
| **Ator** | Concorrente ou atacante externo |
| **Objetivo** | Inviabilizar a operação de um restaurante específico durante o período de maior demanda |
| **Condições necessárias** | O sistema não limita a quantidade de pedidos simultâneos por cliente; os pedidos são encaminhados ao restaurante antes da confirmação do pagamento; os endpoints públicos não possuem limitação de requisições |
| **Impacto esperado** | Prejuízo financeiro e operacional ao restaurante, perda de vendas para a plataforma no horário mais rentável e dano à reputação do estabelecimento perante os clientes |
| **Ameaças relacionadas** | D01 e D02 |
| **Categorias STRIDE** | Denial of Service |

**Sequência de ações**

1. O atacante cria diversas contas de cliente, aproveitando a ausência de verificação no cadastro.
2. No horário de maior movimento, dispara simultaneamente um grande volume de pedidos ao restaurante-alvo, sem concluir os pagamentos.
3. O restaurante recebe as solicitações e inicia o preparo, pois a plataforma as encaminha antes da confirmação do pagamento.
4. A capacidade de produção do estabelecimento é integralmente consumida por pedidos que nunca serão pagos nem retirados.
5. Clientes legítimos encontram o restaurante indisponível ou enfrentam atrasos, e os insumos utilizados no preparo são desperdiçados.

### CA07 — Elevação de privilégio para obtenção de acesso administrativo

| Item | Descrição |
|------|-----------|
| **Ator** | Cliente autenticado com intenção maliciosa |
| **Objetivo** | Obter permissões de administrador e assumir o controle da plataforma |
| **Condições necessárias** | O perfil do usuário é recebido como campo da requisição e gravado sem validação no servidor; a restrição de acesso é aplicada apenas na interface; o token de sessão carrega o perfil sem verificação adequada da assinatura |
| **Impacto esperado** | Comprometimento total da plataforma, com acesso irrestrito aos dados de todos os usuários e capacidade de manter o acesso e apagar os vestígios da invasão |
| **Ameaças relacionadas** | E01, E02 e E03 |
| **Categorias STRIDE** | Elevation of Privilege |

**Sequência de ações**

1. O atacante intercepta a requisição de atualização do próprio cadastro e altera o campo que define seu perfil de "cliente" para "administrador".
2. Como o servidor grava o valor recebido sem validá-lo, a alteração é efetivada sem qualquer aprovação da plataforma.
3. Caso esse caminho falhe, invoca diretamente as rotas de gerenciamento de usuários e de restaurantes, que não verificam o papel do usuário autenticado por confiarem na ocultação feita pela interface.
4. Como alternativa, forja um token de sessão com perfil administrativo, explorando a ausência de verificação da assinatura ou o uso de um segredo previsível.
5. De posse do acesso administrativo, consulta os dados de todos os usuários, altera as permissões de contas sob seu controle e acessa os registros de auditoria.

### CA08 — Abuso de privilégio administrativo com supressão de evidências

| Item | Descrição |
|------|-----------|
| **Ator** | Administrador com acesso legítimo à plataforma |
| **Objetivo** | Utilizar o próprio nível de privilégio para fins indevidos e eliminar os registros que permitiriam identificá-lo |
| **Condições necessárias** | O perfil de administrador concentra todas as capacidades do sistema, sem segregação de funções; a alteração de permissões não exige reautenticação nem gera registro de auditoria; os administradores possuem acesso direto ao banco de dados onde os logs são armazenados |
| **Impacto esperado** | Abuso continuado de privilégio sem possibilidade de detecção ou responsabilização, comprometendo a confiabilidade de todo o mecanismo de auditoria e de controle de acesso |
| **Ameaças relacionadas** | E05, E06 e R04 |
| **Categorias STRIDE** | Elevation of Privilege e Repudiation |

**Sequência de ações**

1. O administrador consulta dados de pagamento e informações pessoais de clientes sem qualquer justificativa operacional, o que é possível porque o perfil não separa as capacidades administrativas.
2. Concede privilégios administrativos a uma segunda conta sob seu controle, sem que a mudança exija reautenticação ou gere alerta.
3. Acessa diretamente o banco de dados e apaga ou edita os registros de log referentes às suas próprias ações.
4. Passa a operar pela conta secundária, mantendo o acesso mesmo que a conta original venha a ser revista.
5. Sem registros íntegros, a plataforma não consegue reconstituir os fatos nem atribuir responsabilidade a quem os praticou.

---

## 7. Considerações Finais

### Principais Ameaças

A análise das seis categorias do STRIDE evidenciou que as ameaças mais preocupantes do Bah Delivery não decorrem de técnicas sofisticadas de ataque, mas de decisões de projeto que depositam confiança indevida em informações controladas pelo usuário. As ameaças a seguir foram consideradas prioritárias por combinarem alta probabilidade de ocorrência com impacto elevado.

| Ameaça | Motivo da priorização |
|--------|-----------------------|
| E01, E02 e E03 — Autorização decidida fora do servidor | Comprometem simultaneamente todos os demais controles: uma vez obtido o perfil de administrador, nenhuma outra proteção da plataforma permanece efetiva |
| I01 — Ausência de verificação de propriedade dos registros | Permite o vazamento de toda a base de clientes a partir de uma conta comum, sem exigir qualquer conhecimento técnico avançado |
| I05 — Armazenamento inseguro de senhas e dados de pagamento | Transforma um único acesso indevido ao banco de dados no comprometimento definitivo de todos os usuários da plataforma |
| T01 — Confiança em valores calculados no cliente | Produz prejuízo financeiro direto e recorrente, e atinge simultaneamente os restaurantes e a plataforma |
| S01 — Ausência de limitação de tentativas e de múltiplo fator | Viabiliza o comprometimento de contas em massa e serve de porta de entrada para a maior parte dos demais cenários |
| R02 e R04 — Ausência de registros íntegros das operações | Impede não apenas a responsabilização após um incidente, mas a própria constatação de que o incidente ocorreu |

### Ativos Críticos

Os ativos classificados como **Críticos** na seção *Usuários, Ativos e Pontos de Interação* concentram a maior parte das ameaças identificadas e concentram, por consequência, a necessidade de proteção:

- **Controle de permissões** — é o mecanismo que separa um cliente de um administrador capaz de gerenciar toda a plataforma, e figura em todas as ameaças de Elevation of Privilege;
- **Credenciais de autenticação** — sustentam a identidade de todos os perfis, e seu comprometimento habilita ataques em todas as demais categorias;
- **Banco de dados** — reúne em um único ponto os dados pessoais, os pedidos, as credenciais e os registros de auditoria do sistema;
- **Informações de pagamento** — representam o ativo de maior interesse econômico para um atacante e o de maior consequência legal em caso de vazamento;
- **Servidor da aplicação** — sua indisponibilidade interrompe simultaneamente a operação de clientes, restaurantes e entregadores.

Os **Logs do sistema**, ainda que classificados como de criticidade alta, merecem destaque por sua natureza dupla: são o principal instrumento de proteção contra a categoria Repudiation, mas passam a constituir um ativo sensível adicional quando registram dados que não deveriam armazenar, conforme descrito na ameaça I07.

### Casos de Abuso de Maior Impacto

| Caso | Justificativa |
|------|---------------|
| CA07 — Elevação de privilégio para acesso administrativo | É o cenário de maior severidade, pois anula todas as demais proteções de uma só vez e abre caminho para o CA08 |
| CA05 — Coleta em massa de dados de clientes | Produz o maior volume de dano por esforço investido: uma conta comum e uma requisição repetida bastam para expor toda a base de clientes |
| CA08 — Abuso de privilégio administrativo com supressão de evidências | Combina dano direto com a eliminação da capacidade de detecção, o que o torna potencialmente permanente e não mensurável |
| CA01 — Sequestro de conta de cliente | É o cenário de maior probabilidade de ocorrência, por depender apenas de credenciais já disponíveis publicamente em vazamentos anteriores |

### Possíveis Medidas de Proteção

Ainda que a proposta de soluções não seja obrigatória nesta etapa, o grupo registra as direções de mitigação que considera mais pertinentes para as ameaças priorizadas:

| Categoria | Medidas indicadas |
|-----------|-------------------|
| Spoofing | Limitação de tentativas de login, múltiplo fator de autenticação, mensagens de erro genéricas e verificação documental no cadastro de restaurantes e entregadores |
| Tampering | Recálculo de valores no servidor, registro do preço praticado no momento da compra e exigência de evidência para a conclusão da entrega |
| Repudiation | Registro de auditoria imutável, com retenção definida e acesso segregado do perfil administrativo |
| Information Disclosure | Verificação de propriedade dos registros a cada requisição, TLS obrigatório, hash com *salt* para senhas e entrega a cada perfil apenas dos dados necessários à sua tarefa |
| Denial of Service | Limitação de requisições por origem, paginação obrigatória nas consultas, confirmação de pagamento antes do envio ao restaurante e limite de entregas simultâneas por entregador |
| Elevation of Privilege | Autorização verificada no servidor a cada requisição, perfil nunca aceito como campo da requisição, segregação de funções administrativas e registro de toda alteração de permissão |

### Dificuldades Encontradas

Durante a elaboração deste trabalho, o grupo enfrentou as seguintes dificuldades:

- **Analisar a segurança de um sistema não implementado.** Sem código, banco de dados ou infraestrutura definidos, foi necessário assumir hipóteses sobre o funcionamento da plataforma para que as ameaças fossem concretas, evitando descrições genéricas que serviriam a qualquer sistema.
- **Distinguir ameaça, vulnerabilidade e caso de abuso.** A separação entre o que o atacante deseja obter, a falha que permite obtê-lo e o percurso completo do ataque exigiu diversas revisões até que as seções ficassem coerentes entre si.
- **Delimitar a fronteira entre as categorias do STRIDE.** Várias ameaças poderiam ser classificadas em mais de uma categoria — como a diferença entre assumir a identidade de outro usuário (*Spoofing*) e ampliar os próprios privilégios (*Elevation of Privilege*), ou entre alterar um registro (*Tampering*) e negar tê-lo alterado (*Repudiation*). O critério adotado foi classificar pela consequência principal da ameaça e registrar as relações cruzadas nos casos de abuso.
- **Reconhecer usuários legítimos como agentes de ameaça.** A tendência inicial foi concentrar a análise em atacantes externos. Reconhecer que o restaurante, o entregador e o próprio administrador podem ser a origem do abuso ampliou significativamente a análise e originou alguns dos casos de maior impacto.
- **Classificar a criticidade dos ativos sem dados de operação real.** A ausência de informações sobre volume de usuários, faturamento e requisitos regulatórios tornou a classificação dependente do julgamento do grupo, tendo sido adotado o critério do dano potencial em caso de comprometimento.

---

## 8. Organização do Projeto

### Etapa 1

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | Identificação dos usuários, ativos e pontos de interação |
| Emanuel Ferreira | Elaboração dos casos de abuso  |
| Guilherme Mundt | Modelagem de ameaças STRIDE (Information Disclosure, Denial of Service e Elevation of Privilege) |
| Lívia Barbosa | Modelagem de diagramas |
| Mariana Padilha | Identificação e descrição do sistema |
| Matheus Ciocca | Modelagem de ameaças STRIDE (Spoofing, Tampering e Repudiation) |
