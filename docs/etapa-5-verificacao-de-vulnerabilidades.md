## Etapa 5 — Análise de Segurança com OWASP ZAP

Nesta etapa foi realizada uma análise de segurança utilizando o **OWASP ZAP (Zed Attack Proxy)** sobre a aplicação **OWASP Juice Shop**, disponibilizada especificamente para treinamento e testes de segurança em aplicações web.

### 1. Sistema e ambiente testado

O sistema submetido à verificação é o **OWASP Juice Shop**, aplicação web mantida pela OWASP e construída deliberadamente com vulnerabilidades para fins de treinamento. A escolha atende à condição estabelecida para esta etapa: o Bah Delivery é um sistema projetado, e não implementado, de modo que não existe alvo próprio para ser testado. O Juice Shop enquadra-se na terceira hipótese autorizada, a de aplicação deliberadamente vulnerável executada para fins educacionais, e sua licença e finalidade dispensam autorização adicional.

Nenhum sistema de terceiros foi alvo da verificação. A delimitação do escopo e a única exceção observada durante a execução estão registradas na seção 3.1.

A aplicação foi executada localmente em um container Docker, sem exposição para fora da máquina do integrante responsável pela execução:

| Item | Valor |
|---|---|
| Aplicação analisada | OWASP Juice Shop (imagem oficial `bkimminich/juice-shop`) |
| Forma de execução | Container Docker |
| Rede Docker | `security-lab`, criada para isolar o laboratório |
| Endereço no host | `http://localhost:3000` |
| Endereço interno da rede Docker | `http://juice-shop:3000` |
| Data da sessão | Noite de 13 de agosto de 2026, com requisições registradas entre 22h44 e 22h53 (horário local) |

A aplicação e a ferramenta foram colocadas na mesma rede Docker para que o ZAP alcançasse o Juice Shop pelo nome do container. Por isso o alvo informado ao ZAP é `http://juice-shop:3000`, e não `http://localhost:3000`: dentro do container da ferramenta, `localhost` designaria o próprio ZAP.

---

### 2. Ferramenta utilizada

A verificação foi conduzida com o **OWASP ZAP (Zed Attack Proxy) 2.17.0**, também executado em container Docker, conforme identificado na barra de título das capturas (`ZAP 2.17.0 (on aa43c1f6fe9c)`, onde o identificador corresponde ao container).

O ZAP opera como proxy de interceptação entre o navegador e a aplicação, o que lhe permite observar integralmente as requisições e respostas trocadas. Sobre esse tráfego atuam dois mecanismos distintos, e a diferença entre eles é relevante para a interpretação dos achados desta etapa:

| Mecanismo | Comportamento | Tráfego adicional gerado |
|---|---|---|
| Varredura passiva (*passive scan*) | Analisa as respostas do tráfego já observado, procurando cabeçalhos ausentes, cookies sem atributos de proteção, informações expostas e padrões inseguros. | Nenhum |
| Varredura ativa (*active scan*) | Envia requisições construídas pela ferramenta para exercitar condições de falha na aplicação. | Sim |

O proxy da ferramenta permaneceu na porta padrão `8080`, e a interface foi utilizada no **Standard Mode**.

---

### 3. Configuração do teste

Na tela **Quick Start > Automated Scan** do ZAP foram utilizados os seguintes parâmetros:

| Parâmetro | Valor utilizado |
|---|---|
| URL to attack | `http://juice-shop:3000` |
| Scan Policy | `Dev Standard` |
| Use traditional spider | Habilitado |
| Use modern spider | `Client Spider`, com `Firefox`, no modo `If Modern` |
| Modo da interface | Standard Mode |
| Proxy | `localhost:8080` |
| Autenticação | Nenhuma. A sessão utilizou o `Default Context` sem usuários configurados, de modo que a varredura observou apenas a superfície acessível sem autenticação |

A escolha da política `Dev Standard` é adequada ao objetivo da etapa: ela mantém o conjunto padrão de verificações e limita a intensidade da varredura, o que é suficiente para interpretar alertas sem submeter a aplicação a um volume elevado de requisições.

Os dois spiders cumprem papéis complementares. O spider tradicional percorre os endereços presentes no HTML retornado pelo servidor. O client spider executa a aplicação em um navegador Firefox real e descobre endereços gerados por JavaScript, o que é necessário no caso do Juice Shop, uma aplicação de página única cujas rotas não aparecem no HTML inicial.

A sessão produziu **34 alertas agrupados**, distribuídos pela ferramenta em 1 de risco alto, 12 de risco médio, 11 de risco baixo e 10 informativos. Os três achados analisados na seção seguinte foram selecionados a partir desse conjunto.

