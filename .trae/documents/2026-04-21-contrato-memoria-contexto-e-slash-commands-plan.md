# Plano — Contrato de Memória/Contexto + Slash Commands

## Summary

Verificar se `chat.md` e os arquivos do diretório `.trae/` refletem tudo que foi combinado (memória, contexto, contratos, gatilhos), identificar gaps/contradições e definir um formato de Slash Commands cadastrável no Trae Solo Web para padronizar os gatilhos.

## Current State Analysis

Arquivos existentes:

- `chat.md`
  - Contém: identidade (ELIS), descrição do ambiente (sandbox), e um “Protocolo” com regras sobre memória/contexto, EXEMPLO/CHECKPOINT/PLAYBOOK e regras de não pedir ações fora do acesso do Eduardo.
  - Gap: parte das regras de persistência ainda referencia “registrar no `chat.md`”, mas a persistência oficial foi movida para `.trae/memoria/contexto.md`.
  - Gap: não contém o mapeamento de gatilhos em formato “/comando” (ex.: `/ex`) nem o gatilho `/melhora` e `/contexto`.

- `.trae/memoria/contexto.md`
  - É a fonte persistente oficial para memória/contexto (entre sessões).
  - Contém: definições, prioridade de fonte de verdade, conflitos, contexto mínimo, limites, EXEMPLO/CHECKPOINT/PLAYBOOK, e protocolo de “SANDBOX vs LOCAL”.
  - Gap: ainda usa gatilhos em formato “palavra-chave” (EXEMPLO/CHECKPOINT/PLAYBOOK), mas não mapeia explicitamente para Slash Commands (`/ex`, etc.).
  - Gap: não define `/contexto` e `/melhora` como gatilhos formais (apenas descreve “versão limpa” como prática).

- `.trae/rules.md` e `.trae/snippet.txt`
  - Regras operacionais gerais (workflow/segurança/stack), não são a fonte principal de “memória e contexto humano”.

## Proposed Changes

### 1) Consolidar “memória e contexto” como fonte persistente oficial

- **Arquivo**: `.trae/memoria/contexto.md`
  - Adicionar uma seção “Gatilhos (Slash Commands)” com tabela de mapeamento:
    - `/ex` → EXEMPLO (responder em PRINCÍPIO + APLICAÇÃO)
    - `/checkpoint` → CHECKPOINT (resumo + pendências + proposta de persistência; só persistir após “ok”)
    - `/playbook` → PLAYBOOK (resumo reutilizável do processo/lógica)
    - `/contexto` → Contexto persistente (propor texto e persistir após “ok”)
    - `/melhora` → Reescrever “versão limpa” para confirmar entendimento (sem tom de correção)
  - Definir também um bloco curto de “compatibilidade”:
    - Continuar aceitando os gatilhos sem barra (EXEMPLO:, CHECKPOINT:, PLAYBOOK:) por hábito, mas recomendar `/...` como padrão.

### 2) Ajustar `chat.md` para não ter contradições e virar “índice humano”

- **Arquivo**: `chat.md`
  - Atualizar as linhas que ainda falam “registrar no chat.md” para apontar para `.trae/memoria/contexto.md`.
  - Opcional (recomendado): reduzir o “Protocolo” do `chat.md` para um resumo + link/ponteiro:
    - “Persistência oficial em `.trae/memoria/contexto.md`”
    - “Gatilhos oficiais: /ex, /checkpoint, /playbook, /contexto, /melhora”
  - Manter no `chat.md` o que é “história do relacionamento” (nome ELIS, stack/limites) e um resumo do contrato.

### 3) Formato para cadastrar Slash Commands no Trae (UI de Commands)

Com base na doc oficial de Slash Commands (`https://docs.trae.ai/solo/slash-commands`), preparar um pacote “copiar/colar” para cadastrar na interface:

- **Runtime**: Cloud (TRAE SOLO Web) quando o projeto estiver puxado do GitHub.
- **Command Name**: apenas letras minúsculas, números e hífens.

Comandos propostos:

1) `/ex`
   - Name: `ex`
   - Description: “Tratar mensagem como EXEMPLO e extrair PRINCÍPIO.”
   - Instructions (resumo):
     - Reescreva o conteúdo em “PRINCÍPIO (generalização)” e “APLICAÇÃO”.
     - Não literalize o exemplo como objetivo.

2) `/checkpoint`
   - Name: `checkpoint`
   - Description: “Checkpoint de contexto e risco de memória.”
   - Instructions:
     - Entregue RESUMO (5–10 linhas) + PENDÊNCIAS + PROPOSTA de texto para persistir em `.trae/memoria/contexto.md`.
     - Só persista após o Eduardo responder “ok”.

3) `/playbook`
   - Name: `playbook`
   - Description: “Gerar playbook reutilizável (lógica) da tarefa.”
   - Instructions:
     - Produzir Objetivo; Restrições; Diagnóstico/sinais; Plano padrão; Critérios de pronto; Como validar; Riscos comuns.
     - Não focar no exemplo literal (arquivo/tela), e sim no método.

4) `/contexto`
   - Name: `contexto`
   - Description: “Registrar contexto persistente (entre sessões).”
   - Instructions:
     - Transformar a mensagem em bullets de contexto objetivo.
     - Propor patch para `.trae/memoria/contexto.md`.
     - Só aplicar após “ok”.

5) `/melhora`
   - Name: `melhora`
   - Description: “Versão limpa para confirmar entendimento.”
   - Instructions:
     - Reescrever a mensagem do Eduardo em versão limpa, curta e fiel (sem julgamento).
     - Perguntar “é isso?” antes de seguir.

## Assumptions & Decisions

- Persistência oficial entre sessões fica em `.trae/memoria/contexto.md` (não no chat).
- Persistência sempre segue a regra “propor e confirmar” antes de gravar.
- Slash Commands são viáveis no Trae Solo Web e serão cadastradas via UI de Commands.
- Mesmo com Slash Commands, continua valendo aceitar gatilhos textuais (EXEMPLO:, etc.) por compatibilidade, mas o padrão recomendado passa a ser `/...`.

## Verification

- Conferir que `chat.md` não contém instruções conflitantes com `.trae/memoria/contexto.md`.
- Conferir que `.trae/memoria/contexto.md` lista e define todos os gatilhos: `/ex`, `/checkpoint`, `/playbook`, `/contexto`, `/melhora`.
- Validar que o “pacote de cadastro” está no formato aceito pela UI (nomes minúsculos e sem caracteres inválidos).

