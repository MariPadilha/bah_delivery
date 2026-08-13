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

A implementação parte de uma separação que o requisito RS02 estabelece: o comando SQL é definido pela aplicação e o valor informado pelo usuário é apenas dado. Toda a proteção decorre de manter essas duas coisas separadas em todo o caminho percorrido pela entrada.

O código está organizado em duas camadas, apresentadas nas subseções seguintes e correspondentes aos controles definidos no plano de tratamento da [Etapa 2](./etapa-2-riscos-nist.md#352-planos-por-risco):

1. **Validação de entrada (C82):** recusa o termo que não corresponda ao formato esperado, antes de qualquer acesso ao banco de dados;
2. **Consulta parametrizada (C81):** vincula o termo como parâmetro, de modo que ele seja recebido pelo banco como valor e nunca como parte da estrutura da consulta.

A ordem importa menos do que a independência entre as duas. A validação é **defesa adicional, e não a proteção principal**, conforme registrado em C82: ela pode ser flexibilizada sem que o sistema fique exposto, porque a garantia de que a entrada não altera a consulta vem da vinculação de parâmetros. É por isso que as duas camadas são implementadas, e não apenas a primeira.

### 1.4.1 Validação de entrada

A validação declara o que é aceito, e não o que é proibido. Uma lista de caracteres permitidos é preferível a uma lista de bloqueio porque não depende de prever todas as formas de escrever uma tentativa de injeção: o que não estiver descrito como aceitável é recusado.

```python
# 1. Vocabulario aceito no campo de busca (C82)

import re
import unicodedata

TAMANHO_MAXIMO_DO_TERMO = 60

# Letras, inclusive acentuadas, digitos, espaco e hifen.
# Aspas, ponto e virgula, parenteses e marcadores de comentario ficam de fora.
CARACTERES_PERMITIDOS = re.compile(r"^[0-9A-Za-zÀ-ÖØ-öø-ÿ \-]+$")


class EntradaInvalida(Exception):
    """Recusa da entrada antes de qualquer acesso ao banco de dados."""


def validar_termo_de_busca(termo_recebido):
    """C82 — valida o termo pelo conjunto de caracteres aceitos e pelo tamanho.

    Defesa adicional, e nao a protecao principal: a garantia de que o termo
    nao altera a estrutura da consulta vem de C81, na secao seguinte.
    """
    if not isinstance(termo_recebido, str):
        raise EntradaInvalida("Termo de busca ausente.")

    termo = unicodedata.normalize("NFC", termo_recebido).strip()

    if not termo:
        raise EntradaInvalida("Termo de busca vazio.")

    if len(termo) > TAMANHO_MAXIMO_DO_TERMO:
        raise EntradaInvalida("Termo de busca acima do tamanho permitido.")

    if not CARACTERES_PERMITIDOS.match(termo):
        raise EntradaInvalida("Termo de busca com caractere nao permitido.")

    return termo
```

### 1.4.2 Consulta parametrizada

O comando SQL é declarado uma única vez, como constante, e o termo do usuário nunca é concatenado a ele. A entrada trafega exclusivamente na tupla de parâmetros, que o banco de dados interpreta como valor.

```python
# 2. Consulta com vinculacao de parametros (C81)

LIMITE_DE_RESULTADOS = 20   # C55 — limite maximo de registros por resposta

CONSULTA_DE_RESTAURANTES = """
    SELECT id, nome, categoria, nota_media
      FROM restaurantes
     WHERE situacao = 'aprovado'
       AND (nome LIKE ? OR categoria LIKE ?)
     ORDER BY nota_media DESC
     LIMIT ?
"""


def buscar_restaurantes(termo_recebido):
    """Busca restaurantes pelo termo informado pelo cliente.

    O comando e fixo no codigo. O termo viaja apenas na tupla de parametros,
    de modo que o banco o recebe como valor, e nao como estrutura.
    """
    termo = validar_termo_de_busca(termo_recebido)      # C82
    padrao = f"%{termo}%"                               # valor, nunca estrutura

    with repositorio.conexao() as conexao:              # C83 — privilegio minimo
        cursor = conexao.cursor()
        cursor.execute(
            CONSULTA_DE_RESTAURANTES,
            (padrao, padrao, LIMITE_DE_RESULTADOS),
        )
        return cursor.fetchall()


# 3. Ponto de entrada: mensagem generica ao cliente, detalhe apenas no registro (C84)

def rota_busca_de_restaurantes(requisicao):
    termo_recebido = requisicao.parametros.get("termo")

    try:
        restaurantes = buscar_restaurantes(termo_recebido)
    except EntradaInvalida as recusa:
        registrar_evento(
            "busca_recusada",
            motivo=str(recusa),
            origem=requisicao.origem,
        )
        return resposta(400, {"erro": "Termo de busca invalido."})
    except ErroDeBancoDeDados as falha:
        # C84 — nem a mensagem do banco nem o comando executado chegam ao cliente.
        # O registro interno alimenta a regra de alerta de C85.
        registrar_erro("falha_na_busca", excecao=falha,
                       consulta=CONSULTA_DE_RESTAURANTES)
        return resposta(500, {"erro": "Nao foi possivel concluir a busca."})

    return resposta(200, {"restaurantes": restaurantes})
```

**Observações:**

- O termo é recusado antes de a conexão com o banco ser aberta, de modo que uma tentativa de injeção com aspas ou ponto e vírgula não chega a produzir consulta alguma;
- Ainda que a lista de caracteres permitidos fosse ampliada, a estrutura da consulta permaneceria intacta, porque `cursor.execute` recebe o comando e os valores separadamente. É essa independência que sustenta a estratégia *evitar* adotada para R13;
- A lista de permitidos **não é suficiente sozinha**, e isso é deliberado: um termo como `1 UNION SELECT senha FROM usuarios` é composto apenas de letras, dígitos e espaços, e portanto atravessa a validação. Ele não produz efeito porque chega ao banco como valor de uma cláusula `LIKE`, e não como comando. O exemplo mostra por que C82 é registrado no plano como defesa adicional: quem impede a injeção é C81;
- A validação também produz recusas legítimas. Um estabelecimento chamado `Pão & Cia` seria rejeitado, porque `&` não consta do conjunto aceito. O ajuste é uma decisão de produto sobre quais caracteres um nome pode conter, e pode ser feito sem revisar a segurança da consulta;
- O caractere `%` do padrão de busca é acrescentado pela aplicação, e não aceito do usuário, para que o cliente não controle o alcance da correspondência;
- **TS01** exercita o caminho completo: o termo `Pizza` é aceito pela validação e vinculado como parâmetro, e a pesquisa continua funcionando normalmente. **TS02** é recusado na primeira camada, e a segunda permanece verificável de forma independente, por qualquer termo aceito pela lista de permitidos;
- A mensagem devolvida ao cliente é sempre genérica, tanto na recusa quanto na falha, o que trata a ameaça I06 sem revelar a estrutura da consulta;
- C83 é pressuposto pela implementação, mas não se realiza no código: a conta de banco utilizada pela aplicação é configurada na infraestrutura, sem permissão de alteração de estrutura, conforme o componente correspondente do diagrama da [Etapa 3](./etapa-3-arquitetura-segura.md#2-diagrama-da-arquitetura-segura).

---

### 1.5 Resultado esperado da prática

O resultado da prática é verificável pelos dois critérios que RS02 estabelece: entradas maliciosas não alteram a consulta executada, e entradas fora do formato esperado são recusadas antes do processamento. Cada um é observado por um teste distinto, e é essa separação que permite afirmar que a proteção não depende de uma única camada.

| Situação | Comportamento esperado | Camada que o produz | Teste que o evidencia |
|---|---|---|---|
| Termo legítimo (`Pizza`) | A pesquisa continua funcionando e devolve os restaurantes correspondentes, com o termo vinculado como valor da cláusula `LIKE` | C81 | TS01 |
| Termo com aspas, ponto e vírgula ou marcador de comentário | Recusa com resposta genérica, antes de a conexão com o banco ser aberta, e registro do evento `busca_recusada` | C82 | TS02 |
| Termo aceito pela validação, mas com intenção de injeção (`1 UNION SELECT senha FROM usuarios`) | Nenhum efeito sobre a estrutura da consulta: o valor chega ao banco como dado da cláusula `LIKE` e não retorna registro algum | C81 | TS01, por qualquer termo aceito |
| Falha do banco durante a execução | Resposta genérica ao cliente, com a exceção e a consulta registradas apenas internamente | C84 | Observação da §1.4 |

A terceira linha é a que sustenta a estratégia *evitar* adotada para R13. Se o resultado dependesse apenas da validação, ele valeria somente para o conjunto de caracteres hoje recusado, e qualquer flexibilização futura da lista, como a necessária para aceitar um nome como `Pão & Cia`, reabriria a exposição. Como o comando é declarado uma única vez e o termo trafega na tupla de parâmetros, a estrutura permanece intacta independentemente do que a validação aceite. É por isso que C82 é registrado no plano de tratamento como defesa adicional, e não como a proteção principal.

Quanto às ameaças de origem, T05 deixa de dispor do caminho que a torna explorável, porque não existe ponto em que o termo do usuário componha a estrutura do comando, e I06 perde a realimentação de que a exploração depende, já que nem a mensagem do banco nem a consulta executada chegam ao cliente.

Três limites permanecem, e registrá-los faz parte do resultado:

1. **A prática cobre a busca de restaurantes, e não o restante da aplicação.** A afirmação de que R13 foi eliminado só se estende aos demais pontos de acesso ao banco depois que o inventário de consultas dinâmicas previsto em C80 for concluído e cada ponto for convertido ao mesmo padrão.
2. **A eliminação não se sustenta sozinha ao longo do tempo.** Código escrito depois desta correção pode reintroduzir a concatenação. Quem preserva o resultado é a regra de análise estática decidida em [DA02](./etapa-3-arquitetura-segura.md#33-da02---acesso-ao-banco-restrito-a-uma-camada-com-consultas-parametrizadas), a ser posicionada no pipeline tratado na Etapa 7, cuja ausência de ocorrências é a evidência prevista para C81.
3. **C83 é pressuposto pela implementação, mas não se realiza no código.** O privilégio mínimo da conta de banco é configuração de infraestrutura, e é o que limita o alcance de uma falha remanescente.

Como o Bah Delivery não está implementado, o resultado descrito nesta seção é o comportamento esperado da prática, e não redução de risco já obtida. Conforme a estimativa de risco residual da [Etapa 2](./etapa-2-riscos-nist.md#37-risco-residual-esperado), a redução efetiva só pode ser afirmada após implementação, execução dos testes e obtenção das evidências.

---

### 1.6 Referências OWASP

| Referência | Trecho utilizado | Onde se materializa nesta prática |
|---|---|---|
| OWASP Top 10 2021, [A03:2021 Injection](https://owasp.org/Top10/A03_2021-Injection/) | A categoria em que a ameaça T05 se enquadra, e a orientação de que a defesa preferencial é manter os dados separados dos comandos, e não filtrar a entrada | Justifica a escolha da prática e a hierarquia adotada entre C81 e C82 |
| [SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html) | *Defense Option 1: Prepared Statements (with Parameterized Queries)*, apresentada como a defesa principal, e a ressalva de que listas de permitidos são defesa adicional | §1.4.2, na declaração da consulta como constante e na vinculação do termo (C81) |
| [Query Parameterization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Query_Parameterization_Cheat_Sheet.html) | Exemplos de vinculação de parâmetros e a recomendação de que o valor nunca componha o texto do comando | Uso de `cursor.execute` com a tupla `(padrao, padrao, LIMITE_DE_RESULTADOS)`, sem interpolação no SQL |
| [Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html) | Validação por lista de permitidos, com verificação de tipo, tamanho e formato, e a advertência de que a validação não substitui a parametrização | §1.4.1, em `validar_termo_de_busca` (C82) |
| [Error Handling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Error_Handling_Cheat_Sheet.html) | Mensagem genérica ao usuário, com o detalhe da exceção mantido apenas no registro interno | `rota_busca_de_restaurantes`, no tratamento de `ErroDeBancoDeDados` (C84), atendendo também a I06 |
| OWASP ASVS 4.0.3, V5.3.4 | "Verify that data selection or database queries (e.g. SQL, HQL, ORM, NoSQL) use parameterized queries, ORMs, entity frameworks, or are otherwise protected from database injection attacks" | Requisito verificável correspondente a C81, e o critério que TS02 exercita |
| OWASP ASVS 4.0.3, V5.1.3 e V5.1.4 | Validação por lista de permitidos e verificação de caracteres aceitos, tamanho e padrão | `CARACTERES_PERMITIDOS` e `TAMANHO_MAXIMO_DO_TERMO`, em C82 |
| OWASP ASVS 4.0.3, V7.4.1 | "Verify that a generic message is shown when an unexpected or security sensitive error occurs" | Respostas 400 e 500 da rota de busca, sem revelar a estrutura da consulta |

A vulnerabilidade catalogada correspondente, CWE-89, está registrada na [Etapa 3](./etapa-3-arquitetura-segura.md#12-vulnerabilidade-catalogada-correspondente) e é a mesma referenciada por V5.3.4 do ASVS, o que mantém a cadeia entre a ameaça T05, o risco R13, o requisito RS02 e a implementação desta seção.

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