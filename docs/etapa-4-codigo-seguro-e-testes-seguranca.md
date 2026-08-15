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

**RS02 — O sistema deve tratar toda entrada fornecida pelo usuário como dado não confiável, validando-a antes do processamento e utilizando consultas parametrizadas em todas as operações realizadas no banco de dados. Entradas fornecidas pelo usuário não devem ser concatenadas diretamente em comandos SQL.**

Com esse requisito, pretende-se impedir que valores recebidos pela aplicação sejam interpretados pelo banco de dados como parte da estrutura de uma consulta.

---

### 1.3. Testes de Segurança

Os testes abaixo foram definidos **antes da implementação da solução**, estabelecendo previamente o comportamento esperado da aplicação para entradas legítimas e maliciosas.

| ID       | Tipo           | Entrada ou ação                                                                                                                                                              | Resultado seguro esperado                                                                                                                                                                                                                    |
| -------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **TS01** | Caso válido    | Cliente pesquisa por um restaurante utilizando o texto `Pizza` no campo de busca.                                                                                            | A pesquisa é concluída normalmente e devolve os restaurantes correspondentes, com o termo vinculado como valor da consulta e nunca como parte de sua estrutura. |
| **TS02** | Caso malicioso | Usuário informa no campo de busca uma entrada contendo caracteres e estrutura típica de tentativa de SQL Injection, com o objetivo de alterar a lógica da consulta original. | A entrada é recusada antes de qualquer acesso ao banco, com resposta genérica e registro do evento `busca_recusada`. Ainda que fosse aceita pela validação, alcançaria o banco como valor e não alteraria a estrutura da consulta. |

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

A busca é concluída sem erro e devolve os restaurantes aprovados cujo nome ou categoria contenham `Pizza`, limitados aos vinte primeiros por ordem de nota média.

O termo é aceito por `validar_termo_de_busca`, porque é composto apenas de caracteres da lista de permitidos e está dentro do tamanho máximo, e alcança o banco vinculado como parâmetro da cláusula `LIKE` definida em `CONSULTA_DE_RESTAURANTES`. Nenhuma mensagem de erro é devolvida ao cliente e nenhum evento `busca_recusada` é registrado.

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

A entrada é recusada por `validar_termo_de_busca` antes de a conexão com o banco ser aberta, porque aspas, ponto e vírgula, parênteses e marcadores de comentário não constam da lista de caracteres permitidos. O cliente recebe a resposta genérica `Termo de busca invalido.`, sem qualquer detalhe sobre a consulta, sobre a exceção ou sobre o banco, e o evento `busca_recusada` é registrado com o motivo e a origem da requisição.

A recusa é o primeiro efeito, mas não é o que sustenta o teste. Mesmo que a lista de permitidos viesse a aceitar a entrada, ela continuaria alcançando o banco como valor da cláusula `LIKE`, e não como comando: a estrutura da consulta permanece a mesma, nenhum registro fora do escopo é devolvido e nenhuma escrita ocorre. É a independência entre as duas camadas, registrada em C81 e C82, que torna o resultado verificável sem depender do conjunto de caracteres hoje recusado.

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
| **TS05** | Caso malicioso/não autorizado | Cadastro enviando o campo `perfil` com o valor `administrador` no corpo da requisição | O servidor descarta o campo, a conta nasce com o perfil `cliente` e não alcança nenhuma operação administrativa |
| **TS06** | Caso malicioso/não autorizado | Requisição apresentando token forjado, expirado ou com o algoritmo de assinatura alterado | O servidor recusa a sessão antes de qualquer verificação de permissão, e o perfil usado na decisão vem sempre da conta armazenada |

