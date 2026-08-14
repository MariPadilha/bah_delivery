# Etapa 6 - Monitoramento e Detecção de Intrusões

Este roteiro descreve como o Bah Delivery perceberia comportamentos suspeitos depois de entrar em operação. Ele não propõe a instalação de um sistema de detecção: parte dos controles de função *Detect* já definidos no [plano de tratamento da Etapa 2](../docs/etapa-2-riscos-nist.md#352-planos-por-risco) e os organiza em eventos, regras e resposta.

---

## Sumário

1. [O que é detecção de intrusões](#1-o-que-é-detecção-de-intrusões)
2. [A diferença entre prevenir e detectar](#2-a-diferença-entre-prevenir-e-detectar)
3. [Eventos que devem ser registrados](#3-eventos-que-devem-ser-registrados)
4. [Regras de detecção](#4-regras-de-detecção)
5. [O que acontece depois de um alerta](#5-o-que-acontece-depois-de-um-alerta)
6. [Distribuição Integrante X Responsabilidades](#6-distribuição-integrante-x-responsabilidades)

---

## 1. O que é detecção de intrusões

A detecção de intrusões consiste no processo de **monitorar eventos e comportamentos de um sistema com o objetivo de identificar atividades suspeitas ou potencialmente maliciosas**. Diferentemente de um mecanismo que simplesmente bloqueia uma ação, a detecção procura reconhecer padrões que possam indicar uma tentativa de ataque, abuso de privilégio ou comprometimento de uma conta.

No contexto do **Bah Delivery**, a detecção depende principalmente dos eventos registrados durante a utilização da plataforma. Tentativas de autenticação, recusas de autorização, alterações de privilégios, uso de tokens de sessão e operações administrativas são exemplos de informações que podem ser observadas para identificar comportamentos anormais.

Um único evento nem sempre representa um ataque. Uma falha de autenticação, por exemplo, pode ocorrer porque um usuário digitou sua senha incorretamente. Entretanto, várias falhas de autenticação em um curto intervalo de tempo podem indicar uma tentativa automatizada de acesso. Dessa forma, a detecção não considera apenas a ocorrência de um evento isolado, mas também seu **contexto, frequência e relação com outros eventos**.

Para que esse processo seja possível, os registros precisam conter informações suficientes para determinar **quem realizou a ação, quando ela ocorreu, qual foi sua origem e qual recurso foi afetado**. A partir desses dados podem ser definidas regras que reconheçam determinados padrões e gerem alertas para análise.

A detecção de intrusões, portanto, funciona como uma camada de observação do sistema. Seu objetivo não é afirmar automaticamente que todo comportamento incomum representa um ataque, mas fornecer informações que permitam identificar situações que precisam ser investigadas e, quando necessário, iniciar o processo de resposta ao incidente.

---

## 2. A diferença entre prevenir e detectar

**Prevenção** e **detecção** são estratégias complementares de segurança, mas atuam em momentos diferentes.

A prevenção procura **impedir que uma ação indevida seja realizada**. Controles de autenticação, autorização, validação de entradas e limitação de tentativas são exemplos de mecanismos preventivos. Quando funcionam corretamente, eles impedem ou dificultam que o ataque alcance seu objetivo.

A detecção, por outro lado, procura **perceber que um comportamento suspeito está ocorrendo ou ocorreu**. Para isso, utiliza eventos e registros produzidos pela aplicação e pela infraestrutura, permitindo identificar padrões que podem representar uma tentativa de ataque ou um incidente de segurança.

Por exemplo, no processo de autenticação do Bah Delivery, limitar tentativas excessivas de login é uma medida de **prevenção**, pois dificulta ataques automatizados. Registrar as falhas de autenticação e gerar um alerta quando uma mesma conta apresenta várias tentativas em um curto período é uma medida de **detecção**, pois permite reconhecer o comportamento suspeito.

Outro exemplo ocorre no controle de acesso. Impedir que um cliente acesse uma rota administrativa é prevenção. Registrar repetidas tentativas desse cliente de acessar recursos administrativos e gerar um alerta sobre esse comportamento é detecção.

A principal diferença pode ser resumida da seguinte forma:

| Prevenção                             | Detecção                                                     |
| ------------------------------------- | ------------------------------------------------------------ |
| Procura impedir a ação indevida       | Procura identificar a atividade suspeita                     |
| Atua antes ou durante a tentativa     | Atua durante ou após a ocorrência dos eventos                |
| Pode bloquear ou limitar uma operação | Pode registrar eventos e gerar alertas                       |
| Exemplo: negar acesso sem autorização | Exemplo: detectar várias tentativas de acesso não autorizado |
| Exemplo: limitar tentativas de login  | Exemplo: alertar sobre sucessivas falhas de autenticação     |

A existência de mecanismos preventivos não elimina a necessidade de detecção. Um controle preventivo pode falhar, ser contornado ou não reconhecer uma ação aparentemente legítima realizada por uma conta comprometida. Por isso, o Bah Delivery utiliza as duas abordagens de forma complementar: **prevenir sempre que possível e detectar comportamentos suspeitos para permitir uma resposta quando a prevenção não for suficiente**.

---

## 3. Eventos que devem ser registrados

Um evento só é útil à detecção se for possível responder três perguntas a partir dele: quem agiu, sobre o que agiu e se aquilo destoa do comportamento habitual. Por isso a escolha dos eventos não é feita pela facilidade de registrá-los, e sim pelas regras que eles precisam alimentar e pelos riscos que o registro precisa tornar visíveis.

### 3.1. Conteúdo mínimo de cada evento

Todo evento registrado segue o conteúdo mínimo definido em **C31**: autor, papel, data e hora, origem da requisição, recurso afetado e valores anterior e posterior, com mascaramento dos campos sensíveis. Sem esses campos, o registro descreve que algo aconteceu, mas não permite decidir se foi indevido, e nenhuma regra da seção 4 poderia ser escrita sobre ele.

Três condições acompanham o conteúdo e vêm do mesmo plano:

- **Onde o registro fica.** Armazenamento segregado do banco da aplicação, em modo apenas de acréscimo e com encadeamento por resumo criptográfico (**C30**), na zona de auditoria do [diagrama da Etapa 3](../docs/etapa-3-arquitetura-segura.md#2-diagrama-da-arquitetura-segura). Registro que o administrador pode apagar não sustenta detecção de abuso de privilégio.
- **Quem lê.** Apenas o papel de auditoria (**C47**), e não todo administrador.
- **Por quanto tempo.** Doze meses, conforme a política de auditoria (**C28**), que também define a relação de eventos de registro obrigatório.

### 3.2. O que não deve ser registrado

O registro não pode se tornar uma segunda cópia dos dados que a plataforma protege. Ficam de fora a senha, o token de sessão, os dados de pagamento e o corpo integral das requisições, com mascaramento aplicado aos campos sensíveis (**C31**, no tratamento de R08). Um registro completo demais desloca o risco em vez de reduzi-lo: passa a ser o alvo mais barato, e sob acesso mais amplo do que o do próprio banco.

### 3.3. Relação dos eventos

| # | Evento | Onde se origina | Risco relacionado | Por que é registrado |
|---|---|---|---|---|
| E1 | Falha de autenticação, com identificação da conta e da origem | Serviço de autenticação | R01 | É a matéria-prima da percepção de tentativas automatizadas, e só faz sentido em série: uma falha isolada é ruído, e a repetição é o sinal |
| E2 | Autenticação bem-sucedida a partir de dispositivo ou origem não reconhecidos | Serviço de autenticação | R01 | Distingue o acesso legítimo do acesso com credencial válida obtida indevidamente, que é o desfecho que a contagem de falhas não enxerga |
| E3 | Exigência, sucesso e recusa do segundo fator | Serviço de autenticação | R01 | Permite verificar se a decisão de DA01 está sendo aplicada nos perfis em que o segundo fator é obrigatório |
| E4 | Emissão, renovação, rotação e revogação de token de sessão, com dispositivo e origem | Serviço de autenticação | R02 | Sem esses eventos não é possível observar o mesmo token em duas origens na mesma janela, nem a reapresentação de token já rotacionado |
| E5 | Recusa de autorização, com a rota, o perfil resolvido no servidor e o recurso solicitado | Regras de autorização | R11, R16 | Uma recusa é o comportamento correto, mas a sequência de recusas contra rotas administrativas é tentativa de elevação de privilégio |
| E6 | Concessão ou alteração de papel administrativo, e o primeiro acesso a rota administrativa por conta recém-elevada | Regras de autorização e gestão de contas | R11 | É o par de eventos exigido por C70: a elevação legítima é rara e conhecida, então toda ocorrência merece verificação |
| E7 | Operação administrativa sensível: exclusão de registro, alteração de dados de repasse e acesso a dados de pagamento, com as aprovações envolvidas | API REST | R12 | Sustenta a dupla aprovação de C75 e permite reconstituir o que foi feito quando a apuração começa |
| E8 | Leitura do repositório de auditoria e tentativa de escrita ou exclusão nele | Zona de auditoria | R12 | Tentar apagar o registro é, por si só, o evento mais significativo do conjunto, e é o que C76 observa |
| E9 | Entrada recusada pela validação, com o motivo e a origem, e erro de execução de consulta | Validação e acesso a dados | R13, I06 | São os eventos `busca_recusada` e `falha_na_busca` já emitidos pelo código da [Etapa 4](../docs/etapa-4-codigo-seguro-e-testes-seguranca.md#142-consulta-parametrizada), e o erro de sintaxe é o rastro típico de uma tentativa de injeção em curso |
| E10 | Transição de estado do pedido ou da entrega fora da sequência esperada, e divergência entre o valor recebido e o recalculado | API REST | R04, R05 | Fraude operacional não produz erro técnico: ela aparece como sequência improvável, e não como falha |
| E11 | Acesso a registros pertencentes a outros titulares, contabilizado por conta | API REST | R07, R16 | Cada acesso pode ser legítimo; o volume em série é o que caracteriza a coleta indevida observada por C39 |
| E12 | Bloqueio ou limitação aplicada na borda, e saturação de recurso da plataforma | CDN com WAF e infraestrutura | R09, R17 | Separa a indisponibilidade causada por demanda legítima da causada por abuso, distinção que a resposta ao incidente exige |
| E13 | Falha na geração de registro e lacuna na cadeia de resumos | Zona de auditoria | R06 | É o evento que vigia o próprio mecanismo: a ausência de registro precisa gerar alerta, porque um registro que não foi gerado não pode ser reconstituído depois |

E13 tem natureza distinta dos demais e é a razão de C33 existir. Todas as regras da seção 4 pressupõem que os eventos cheguem ao repositório; se essa suposição falhar em silêncio, a plataforma passa a parecer segura exatamente quando deixou de observar a si mesma.

### 3.4. Cobertura das regras pelos eventos

Os treze eventos foram escolhidos de modo que as três regras da seção seguinte se apoiem em dados já previstos no plano de tratamento, sem exigir instrumentação nova. Os eventos que não alimentam nenhuma das três permanecem na relação porque são exigidos pela política de auditoria (C28) e sustentam a rastreabilidade de R06 e R12, que existe independentemente de haver alerta associado.

---

## 4. Regras de detecção

>> A seguir são apresentadas três regras de detecção, cada uma indicando o risco observado, a fonte de dados utilizada, a condição de alerta e a resposta inicial prevista.

| Risco observado | Fonte de dados | Condição de alerta | Resposta inicial |
|---|---|---|---|
| | **R01 — Comprometimento da conta de cliente por obtenção indevida de credenciais** | Eventos **E1** e **E2**. O evento E1 registra falhas de autenticação com identificação da conta e da origem; o evento E2 registra autenticação bem-sucedida a partir de dispositivo ou origem não reconhecidos. Ambos são gerados pelo serviço de autenticação | Duas condições independentes, e basta uma delas (**C06**): **(a)** cinco ou mais falhas de autenticação para a mesma conta em um intervalo de dez minutos; **(b)** autenticação bem-sucedida a partir de dispositivo ou origem não reconhecidos | Nenhuma ação humana imediata. A limitação progressiva prevista em **C03** atua automaticamente sobre as tentativas repetidas. Caso o padrão persista, a conta e a origem são encaminhadas para triagem da Segurança no mesmo dia. Se o comprometimento for confirmado, aplicam-se o bloqueio da conta e a revogação das sessões previstos em **C07** |
| **R11 — Obtenção indevida de privilégios administrativos** (Crítico, pontuação 12, segundo em prioridade no registro), originado das ameaças E01, E02 e E03 | Evento **E6**, nas duas formas exigidas por **C70**: a concessão ou alteração de papel administrativo, registrada pelo fluxo administrativo com a reautenticação de **C69**, e o primeiro acesso a rota administrativa por conta recém-elevada. Evento **E5** na forma em que a aplicação já o emite: o registro `acesso_negado` com a operação, o motivo `operacao_fora_do_perfil` e o perfil resolvido no servidor, produzido pela função `autorizar` da [prática 2 da Etapa 4](../docs/etapa-4-codigo-seguro-e-testes-seguranca.md#24-implementação). Em ambos, o papel que consta do evento é o lido da conta armazenada (**C68**), e não o declarado no token. Servem de referência do que é concessão esperada as aprovações nominais da política **C64** e o inventário de permissões vigentes de **C65** | Três condições independentes, e basta uma delas (**C70**): **(a)** concessão ou alteração de papel administrativo sem aprovação nominal correspondente registrada em C64, ou realizada por caminho diverso do fluxo administrativo, sem limiar; **(b)** primeiro acesso a rota administrativa por conta elevada nas últimas vinte e quatro horas, também sem limiar; **(c)** cinco ou mais `acesso_negado` com motivo `operacao_fora_do_perfil` contra rotas administrativas, partindo da mesma conta ou da mesma origem, em dez minutos | Nas condições (a) e (b), revogação dos privilégios obtidos e das concessões deles derivadas (**C71**) e encerramento de todas as sessões e tokens de renovação da conta (**C07**), em minutos e sem aguardar confirmação, por serem ações reversíveis; a verificação junto ao aprovador nomeado em C64 vem depois, e o privilégio legítimo é restabelecido pelo próprio fluxo administrativo. Na condição (c), nenhuma revogação e nenhum bloqueio de conta, porque nada foi obtido: sinalização da conta e da origem para triagem da Segurança no mesmo dia, com verificação de que as rotas alcançadas constam da matriz de **C100** |
| **R13 — Comprometimento da integridade do banco de dados por injeção de comandos nas consultas da aplicação** (Crítico, pontuação 16), originado das ameaças T05 e I06 | Eventos **E9**, nas duas formas em que a aplicação os emite: `busca_recusada`, produzido pela validação de entrada (**C82**), e `falha_na_busca`, produzido pelo tratamento de erro que mantém a exceção e a consulta apenas no registro interno (**C84**) — ambos já implementados na [prática 1 da Etapa 4](../docs/etapa-4-codigo-seguro-e-testes-seguranca.md#1-prática-1--consultas-parametrizadas-e-validação-de-entrada). Somam-se a eles os registros de assinatura de injeção reconhecida na borda, na CDN com WAF | Duas condições independentes, e basta uma delas (**C85**): **(a)** qualquer ocorrência isolada de `falha_na_busca` cujo detalhe registrado indique erro de sintaxe na consulta; **(b)** dez ou mais `busca_recusada` da mesma origem em cinco minutos, ou qualquer assinatura de injeção reconhecida na borda | Bloqueio da origem identificada, que é reversível e não depende de confirmação, e abertura da triagem para a Infraestrutura com prazo de até uma hora. A desativação temporária do endpoint afetado (**C86**) só ocorre depois de confirmada a triagem. O registro do período não é expurgado, porque é dele que dependem a apuração e a comparação com o backup previstas em **C87** |
**Sobre a regra 1.** As duas condições observam momentos diferentes do mesmo risco. A contagem de falhas de autenticação procura identificar uma tentativa de acesso ainda em andamento, enquanto a autenticação bem-sucedida a partir de dispositivo ou origem não reconhecidos cobre o cenário em que o atacante já possui uma credencial válida. Por isso, a regra utiliza os eventos **E1** e **E2** em conjunto, conforme previsto em **C06**.

A primeira condição utiliza um limiar porque uma falha isolada de autenticação é comum e não representa, por si só, atividade maliciosa. O limite de cinco falhas para a mesma conta em dez minutos transforma a repetição em um sinal de comportamento suspeito. A limitação progressiva de **C03** reduz automaticamente a velocidade das novas tentativas sem aplicar bloqueio permanente da conta.

A segunda condição não depende de uma sequência de erros. Uma autenticação bem-sucedida a partir de dispositivo ou origem não reconhecidos pode indicar uso de credenciais obtidas indevidamente, mas também pode ocorrer de forma legítima, como em uma troca de aparelho ou de rede. Por isso, o alerta não confirma sozinho o comprometimento da conta. A classificação é **Média**, com triagem pela Segurança no mesmo dia; caso o comprometimento seja confirmado, aplicam-se as ações de contenção previstas para R01.
**Sobre a regra 2.** A regra observa os dois lados de uma mesma tentativa: o privilégio que alguém obteve, nas condições (a) e (b), e o privilégio que alguém tentou usar sem ter, na condição (c). As duas primeiras dispensam limiar porque o evento é raro por decisão: a elevação exige o fluxo administrativo, a reautenticação de **C69** e a aprovação nominal de **C64**, de modo que C70 compara cada concessão com a relação de aprovações em vez de contar ocorrências. A condição (b) existe porque concessão e uso podem estar separados no tempo — um papel dormente atravessa a revisão trimestral como linha esperada do inventário, e é no primeiro acesso administrativo que ele volta a ser visível.

A condição (c) exige volume pelo motivo inverso. Uma recusa de autorização é o comportamento correto de **C66**, que nega por padrão, e acontece com usuário legítimo; recusa isolada é ruído, mas a sequência de recusas contra rotas administrativas é o mapeamento de quais rotas existem e quais respondem, que costuma preceder a elevação. A contagem é feita por conta **e** por origem porque as duas formas do ataque diferem: a conta única que insiste e a origem única que percorre contas distintas. A resposta acompanha essa distinção: em (a) e (b) houve efeito, e a contenção precede a confirmação, porque revogar privilégio e encerrar sessão são ações reversíveis, o que as coloca na classe **Imediata** da [seção 5.2](#52-classes-de-alerta-primeira-ação-e-prazo); em (c) nada foi obtido — as recusas são a prova de que o controle preventivo funcionou —, e o bloqueio automático de conta reproduziria a condição D06 já registrada em **C03**.

Três limites acompanham a regra. Ela não vê o abuso praticado por privilégio concedido legitimamente, que deixa de ser R11 e passa a ser R12, sob o alerta de **C76**; não vê o administrador cuja conta foi tomada, porque nesse caminho não há elevação a registrar e o sinal aparece antes, em E1, E2 e E4, sob **C06** e **C11**; e é silenciosa quando o perfil está correto e o escopo não, porque a recusa com motivo `recurso_fora_do_escopo` pertence a R16 e R07 e é observada por **C103** e **C39**. O último não é lacuna, e sim a mesma separação entre "quem é você" e "isto é seu" registrada na [prática 2 da Etapa 4](../docs/etapa-4-codigo-seguro-e-testes-seguranca.md#25-resultado-esperado-da-prática): contar os dois motivos juntos faria a regra tratar erro de escopo com a urgência de uma tentativa de elevação.

**Sobre a regra 3.** As duas condições existem porque os dois eventos têm qualidades de sinal opostas, e um limiar único trataria mal as duas.

A condição (a) dispensa limiar. A aplicação não produz consulta sintaticamente inválida em operação normal: a estrutura do comando é fixa e o valor do usuário chega vinculado como parâmetro, conforme **C81**. Um erro de sintaxe registrado é, portanto, indício de que alguma entrada alcançou a estrutura da consulta em algum ponto não previsto, e uma única ocorrência já merece verificação. É também o motivo pelo qual a mensagem detalhada não pode voltar ao cliente: o mesmo dado que sustenta a detecção é o que realimentaria a exploração descrita em I06.

A condição (b) exige volume, pelo motivo inverso. Uma recusa da validação é o comportamento correto e acontece com usuário legítimo — o exemplo de `Pão & Cia` registrado na Etapa 4 é exatamente isso. Recusa isolada é ruído; sequência de recusas da mesma origem em janela curta é tentativa de descobrir quais caracteres atravessam a lista de permitidos.

A regra tem uma limitação conhecida, e registrá-la é parte de defini-la: uma injeção que atravesse a validação sem provocar erro de sintaxe — como o termo `1 UNION SELECT senha FROM usuarios`, que é composto apenas de caracteres aceitos — não satisfaz nenhuma das duas condições. Ela não produz alerta porque também não produz efeito: chega ao banco como valor da cláusula `LIKE`, e não como comando. Quem impede o dano nesse caso é **C81**, e não esta regra. O ponto importa para a triagem: o silêncio desta regra é evidência de ausência de erro de execução, e não de ausência de tentativa.

---

## 5. O que acontece depois de um alerta

As regras da seção anterior terminam onde esta seção começa: no momento em que o alerta é gerado. O plano de tratamento da Etapa 2 já define, para cada risco, os controles de função *Respond* e *Recover*; o que falta descrever é o caminho entre uma coisa e outra — quem recebe o alerta, o que faz primeiro, em quanto tempo, e o que acontece quando o alerta se revela infundado.

### 5.1. Triagem: a primeira pergunta não é a gravidade

Antes de perguntar quanto um alerta é grave, é preciso perguntar se ele é verdadeiro. A análise da [Etapa 5](../docs/etapa-5-verificacao-de-vulnerabilidades.md#limitações-e-possíveis-falsos-positivos) mostrou isso na prática: dos três achados examinados, um estava correto mas se referia a um endereço fora do alvo, e outro foi levantado pela própria ferramenta com confiança baixa. Uma regra de detecção tem o mesmo comportamento — ela observa um padrão, não uma intenção.

Todo alerta chega acompanhado de quatro informações, que vêm dos eventos da seção 3 e sem as quais a triagem não é possível: a regra que disparou, os eventos que a satisfizeram, a conta e a origem envolvidas, e a resposta inicial já definida para aquela regra. A triagem tem três desfechos possíveis:

- **Falso positivo.** O limiar ou a condição da regra é ajustado, e o ajuste é registrado. Regra que alerta demais é desligada na prática, mesmo permanecendo ligada no papel: as pessoas param de ler.
- **Comportamento legítimo.** O padrão é real, mas esperado — uma integração, uma campanha, uma operação de manutenção. Registra-se a exceção com prazo de revisão, e não em caráter permanente, porque exceção sem prazo vira ponto cego.
- **Incidente confirmado.** Segue para a contenção descrita a seguir.

Os dois primeiros desfechos não são desperdício: são o que mantém a lista de alertas legível. O terceiro é o único que aciona a resposta.

### 5.2. Classes de alerta, primeira ação e prazo

A classe do alerta não é a mesma coisa que o nível do risco a que ele se refere. Ela responde a uma pergunta diferente: quanto tempo se pode esperar antes de agir sem que a janela de contenção se feche.

| Classe | Alerta e regra de origem | Primeira ação | Prazo de primeira resposta | Quem conduz |
|---|---|---|---|---|
| **Imediata** | Tentativa de escrita ou exclusão no repositório de auditoria (**C76**, evento E8) | Suspender o administrador envolvido e preservar a cópia segregada antes de qualquer outra medida (**C77**) | Minutos, sem esperar confirmação | Segurança |
| **Imediata** | Concessão de privilégio administrativo fora do fluxo aprovado, ou primeiro acesso administrativo de conta recém-elevada (**C70**, eventos E6 e E5) | Revogar os privilégios obtidos e as concessões derivadas, e encerrar as sessões da conta (**C71**, **C07**) | Minutos | Segurança e Backend |
| **Alta** | Erro de sintaxe de consulta ou assinatura de injeção na borda (**C85**, evento E9) | Bloquear a origem e desativar temporariamente o endpoint afetado até a verificação (**C86**) | Até uma hora | Infraestrutura |
| **Alta** | Mesmo token de sessão usado em duas origens na mesma janela, ou token de renovação reapresentado (**C11**, evento E4) | Revogar todas as sessões e tokens da conta (**C07**) | Até uma hora | Backend |
| **Alta** | Volume anômalo de leituras de registros de outros titulares (**C39**, evento E11) | Suspender a conta envolvida e bloquear o padrão de acesso identificado (**C40**) | Até uma hora | Segurança e Suporte |
| **Média** | Cinco falhas de autenticação na mesma conta em dez minutos, ou acesso de origem não reconhecida (**C06**, eventos E1 e E2) | Nenhuma ação humana imediata: a limitação progressiva de **C03** já atua sozinha. A ação humana só ocorre se o padrão persistir depois do atraso aplicado | Mesmo dia | Segurança |
| **Média** | Divergência repetida entre valor recebido e recalculado, ou transição de pedido fora da sequência (**C20**, **C25**, evento E10) | Sinalizar a conta para análise e congelar a entrega ou o pedido sob apuração (**C21**, **C26**) | Mesmo dia | Backend e Suporte |
| **Vigilância** | Lacuna na cadeia de resumos ou falha na geração de registro (**C33**, evento E13) | Restabelecer a geração do registro antes de qualquer outra apuração, e tratar o período da lacuna como não observado | Imediata, com prioridade sobre os demais | Infraestrutura e Segurança |

A última linha tem precedência sobre todas as outras, e a razão está na seção 3: enquanto o registro não estiver íntegro, o silêncio das demais regras não significa ausência de incidente. Apurar qualquer outro alerta com a cadeia rompida é trabalhar sobre dado que pode estar incompleto.

### 5.3. Da contenção à recuperação

A contenção interrompe o que está em curso; a recuperação recompõe o que foi alterado. As duas já estão definidas por risco no plano de tratamento, e o alerta apenas seleciona qual delas se aplica.

| Alerta confirmado | Contenção (*Respond*) | Recuperação (*Recover*) | Risco |
|---|---|---|---|
| Conta de cliente comprometida | Bloqueio da conta e revogação das sessões (**C07**) | Devolução do acesso ao titular com verificação de identidade e estorno dos pedidos do período (**C08**) | R01, R02 |
| Elevação indevida de privilégio | Revogação em cadeia das permissões concedidas (**C71**) | Reversão das alterações administrativas a partir do registro de auditoria (**C72**) | R11 |
| Abuso de privilégio administrativo | Suspensão do administrador e revisão de suas ações no período (**C77**) | Recomposição dos registros a partir da cópia segregada (**C78**) | R12 |
| Tentativa de injeção em curso | Bloqueio da origem e desativação do endpoint (**C86**) | Restauração a partir do backup e apuração das alterações entre o backup e a detecção (**C59**, **C87**) | R13 |
| Exposição de dados de clientes | Suspensão da conta e bloqueio do padrão de acesso (**C40**) | Apuração da extensão e comunicação aos titulares e à autoridade nos prazos legais (**C41**) | R07 |
| Indisponibilidade por abuso | Modo de proteção na borda e degradação controlada das funções não essenciais (**C58**) | Restauração pelo backup, com o tempo de recuperação registrado (**C59**) | R09, R17 |

Duas restrições valem para toda contenção, e nenhuma delas é detalhe de implementação.

**A contenção automática só é aceitável quando reversível.** O bloqueio permanente acionado por contagem é exatamente a condição D06, em que o mecanismo de proteção passa a ser o meio de indisponibilizar contas alheias. É a ressalva já registrada em **C03** e reaproveitada no tratamento de R10: atraso e desafio, sim; bloqueio definitivo automático, não.

**A contenção não pode apagar o rastro que a apuração vai usar.** O registro de auditoria é a fonte única da reconstituição prevista em C72, C78 e C87. Encerrar sessões, suspender contas e desativar endpoints são ações que preservam o registro; excluir dados da conta envolvida, não.

### 5.4. Encerramento e realimentação

Um alerta só é encerrado quando as quatro perguntas abaixo têm resposta registrada:

1. **Qual evento permitiu perceber?** Se nenhum evento da seção 3 corresponde ao que aconteceu, a relação de eventos tem lacuna e precisa ser ampliada.
2. **Existia controle que deveria ter impedido, e por que não impediu?** A resposta distingue controle ausente de controle presente e ineficaz, que exigem correções diferentes.
3. **O alerta chegou em tempo útil?** Um alerta correto que chega depois da consumação continua servindo à apuração, mas deixou de servir à contenção, e o limiar precisa ser revisto.
4. **Quanto ruído a regra produziu até aqui?** É o indicador que decide se a regra continua como está, é ajustada ou é substituída.

As respostas não ficam no incidente. Elas voltam para as etapas anteriores: um alerta recorrente indica risco subavaliado na [Etapa 2](../docs/etapa-2-riscos-nist.md#23-priorização-dos-riscos), ou condição de aceitação do risco residual que deixou de ser verdadeira; um incidente sem evento correspondente indica lacuna nesta etapa; e uma falha que passou por todas as verificações anteriores indica teste que faltava no pipeline da Etapa 7. É esse retorno que impede o monitoramento de virar coleta de registro sem consequência.

### 5.5. Três coisas que não podem acontecer

- **Alerta sem dono.** Regra cujo responsável não esteja definido antes de ela ser ligada produz apenas registro. A definição precede o disparo, e não o sucede.
- **Resposta automática irreversível.** Toda ação executada sem decisão humana precisa poder ser desfeita, porque ela também será executada nos falsos positivos.
- **Silêncio interpretado como normalidade.** A ausência de alertas tem duas causas possíveis, e elas são indistinguíveis sem a verificação diária de **C33**: ou não houve incidente, ou o sistema parou de observar a si mesmo.

---

## 6. Distribuição Integrante X Responsabilidades

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | Definir a regra de detecção 1. |
| Emanuel Ferreira | Descrever o que acontece depois de um alerta. |
| Guilherme Mundt | Definir a regra de detecção 3. |
| Lívia Barbosa | Definir a regra de detecção 2. |
| Mariana Padilha | Apresentar o conceito de detecção de intrusões e a diferença entre prevenir e detectar. |
| Matheus Ciocca | Definir os eventos do sistema que devem ser registrados. |
