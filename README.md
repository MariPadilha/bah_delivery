# Bah Delivery

Trabalho da disciplina de **Engenharia de Software Seguro**.

O Bah Delivery é uma plataforma web voltada para o gerenciamento integrado de pedidos e entregas de refeições, conectando diferentes perfis de usuário em um único ambiente digital. O sistema permite que clientes consultem cardápios, realizem pedidos e acompanhem o status das entregas, enquanto restaurantes podem administrar seus produtos, receber e organizar pedidos. Os entregadores possuem acesso às solicitações de entrega e informações necessárias para a execução do serviço, e os administradores são responsáveis pelo gerenciamento e controle geral da plataforma.

O sistema não é implementado: o objetivo do trabalho é analisar sua segurança, das ameaças ao pipeline de desenvolvimento.

---

## Entregáveis

| Etapa | Conteúdo | Documento | Situação |
|---|---|---|---|
| 1 | Casos de abuso e modelagem de ameaças com STRIDE | [docs/etapa-1-ameacas-stride.md](./docs/etapa-1-ameacas-stride.md) | Concluída |
| 2 | Análise, priorização e tratamento de riscos com o NIST CSF | [docs/etapa-2-riscos-nist.md](./docs/etapa-2-riscos-nist.md) | Em andamento |
| 3 | Projeto de uma arquitetura segura | — | Pendente |
| 4 | Código seguro e testes de segurança | — | Pendente |
| 5 | Verificação de vulnerabilidades | — | Pendente |
| 6 | Monitoramento e detecção de intrusões | — | Pendente |
| 7 | DevSecOps e vídeo final | — | Pendente |

---

## Integrantes

A tabela relaciona cada integrante ao nome que aparece no histórico de commits, permitindo identificar a autoria das contribuições.

| Integrante | Usuário do GitHub |
|---|---|
| Arthur Medeiros | Arthur Medeiros |
| Emanuel Ferreira | cwrricio |
| Guilherme Mundt | Guilherme Chagas |
| Lívia Barbosa | Lívia Barbosa |
| Mariana Padilha | MariPadilha |
| Matheus Ciocca | MatheusCiocca |

As responsabilidades de cada integrante são registradas na seção *Organização do Projeto* ao final do documento de cada etapa.

---

## Organização do Repositório

```text
bah_delivery/
├── README.md                             Página inicial do projeto
│
├── docs/
│   ├── etapa-1-ameacas-stride.md         Ativos, ameaças STRIDE e casos de abuso
│   └── etapa-2-riscos-nist.md            Registro, avaliação e tratamento de riscos
│
└── diagramas/
    └── etapa-1/
        ├── diagrama-casos-de-uso.png
        └── diagrama-de-contexto.png
```
