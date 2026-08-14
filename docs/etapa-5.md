## Etapa 5 — Análise de Segurança com OWASP ZAP

Nesta etapa foi realizada uma análise de segurança utilizando o **OWASP ZAP (Zed Attack Proxy)** sobre a aplicação **OWASP Juice Shop**, disponibilizada especificamente para treinamento e testes de segurança em aplicações web.

### Ambiente utilizado

A aplicação e a ferramenta de análise foram executadas utilizando **Docker**, em uma rede isolada destinada ao laboratório.

* **Aplicação analisada:** OWASP Juice Shop
* **Ferramenta:** OWASP ZAP
* **Tipo de análise:** Spider + Passive Scan
* **Ambiente:** Docker
* **Rede Docker:** `security-lab`
* **Endereço da aplicação no host:** `http://localhost:3000`
* **Endereço utilizado pelo ZAP:** `http://juice-shop:3000`

### Execução da análise

Inicialmente, o OWASP Juice Shop foi executado em um container Docker e disponibilizado localmente na porta `3000`.

O OWASP ZAP também foi executado em um container Docker conectado à mesma rede da aplicação. Dessa forma, o ZAP conseguiu acessar o Juice Shop utilizando o nome do container `juice-shop`.

Na interface do ZAP foi utilizada a funcionalidade **Automated Scan**, tendo como alvo:

```text
http://juice-shop:3000
```

### Resultados


### Evidências

As capturas de tela dos três achados identificados pelo OWASP ZAP foram armazenadas no diretório:

```text
evidencias/
└── etapa-5/
    ├── A01.png
    ├── A02.png
    └── A03.png
```