RS03 estabelece três critérios verificáveis, e cada um corresponde a uma das ameaças de origem. TS03 e TS04 exercitam o primeiro deles — a decisão de autorização tomada no servidor, com negação por padrão, que responde a **E01**. Os dois testes seguintes cobrem os critérios restantes: TS05 responde a **E02**, o perfil recebido como campo da requisição, e TS06 responde a **E03**, o perfil obtido de um token cuja assinatura não é verificada. Os quatro exercitam o mesmo caminho — `resolver_usuario_autenticado` e `autorizar` —, mas em pontos distintos, porque as três ameaças alcançam a elevação de privilégio por rotas diferentes.

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

### 2.3.3 TS05 — Descarte do perfil enviado na requisição

**Objetivo:** verificar se o perfil informado no corpo da requisição de cadastro é ignorado pelo servidor, de modo que um usuário não consiga declarar-se administrador ao criar a própria conta. Corresponde à ameaça **E02** e ao controle **C67**.

**Pré-condição:** o endpoint de cadastro aceita o corpo da requisição enviado pelo cliente, sem que a interface restrinja quais campos podem ser incluídos.

```python
def test_cadastro_descarta_perfil_enviado_na_requisicao():
    dados_da_requisicao = {
        "nome": "Ana Souza",
        "email": "ana@exemplo.com",
        "senha": "senha-forte",
        "telefone": "51999990000",
        "perfil": "administrador",      # campo acrescentado pelo atacante
    }

    conta = cadastrar_usuario(dados_da_requisicao)

    # O campo nao consta de 'campos_aceitos' e nao chega ao repositorio.
    assert conta.perfil == "cliente"

    # A conta criada tambem nao alcanca a operacao administrativa.
    usuario = {"id": conta.id, "perfil": conta.perfil, "autenticado": True}

    try:
        executar_acao_administrativa(usuario)
        elevacao_obtida = True
    except PermissionError:
        elevacao_obtida = False

    assert elevacao_obtida is False
```

**Resultado esperado:** a conta é criada com o perfil `cliente`, porque `cadastrar_usuario` copia apenas os campos declarados em `campos_aceitos` e fixa o perfil no servidor. O valor `administrador` enviado na requisição não chega ao repositório de contas e não produz efeito algum.

A segunda parte do teste existe porque o primeiro `assert` verifica o estado da conta, e não a consequência dele. Ao invocar a operação administrativa com a conta recém-criada, o teste confirma que a elevação não ocorreu de fato: `autorizar` recusa por negação por padrão, com o motivo `operacao_fora_do_perfil`, e a recusa é registrada em auditoria.

**Critério de aprovação:** o teste será considerado aprovado se a conta nascer com o perfil `cliente` independentemente do valor enviado e se a operação administrativa for recusada com `PermissionError`.

### 2.3.4 TS06 — Recusa de token forjado, expirado ou com algoritmo alterado

**Objetivo:** verificar se um token cuja assinatura não é válida é recusado antes de qualquer verificação de permissão, e se o perfil usado na decisão de autorização vem da conta armazenada, e não do conteúdo do token. Corresponde à ameaça **E03** e ao controle **C68**.

**Pré-condição:** `token_de_teste` é um auxiliar do próprio teste, que monta um token com o identificador, o perfil, o segredo, a validade e o algoritmo informados, permitindo produzir cada variante recusada sem depender do fluxo de login.

```python
def test_token_invalido_e_recusado_antes_da_autorizacao():
    tokens_recusados = {
        "assinatura_forjada": token_de_teste(
            id_usuario=10, perfil="administrador", segredo="segredo-errado"),
        "expirado": token_de_teste(
            id_usuario=10, perfil="cliente", validade_em_segundos=-1),
        "algoritmo_alterado": token_de_teste(
            id_usuario=10, perfil="administrador", algoritmo="none"),
    }

    for caso, token in tokens_recusados.items():
        try:
            resolver_usuario_autenticado(token)
            sessao_aceita = True
        except PermissionError:
            sessao_aceita = False

        assert sessao_aceita is False, caso


def test_perfil_vem_da_conta_e_nao_do_token():
    # Conta legitimamente cadastrada como 'cliente'.
    conta = repositorio_de_contas.criar(
        nome="Ana Souza", email="ana@exemplo.com", perfil="cliente")

    # Token com assinatura valida, mas com o perfil adulterado no conteudo.
    token = token_de_teste(id_usuario=conta.id, perfil="administrador")

    usuario = resolver_usuario_autenticado(token)

    assert usuario["perfil"] == "cliente"

    try:
        executar_acao_administrativa(usuario)
        elevacao_obtida = True
    except PermissionError:
        elevacao_obtida = False

    assert elevacao_obtida is False
```

