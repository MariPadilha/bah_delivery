# Bah Delivery

O Bah Delivery é uma plataforma web voltada para o gerenciamento integrado de pedidos e entregas de refeições, conectando diferentes perfis de usuário em um único ambiente digital. O sistema permite que clientes consultem cardápios, realizem pedidos e acompanhem o status das entregas, enquanto restaurantes podem administrar seus produtos, receber e organizar pedidos. Os entregadores possuem acesso às solicitações de entrega e informações necessárias para a execução do serviço, e os administradores são responsáveis pelo gerenciamento e controle geral da plataforma.



## Identificação do Sistema

| Item | Descrição |
|------|-----------|
| **Nome do sistema** | Bah Delivery |
| **Disciplina** | Engenharia de Software Seguro |
| **Integrantes** | Arthur Medeiros, Emanuel Ferreira, Guilherme Mundt, Lívia Barbosa, Mariana Padilha e Matheus Ciocca |
| **Repositório** | https://github.com/MariPadilha/bah_delivery |



## Justificativa

O sistema de delivery foi escolhido por representar uma aplicação real e amplamente utilizada, que envolve múltiplos usuários, processamento de pagamentos e armazenamento de dados sensíveis. Essas características possibilitam explorar diversos aspectos relacionados à segurança da informação e à modelagem de ameaças, atendendo aos objetivos da disciplina.



## Problema que o sistema resolve

O processo tradicional de pedidos de refeições frequentemente envolve comunicação fragmentada entre clientes, restaurantes e entregadores, podendo causar atrasos, erros nas solicitações e dificuldades no acompanhamento das entregas. O Bah Delivery busca solucionar esses problemas ao automatizar e organizar o fluxo de pedidos, centralizando as informações em uma única plataforma, reduzindo falhas de comunicação e proporcionando maior agilidade e transparência durante todo o processo de compra e entrega.



## Perfis de Usuário

### Cliente

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

### Restaurante

Usuário responsável pelo gerenciamento do estabelecimento, produtos e atendimento dos pedidos recebidos.

**Principais funcionalidades:**
    
    - Cadastro e gerenciamento do restaurante;
    - Gerenciamento do cardápio;
    - Cadastro, edição e remoção de produtos;
    - Visualização dos pedidos recebidos;
    - Atualização do status dos pedidos;
    - Consulta do histórico de pedidos recebidos.

### Entregador

Usuário responsável pela retirada e entrega dos pedidos aos clientes.

**Principais funcionalidades:**

    - Cadastro e autenticação no sistema;
    - Visualização de entregas disponíveis;
    - Aceitação de solicitações de entrega;
    - Consulta das informações necessárias para entrega;
    - Atualização do status da entrega;
    - Consulta do histórico de entregas realizadas.

### Administrador

Usuário responsável pelo gerenciamento e controle geral da plataforma.

**Principais funcionalidades:**

    - Gerenciamento de usuários;
    - Controle de permissões de acesso;
    - Gerenciamento de restaurantes cadastrados;
    - Monitoramento das atividades do sistema;
    - Consulta de logs e registros da aplicação;
    - Administração geral da plataforma.



## Principais Funcionalidades do Sistema

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


## Informações Armazenadas ou Transmitidas

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



## Recursos que Precisam ser Protegidos

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



## Usuários, Ativos e Pontos de Interação

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



### Justificativa

A identificação dos ativos permite reconhecer quais recursos possuem maior valor para o funcionamento da plataforma e quais necessitam de maior nível de proteção. Ativos classificados como **Críticos** podem comprometer diretamente a segurança do sistema caso sejam violados, enquanto ativos classificados como **Alta** ou **Média** representam recursos importantes para a continuidade das operações e para a proteção dos dados dos usuários.

Esses ativos servirão como base para a etapa de Modelagem de Ameaças (STRIDE), permitindo identificar possíveis vulnerabilidades relacionadas à autenticação, integridade dos dados, disponibilidade do sistema, confidencialidade das informações e controle de acesso.

## Visão Geral da Arquitetura
Esta seção apresenta os diagramas desenvolvidos a fim de oferecer uma visão simplificada de como os usuários e componentes interagem dentro do sistema de delivery.

- [Diagrama de Casos de Uso](./diagramas/diagrama-caso-de-uso.png)

---

## Modelagem de Ameaças (STRIDE)

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

## Casos de Abuso

### CA01

**Título**

-

**Ator**

-

**Objetivo**

-

**Condições**

-

**Fluxo**

1.
2.
3.

**Impacto**

-

**Categorias STRIDE**

-

---

## Considerações Finais

### Principais ameaças

-

### Ativos críticos

-

### Casos de abuso de maior impacto

-

### Dificuldades encontradas

-

---

## Organização do Projeto

### Etapa 1

| Integrante | Responsabilidades |
|------------|------------------|
| Arthur Medeiros | Identificação dos usuários, ativos e pontos de interação |
| Emanuel Ferreira | |
| Guilherme Mundt | |
| Lívia Barbosa | Modelagem de diagramas |
| Mariana Padilha | Identificaçao e Descrição do Sistema |
| Matheus Ciocca | Modelagem de ameaças STRIDE (Spoofing, Tampering e Repudiation) |
