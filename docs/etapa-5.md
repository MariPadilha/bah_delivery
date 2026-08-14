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
