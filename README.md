# Bah Delivery

O Bah Delivery é uma plataforma web voltada para o gerenciamento integrado de pedidos e entregas de refeições, conectando diferentes perfis de usuário em um único ambiente digital. O sistema permite que clientes consultem cardápios, realizem pedidos e acompanhem o status das entregas, enquanto restaurantes podem administrar seus produtos, receber e organizar pedidos. Os entregadores possuem acesso às solicitações de entrega e informações necessárias para a execução do serviço, e os administradores são responsáveis pelo gerenciamento e controle geral da plataforma.

---

## Identificação do Sistema

| Item | Descrição |
|------|-----------|
| **Nome do sistema** | Bah Delivery |
| **Disciplina** | Engenharia de Software Seguro |
| **Integrantes** | Arthur Medeiros, Emanuel Ferreira, Guilherme Mundt, Lívia Barbosa, Mariana Padilha e Matheus Ciocca |
| **Repositório** | https://github.com/MariPadilha/bah_delivery |

---

## Justificativa

O sistema de delivery foi escolhido por representar uma aplicação real e amplamente utilizada, que envolve múltiplos usuários, processamento de pagamentos e armazenamento de dados sensíveis. Essas características possibilitam explorar diversos aspectos relacionados à segurança da informação e à modelagem de ameaças, atendendo aos objetivos da disciplina.

---

## Problema que o sistema resolve

O processo tradicional de pedidos de refeições frequentemente envolve comunicação fragmentada entre clientes, restaurantes e entregadores, podendo causar atrasos, erros nas solicitações e dificuldades no acompanhamento das entregas. O Bah Delivery busca solucionar esses problemas ao automatizar e organizar o fluxo de pedidos, centralizando as informações em uma única plataforma, reduzindo falhas de comunicação e proporcionando maior agilidade e transparência durante todo o processo de compra e entrega.

---

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

---

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
---

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

---

## Recursos que Precisam ser Protegidos

Os principais recursos que necessitam de proteção são:

---

## Usuários, Ativos e Pontos de Interação

### Usuários

- Cliente
- Restaurante
- Entregador
- Administrador

### Ativos

### Pontos de interação

---

## Visão Geral da Arquitetura
Esta seção apresenta os diagramas desenvolvidos a fim de oferecer uma visão simplificada de como os usuários e componentes interagem dentro do sistema de delivery.

- [Diagrama de Casos de Uso](./diagramas/diagrama-caso-de-uso.png)

---

## Modelagem de Ameaças (STRIDE)

| ID | Categoria | Componente | Ameaça | Impacto |
|----|-----------|------------|--------|---------|
| T01 | Spoofing | | | |
| T02 | Tampering | | | |
| T03 | Repudiation | | | |
| T04 | Information Disclosure | | | |
| T05 | Denial of Service | | | |
| T06 | Elevation of Privilege | | | |

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
