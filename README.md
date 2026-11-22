# Playwright Mark L

Testes E2E com **Playwright** e **TypeScript** para o **Mark L**, uma aplicação de gerenciamento de tarefas (API + web). Projeto desenvolvido no curso **Playwright eXpress**, de Fernando Papito (QAx).

## Stack

- [Playwright](https://playwright.dev/) + TypeScript
- Page Objects
- Massa de teste preparada via API (`request` do Playwright)
- Fixtures em JSON
- Variáveis de ambiente com `dotenv`
- Execução em Chromium, Firefox, WebKit e viewports mobile

## Cenários cobertos

| Funcionalidade | Cenário |
|---|---|
| Home | Webapp deve estar online |
| Cadastro | Deve cadastrar uma nova tarefa |
| Cadastro | Não deve permitir tarefa duplicada |
| Cadastro | Campo obrigatório |
| Atualização | Deve concluir uma tarefa |
| Exclusão | Deve excluir uma tarefa |

## Estrutura

```
apps/
  api/            # API do Mark L (Node.js + SQLite)
  web/            # Front-end do Mark L
tests/
  fixtures/       # Modelo de tarefa e massa de teste (tasks.json)
  support/
    pages/tasks/  # Page Object da tela de tarefas
    helpers.ts    # Funções de apoio via API (criar e excluir tarefas)
  home.spec.ts
  tasks.spec.ts
playwright.config.ts
```

## Pré-requisitos

- Node.js 20 ou superior
- Yarn (`npm install -g yarn`)

## Como executar

### 1. Subir a API (porta 3333)

```bash
cd apps/api
yarn install --ignore-engines
yarn db:init
yarn dev
```

### 2. Subir o front-end (porta 8080)

Em outro terminal:

```bash
cd apps/web
yarn install --ignore-engines
yarn dev
```

### 3. Rodar os testes

Em um terceiro terminal, na raiz do projeto:

```bash
yarn install --ignore-engines
yarn playwright install
yarn playwright test
```

Para ver o relatório HTML:

```bash
yarn playwright show-report
```

## Variáveis de ambiente

O arquivo `.env` na raiz define os endereços usados nos testes:

```
BASE_URL=http://localhost:8080
BASE_API=http://localhost:3333
```

## Observações

- Os testes rodam com `workers: 1`. Como todos os navegadores usam o mesmo banco de dados da API, execuções em paralelo disputavam a mesma massa de teste e geravam falhas intermitentes (testes flaky).
- A dependência `better-sqlite3` da API foi atualizada para a versão 8.7.0, que tem binários prontos para o Node 20.
