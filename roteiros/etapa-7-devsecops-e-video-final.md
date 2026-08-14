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
| 5 | Análise de código e dependências | *Em preenchimento, integrante 4* | | |
| 6 | Teste dinâmico ou pentest | *Em preenchimento, integrante 4* | | |
| 7 | Implantação | Verificação da configuração que os controles pressupõem, promoção de artefato já aprovado nos momentos anteriores e registro da implantação como evento de auditoria | Registro da implantação com versão, autor e data; resultado da conferência de TLS, de privilégio mínimo do banco e de ausência de segredos; exercício de reversão concluído | Nenhum segredo presente no artefato ou no repositório, configuração conferida e reversão testada |
| 8 | Monitoramento e resposta | Coleta dos eventos definidos na Etapa 6, regras de alerta ativas, verificação diária da integridade do registro e procedimento de resposta com responsável e prazo | Alertas gerados nas simulações previstas nos controles de função *Detect*; relatório dos incidentes tratados; resultado da verificação da cadeia de resumos | Nenhum alerta crítico em aberto sem tratamento e geração de registro comprovadamente ativa |

---

## 2. Detalhamento dos momentos

> **Em preenchimento.** Momentos 1 a 6, a cargo dos integrantes 1, 2 e 4.

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