Nenhuma vulnerabilidade foi explorada. Não houve tentativa de obter acesso indevido, extrair dados ou manter presença na aplicação: o objetivo, conforme o enunciado, é interpretar os resultados apresentados pela ferramenta.

#### 3.1. Delimitação do escopo

O escopo pretendido da verificação é o host `juice-shop:3000` e apenas ele.

Essa delimitação não foi imposta por configuração. A sessão foi executada com o `Default Context` padrão, sem que o alvo fosse marcado como escopo (*Include in Context*) e sem que o *Automated Scan* fosse restringido a ele. Em consequência, o client spider seguiu links externos presentes nas páginas do Juice Shop, e a varredura passiva registrou alertas sobre respostas de hosts que não pertencem ao alvo, entre eles `github.com` e `www.reddit.com`, este último visível em uma das capturas de evidência.

Duas observações delimitam o alcance desse desvio:

1. Nenhum tráfego de ataque foi dirigido a esses hosts. Os alertas registrados são de origem passiva (`Source: Passive` no detalhe de cada alerta), isto é, resultam da leitura de respostas a requisições de navegação comuns, e não de requisições construídas pela ferramenta. A varredura ativa, que é a que geraria tráfego dessa natureza, não foi dirigida a eles.
2. A análise dos achados desta etapa considera exclusivamente alertas cuja URL pertence a `http://juice-shop:3000`. Alertas referentes a hosts externos são descartados por estarem fora do escopo, e não por serem falsos positivos.

Em uma repetição desta sessão, a correção é anterior à execução: incluir `http://juice-shop:3000` em um contexto e restringir a varredura a ele, de modo que os spiders não sigam para fora do alvo. O registro deste ponto é intencional, pois o uso de ambiente autorizado é condição da etapa e a delimitação do escopo é parte da configuração do teste, não um detalhe operacional.

---

### 4. Execução da análise

Inicialmente, o OWASP Juice Shop foi executado em um container Docker e disponibilizado localmente na porta `3000`.

O OWASP ZAP também foi executado em um container Docker conectado à mesma rede da aplicação. Dessa forma, o ZAP conseguiu acessar o Juice Shop utilizando o nome do container `juice-shop`.

Na interface do ZAP foi utilizada a funcionalidade **Automated Scan**, tendo como alvo:

```text
http://juice-shop:3000
```

---

### 5. Resultados

Foram selecionados três achados do conjunto descrito na seção 3, um por integrante responsável. A tabela abaixo segue o formato sugerido no enunciado e reúne, para cada achado, a evidência que o sustenta, a consequência possível, a categoria a que pertence e a medida proposta. O detalhamento de cada linha vem nas subseções seguintes.

| ID | Alerta ou achado | Evidência | Possível impacto | Relação com OWASP ou CWE | Correção proposta |
|---|---|---|---|---|---|
| A01 | *Em preenchimento, integrante 1* | | | | |
| A02 | *Em preenchimento, integrante 3* | | | | |
| A03 | **CSP: Failure to Define Directive with No Fallback.** O cabeçalho `Content-Security-Policy` da resposta não declara diretivas que não herdam de `default-src`. Risco **Médio** e confiança **Alta** atribuídos pelo ZAP; scanner passivo 10055, referência 10055-13 | `A03.png`, com `Parameter: Content-Security-Policy` e `Other Info: The directive(s): frame-ancestors is/are among the directives that do not fallback to default-src`. A URL do alerta é externa ao alvo, conforme a subseção 5.3.1. A mesma condição no alvo está em `A02.png`: a resposta de `http://juice-shop:3000/profile` traz `Content-Security-Policy: img-src 'self' /assets/public/images/uploads/default.svg; script-src 'self' 'unsafe-eval'`, sem `frame-ancestors`, `form-action`, `base-uri` nem `default-src` | Sem `frame-ancestors`, a página pode ser embutida por terceiro e usada para induzir cliques em ações autenticadas. Sem `form-action`, uma injeção de HTML redireciona a submissão de um formulário para servidor do atacante. Sem `default-src`, tudo o que não foi listado permanece irrestrito, de modo que a política deixa de valer como malha de fundo | CWE-693 (*Protection Mechanism Failure*), atribuída pelo ZAP, e CWE-1021 (*Improper Restriction of Rendered UI Layers or Frames*) para o efeito de enquadramento; WASC-15; A05:2021 – *Security Misconfiguration* | Declarar `default-src` e, explicitamente, `frame-ancestors`, `form-action`, `base-uri` e `object-src`; remover `'unsafe-eval'` de `script-src`; manter `X-Frame-Options` como defesa em profundidade; publicar primeiro em `Content-Security-Policy-Report-Only` e reexecutar a varredura com o escopo restrito ao alvo |

