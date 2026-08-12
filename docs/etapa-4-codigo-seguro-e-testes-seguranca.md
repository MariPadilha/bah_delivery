# Etapa 4 — Código Seguro e Testes de Segurança

Esta etapa apresenta práticas de implementação segura definidas a partir da  [Etapa 3 — Arquitetura Segura](./etapa-3-arquitetura-segura.md) do projeto Bah Delivery.

Para cada prática selecionada, os testes de segurança são definidos **antes da implementação**, de forma que o comportamento seguro esperado pelo sistema seja estabelecido previamente.

---

## Sumário

1. [Prática 1 — Consultas Parametrizadas e Validação de Entrada](#1-prática-1--consultas-parametrizadas-e-validação-de-entrada)
2. [Prática 2 — *a definir*](#2-prática-2--nome-da-prática)
3. [Rastreabilidade das Práticas Definidas](#3-rastreabilidade-das-práticas-definidas)
4. [Considerações Finais](#4-considerações-finais)
5. [Distribuição Integrante X Responsabilidades](#5-distribuição-integrante-x-responsabilidades)

---

## 1. Prática 1 — Consultas Parametrizadas e Validação de Entrada

### 1.1. Risco relacionado

**R13 — Comprometimento da integridade do banco de dados por injeção de comandos nas consultas da aplicação.**

O risco está relacionado à possibilidade de entradas controladas pelo usuário serem utilizadas de forma insegura na construção de consultas ao banco de dados. Caso essas entradas sejam concatenadas diretamente ao comando SQL, um atacante pode modificar a estrutura da consulta originalmente planejada pela aplicação.

O risco R13 está relacionado às ameaças:

* **T05 — Tampering:** injeção de comandos SQL nos campos de busca de restaurantes e produtos;
* **I06 — Information Disclosure:** exposição de informações internas por meio de mensagens de erro detalhadas, facilitando a exploração de falhas como SQL Injection.

O R13 foi classificado anteriormente como **Crítico**, com:

* Probabilidade: **4 — Alta**
* Impacto: **4 — Muito alto**
* Pontuação: **16**

---

### 1.2. Requisito de segurança

**RS13 — O sistema deve tratar toda entrada fornecida pelo usuário como dado não confiável, validando-a antes do processamento e utilizando consultas parametrizadas em todas as operações realizadas no banco de dados. Entradas fornecidas pelo usuário não devem ser concatenadas diretamente em comandos SQL.**

Com esse requisito, pretende-se impedir que valores recebidos pela aplicação sejam interpretados pelo banco de dados como parte da estrutura de uma consulta.

---

### 1.3. Testes de Segurança

Os testes abaixo foram definidos **antes da implementação da solução**, estabelecendo previamente o comportamento esperado da aplicação para entradas legítimas e maliciosas.

| ID       | Tipo           | Entrada ou ação                                                                                                                                                              | Resultado seguro esperado                                                                                                                                                                                                                    |
| -------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **TS01** | Caso válido    | Cliente pesquisa por um restaurante utilizando o texto `Pizza` no campo de busca.                                                                                            | [A preencher]                           |
| **TS02** | Caso malicioso | Usuário informa no campo de busca uma entrada contendo caracteres e estrutura típica de tentativa de SQL Injection, com o objetivo de alterar a lógica da consulta original. | [A preencher|

### 1.3.1. TS01 — Pesquisa válida

**Objetivo:** verificar se uma entrada legítima continua sendo processada corretamente após a aplicação dos controles de segurança.

**Pré-condição:** existem restaurantes cadastrados contendo o termo `Pizza` em seu nome ou descrição.

**Entrada:**

```text
Pizza
```

**Ação:**

O cliente informa `Pizza` no campo de pesquisa de restaurantes.

**Resultado esperado:**

[A preencher]

**Critério de aprovação:**

O teste será considerado aprovado se a funcionalidade de pesquisa continuar funcionando normalmente utilizando a entrada como **dado da consulta**, e não como parte do comando SQL.

---

### 1.3.2. TS02 — Tentativa de SQL Injection

**Objetivo:** verificar se uma entrada maliciosa consegue alterar a estrutura da consulta executada pela aplicação.

**Pré-condição:** o campo de pesquisa de restaurantes aceita entrada fornecida diretamente pelo usuário.

**Ação:**

O usuário envia pelo campo de pesquisa um valor contendo elementos normalmente utilizados para tentar modificar a lógica de uma consulta SQL.

Exemplo de categoria da entrada testada:

```text
entrada contendo operadores, delimitadores ou comandos SQL inesperados
```

**Resultado esperado:**

[A preencher]

**Critério de aprovação:**

O teste será considerado aprovado se a tentativa não modificar o comportamento da consulta nem permitir acesso, alteração ou exclusão indevida de informações armazenadas no banco de dados.

---

### 1.4. Implementação

### 1.4.1 Validação de entrada

### 1.4.2 Consulta parametrizada


### 1.5 Resultado esperado da prática

---

### 1.6 Referências OWASP

---

## 2. Prática 2 — [Nome da prática]

### 2.1 Risco relacionado

---

### 2.2 Requisito de segurança

---

### 2.3 Testes de Segurança

| ID       | Tipo                          | Entrada ou ação | Resultado seguro esperado |
| -------- | ----------------------------- | --------------- | ------------------------- |
| **TS03** | Caso válido                   | [A definir]     | [A definir]               |
| **TS04** | Caso malicioso/não autorizado | [A definir]     | [A definir]               |

### 2.3.1 TS03 — [Nome]

### 2.3.2 TS04 — [Nome]

---

### 2.4 Implementação

---

### 2.5 Resultado esperado da prática

---

### 2.6 Referências OWASP

---

## 3. Rastreabilidade das Práticas Definidas

| Prática                                         | Ameaças  | Risco | Requisito | Testes     |
| ----------------------------------------------- | -------- | ----- | --------- | ---------- |
| Consultas parametrizadas e validação de entrada | T05, I06 | R13   | RS13      | TS01, TS02 |
| [Prática 2]                                     | [TXX]    | [RXX] | [RSXX]    | TS03, TS04 |

---

## 4. Considerações Finais

As práticas apresentadas nesta etapa demonstram como os riscos identificados durante a modelagem de ameaças e a análise de riscos podem ser transformados em requisitos verificáveis e controles concretos de implementação.

A definição dos testes antes da implementação permite estabelecer previamente qual comportamento é considerado seguro, aproximando o desenvolvimento do sistema de práticas de segurança orientadas a testes.

---

## 5. Distribuição Integrante X Responsabilidades

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | Definir dois testes para a Prática 2. |
| Emanuel Ferreira | Descrever resultado esperado e referência OWASP para a Prática 2. |
| Guilherme Mundt | Implementar a Prática 1 (código ou pseudocódigo). |
| Lívia Barbosa | Implementar a Prática 2 (código ou pseudocódigo). |
| Mariana Padilha | Definir dois testes para a Prática 1. |
| Matheus Ciocca | Descrever resultado esperado e referência OWASP para a Prática 1. |