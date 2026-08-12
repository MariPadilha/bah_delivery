# Etapa 4 — Código Seguro e Testes de Segurança

Esta etapa apresenta práticas de implementação segura definidas a partir da  [Etapa 3 — Arquitetura Segura](./etapa-3-arquitetura-segura.md) do projeto Bah Delivery.

Para cada prática selecionada, os testes de segurança são definidos **antes da implementação**, de forma que o comportamento seguro esperado pelo sistema seja estabelecido previamente.

---

## Sumário

1. [Prática 1 — Consultas Parametrizadas e Validação de Entrada](#1-prática-1--consultas-parametrizadas-e-validação-de-entrada)
2. [Prática 2 — Autorização no servidor](#2-prática-2-autorização-no-servidor)
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

---

### 1.5 Resultado esperado da prática

---

### 1.6 Referências OWASP

---

## 2. Prática 2: Autorização no servidor

### 2.1. Risco relacionado

- **R11 — Obtenção indevida de privilégios administrativos.**

A prática busca garantir que as permissões sejam verificadas no servidor, impedindo que usuários executem ações administrativas ou acessem dados que estejam fora do escopo permitido para seu perfil.

---

### 2.2. Requisito de segurança

O sistema deve validar, no servidor, a identidade e o perfil do usuário antes de permitir o acesso a recursos protegidos. Usuários autenticados devem acessar somente os recursos e operações autorizados para seu perfil, impedindo a obtenção indevida de privilégios administrativos e o acesso a dados fora do escopo permitido.

---

### 2.3. Testes de Segurança

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

A implementação considera que o servidor é a única autoridade sobre a autorização, isto é, nenhuma decisão é delegada à interface, ao corpo da requisição ou ao conteúdo do token, e tudo que não estiver explicitamente permitido é negado.

O código está separado e organizado em quatro partes:
1. **Matriz de autorização:** declara quais operações são permitidas e sob qual condição de escopo para cada perfil;
2. **Resolução da identidade:** o perfil é obtido no servidor a partir da conta armazenada, e não do token nem da requisição;
3. **Função de autorização:** ponto único de decisão, com negação por padrão, aplicado antes de qualquer operação protegida;
4. **Operações protegidas:** os pontos de entrada usados pelos testes TS03 e TS04, que apenas executam a regra de negócio depois que a autorização foi concedida.

```python
# 1. Matriz de autorização (C100)

IRRESTRITO = "irrestrito"
PROPRIO_REGISTRO = "proprio_registro"
PROPRIO_ESTABELECIMENTO = "proprio_estabelecimento"
ENTREGA_ATRIBUIDA = "entrega_atribuida"

MATRIZ_DE_AUTORIZACAO = {
    "cliente": {
        "consultar_dados_do_cliente": PROPRIO_REGISTRO,
        "consultar_pedido": PROPRIO_REGISTRO,
        "criar_pedido": PROPRIO_REGISTRO,
    },
    "restaurante": {
        "gerenciar_produto": PROPRIO_ESTABELECIMENTO,
        "consultar_pedido": PROPRIO_ESTABELECIMENTO,
    },
    "entregador": {
        "consultar_entrega": ENTREGA_ATRIBUIDA,
    },
    "administrador": {
        "gerenciar_usuarios": IRRESTRITO,
        "gerenciar_restaurantes": IRRESTRITO,
    },
}

DONO_DO_RECURSO = {
    PROPRIO_REGISTRO: "cliente_id",
    PROPRIO_ESTABELECIMENTO: "restaurante_id",
    ENTREGA_ATRIBUIDA: "entregador_id",
}


# 2. Resolução da identidade e do perfil no servidor (C68)

ALGORITMO_DE_ASSINATURA = "HS256"  

def resolver_usuario_autenticado(token):
    """Devolve a identidade confiável do usuário ou recusa a requisição."""
    conteudo = verificar_assinatura(
        token,
        segredo=gerenciador_de_segredos.obter("sessao"),
        algoritmos_aceitos=[ALGORITMO_DE_ASSINATURA],
    )
    if conteudo is None or sessao_revogada(conteudo["id_sessao"]):
        raise PermissionError("Sessão inválida.")

    conta = repositorio_de_contas.buscar_por_id(conteudo["id_usuario"])
    if conta is None or not conta.ativa:
        raise PermissionError("Sessão inválida.")

    return {
        "id": conta.id,
        "perfil": conta.perfil,
        "autenticado": True,
    }


def cadastrar_usuario(dados_da_requisicao):
    """C67 — o campo 'perfil' enviado na requisição é descartado.

    Todo cadastro nasce com o perfil 'cliente'. A elevação de perfil só
    ocorre pelo fluxo administrativo, que passa por executar_acao_administrativa.
    """
    campos_aceitos = ("nome", "email", "senha", "telefone")
    dados = {campo: dados_da_requisicao[campo]
             for campo in campos_aceitos
             if campo in dados_da_requisicao}

    return repositorio_de_contas.criar(**dados, perfil="cliente")


# 3. Função de autorização: ponto único de decisão, com negação por padrão (C66)

def pertence_ao_usuario(usuario, condicao, recurso):
    """C101 — verifica se o registro manipulado pertence ao usuário."""
    if condicao == IRRESTRITO:
        return True

    campo_do_dono = DONO_DO_RECURSO.get(condicao)
    if campo_do_dono is None or recurso is None:
        return False  # condição desconhecida ou recurso ausente: nega

    return recurso.get(campo_do_dono) == usuario["id"]


def autorizar(usuario, operacao, recurso=None):
    """Autoriza a operação ou interrompe a requisição com PermissionError.

    Deve ser chamada antes de qualquer leitura ou escrita de recurso protegido.
    A mensagem devolvida é sempre genérica, para não revelar a existência do
    registro nem a regra que provocou a recusa (relacionado à ameaça I06).
    """
    if not usuario or not usuario.get("autenticado"):
        registrar_auditoria("acesso_negado", usuario=None, operacao=operacao,
                            motivo="usuario_nao_autenticado")
        raise PermissionError("Acesso negado.")

    permissoes_do_perfil = MATRIZ_DE_AUTORIZACAO.get(usuario.get("perfil"), {})

    # Negação por padrão: operação ausente na matriz do perfil é recusada.
    if operacao not in permissoes_do_perfil:
        registrar_auditoria("acesso_negado", usuario=usuario, operacao=operacao,
                            motivo="operacao_fora_do_perfil")
        raise PermissionError("Acesso negado.")

    condicao_de_escopo = permissoes_do_perfil[operacao]
    if not pertence_ao_usuario(usuario, condicao_de_escopo, recurso):
        registrar_auditoria("acesso_negado", usuario=usuario, operacao=operacao,
                            motivo="recurso_fora_do_escopo")
        raise PermissionError("Acesso negado.")

    return True


# 4. Operações protegidas (pontos de entrada exercitados por TS03 e TS04)

def acessar_recurso(usuario, recurso):
    """Consulta de dados do cliente, restrita ao próprio registro."""
    autorizar(usuario, "consultar_dados_do_cliente", recurso)
    return recurso["dados"]


def executar_acao_administrativa(usuario, alvo=None):
    """Operação de gerenciamento, restrita ao perfil 'administrador'."""
    autorizar(usuario, "gerenciar_usuarios", alvo)
    registrar_auditoria("acao_administrativa", usuario=usuario,
                        operacao="gerenciar_usuarios", alvo=alvo)
    return repositorio_de_contas.aplicar_alteracao(alvo)


# 5. Uso na rota: a autorização acontece no servidor, não na interface

def rota_dados_do_cliente(requisicao):
    # O perfil eventualmente enviado em requisicao.corpo é ignorado (C67).
    usuario = resolver_usuario_autenticado(requisicao.cabecalhos["Authorization"])
    recurso = repositorio_de_clientes.buscar_por_id(requisicao.parametros["id"])

    try:
        return resposta(200, acessar_recurso(usuario, recurso))
    except PermissionError:
        return resposta(403, {"erro": "Acesso negado."})
```

**Observações:**
- A interface continua ocultando as opções indisponíveis a cada perfil, mas a recusa efetiva ocorre em `autorizar`, no servidor, mesmo que a rota seja invocada diretamente;
- O perfil enviado como campo da requisição nunca é lido, dado que a função `cadastrar_usuario` copia apenas os campos declarados e fixa o perfil `cliente`;
- O perfil usado na decisão vem de `repositorio_de_contas`, e não do token. A assinatura é verificada com algoritmo fixado no servidor, de modo que um token forjado ou com algoritmo alterado é recusado antes de qualquer verificação de permissão;
- A verificação de escopo em `pertence_ao_usuario` impede que um perfil legítimo alcance registros de outro cliente, estabelecimento ou entregador;
- Toda recusa gera registro de auditoria e devolve sempre a mesma mensagem genérica ao cliente.

---

### 2.5 Resultado esperado da prática

---

### 2.6 Referências OWASP

---

## 3. Rastreabilidade das Práticas Definidas

| Prática | Ameaças | Risco | Requisito | Testes |
|---|---|---|---|---|
| Consultas parametrizadas e validação de entrada | T05, I06      | R13   | RS02      | TS01, TS02 |
| Autorização no servidor                         | E01, E02, E03 | R11   | RS03      | TS03, TS04 |

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