**Resultado esperado:** nas três variantes do primeiro teste, `resolver_usuario_autenticado` interrompe a requisição com `PermissionError` e a mensagem genérica `Sessão inválida.`, sem consultar a matriz de autorização. A assinatura forjada e o algoritmo alterado são recusados por `verificar_assinatura`, que aceita apenas o algoritmo fixado em `ALGORITMO_DE_ASSINATURA` e o segredo obtido do gerenciador; o token expirado é recusado pela mesma verificação, antes da consulta ao repositório de contas.

O segundo teste cobre o caso que os três anteriores não alcançam: um token cuja assinatura é legítima, mas cujo conteúdo declara um perfil que a conta não possui. Como `resolver_usuario_autenticado` devolve o perfil lido de `repositorio_de_contas`, e não o que consta do token, a identidade resolvida permanece `cliente` e a operação administrativa é recusada. É essa leitura que torna o perfil declarado no token irrelevante para a decisão.

**Critério de aprovação:** o teste será considerado aprovado se as três variantes de token inválido forem recusadas antes de qualquer verificação de permissão e se o perfil resolvido corresponder ao da conta armazenada, ainda que o token declare outro.

---

### 2.4 Implementação

A implementação considera que o servidor é a única autoridade sobre a autorização, isto é, nenhuma decisão é delegada à interface, ao corpo da requisição ou ao conteúdo do token, e tudo que não estiver explicitamente permitido é negado.

O código está separado e organizado em quatro partes:
1. **Matriz de autorização:** declara quais operações são permitidas e sob qual condição de escopo para cada perfil;
2. **Resolução da identidade:** o perfil é obtido no servidor a partir da conta armazenada, e não do token nem da requisição;
3. **Função de autorização:** ponto único de decisão, com negação por padrão, aplicado antes de qualquer operação protegida;
4. **Operações protegidas:** os pontos de entrada usados pelos testes TS03 a TS06, que apenas executam a regra de negócio depois que a autorização foi concedida.

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


# 4. Operações protegidas (pontos de entrada exercitados por TS03 a TS06)

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

O resultado da prática é verificável pelos três critérios que RS03 estabelece: rota administrativa invocada por conta sem privilégio é recusada, o campo de perfil enviado na requisição é ignorado, e token com assinatura inválida ou algoritmo alterado é rejeitado. Cada critério corresponde a uma das ameaças de origem, e nenhum deles depende da interface: os três são decididos no servidor, em `autorizar` e em `resolver_usuario_autenticado`.

| Situação | Comportamento esperado | Camada que o produz | Teste que o evidencia |
|---|---|---|---|
| Cliente consulta o próprio registro | Acesso concedido: a operação consta da matriz do perfil e o campo `cliente_id` do recurso corresponde ao usuário autenticado | C100 e C101 | TS03 |
| Cliente invoca operação administrativa | Recusa com `PermissionError` e mensagem genérica, antes de qualquer leitura ou escrita, com registro `acesso_negado` e motivo `operacao_fora_do_perfil` | C66 | TS04 |
| Cliente autenticado solicita registro de outro cliente | Recusa pelo mesmo caminho, com motivo `recurso_fora_do_escopo`: o perfil está correto, mas o recurso não lhe pertence | C101 | `pertence_ao_usuario`, observação da §2.4 |
| Cadastro enviando `"perfil": "administrador"` no corpo | O campo é descartado por `cadastrar_usuario`, e a conta nasce com o perfil `cliente` | C67 | TS05 |
| Token forjado, expirado ou com algoritmo alterado | Recusa em `resolver_usuario_autenticado`, antes de qualquer verificação de permissão, porque o algoritmo é fixado no servidor e o perfil vem de `repositorio_de_contas` | C68 | TS06 |
| Token de assinatura válida declarando perfil que a conta não possui | O perfil resolvido é o da conta armazenada, de modo que o valor declarado no token não altera a decisão | C68 | TS06 |
| Operação nova exposta pela API sem entrada na matriz | Recusa por ausência: nenhum perfil a possui, e a negação por padrão a torna inacessível até que a matriz seja atualizada | C66 | TS04, pelo mesmo caminho |