#### 5.1. A01 — Off-site Redirect

> **Em preenchimento.** Integrante 1.

#### 5.2. A02 — Absence of Anti-CSRF Tokens

> **Em preenchimento.** Integrante 3.

#### 5.3. A03 — CSP: Failure to Define Directive with No Fallback

**O que a ferramenta apontou.** O scanner passivo 10055 verifica se a política de segurança de conteúdo declara as diretivas que não herdam de `default-src`. Quando uma delas não é declarada, não há restrição alguma para o recurso correspondente, e é isso que a descrição do alerta resume: *"Missing/excluding them is the same as allowing anything"*. O ZAP classificou o achado como risco Médio com confiança Alta, e a confiança é alta porque a regra é objetiva — ela lê o cabeçalho devolvido e verifica a presença de um nome de diretiva, sem inferência sobre comportamento da aplicação.

##### 5.3.1. O alerta capturado está fora do alvo

A URL registrada no alerta é `https://www.reddit.com/r/owasp_juiceshop/`, host de terceiro que o client spider alcançou pelos links externos da aplicação, conforme já registrado na [seção 3.1](#31-delimitação-do-escopo). Pelo critério ali estabelecido, o alerta é descartado como resultado sobre o alvo.

O descarte é de contexto, e não de regra. A leitura do cabeçalho está correta e o alerta é verdadeiro a respeito do site que o devolveu; o que ele não é, é resultado desta verificação. Um achado sobre a configuração de outro site não descreve a aplicação analisada, e contá-lo seria atribuir ao alvo uma propriedade que não foi observada nele.

##### 5.3.2. A mesma condição é verificável no alvo, com a evidência já produzida

O descarte da URL não encerra o achado, porque a mesma regra aparece sete vezes na árvore de alertas da sessão, e a resposta do próprio alvo capturada em `A02.png` permite verificar a condição diretamente. O Juice Shop devolve, em `http://juice-shop:3000/profile`:

```text
Content-Security-Policy: img-src 'self' /assets/public/images/uploads/default.svg; script-src 'self' 'unsafe-eval'
```

A comparação entre os dois cabeçalhos mostra que a condição apontada pelo alerta não apenas existe no alvo, como é mais extensa nele:

| Diretiva que não herda de `default-src` | `www.reddit.com` (A03, fora do alvo) | `juice-shop:3000` (A02, no alvo) |
|---|---|---|
| `frame-ancestors` | Ausente — é a diretiva citada pelo alerta | Ausente |
| `form-action` | Presente, com `'self'` | Ausente |
| `base-uri` | Ausente | Ausente |
| `default-src`, que serve de malha de fundo às demais | Presente, com `'none'` | Ausente |

No host externo, a política declara `default-src 'none'` e falha em uma única diretiva. No alvo, não há malha de fundo alguma: as duas únicas diretivas declaradas são `img-src` e `script-src`, de modo que conexões, fontes, objetos, quadros e destino de formulário permanecem irrestritos, e `script-src` ainda admite `'unsafe-eval'`.

##### 5.3.3. Possível impacto

**Enquadramento em página de terceiro.** Sem `frame-ancestors`, nada na política impede que a aplicação seja carregada dentro de um quadro em site controlado por atacante, que sobrepõe elementos para induzir o usuário autenticado a acionar uma ação que ele não pretendia. No alvo o efeito está atenuado, porque a mesma resposta traz `X-Frame-Options: SAMEORIGIN` — atenuação parcial, e não equivalente: o cabeçalho legado não admite lista de origens permitidas e é o mecanismo que `frame-ancestors` substitui.

**Desvio da submissão de formulário.** Sem `form-action`, o destino de um `<form>` não é restringido pela política. Se houver injeção de HTML em qualquer página, o atributo `action` pode apontar para servidor externo, e a submissão — inclusive de credenciais — parte do navegador da vítima para o atacante. O ponto é concreto na página em que o alerta foi observado: `A02.png` mostra que `/profile` contém um formulário de upload com `method="post"`.

**Ausência de restrição residual.** Sem `default-src`, a política não tem comportamento padrão para o que não foi declarado, e `base-uri` ausente permite que uma injeção reescreva a base das URLs relativas da página. Somado ao `'unsafe-eval'` em `script-src` — que a própria sessão registrou em alerta separado —, o resultado é uma política que existe no cabeçalho mas cobre pouco do que deveria cobrir.

##### 5.3.4. O que o achado diz sobre o Bah Delivery

Nada diretamente, e o registro dessa ausência é parte da análise. A modelagem da [Etapa 1](./etapa-1-ameacas-stride.md) não contém ameaça de enquadramento ou de execução de conteúdo no navegador, e nenhum dos controles do [plano de tratamento da Etapa 2](./etapa-2-riscos-nist.md#352-planos-por-risco) trata de cabeçalhos de resposta: o mais próximo é **C89**, que exige TLS e transporte estrito para o risco R14, e cuida do trânsito, não da renderização.

A observação fica registrada aqui como lacuna percebida pela ferramenta, e não como alteração do registro de riscos, pela razão dada na seção de limitações — a aplicação analisada não é o Bah Delivery, e um achado obtido no laboratório não é evidência sobre a plataforma modelada. O que ele oferece é a pergunta que a [Etapa 6](../roteiros/etapa-6-deteccao-de-intrusoes.md#54-encerramento-e-realimentação) formaliza no encerramento de um alerta: existia controle que deveria ter impedido? Neste caso não existia, e é esse tipo de resposta que realimenta as etapas anteriores.

##### 5.3.5. Correção proposta

A correção é de configuração, aplicada no servidor de aplicação ou na borda, e consiste em declarar explicitamente as diretivas que não herdam:

```text
Content-Security-Policy:
  default-src 'self';
  script-src 'self';
  style-src 'self';
  img-src 'self' data:;
  connect-src 'self';
  object-src 'none';
  base-uri 'none';
  form-action 'self';
  frame-ancestors 'none';
```

Três observações acompanham a medida:

1. **`'unsafe-eval'` sai de `script-src`.** Mantê-lo preserva justamente a construção que a política existe para bloquear, e a diretiva passa a declarar uma restrição que não restringe.
2. **`X-Frame-Options` permanece.** Não porque seja suficiente, mas porque é o que responde em navegador que não implemente `frame-ancestors`. A política nova é o controle; o cabeçalho legado é a defesa em profundidade.
3. **A publicação é gradual.** A política entra primeiro como `Content-Security-Policy-Report-Only`, com coleta das violações relatadas, e só depois passa a bloquear. Política restritiva aplicada de uma vez quebra funcionalidade legítima, e o desfecho previsível é o afrouxamento apressado para `'unsafe-inline'`, que anula o ganho.

A verificação do resultado repete a sessão com o alvo incluído em contexto e a varredura restrita a ele, conforme a correção de configuração já prevista na seção 3.1. É a mesma execução que confirmaria, sem depender da inferência feita em 5.3.2, quais das sete instâncias da regra pertencem a `http://juice-shop:3000`.

##### 5.3.6. Limitações desta análise

O achado é de origem passiva e nenhuma tentativa de enquadramento ou de desvio de formulário foi executada — o que está demonstrado é a ausência das diretivas no cabeçalho, não a exploração dela. A relação estabelecida em 5.3.2 entre a regra e o alvo apoia-se na leitura do cabeçalho capturado em `A02.png`, e não na lista de URLs de cada uma das sete instâncias do alerta, que a captura disponível não detalha. E o impacto descrito em 5.3.3 é condicional: o desvio de submissão pressupõe uma injeção de HTML que esta sessão não verificou existir.

---

### 6. Evidências

As capturas de tela dos três achados identificados pelo OWASP ZAP foram armazenadas no diretório:

```text
evidencias/
└── etapa-5/
    ├── A01.png
    ├── A02.png
    └── A03.png
```

### Limitações e possíveis falsos positivos

Uma ferramenta automatizada devolve indícios, e não conclusões. Esta seção registra o que a execução descrita acima permite afirmar e o que ela não permite, porque tratar um alerta como vulnerabilidade confirmada custa tempo de correção em algo que talvez não exista, e tratar a ausência de alerta como ausência de falha custa mais caro ainda.

#### Limitações da análise

**A análise foi passiva.** Os três achados registrados têm origem `Passive` no próprio ZAP — os scanners 10028, 10202 e 10055. O scanner passivo observa as requisições e respostas que passaram pelo proxy e aponta padrões nelas; ele não envia carga de ataque nem confirma exploração. Nenhum dos achados foi validado por exploração manual, de modo que o que a execução produziu é uma lista de indícios a verificar, e não de falhas comprovadas.

**A cobertura é a do spider, e sem sessão autenticada.** Só foi analisado o que o spider alcançou a partir da página inicial. As áreas acessíveis apenas após autenticação não foram percorridas com sessão válida, e é justamente nelas que estariam as falhas de autorização — a mesma classe tratada pela [prática 2 da Etapa 4](./etapa-4-codigo-seguro-e-testes-seguranca.md#2-prática-2-autorização-no-servidor). A consequência é direta: a ausência de achado de controle de acesso nesta execução não é evidência de que ele esteja correto, e sim de que não foi exercitado.

**Parte do que foi percorrido está fora do alvo.** O alvo declarado foi `http://juice-shop:3000`, mas o spider seguiu links externos da aplicação, e o achado A03 refere-se à URL `https://www.reddit.com/r/owasp_juiceshop/`, que é host de terceiro. Um alerta levantado sobre o cabeçalho de outro site não diz nada sobre a aplicação analisada e não deveria ser contado no resultado. Vale também como cuidado operacional: a própria tela inicial do ZAP adverte que só se deve analisar aplicação para a qual se tem autorização, e limitar o escopo ao alvo é o que garante isso.

**A aplicação analisada não é o Bah Delivery.** O Juice Shop é deliberadamente vulnerável e mantido para treinamento. Os achados descrevem o laboratório, e não a plataforma modelada nas etapas anteriores. O que esta etapa demonstra é o uso da ferramenta e a leitura crítica de um achado, e nenhum resultado aqui altera o registro de riscos da [Etapa 2](./etapa-2-riscos-nist.md).

**O resultado é um instantâneo.** A execução usou o ZAP 2.17.0 com a política *Dev Standard*, em uma única passagem. Outra política, uma varredura ativa ou uma execução em outro momento produziriam um conjunto diferente de alertas, inclusive para a mesma aplicação.

**Volume de alertas não é medida de risco.** A execução acumulou 34 alertas, distribuídos pelo ZAP em 1 de risco alto, 12 médios, 11 baixos e 10 informativos. Vários são marcados como sistêmicos e repetem-se em muitas URLs — a mesma ocorrência de CSP aparece sete vezes, e a de cabeçalho ausente, em quase todas as páginas. Contar linhas da árvore de alertas superestima o problema; o que interessa é o número de causas distintas.

#### Achados que exigem confirmação

| Achado | Risco e confiança atribuídos pelo ZAP | Por que pode não se confirmar | Como confirmar |
|---|---|---|---|
| **A01** — Off-site Redirect (CWE-601) | Alto, confiança média | O alerta é levantado porque o parâmetro `to` aparece no cabeçalho de redirecionamento. O redirecionamento é funcionalidade legítima da aplicação, e o alerta não distingue destino validado de destino arbitrário | Repetir a requisição com destino não previsto pela aplicação e verificar se o redirecionamento se completa; se houver lista de domínios permitidos, o achado não se confirma |
| **A02** — Absence of Anti-CSRF Tokens (CWE-352) | Médio, confiança baixa | É o achado mais sujeito a falso positivo, e o próprio ZAP registra confiança baixa: o scanner procura nomes conhecidos de campo de token no formulário HTML e não enxerga proteções fora dele, como token enviado em cabeçalho, cookie `SameSite` ou autenticação por *Bearer* | Reenviar a submissão a partir de origem distinta, com a sessão da vítima, e observar se a operação se completa; se for recusada, a proteção existe em mecanismo que o scanner não inspeciona |
| **A03** — CSP: Failure to Define Directive with No Fallback (CWE-693) | Médio, confiança alta | A regra é objetiva e a leitura do cabeçalho não deixa margem, mas a URL do achado é `https://www.reddit.com/r/owasp_juiceshop/`. É falso positivo de contexto, e não de regra: o alerta está correto sobre um site que não é o alvo da análise | Refazer a execução com o escopo restrito ao alvo e verificar se a mesma diretiva aparece ausente no cabeçalho devolvido por `http://juice-shop:3000` |

#### O que se conclui

Dos três achados, um é objetivamente válido mas está fora do escopo (A03), um depende de verificação para distinguir funcionalidade de vulnerabilidade (A01) e um foi levantado com confiança baixa pela própria ferramenta (A02). Nenhum deles pode ser tratado como vulnerabilidade confirmada apenas com o que esta execução produziu.

É esse o motivo pelo qual, no pipeline proposto na Etapa 7, o resultado de uma varredura não bloqueia a continuidade por si só: o achado precisa antes de triagem, com responsável identificado e verificação registrada. Ferramenta automatizada é o que amplia o alcance da revisão; ela não substitui a decisão sobre o que é, de fato, um risco.
