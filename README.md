# Bah Delivery

Trabalho da disciplina de **Engenharia de Software Seguro**.

O Bah Delivery é uma plataforma web voltada para o gerenciamento integrado de pedidos e entregas de refeições, conectando diferentes perfis de usuário em um único ambiente digital. O sistema permite que clientes consultem cardápios, realizem pedidos e acompanhem o status das entregas, enquanto restaurantes podem administrar seus produtos, receber e organizar pedidos. Os entregadores possuem acesso às solicitações de entrega e informações necessárias para a execução do serviço, e os administradores são responsáveis pelo gerenciamento e controle geral da plataforma.

O sistema não é implementado: o objetivo do trabalho é analisar sua segurança, das ameaças ao pipeline de desenvolvimento.

---

## Entregáveis

| Etapa | Conteúdo | Documento | Situação |
|---|---|---|---|
| 1 | Casos de abuso e modelagem de ameaças com STRIDE | [docs/etapa-1-ameacas-stride.md](./docs/etapa-1-ameacas-stride.md) | Concluída |
| 2 | Análise, priorização e tratamento de riscos com o NIST CSF | [docs/etapa-2-riscos-nist.md](./docs/etapa-2-riscos-nist.md) | Concluída |
| 3 | Projeto de uma arquitetura segura | [docs/etapa-3-arquitetura-segura.md](./docs/etapa-3-arquitetura-segura.md) | Concluída |
| 4 | Código seguro e testes de segurança | [docs/etapa-4-codigo-seguro-e-testes-seguranca.md](./docs/etapa-4-codigo-seguro-e-testes-seguranca.md) | Concluída |
| 5 | Verificação de vulnerabilidades | [docs/etapa-5-verificacao-de-vulnerabilidades.md](./docs/etapa-5-verificacao-de-vulnerabilidades.md) | Concluída |
| 6 | Monitoramento e detecção de intrusões | [roteiros/etapa-6-deteccao-de-intrusoes.md](./roteiros/etapa-6-deteccao-de-intrusoes.md) | Concluída |
| 7 | DevSecOps e vídeo final | [roteiros/etapa-7-devsecops-e-video-final.md](./roteiros/etapa-7-devsecops-e-video-final.md) | Concluída |

A apresentação e o vídeo final são entregues fora do repositório e, por isso, não são versionados aqui.

---

## Integrantes

A tabela relaciona cada integrante ao nome que aparece no histórico de commits, permitindo identificar a autoria das contribuições.

| Integrante | Usuário do GitHub |
|---|---|
| Arthur Medeiros | medeiros08 |
| Emanuel Ferreira | cwrricio |
| Guilherme Mundt | doffyGC |
| Lívia Barbosa | liviavbarbosa |
| Mariana Padilha | MariPadilha |
| Matheus Ciocca | MatheusCiocca |

As responsabilidades de cada integrante são registradas em seção própria ao final do documento de cada etapa.

---

## Organização do Repositório

```text
bah_delivery/
├── README.md                                            Página inicial do projeto
│
├── docs/                                                Documentos das etapas 1 a 5 e as evidências da Etapa 5
│   ├── etapa-1-ameacas-stride.md                        Ativos, ameaças STRIDE e casos de abuso
│   ├── etapa-2-riscos-nist.md                           Registro, avaliação e tratamento de riscos
│   ├── etapa-3-arquitetura-segura.md                    Requisitos de segurança, vulnerabilidades
│   │                                                    catalogadas e arquitetura por zonas de confiança
│   ├── etapa-4-codigo-seguro-e-testes-seguranca.md      Práticas de código seguro, com os testes
│   │                                                    definidos antes da implementação
│   ├── etapa-5-verificacao-de-vulnerabilidades.md       Sessão do OWASP ZAP contra o Juice Shop, com a
│   │                                                    análise dos achados A01 a A03
│   │
│   └── evidencias/                                      Capturas de tela que sustentam os achados
│       └── etapa-5/                                     Evidências da varredura da Etapa 5
│           ├── .gitkeep                                 Mantém o diretório no versionamento
│           ├── A01.png                                  Achado A01 — redirecionamento para fora do site
│           ├── A02.png                                  Achado A02 — ausência de token anti-CSRF
│           └── A03.png                                  Achado A03 — diretiva de CSP sem fallback definido
│
├── roteiros/                                            Roteiros das etapas 6 e 7
│   ├── etapa-6-deteccao-de-intrusoes.md                 Eventos a registrar, regras de detecção e
│   │                                                    resposta a alerta
│   └── etapa-7-devsecops-e-video-final.md               Pipeline DevSecOps, condições que impedem a
│                                                        continuidade e roteiro do vídeo final
│
└── diagramas/                                           Diagramas citados pelos documentos
    ├── etapa-1/                                         Diagramas de contexto e de casos de uso
    │   ├── diagrama-casos-de-uso.png                    Perfis de usuário e operações da plataforma
    │   └── diagrama-de-contexto.png                     Fronteiras do sistema e agentes externos
    └── etapa-3/                                         Arquitetura segura por zonas de confiança
        ├── arquitetura-segura.mmd                       Arquivo-fonte do diagrama, em Mermaid
        └── arquitetura-segura.png                       Imagem renderizada a partir do fonte
```
