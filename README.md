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

### Spoofing — Falsificação de identidade

O sistema possui quatro perfis distintos de usuário, e todas as operações relevantes dependem da correta identificação de quem as executa. Como o cadastro de restaurantes e entregadores concede acesso a dados pessoais de clientes, falhas na verificação de identidade permitem que um atacante assuma o papel de um usuário legítimo.

| ID | Componente ou ativo | Ameaça identificada | Possível impacto |
|----|---------------------|---------------------|------------------|
| S01 | Credenciais de autenticação | Um atacante utiliza senhas vazadas de outros serviços para tentar acessar contas de clientes em massa (*credential stuffing*), pois o sistema não limita tentativas de login nem exige múltiplo fator de autenticação | Acesso ao histórico de pedidos, endereços residenciais e meios de pagamento salvos; realização de pedidos fraudulentos em nome da vítima |
| S02 | API REST e sessões de usuário | O token de sessão é interceptado ou obtido pelo atacante e reutilizado para consumir a API como se fosse o usuário legítimo | O atacante executa qualquer operação permitida ao perfil da vítima sem conhecer a senha da conta |
| S03 | Dados dos restaurantes | Um usuário mal-intencionado cadastra um restaurante inexistente, pois o sistema não valida CNPJ, alvará ou endereço do estabelecimento | Recebimento de pagamentos por pedidos que nunca serão entregues, coleta de dados pessoais de clientes e perda de confiança na plataforma |
| S04 | Dados dos entregadores | Uma conta de entregador é compartilhada ou vendida e passa a ser utilizada por uma pessoa que nunca foi verificada pela plataforma | Uma pessoa não identificada recebe o endereço residencial dos clientes e retira pedidos nos estabelecimentos em nome de outro entregador |
| S05 | Credenciais de autenticação | Envio de mensagens de *phishing* que imitam as notificações oficiais da plataforma, direcionando o cliente a uma página falsa de login | Captura das credenciais do cliente, viabilizando o acesso indevido descrito em S01 |

### Tampering — Alteração indevida de dados

O fluxo de compra do Bah Delivery envolve dados que circulam entre o navegador do cliente, a API e o banco de dados, passando ainda pelo restaurante e pelo entregador. Cada etapa em que um valor, um endereço ou um status é informado por uma das partes representa uma oportunidade de alteração indevida, especialmente quando o servidor confia em informações enviadas pelo cliente sem validá-las novamente.

| ID | Componente ou ativo | Ameaça identificada | Possível impacto |
|----|---------------------|---------------------|------------------|
| T01 | Pedidos e carrinho de compras | O valor total do pedido é calculado no navegador e aceito pelo servidor sem recálculo, permitindo que o cliente altere o preço antes de enviar o pedido ao pagamento | Pagamento de valor inferior ao devido, gerando prejuízo financeiro ao restaurante e à plataforma |
| T02 | Pedidos | O endereço de entrega é alterado após a confirmação do pagamento, sem que o pedido seja revalidado ou o restaurante notificado | Desvio da mercadoria para um endereço controlado por terceiro |
| T03 | Cardápios | O restaurante altera o preço de um produto depois que o pedido já foi confirmado pelo cliente, pois o pedido referencia o cardápio em vez de armazenar o preço praticado no momento da compra | Cobrança divergente do valor acordado e disputa entre cliente e estabelecimento |
| T04 | Pedidos e histórico de entregas | O entregador marca o pedido como entregue sem ter realizado a entrega, uma vez que a atualização de status não exige nenhuma evidência ou confirmação do cliente | Cliente permanece sem o produto e o pagamento é liberado indevidamente ao entregador e ao restaurante |
| T05 | Banco de dados | Injeção de comandos SQL nos campos de busca de restaurantes e produtos, decorrente da concatenação direta da entrada do usuário nas consultas | Alteração ou destruição de pedidos, cardápios e avaliações, comprometendo a integridade de toda a base |
| T06 | Avaliações | Inserção ou edição fraudulenta de avaliações, seja por meio de contas descartáveis, seja por requisições que não verificam se o autor realmente realizou o pedido | Distorção da reputação de restaurantes e entregadores, levando clientes a decidirem com base em informação falsa |

### Repudiation — Possibilidade de negar uma ação realizada

*Em elaboração.*

### Information Disclosure — Exposição indevida de informações

*Reservado — Integrante 5.*

### Denial of Service — Indisponibilidade ou degradação do serviço

*Reservado — Integrante 5.*

### Elevation of Privilege — Obtenção indevida de permissões

*Reservado — Integrante 5.*

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
| Arthur Medeiros | |
| Emanuel Ferreira | |
| Guilherme Mundt | |
| Lívia Barbosa | Modelagem de diagramas |
| Mariana Padilha | Identificaçao e Descrição do Sistema |
| Matheus Ciocca | |
