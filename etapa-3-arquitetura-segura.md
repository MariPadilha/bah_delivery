# Etapa 3 — Arquitetura Segura

## RS01 — Proteção contra tentativas automatizadas de autenticação

**Origem:** S01 — Credential Stuffing

**Risco relacionado:** R01 — Comprometimento da conta de um cliente por obtenção indevida de suas credenciais.

### Requisito de Segurança

O Bah Delivery deve proteger o processo de autenticação contra tentativas
automatizadas e repetidas de acesso. O sistema deve limitar tentativas
excessivas de autenticação e aplicar mecanismos adicionais de verificação
de identidade em situações de risco, como tentativas repetidas de login
ou acesso a partir de dispositivo não reconhecido.

### Justificativa

A ameaça S01 considera que um atacante pode utilizar credenciais obtidas
em vazamentos de outros serviços para realizar tentativas de autenticação
em massa contra contas de clientes (*credential stuffing*).

Sem mecanismos que dificultem tentativas automatizadas de autenticação,
credenciais reutilizadas podem resultar no comprometimento de contas
legítimas. Isso pode permitir acesso ao histórico de pedidos, endereços
residenciais e outras informações associadas ao cliente, além da
realização de pedidos fraudulentos em nome da vítima.

### Vulnerabilidade Catalogada Correspondente

**CWE-307 — Improper Restriction of Excessive Authentication Attempts**

A CWE-307 representa situações em que um sistema não restringe
adequadamente a quantidade ou a frequência de tentativas de autenticação.

Essa fraqueza está relacionada ao cenário identificado em S01, pois a
ausência de limitação permite que um atacante realize um grande número
de tentativas de login utilizando credenciais obtidas anteriormente.

### Referência OWASP

**OWASP — Credential Stuffing Prevention Cheat Sheet**

A OWASP apresenta medidas para reduzir ataques de *credential stuffing*,
incluindo autenticação multifator, identificação de comportamentos
suspeitos e mecanismos destinados a dificultar tentativas automatizadas
de autenticação.

### Rastreabilidade

S01 → R01 → RS01 → CWE-307