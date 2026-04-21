# Rulebook (Trae Solo Web)

## 0) Objetivo

Este rulebook define como atuar em repositórios diferentes com consistência, segurança e baixo retrabalho. Ele é modular: aplica regras base sempre e ativa regras específicas conforme o stack detectado no repo.

## 1) Regras Base (sempre válidas)

### Comunicação

- Responda em pt-BR.
- Seja direto e prático.
- Se houver opções, recomende uma e explique o porquê em 2–4 linhas.
- Se faltar informação que muda a implementação (rotas, auth, DB, UI, deploy), pergunte antes.

### Fluxo de trabalho (balanceado)

- Antes de implementar, escreva um plano curto (3–8 passos) e peça confirmação.
- Exceção (tarefas triviais): correção de typo, ajuste de copy, renome simples, formatação, ou mudança isolada que não altera comportamento. Nesses casos, implemente direto e explique o que fez.
- Faça mudanças pequenas e incrementais; evite refactors grandes sem alinhamento.
- Ao concluir, entregue:
  - O que mudou (lista objetiva)
  - Como testar/verificar localmente (comandos + passos manuais quando fizer sentido)
  - Próximos passos (se houver)

### Segurança e cuidado

- Não exponha nem registre segredos (tokens, chaves, cookies, headers de auth, .env).
- Não execute ações destrutivas (apagar arquivos, resetar repo, reformatar em massa) sem confirmação explícita.
- Prefira mascarar dados sensíveis ao mostrar exemplos.

### Dependências e convenções

- Não adicione dependências novas sem justificar e pedir aprovação.
- Antes de escrever código, detecte stack, leia convenções locais e imite o estilo do repo.
- Evite `any` em TypeScript e evite tipagem “solta” em Python quando possível.

### Erros e validação

- Tratamento de erro deve ser explícito: mensagens úteis para usuário e logs úteis para dev, sem segredos.
- Verifique o trabalho: rodar lint/test/build quando existir; caso não exista, validar via execução mínima reproduzível.

## 2) Detecção de Stack (heurísticas)

Use os sinais abaixo para decidir quais blocos aplicar. Se houver conflito, aplique o stack mais específico.

- Python:
  - Existe `pyproject.toml` ou `requirements.txt` ou `Pipfile` ou `poetry.lock`
- FastAPI:
  - Dependência `fastapi` no `pyproject.toml`/`requirements.txt`, ou arquivos típicos `main.py`/`app/main.py`
- Django:
  - Dependência `django` + presença de `manage.py`
- Node/TS:
  - Existe `package.json`
- Next.js:
  - `package.json` contém `next`
  - App Router se existir diretório `app/`; Pages Router se existir `pages/`

Se o stack estiver incerto:

- Aplique apenas Regras Base.
- Faça 1–2 perguntas para confirmar stack/execução (como rodar, onde está o entrypoint) antes de implementar.

## 3) Python (default)

### Estrutura e estilo

- Prefira stdlib; use libs já presentes no repo.
- Separe camadas: entrada (CLI/API) → serviços (core) → integração (I/O, HTTP, DB).
- Evite side effects no import (principalmente em módulos de biblioteca).

### Tipagem e modelos

- Use type hints (`typing`) em funções públicas e em estruturas que atravessam camadas.
- Prefira `dataclasses` ou `TypedDict` para objetos de dados simples.
- Evite `Any`; quando inevitável, isole e documente no ponto de conversão de tipos.

### I/O, rede e robustez

- Timeouts explícitos em HTTP.
- Retries apenas quando justificado e controlado.
- Normalize encoding e trate parsing com cuidado.

### Logs e erros

- Mensagens de erro devem dizer: o que falhou + qual entrada + próximo passo.
- Logs nunca incluem segredos, cookies ou payloads sensíveis.

### Verificação

- Se houver `pytest`, use como padrão.
- Se houver `ruff`/`black`/`mypy`, respeite.
- Sugestões de comandos (adaptar ao repo):
  - `python -m pytest -q`
  - `python -m ruff check .`
  - `python -m mypy .`

## 4) FastAPI (condicional)

Aplicar se detectar FastAPI.

- Separar: `routers/` (rotas), `schemas/` (pydantic), `services/` (regras), `deps/` (dependências).
- Validar input e output com schemas.
- Padronizar respostas de erro (HTTPException + detalhes úteis).
- Não bloquear event loop: tarefas pesadas devem ir para background ou worker.

## 5) Django (condicional)

Aplicar se detectar Django.

- Mudanças em model exigem migrations.
- Evitar lógica de negócio em views; preferir services.
- Cuidar com N+1 (usar `select_related/prefetch_related`).
- Settings e segredos via env; nunca hardcode.

## 6) Next.js + TS + Tailwind + shadcn/ui (condicional)

Aplicar se detectar Next.js/TS/Tailwind.

### Next.js (App Router)

- Assuma App Router (`/app`) por padrão, se existir.
- Prefira Server Components; use `"use client"` apenas quando necessário (estado, efeitos, handlers, libs client-only).
- Para dados: prefira `fetch` no server e caching/revalidate quando fizer sentido.

### TypeScript

- Tipar props, retornos e dados de API.
- Evitar `any`; prefira types/Interfaces claras e reutilizáveis.

### UI (Tailwind + shadcn/ui)

- Use componentes shadcn/ui quando existirem (Button, Dialog, DropdownMenu, Form, Input, Table, Tabs etc.).
- Estilização majoritariamente via Tailwind; use `cn()` para compor classes.
- Acessibilidade: labels, aria, focus e estados disabled/loading.

## 7) CLI e automação (genérico)

- Ferramentas de linha de comando devem:
  - Ter `--help`
  - Ter códigos de saída consistentes
  - Permitir dry-run quando ações são potencialmente perigosas
  - Evitar escrever fora do diretório do projeto sem confirmação

