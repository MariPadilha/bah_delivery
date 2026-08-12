# Etapa 4 — Código Seguro e Testes de Segurança

Esta etapa apresenta práticas de implementação segura definidas a partir da  [Etapa 3 — Arquitetura Segura](./etapa-3-arquitetura-segura.md) do projeto Bah Delivery.

Para cada prática selecionada, os testes de segurança são definidos **antes da implementação**, de forma que o comportamento seguro esperado pelo sistema seja estabelecido previamente.

---

## Sumário

1. [Prática 1 — Consultas Parametrizadas e Validação de Entrada](#1-prática-1--consultas-parametrizadas-e-validação-de-entrada)
2. [Prática 2 — Autorização no servidor](#2-prática-2--autorização-no-servidor)
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

## 2. ### Testes — Prática 2: Autorização no servidor

### 2.1 Risco relacionado

- **R11 — Obtenção indevida de privilégios administrativos.**
- **R16 — Acesso a dados de clientes e de estabelecimentos por identidade não verificada ou por perfil sem delimitação de escopo.**

A prática busca garantir que as permissões sejam verificadas no servidor, impedindo que usuários executem ações administrativas ou acessem dados que estejam fora do escopo permitido para seu perfil.

---

### 2.2 Requisito de segurança

O sistema deve validar, no servidor, a identidade e o perfil do usuário antes de permitir o acesso a recursos protegidos. Usuários autenticados devem acessar somente os recursos e operações autorizados para seu perfil, impedindo a obtenção indevida de privilégios administrativos e o acesso a dados fora do escopo permitido.

---

### 2.3 Testes de Segurança

| ID | Tipo | Entrada ou ação | Resultado seguro esperado |
| --- | --- | --- | --- |
| **TS03** | Caso válido | Usuário autenticado e autorizado solicita acesso a um recurso permitido para seu perfil | O servidor valida a autorização e permite o acesso ao recurso |
| **TS04** | Caso malicioso/não autorizado | Usuário comum tenta executar uma ação administrativa ou acessar dados fora do escopo permitido para seu perfil | O servidor nega a operação e não retorna os dados ou executa a ação protegida |

### 2.3.1 TS03 — Acesso autorizado conforme o perfil

**Objetivo:** verificar se um usuário autenticado e devidamente autorizado consegue acessar normalmente um recurso permitido para seu perfil.

```python
def test_usuario_autorizado_pode_acessar_recurso():
    usuario = {
        "id": 10,
        "perfil": "cliente",
        "autenticado": True
    }

    recurso = {
        "cliente_id": 10,
        "dados": "Dados do cliente"
    }

    resultado = acessar_recurso(usuario, recurso)

    assert resultado == "Dados do cliente"
```

**Resultado esperado:** o servidor deve validar a identidade e as permissões do usuário e permitir o acesso, pois o recurso solicitado está dentro do escopo autorizado para seu perfil.

### 2.3.2 TS04 — Bloqueio de acesso não autorizado

**Objetivo:** verificar se o servidor impede que um usuário obtenha privilégios administrativos ou acesse informações fora do escopo permitido para seu perfil.

```python
def test_usuario_nao_pode_acessar_recurso_sem_autorizacao():
    usuario = {
        "id": 10,
        "perfil": "cliente",
        "autenticado": True
    }

    try:
        executar_acao_administrativa(usuario)
        acesso_permitido = True
    except PermissionError:
        acesso_permitido = False

    assert acesso_permitido is False
```

**Resultado esperado:** o servidor deve negar a operação, pois um usuário com perfil `cliente` não possui autorização para executar ações administrativas. A mesma validação deve impedir o acesso a dados de clientes ou estabelecimentos que estejam fora do escopo autorizado para o usuário.

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