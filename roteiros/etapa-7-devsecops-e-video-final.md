# Etapa 7 - DevSecOps e Vídeo Final

Este roteiro descreve um pipeline em que a segurança acompanha o ciclo de desenvolvimento do Bah Delivery, e não é verificada apenas ao final. Cada momento reaproveita o que as etapas anteriores produziram: as ameaças da Etapa 1, os controles da Etapa 2, as decisões da Etapa 3, as práticas e testes da Etapa 4, a verificação da Etapa 5 e os eventos e regras da Etapa 6. O pipeline não é implementado, conforme o enunciado.

---

## Sumário

1. [Pipeline DevSecOps proposto](#1-pipeline-devsecops-proposto)
2. [Detalhamento dos momentos](#2-detalhamento-dos-momentos)
3. [Condições que impedem a continuidade](#3-condições-que-impedem-a-continuidade)
4. [Roteiro do vídeo final](#4-roteiro-do-vídeo-final)
5. [Distribuição Integrante X Responsabilidades](#5-distribuição-integrante-x-responsabilidades)

---

## 1. Pipeline DevSecOps proposto

| # | Momento | Atividade de segurança | Evidência produzida | Condição para continuar |
|---|---|---|---|---|
| 1 | Planejamento e análise de ameaças | Levantamento dos ativos e casos de abuso da Etapa 1, modelagem das ameaças com STRIDE e priorização dos riscos definidos na Etapa 2. O objetivo é identificar antecipadamente quais ameaças precisam ser tratadas e quais controles devem ser previstos antes da implementação | Registro dos ativos, casos de abuso e ameaças identificadas na Etapa 1, além da matriz de riscos e do plano de tratamento da Etapa 2 | Nenhum risco crítico pode permanecer sem estratégia de tratamento definida, e nenhuma ameaça relevante pode seguir sem controle ou decisão associada |
| 2 | Requisitos e decisões de arquitetura | Transformação dos riscos priorizados em requisitos de segurança e decisões de arquitetura. Os requisitos da Etapa 3 definem o comportamento esperado de segurança, enquanto as decisões arquiteturais determinam onde os controles serão aplicados, incluindo autenticação, autorização, acesso a dados e auditoria | Requisitos de segurança, vulnerabilidades catalogadas, decisões DA01 a DA03 e diagrama de arquitetura segura da Etapa 3 | Nenhum requisito de segurança associado a risco relevante pode seguir sem decisão arquitetural ou controle correspondente, e as decisões precisam estar registradas antes da implementação |
| 3 | Implementação segura | Aplicação das práticas da Etapa 4: validação de entrada e consultas parametrizadas no acesso ao banco (**C81**, **C82**), e autorização centralizada no servidor com negação por padrão, resolução do perfil a partir da conta armazenada e verificação do escopo do recurso (**C66**, **C68**, **C100**, **C101**) |  Código ou pseudocódigo que materializa os controles da Etapa 4, mantendo a consulta separada dos valores recebidos e concentrando a decisão de autorização em um único ponto no servidor | Nenhuma entrada controlada pelo usuário pode compor a estrutura de uma consulta; nenhuma operação protegida pode ser executada sem passar pelo ponto central de autorização; e o perfil utilizado na decisão não pode ser controlado pelo cliente |
| 4 | Testes automatizados |  Execução dos testes de segurança definidos antes da implementação na Etapa 4: **TS01** e **TS02** para a busca de restaurantes e **TS03** e **TS04** para autorização, complementados pelos testes ainda necessários para os critérios de RS03 não cobertos por TS03 e TS04  | Resultado da suíte automatizada, identificando os testes executados e seus resultados, incluindo evidência de pesquisa válida, tentativa de injeção tratada como dado, acesso legítimo permitido e operação não autorizada recusada | TS01 a TS04 devem ser aprovados e os critérios de RS03 relativos ao descarte do perfil enviado pelo cliente e à rejeição de token inválido devem possuir testes automatizados próprios antes da promoção |
| 5 | Análise de código e dependências | Execução das regras de análise estática exigidas por **C79** sobre o código submetido, inventário das dependências externas com verificação de vulnerabilidade conhecida, e busca por segredos no código e no histórico do repositório (**C46**) | Relatório datado da análise estática, com a relação das ocorrências e o responsável pela correção de cada uma (**C80**); inventário das dependências com a versão em uso e as vulnerabilidades conhecidas; resultado da busca por segredos; registro da triagem de cada achado, com o desfecho | Nenhuma ocorrência de concatenação de entrada do usuário em consulta e nenhuma decisão de autorização fora do ponto único previsto em DA03; nenhum segredo encontrado; nenhum achado de severidade alta sem triagem registrada e responsável identificado |
| 6 | Teste dinâmico ou pentest | Varredura da aplicação em execução no ambiente de homologação, sobre o mesmo artefato que será promovido, com escopo declarado e restrito ao alvo e com sessão autenticada de cada perfil, seguida da triagem de cada achado | Relatório da sessão com alvo, política, escopo, data e cobertura percorrida; capturas armazenadas em `evidencias/`; registro, por achado, do desfecho da triagem — falso positivo, comportamento esperado ou vulnerabilidade confirmada | Nenhum achado confirmado de severidade alta ou média em aberto, e cobertura mínima comprovada: as rotas de cada perfil precisam ter sido percorridas com sessão válida, porque ausência de achado sem cobertura não é aprovação |
| 7 | Implantação | Verificação da configuração que os controles pressupõem, promoção de artefato já aprovado nos momentos anteriores e registro da implantação como evento de auditoria | Registro da implantação com versão, autor e data; resultado da conferência de TLS, de privilégio mínimo do banco e de ausência de segredos; exercício de reversão concluído | Nenhum segredo presente no artefato ou no repositório, configuração conferida e reversão testada |
| 8 | Monitoramento e resposta | Coleta dos eventos definidos na Etapa 6, regras de alerta ativas, verificação diária da integridade do registro e procedimento de resposta com responsável e prazo | Alertas gerados nas simulações previstas nos controles de função *Detect*; relatório dos incidentes tratados; resultado da verificação da cadeia de resumos | Nenhum alerta crítico em aberto sem tratamento e geração de registro comprovadamente ativa |

---

## 2. Detalhamento dos momentos

### 2.1. Planejamento e análise de ameaças

O primeiro momento do pipeline ocorre antes de qualquer implementação. Seu objetivo é identificar o que precisa ser protegido, quais comportamentos indevidos precisam ser considerados e quais riscos podem comprometer o sistema. No Bah Delivery, esse trabalho parte dos ativos, dos casos de abuso e da modelagem STRIDE registrados na Etapa 1.

A modelagem de ameaças transforma situações genéricas de segurança em cenários concretos relacionados à plataforma. Dessa forma, autenticação, autorização, integridade dos pedidos, proteção de dados e disponibilidade deixam de ser apenas preocupações abstratas e passam a estar associadas a ameaças identificadas e rastreáveis.

Essas ameaças alimentam a análise de riscos da Etapa 2. Nela, os riscos são priorizados e recebem estratégias de tratamento e controles correspondentes. O planejamento, portanto, não termina na identificação da ameaça: ele precisa produzir uma decisão sobre como cada risco relevante será tratado.

A condição de continuidade deste momento é que nenhum risco crítico permaneça sem estratégia de tratamento definida e que as ameaças relevantes possuam controle ou decisão correspondente. Permitir que o pipeline avance sem essa definição significaria transferir para a implementação uma decisão que deveria ter sido tomada antes de existir código.

### 2.2. Requisitos e decisões de arquitetura

O segundo momento transforma o resultado do planejamento em propriedades verificáveis do sistema. Os riscos e controles definidos anteriormente são convertidos em requisitos de segurança e decisões de arquitetura, registrados na Etapa 3.

Os requisitos de segurança descrevem o comportamento que o Bah Delivery deve apresentar diante das ameaças identificadas. As decisões de arquitetura definem onde esses comportamentos serão garantidos e procuram concentrar controles críticos em pontos verificáveis do sistema.

Entre as decisões registradas na Etapa 3, destacam-se as que centralizam o acesso aos dados e a autorização. Essa concentração permite que as etapas seguintes verifiquem se o sistema realmente respeita as decisões tomadas: testes podem exercitar o ponto de autorização, e regras de análise estática podem identificar código que tente contornar o acesso definido pela arquitetura.

A arquitetura funciona, portanto, como ligação entre a análise de risco e a implementação. Um risco identificado no planejamento precisa resultar em requisito ou controle verificável; caso contrário, ele permanece apenas documentado, sem efeito sobre o desenvolvimento.

A condição de continuidade deste momento é que todo requisito de segurança associado a risco relevante possua uma decisão arquitetural ou controle correspondente e que essas decisões estejam registradas antes do início da implementação. Isso garante que as práticas de código seguro, os testes e as verificações dos momentos seguintes tenham uma referência objetiva do comportamento esperado.                                    

### 2.3. Implementação segura

O terceiro momento do pipeline transforma os requisitos e decisões de arquitetura das etapas anteriores em controles presentes no código. No Bah Delivery, esse momento reaproveita diretamente as duas práticas definidas na Etapa 4: **consultas parametrizadas com validação de entrada** e **autorização realizada no servidor**.

Na primeira prática, relacionada ao risco **R13**, a entrada fornecida pelo usuário é considerada não confiável e percorre duas camadas independentes de proteção. A validação (**C82**) verifica tipo, tamanho e formato antes do acesso ao banco, enquanto a consulta parametrizada (**C81**) mantém o comando SQL separado do valor recebido. A parametrização é a proteção principal: mesmo que um valor com intenção de injeção seja aceito pela validação, ele continua chegando ao banco exclusivamente como dado e não como parte da estrutura do comando.

A implementação também evita que detalhes internos do banco sejam devolvidos ao cliente. Entradas recusadas e falhas de execução produzem mensagens genéricas, enquanto as informações necessárias para auditoria permanecem nos registros internos. Dessa forma, a implementação trata não apenas a possibilidade de alteração da consulta associada a **T05**, mas também a exposição de informações internas relacionada a **I06**.

Na segunda prática, relacionada ao risco **R11**, a autorização é tratada exclusivamente como responsabilidade do servidor. A implementação utiliza uma matriz que declara as operações permitidas para cada perfil e aplica **negação por padrão**: uma operação que não esteja explicitamente autorizada é recusada.

O perfil utilizado nessa decisão não é aceito a partir do corpo da requisição nem confiado diretamente ao conteúdo do token. A identidade é resolvida pelo servidor e o perfil é recuperado da conta armazenada. Além de verificar o perfil, o controle considera o escopo do recurso, impedindo, por exemplo, que um cliente autenticado consulte o registro pertencente a outro cliente.

A função `autorizar` representa o ponto único de decisão e deve ser executada antes de qualquer leitura ou escrita de um recurso protegido. Essa centralização reduz a possibilidade de uma nova rota ser criada sem controle de acesso, pois operações não declaradas são recusadas por padrão.

A evidência deste momento é o código ou pseudocódigo que materializa esses controles. A condição para continuidade é que os valores recebidos permaneçam separados da estrutura das consultas e que toda operação protegida passe pelo mecanismo central de autorização utilizando identidade, perfil e escopo determinados pelo servidor.

### 2.4. Testes automatizados

O quarto momento verifica se as propriedades definidas para a implementação realmente apresentam o comportamento esperado. No Bah Delivery, os testes de segurança foram especificados **antes da implementação**, estabelecendo antecipadamente o que seria considerado um resultado seguro.

Para a prática de consultas parametrizadas e validação de entrada foram definidos **TS01** e **TS02**. O TS01 representa o caminho legítimo: o cliente pesquisa por `Pizza` e a funcionalidade deve continuar funcionando normalmente, com o termo utilizado apenas como valor da consulta. O TS02 representa uma tentativa de SQL Injection e verifica se uma entrada maliciosa é incapaz de modificar a estrutura ou o comportamento da consulta, acessar informações indevidas ou realizar alterações no banco.

A existência dos dois testes é importante porque segurança não significa simplesmente bloquear entradas. O sistema também precisa continuar executando corretamente operações legítimas. Além disso, a parametrização permanece verificável independentemente da validação: mesmo um termo que atravesse a lista de caracteres permitidos deve continuar sendo interpretado apenas como dado.

Para a autorização foram definidos **TS03** e **TS04**. O TS03 verifica o caminho permitido, no qual um usuário autenticado acessa um recurso pertencente ao seu próprio escopo. O TS04 verifica o caminho de negação, tentando executar uma operação administrativa com um perfil `cliente`. Nesse caso, o servidor deve recusar a operação antes da execução da ação protegida.

A própria Etapa 4 identifica, entretanto, uma limitação na cobertura atual: TS03 e TS04 verificam a autorização e a negação por padrão, mas ainda não exercitam diretamente dois outros critérios de **RS03**, o descarte de um campo `perfil` enviado pelo cliente durante o cadastro e a rejeição de um token forjado, expirado ou com algoritmo alterado. No pipeline DevSecOps, essa lacuna precisa ser transformada em testes automatizados adicionais, pois a leitura do código não substitui a evidência produzida pela execução desses casos.

Os testes são executados novamente a cada alteração relevante antes que o código avance para as verificações seguintes. Dessa forma, além de validar a implementação inicial, funcionam como testes de regressão: uma alteração futura que volte a permitir comportamento inseguro deve fazer a suíte falhar.

A evidência deste momento é o resultado registrado da suíte automatizada, indicando os casos executados e seus resultados. A continuidade exige a aprovação de **TS01 a TS04** e a cobertura automatizada dos critérios restantes de RS03. Uma falha devolve o código ao momento de implementação, em vez de permitir que uma propriedade de segurança não atendida avance para as etapas seguintes do pipeline.

### 2.5. Análise de código e dependências

O momento anterior executa o código e observa o comportamento; este lê o código e procura a construção proibida. A distinção não é formal. Um teste demonstra que a consulta que ele exercita está parametrizada, e nada afirma sobre as demais; uma regra de análise estática percorre o código inteiro e demonstra que nenhuma consulta concatena entrada do usuário. Cobertura por amostragem e cobertura por varredura respondem a perguntas diferentes, e a segunda é a única que sustenta uma afirmação sobre a totalidade do código.

O plano de tratamento já prevê a regra e já a trata como bloqueante: **C79** define o padrão de codificação que obriga o uso de consultas parametrizadas *"com regra de análise estática impedindo a integração de código que concatene entrada do usuário em consulta"*, e a evidência de **C81** é justamente uma nova execução da análise sem ocorrências. O inventário exigido por **C80** é o que dá sentido à primeira execução: sem a relação datada das consultas dinâmicas existentes, não há como afirmar que a exposição foi eliminada, apenas que não foi encontrada.

O conjunto de regras não se esgota nas consultas. As decisões da [Etapa 3](../docs/etapa-3-arquitetura-segura.md#4-considerações-finais) tornam verificáveis outras duas condições, e por um motivo que a própria etapa registrou: uma propriedade de segurança só é verificável quando existe um lugar onde ela pode ser observada. Como DA02 concentra o acesso ao banco em uma camada e DA03 concentra a autorização em uma função, é possível escrever regras que recusem a integração de código que acesse o banco fora da camada de dados ou que decida autorização fora do ponto único — inclusive a leitura do papel a partir do token, que **C68** proíbe expressamente. Se a arquitetura tivesse distribuído essas decisões, a regra não teria como distinguir a chamada legítima da indevida.

A verificação das dependências entra aqui por proximidade de momento, e não por já estar coberta. O plano exige o levantamento dos componentes críticos, *"inclusive dependências externas"* (**C52**), mas o exige sob a função *Identify* e com a finalidade de mapear pontos únicos de falha, o que é uma preocupação de disponibilidade. Nenhum controle do registro exige verificar vulnerabilidade conhecida nas versões em uso. A lacuna fica registrada aqui como observação — o inventário que este momento produz é o que a tornaria tratável —, sem alterar o registro de riscos da [Etapa 2](../docs/etapa-2-riscos-nist.md), que continua sendo o documento em que riscos são abertos.

A busca por segredos completa o momento e é a mesma evidência prevista em **C46**, que exige a busca no *histórico* do repositório, e não apenas na versão atual. A distinção é o que define a resposta: um segredo publicado não é corrigido pelo commit seguinte, porque permanece recuperável no histórico, e o que se aplica é a revogação de **C49** — resposta a incidente. A verificação equivalente reaparece no momento 7 sobre o artefato, e a repetição é deliberada: repositório e pacote publicado são superfícies distintas, e passar em uma não é passar na outra.

Por fim, a condição de continuidade não é a ausência de achados. Achado de ferramenta é indício, e a [Etapa 5](../docs/etapa-5-verificacao-de-vulnerabilidades.md#limitações-e-possíveis-falsos-positivos) mostrou o custo de tratá-lo como conclusão. O que impede a continuidade é achado de severidade alta **sem triagem registrada e sem responsável identificado**. A diferença é prática: a primeira formulação leva o time a desligar a regra que incomoda; a segunda obriga a decidir sobre cada achado e deixa a decisão no registro.

### 2.6. Teste dinâmico ou pentest

Este é o momento em que a [Etapa 5](../docs/etapa-5-verificacao-de-vulnerabilidades.md) deixa de ser sessão única e passa a ser passo recorrente. Ele exercita a aplicação em execução no ambiente de homologação, sobre o mesmo artefato que o momento 7 promoverá, e alcança o que a leitura do código não alcança: cabeçalhos de resposta, comportamento de sessão, configuração do servidor e tudo o que só existe depois que as partes estão montadas. O achado A03 daquela etapa é o exemplo — uma política de segurança de conteúdo incompleta não aparece em nenhuma linha do código-fonte.

Três decisões de execução vêm diretamente do que a Etapa 5 produziu, e cada uma corrige algo observado lá.

1. **O escopo é declarado antes da execução.** Na sessão da Etapa 5 o alvo não foi incluído em contexto, o spider seguiu links externos e a varredura registrou alertas sobre hosts de terceiros. No pipeline isso deixa de ser apenas ruído no relatório: dirigir requisições construídas pela ferramenta a sistema de terceiro sem autorização é o que a própria tela inicial do ZAP adverte. O alvo é incluído em contexto, a varredura é restrita a ele, e a execução ocorre em homologação — nunca em produção —, que é a condição que torna a varredura ativa admissível.
2. **A sessão é autenticada, e em todos os perfis.** Esta é a correção mais importante. A Etapa 5 percorreu apenas a superfície acessível sem autenticação, e é nas rotas autenticadas que estariam as falhas de autorização — a classe tratada por RS03 e pela [prática 2 da Etapa 4](../docs/etapa-4-codigo-seguro-e-testes-seguranca.md#2-prática-2-autorização-no-servidor). Sem sessão válida de cliente, restaurante, entregador e administrador, a ausência de achado de controle de acesso não significa que ele esteja correto, e sim que não foi exercitado. O teste inclui, para cada perfil, a tentativa de acessar recurso pertencente a outro titular e rota de perfil superior, que são os comportamentos de E01, E02 e E03.
3. **A triagem antecede a decisão.** Dos três achados analisados na Etapa 5, um era objetivamente válido mas referia-se a host fora do alvo e outro foi levantado com confiança baixa pela própria ferramenta. Um relatório sem triagem transforma volume de alertas em medida de risco, e as duas coisas não têm relação.

O momento também fecha um ciclo com o momento 4. Os testes TS01 a TS04 exercitam as práticas da Etapa 4 em nível de unidade, sobre a função; aqui os mesmos comportamentos são exercitados sobre o sistema em execução, por requisição HTTP. A diferença é o que está sendo verificado: lá, que a função recusa a entrada; aqui, que a rota que a chama efetivamente a chama.

A condição de continuidade tem duas partes, e a segunda costuma ser esquecida. A primeira é a usual: nenhum achado confirmado de severidade alta ou média permanece em aberto. A segunda é a cobertura — o relatório precisa declarar quais perfis e quais rotas foram percorridos e, sobretudo, **o que não foi**. Um relatório limpo obtido sobre uma superfície pequena é indistinguível de um relatório limpo obtido sobre a aplicação inteira, e apenas o segundo é aprovação. Declarar a cobertura é o que permite ler o resultado; sem isso, o momento produz confiança sem produzir verificação.

### 2.7. Implantação

A implantação é o ponto em que os controles deixam de ser código e passam a ser configuração. Boa parte do que a Etapa 2 definiu não vive no repositório: o privilégio mínimo da conta de banco (**C83**), o TLS 1.2 ou superior em toda a comunicação (**C89**), a criptografia em repouso do banco e dos backups e os segredos mantidos em gerenciador dedicado (**C46**), e a segregação do armazenamento de auditoria (**C30**). Um artefato aprovado em todos os momentos anteriores ainda pode ser implantado em um ambiente que não atende a nenhuma dessas condições, e nesse caso a aprovação anterior não significa nada.

Por isso este momento verifica ambiente, e não código. Três verificações são executadas antes da publicação:

1. **Ausência de segredos no artefato e no repositório**, que é a evidência já prevista em C46. Um segredo publicado não é corrigido por implantação nova: exige revogação, e a revogação é resposta a incidente, não manutenção.
2. **Conferência da configuração de que os controles dependem**: TLS na versão exigida, criptografia em repouso ativa, conta de aplicação sem permissão de alteração de estrutura e repositório de auditoria em modo apenas de acréscimo.
3. **Reversão testada**, para que a resposta a um incidente detectado no momento seguinte não dependa de um caminho nunca exercitado.

A promoção usa o mesmo artefato já verificado, sem reconstrução, para que a análise de código, os testes e o teste dinâmico digam respeito ao que efetivamente entrou em produção. A própria implantação é registrada como evento de auditoria, com autor, versão e data, o que permite relacionar uma anomalia observada em operação à mudança que a precedeu.

### 2.8. Monitoramento e resposta

Este momento consome o que a Etapa 6 definiu. Os eventos da relação daquele roteiro são coletados no armazenamento segregado, com o conteúdo mínimo de **C31**, e alimentam as regras de alerta distribuídas pelo plano de tratamento: cinco falhas de autenticação na mesma conta em dez minutos e autenticação a partir de origem não reconhecida (**C06**), reuso de token em duas origens na mesma janela (**C11**), concessão de privilégio administrativo (**C70**), ação sobre dados de pagamento e tentativa de escrita no repositório de auditoria (**C76**), e erro de sintaxe de consulta ou assinatura de injeção na borda (**C85**).

Duas exigências acompanham o alerta:

- **Cada regra tem responsável e resposta inicial definidos antes de disparar.** Alerta sem destinatário é registro, não detecção. As respostas já estão previstas nos controles correspondentes, como o bloqueio da origem e a desativação temporária do endpoint afetado (**C86**), a revogação de segredos comprometidos (**C49**) e a suspensão do administrador sob apuração (**C77**).
- **O mecanismo de registro é vigiado.** A verificação diária da cadeia de resumos (**C33**) alerta para lacuna na sequência de eventos ou falha na geração do registro. Sem ela, a ausência de alertas seria indistinguível de um sistema que parou de observar a si mesmo.

Este é o momento que fecha o ciclo, e não uma etapa final. O que a operação observa realimenta o planejamento: um alerta recorrente indica risco mal avaliado na Etapa 2, e um incidente sem evento correspondente indica lacuna na relação de eventos da Etapa 6. É também aqui que a estimativa de risco residual deixa de ser estimativa, porque passa a existir evidência de operação, e não apenas de teste.

---

## 3. Condições que impedem a continuidade

> **Em preenchimento.** Integrante 3, com pelo menos três condições.

---

## 4. Roteiro do vídeo final

O vídeo final não apresenta um sistema em funcionamento, porque o Bah Delivery não foi implementado — o que existe para apresentar é a análise, e é ela que o roteiro expõe. Filmar uma demonstração inexistente seria contradizer, na própria gravação, a premissa registrada desde a Etapa 1: o trabalho avalia a segurança de um projeto, não o comportamento de um software. O roteiro segue, por isso, o mesmo pipeline definido na seção 1, na mesma ordem em que os momentos aparecem ali, porque essa ordem já expressa a dependência entre eles — não se explica a análise de código antes de existir código a analisar, nem o monitoramento antes de existir o que monitorar.

Cada bloco é narrado por quem escreveu o conteúdo correspondente, e essa escolha não é protocolar. Quem redigiu uma seção sabe qual foi a dúvida por trás de cada decisão registrada nela — por que a condição de continuidade do momento 5 é a triagem, e não a ausência de achados; por que o A02 é o achado de menor confiança da Etapa 5 — e consegue explicar isso sem reler o texto em voz alta. Um narrador único, apresentando o trabalho de outra pessoa, reproduziria a redação sem o raciocínio, e o vídeo perderia exatamente o que o distingue do documento escrito. A exceção é a abertura e o encerramento, que não pertencem a nenhum momento isolado do pipeline e cabem a quem amarra o conjunto.

O apoio visual de cada bloco vem exclusivamente do que já foi produzido nas etapas anteriores — diagramas, tabelas e capturas de tela já existentes nos documentos —, pelo mesmo motivo que impede a demonstração ao vivo: gravar qualquer tela do Bah Delivery sugeriria a existência de um sistema que o trabalho, desde o início, declara não implementado.

### 4.1. Estrutura por blocos

| Bloco | Conteúdo | Apresenta | Apoio visual |
|---|---|---|---|
| Abertura | O que é o Bah Delivery, o que o trabalho analisa e a lógica do pipeline | Emanuel Ferreira | Sumário do README |
| Momentos 1 e 2 — Planejamento e arquitetura | Casos de abuso e ameaças STRIDE da Etapa 1; requisitos e decisões de arquitetura da Etapa 3 | Arthur Medeiros | Diagramas de `diagramas/etapa-1/` e `diagramas/etapa-3/` |
| Momentos 3 e 4 — Implementação e testes | Práticas de código seguro e testes definidos antes da implementação, na Etapa 4 | Mariana Padilha | Trechos de código/pseudocódigo da Etapa 4 |
| Momento 5 — Análise de código e dependências | Regras estáticas de C79, inventário de dependências, busca por segredos | Guilherme Mundt | Tabela da seção 2.5 |
| Momento 6 — Teste dinâmico e a sessão do ZAP | A varredura da Etapa 5 e as três correções que o pipeline aplica a ela | Guilherme Mundt | Capturas de `docs/evidencias/etapa-5/` |
| Achados A01 a A03 e suas limitações | O que cada achado do ZAP permite afirmar e o que não permite | Arthur Medeiros, Lívia Barbosa, Guilherme Mundt, Emanuel Ferreira | Tabela da seção 5 da Etapa 5 |
| Momentos 7 e 8 — Implantação e monitoramento | Verificação de ambiente; eventos e regras de detecção da Etapa 6 | Matheus Ciocca | Tabelas das seções 3.3 e 4 da Etapa 6 |
| Condições que impedem a continuidade | As condições que barram o pipeline antes da implantação | Lívia Barbosa | Seção 3 deste roteiro |
| Encerramento | Por que o momento 8 fecha um ciclo, e não uma etapa final | Emanuel Ferreira | — |

### 4.2. Abertura

A abertura tem uma única tarefa: situar quem assiste antes de qualquer detalhe técnico aparecer. Ela define o Bah Delivery pela descrição já usada no README — uma plataforma de pedidos e entregas com quatro perfis de usuário — e, na sequência imediata, declara a condição que rege todo o restante do vídeo: o sistema não foi implementado, e o que o grupo produziu é a análise de sua segurança, do levantamento de ameaças ao desenho do pipeline de desenvolvimento. Essa frase precede qualquer outra, porque sem ela um espectador que entrasse no vídeo a partir do momento 6, por exemplo, poderia interpretar as capturas do ZAP como evidência colhida do próprio Bah Delivery, quando na verdade pertencem ao Juice Shop, usado como alvo de treinamento na Etapa 5.

A abertura também expõe a lógica do pipeline antes de percorrê-lo bloco a bloco: cada momento reaproveita o que o anterior produziu, do mesmo modo como este roteiro reaproveita o texto já escrito nas seis etapas anteriores. É essa mesma ideia — nenhuma etapa se sustenta isolada da que veio antes — que o encerramento retoma ao final, fechando o vídeo com a mesma lógica com que ele abre.

### 4.3. Momentos 1 e 2 — Planejamento e arquitetura

Este bloco recupera o ponto de partida de todo o trabalho: os ativos e casos de abuso da Etapa 1, a modelagem STRIDE que os transforma em ameaças identificadas por código, e as decisões de arquitetura da Etapa 3 que respondem a parte delas — em particular DA02 e DA03, que concentram acesso a dados e autorização em pontos únicos, e que são citadas repetidamente nas etapas seguintes como o que torna certas regras de detecção e de análise estática possíveis de escrever. A apresentação usa o diagrama de contexto e o diagrama de casos de uso da Etapa 1, e o diagrama de arquitetura segura da Etapa 3, porque esses três diagramas comunicam em poucos segundos uma estrutura que o texto correspondente leva páginas para descrever, e é isso que a passagem para vídeo deve aproveitar.

### 4.4. Momentos 3 e 4 — Implementação e testes

Aqui o vídeo desce da arquitetura para o código: as duas práticas de código seguro definidas na Etapa 4 — consultas parametrizadas com validação de entrada, e autorização resolvida no servidor a partir do papel armazenado na conta — e os testes escritos antes da implementação para cada uma delas. O ponto que este bloco precisa deixar visível é a ordem: os testes foram definidos primeiro, e o pseudocódigo depois, o que é o próprio sentido de "testes de segurança" no título da etapa. Mostrar o trecho de pseudocódigo ao lado do teste que ele satisfaz é mais direto do que descrever os dois separadamente, e evita que o vídeo pareça apresentar a implementação como se fosse anterior à decisão de o que testar.

### 4.5. Momento 5 — Análise de código e dependências

Este bloco explica por que a análise estática é bloqueante e não apenas informativa: C79 exige que nenhuma consulta concatenando entrada do usuário seja integrada, e as decisões DA02 e DA03 do momento anterior são o que torna possível escrever regras equivalentes para acesso ao banco fora da camada de dados e para autorização fora do ponto único. A apresentação também cobre o inventário de dependências e a busca por segredos no histórico do repositório, e fecha no mesmo ponto em que a seção 2.5 fecha: a condição de continuidade não é a ausência de achados, é o achado de severidade alta sem triagem e sem responsável — distinção que evita que o time simplesmente desligue a regra que mais incomoda.

### 4.6. Momento 6 — Teste dinâmico e a sessão do ZAP

Este é o bloco com maior apoio visual do vídeo, porque é o único momento do pipeline com execução já registrada: a sessão do OWASP ZAP contra o Juice Shop, documentada na Etapa 5. A apresentação percorre as três correções que o pipeline aplica ao que aquela sessão expôs — escopo declarado e restrito ao alvo, porque o spider da Etapa 5 seguiu links externos sem essa restrição; sessão autenticada em todos os perfis, porque a varredura original cobriu apenas a superfície pública; e triagem antes da decisão, porque dois dos três achados exigiam confirmação antes de contar como vulnerabilidade. As capturas de `docs/evidencias/etapa-5/` ilustram diretamente esse último ponto, e o bloco seguinte aprofunda cada uma delas.

### 4.7. Achados A01 a A03 e suas limitações

Cada achado é apresentado por quem o analisou na Etapa 5, e a ordem segue a tabela da seção 5 daquele documento: A01, o redirecionamento aberto cuja evidência não prova destino arbitrário; A02, o alerta de menor confiança, levantado por um scanner que não enxerga proteção fora do HTML; e A03, tecnicamente correto mas capturado fora do escopo do alvo. O fio condutor deste bloco não é o achado em si, é o que ele ensina sobre ler resultado de ferramenta automatizada: nenhum dos três se sustenta como vulnerabilidade confirmada sem a verificação adicional que a própria Etapa 5 descreve, e é essa mesma leitura crítica que justifica, no momento 6, a exigência de triagem antes de qualquer decisão do pipeline.

### 4.8. Momentos 7 e 8 — Implantação e monitoramento

O bloco final do pipeline cobre dois momentos que não dependem do código, e sim do ambiente e da operação. A implantação verifica configuração — TLS, privilégio mínimo do banco, ausência de segredos no artefato — porque um artefato aprovado em todos os momentos anteriores ainda pode ser publicado num ambiente que não atende a nenhuma dessas condições. O monitoramento retoma os treze eventos e as três regras de detecção da Etapa 6, com ênfase na verificação diária da cadeia de resumos: sem ela, a ausência de alertas seria indistinguível de um sistema que parou de observar a si mesmo. As tabelas de eventos e de regras da Etapa 6 sustentam a apresentação sem necessidade de reconstruir os exemplos em tela.

### 4.9. Condições que impedem a continuidade

Este bloco expõe as condições definidas na seção 3 deste roteiro como o mecanismo que impede um pipeline aprovado por engano: cada momento anterior já declara sua própria condição de continuidade, e este bloco reúne o que, entre elas, é suficiente para travar a promoção do artefato — não uma lista nova, e sim a leitura conjunta do que já foi apresentado em cada momento.

### 4.10. Encerramento

O encerramento não resume o que foi dito nos blocos anteriores, porque um resumo apenas repetiria em voz mais rápida o que o espectador acabou de ouvir. Em vez disso, ele aponta para fora do vídeo: explica por que o momento 8 fecha um ciclo, e não uma etapa final. Um alerta recorrente, gerado pelas regras da Etapa 6, indica risco subavaliado no registro da Etapa 2. Um incidente sem evento correspondente indica lacuna na relação de eventos da própria Etapa 6. É esse retorno — da operação de volta ao planejamento — que a seção 2.8 já descreve como o que impede o monitoramento de virar coleta de registro sem consequência, e é com essa mesma ideia que o vídeo termina: a análise apresentada não se conclui na implantação, ela se realimenta a cada momento em que a operação encontra algo que o planejamento não previu.

---

## 5. Distribuição Integrante X Responsabilidades

| Integrante | Responsabilidades |
|------------|-------------------|
| Arthur Medeiros | Descrever os momentos de planejamento e análise de ameaças, e de requisitos e decisões de arquitetura. |
| Emanuel Ferreira | Elaborar o roteiro do vídeo final. |
| Guilherme Mundt | Descrever os momentos de análise de código e dependências, e de teste dinâmico ou pentest. |
| Lívia Barbosa | Definir as condições que impedem a continuidade do pipeline. |
| Mariana Padilha | Descrever os momentos de implementação segura e de testes automatizados. |
| Matheus Ciocca | Descrever os momentos de implantação, e de monitoramento e resposta. |