A última linha é a que sustenta o resultado ao longo do tempo. Se a autorização fosse escrita rota a rota, a rota acrescentada amanhã ficaria exposta por esquecimento, e o comportamento seguro dependeria de alguém lembrar de verificar. Com a matriz declarada em um único lugar e a negação por padrão em `autorizar`, o esquecimento produz recusa, e não exposição. É a mesma inversão que a [decisão DA03](./etapa-3-arquitetura-segura.md#34-da03---autorização-como-ponto-único-de-decisão-no-servidor-com-negação-por-padrão) registra na Etapa 3.

A terceira linha é a que liga esta prática a R16. O perfil correto responde "quem é você" e não responde "isto é seu": um restaurante legítimo que altera o produto de outro atravessa qualquer verificação de papel. Por isso a condição de escopo acompanha cada operação da matriz, e `pertence_ao_usuario` é chamada mesmo quando o perfil já foi aceito.

Quanto às ameaças de origem, E01 deixa de ter caminho porque a interface não é mais ponto de restrição e a rota só executa depois que `autorizar` concedeu; E02 deixa de existir porque o perfil não é aceito como campo, e sim fixado no cadastro; e E03 perde efeito porque o papel usado na decisão é lido da conta armazenada, de modo que alterar o perfil dentro do token não muda o resultado, e o token com assinatura inválida é recusado antes.

Três limites permanecem, e registrá-los faz parte do resultado:

1. **A prática cobre as operações declaradas na matriz, e não a API inteira.** A afirmação de que R11 foi tratado só se estende às demais rotas depois que o inventário de permissões previsto em C65 e a matriz completa prevista em C100 estiverem concluídos, conforme a posição 5 da [ordem de implementação](./etapa-2-riscos-nist.md#36-ordem-de-implementação-dos-controles).
2. **Os três critérios de RS03 têm teste próprio, mas apenas em nível de unidade.** TS03 e TS04 cobrem a negação por padrão, TS05 o descarte do campo de perfil (C67) e TS06 a recusa do token e a resolução do perfil na conta armazenada (C68). Os quatro exercitam as funções `autorizar`, `cadastrar_usuario` e `resolver_usuario_autenticado` diretamente, e não a rota que as chama: o que resta verificar é que cada rota exposta pela API efetivamente as invoque, o que só o teste dinâmico do [momento 6 do pipeline](../roteiros/etapa-7-devsecops-e-video-final.md#26-teste-dinâmico-ou-pentest) alcança.
3. **C69 e C70 não se realizam neste código.** A exigência de nova autenticação em alteração de permissões é fluxo administrativo completo, e o alerta de concessão de privilégio é regra de detecção, tratada na [Etapa 6](../roteiros/etapa-6-deteccao-de-intrusoes.md). O que este código entrega para ela é o registro `acesso_negado`, com rota, perfil resolvido e motivo.

Como o Bah Delivery não está implementado, o resultado descrito nesta seção é o comportamento esperado da prática, e não redução de risco já obtida. A [estimativa de risco residual da Etapa 2](./etapa-2-riscos-nist.md#37-risco-residual-esperado) mantém R11 em nível Médio mesmo com o plano executado, e a condição de aceitação ali registrada é exatamente a desta prática: toda rota administrativa coberta por teste de negação por padrão e papel resolvido no servidor.

---

### 2.6 Referências OWASP

| Referência | Trecho utilizado | Onde se materializa nesta prática |
|---|---|---|
| OWASP Top 10 2021, [A01:2021 Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) | A categoria em que E01, E02 e E03 se enquadram, e as orientações de negar por padrão e de aplicar o controle de acesso em código confiável do servidor, e não no cliente | Justifica a escolha da prática e a estrutura de `autorizar` como ponto único de decisão |
| [Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html) | *Enforce Least Privileges*, *Deny by Default* e a recomendação de validar a permissão em toda requisição, em vez de confiar em decisão tomada uma única vez | Negação por padrão na consulta à `MATRIZ_DE_AUTORIZACAO` e chamada de `autorizar` antes de cada operação protegida (C66) |
| [Insecure Direct Object Reference Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html) | Verificação de que o registro solicitado pertence ao usuário autenticado, e não apenas de que ele tem o papel adequado | `pertence_ao_usuario` e o mapa `DONO_DO_RECURSO` (C101), que tratam também R16 |
| [Mass Assignment Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mass_Assignment_Cheat_Sheet.html) | Uso de lista de campos aceitos na criação de registros, impedindo que atributos sensíveis sejam definidos pela requisição | `campos_aceitos` em `cadastrar_usuario`, com o perfil fixado no servidor (C67), correspondente a E02 |
| [JSON Web Token for Java Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_for_Java_Cheat_Sheet.html) | Fixação do algoritmo aceito no servidor, recusa de token cuja assinatura não seja verificada e uso de segredo forte sob custódia | `ALGORITMO_DE_ASSINATURA`, a lista `algoritmos_aceitos` e o segredo obtido do gerenciador em `resolver_usuario_autenticado` (C68), correspondente a E03 |
| OWASP ASVS 4.0.3, V4.1.1 | "Verify that the application enforces access control rules on a trusted service layer, especially if client-side access control is present and could be bypassed" | Decisão tomada em `autorizar`, no servidor, com a interface mantida apenas como ocultação de opções |
| OWASP ASVS 4.0.3, V4.1.3 e V4.1.5 | Princípio do menor privilégio por função e recurso, e exigência de que o controle de acesso falhe de modo seguro, inclusive diante de exceção | Condição de escopo por operação na matriz e recusa em qualquer caminho não previsto, inclusive condição desconhecida em `pertence_ao_usuario` |
| OWASP ASVS 4.0.3, V4.2.1 | "Verify that sensitive data and APIs are protected against Insecure Direct Object Reference (IDOR) attacks targeting creation, reading, updating and deletion of records" | Requisito verificável correspondente a C101, e o critério que a terceira linha da tabela de §2.5 exercita |
| OWASP ASVS 4.0.3, V5.1.2 | Proteção contra atribuição em massa de parâmetros, por marcação de campos ou contramedida equivalente | Cópia apenas dos campos declarados em `cadastrar_usuario` |

As vulnerabilidades catalogadas correspondentes — CWE-862 para a rota que não verifica o papel, CWE-915 para o perfil gravado a partir da requisição e CWE-347 para o token aceito sem verificação da assinatura — estão registradas na [Etapa 3](./etapa-3-arquitetura-segura.md#12-vulnerabilidade-catalogada-correspondente), uma para cada ameaça de origem. É o que mantém a cadeia entre as ameaças E01, E02 e E03, o risco R11, o requisito RS03, a decisão DA03 e a implementação desta seção.

---

## 3. Rastreabilidade das Práticas Definidas

| Prática | Ameaças | Risco | Requisito | Testes |
|---|---|---|---|---|
| Consultas parametrizadas e validação de entrada | T05, I06      | R13   | RS02      | TS01, TS02 |
| Autorização no servidor                         | E01, E02, E03 | R11   | RS03      | TS03, TS04, TS05, TS06 |

Na segunda prática, cada ameaça de origem tem teste correspondente: E01 é exercitada por TS03 e TS04, E02 por TS05 e E03 por TS06.

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