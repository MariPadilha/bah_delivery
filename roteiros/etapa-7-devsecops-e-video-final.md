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
| 1 | Planejamento e análise de ameaças | *Em preenchimento, integrante 1* | | |
| 2 | Requisitos e decisões de arquitetura | *Em preenchimento, integrante 1* | | |
| 3 | Implementação segura | *Em preenchimento, integrante 2* | | |
| 4 | Testes automatizados | *Em preenchimento, integrante 2* | | |
| 5 | Análise de código e dependências | Execução das regras de análise estática exigidas por **C79** sobre o código submetido, inventário das dependências externas com verificação de vulnerabilidade conhecida, e busca por segredos no código e no histórico do repositório (**C46**) | Relatório datado da análise estática, com a relação das ocorrências e o responsável pela correção de cada uma (**C80**); inventário das dependências com a versão em uso e as vulnerabilidades conhecidas; resultado da busca por segredos; registro da triagem de cada achado, com o desfecho | Nenhuma ocorrência de concatenação de entrada do usuário em consulta e nenhuma decisão de autorização fora do ponto único previsto em DA03; nenhum segredo encontrado; nenhum achado de severidade alta sem triagem registrada e responsável identificado |
| 6 | Teste dinâmico ou pentest | Varredura da aplicação em execução no ambiente de homologação, sobre o mesmo artefato que será promovido, com escopo declarado e restrito ao alvo e com sessão autenticada de cada perfil, seguida da triagem de cada achado | Relatório da sessão com alvo, política, escopo, data e cobertura percorrida; capturas armazenadas em `evidencias/`; registro, por achado, do desfecho da triagem — falso positivo, comportamento esperado ou vulnerabilidade confirmada | Nenhum achado confirmado de severidade alta ou média em aberto, e cobertura mínima comprovada: as rotas de cada perfil precisam ter sido percorridas com sessão válida, porque ausência de achado sem cobertura não é aprovação |
| 7 | Implantação | Verificação da configuração que os controles pressupõem, promoção de artefato já aprovado nos momentos anteriores e registro da implantação como evento de auditoria | Registro da implantação com versão, autor e data; resultado da conferência de TLS, de privilégio mínimo do banco e de ausência de segredos; exercício de reversão concluído | Nenhum segredo presente no artefato ou no repositório, configuração conferida e reversão testada |
| 8 | Monitoramento e resposta | Coleta dos eventos definidos na Etapa 6, regras de alerta ativas, verificação diária da integridade do registro e procedimento de resposta com responsável e prazo | Alertas gerados nas simulações previstas nos controles de função *Detect*; relatório dos incidentes tratados; resultado da verificação da cadeia de resumos | Nenhum alerta crítico em aberto sem tratamento e geração de registro comprovadamente ativa |

---

## 2. Detalhamento dos momentos

> **Em preenchimento.** Momentos 1 a 4, a cargo dos integrantes 1 e 2.

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

> **Em preenchimento.** Integrante 6.

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
