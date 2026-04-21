# Plano — Auditoria do chat.md + Gatilhos (Slash Commands)

## Summary

Verificar se `chat.md` contém tudo o que foi combinado (contratos, memória/contexto, gatilhos) e ajustar para que ele sirva como “fonte de verdade” para gerar planos e identificar gaps. Padronizar os gatilhos em Slash Commands (/ex, /checkpoint, /playbook, /contexto, /melhora) em um formato cadastrável na interface do Trae.

## Current State Analysis

Arquivos existentes (no repo atual):

- `chat.md`
  - Contém: identidade (ELIS), descrição do ambiente (sandbox) e o “Protocolo” com regras de memória/contexto, EXEMPLO/CHECKPOINT/PLAYBOOK e “não pedir o que o Eduardo não pode fazer”.
  - Gap: os gatilhos estão no formato textual (EXEMPLO:, CHECKPOINT:, PLAYBOOK:), mas o objetivo atual é padronizar gatilhos em Slash Commands (`/ex`, etc.) e também incluir `/contexto` e `/melhora`.
  - Gap: faltam instruções em formato “copiar/colar” para cadastrar Slash Commands na UI do Trae.

- `.trae/memoria/contexto.md`
  - Existe no repo, mas o requisito atual é: deixar em branco por enquanto (sem memória persistente ativa).

## Proposed Changes

### 1) Completar o `chat.md` para cobrir gatilhos e gerar planos sem gaps

- **Arquivo**: `chat.md`
  - Adicionar uma seção “Gatilhos (Slash Commands)” que traduza os contratos atuais para a forma com barra:
    - `/ex` = equivalente a “EXEMPLO:” (responder em PRINCÍPIO + APLICAÇÃO; não literalizar)
    - `/checkpoint` = equivalente a “CHECKPOINT:” (resumo + pendências + proposta de registro; só registrar após “ok”)
    - `/playbook` = equivalente a “PLAYBOOK:” (processo reutilizável; não focar no exemplo literal)
    - `/contexto` = gatilho de contexto/dados persistentes (por enquanto: propor e confirmar; não assumir persistência ativa)
    - `/melhora` = gatilho de “versão limpa” (reescrever fiel e perguntar “é isso?”)
  - Adicionar uma seção “Como usar para gerar plano”:
    - o que a ELIS deve extrair do chat (objetivo, restrições, acordos)
    - sinais de “gap” (ambiguidade, conflito, falta de contexto mínimo)

### 2) Zerar o arquivo de memória persistente (por enquanto)

- **Arquivo**: `.trae/memoria/contexto.md`
  - Limpar o conteúdo e deixar em branco, conforme requisito: “nenhuma memória persistente por enquanto”.

### 3) Formato para cadastrar Slash Commands no Trae (UI de Commands)

Com base na doc oficial de Slash Commands (`https://docs.trae.ai/solo/slash-commands`) e no requisito do Eduardo, preparar um pacote “copiar/colar” para cadastrar na interface do Trae (Settings → Skills & Commands → Create Command).

- **Runtime**: Cloud (TRAE SOLO Web) quando o projeto estiver puxado do GitHub.
- **Command Name**: apenas letras minúsculas, números e hífens.

Comandos propostos (para documentar dentro do `chat.md`):

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
     - Entregue RESUMO (5–10 linhas) + PENDÊNCIAS.
     - Se o Eduardo pedir persistência, proponha o texto e só registre após “ok”.

3) `/playbook`
   - Name: `playbook`
   - Description: “Gerar playbook reutilizável (lógica) da tarefa.”
   - Instructions:
     - Produzir Objetivo; Restrições; Diagnóstico/sinais; Plano padrão; Critérios de pronto; Como validar; Riscos comuns.
     - Não focar no exemplo literal (arquivo/tela), e sim no método.

4) `/contexto`
   - Name: `contexto`
   - Description: “Contexto objetivo (sem assumir acesso do Eduardo).”
   - Instructions:
     - Transformar a mensagem em bullets de contexto objetivo para a sessão.
     - Não assumir que existe persistência habilitada.
     - Se o Eduardo pedir para persistir, proponha onde registrar e só aplique após “ok”.

5) `/melhora`
   - Name: `melhora`
   - Description: “Versão limpa para confirmar entendimento.”
   - Instructions:
     - Reescrever a mensagem do Eduardo em versão limpa, curta e fiel (sem julgamento).
     - Perguntar “é isso?” antes de seguir.

## Assumptions & Decisions

- O `chat.md` é a fonte principal para refletir tudo que foi discutido e servir de base para gerar planos.
- Por enquanto, **não existe memória persistente ativa**; `.trae/memoria/contexto.md` fica em branco.
- Quando o Eduardo usar `/contexto`, a ELIS sempre segue “propor e confirmar” antes de registrar qualquer coisa.
- Slash Commands são viáveis no Trae Solo Web e serão cadastradas via UI de Commands.
- O padrão recomendado passa a ser `/...` (Slash Commands), mas a ELIS pode entender versões textuais quando aparecerem.

## Verification

- Conferir que `chat.md` contém: contrato de memória/contexto + lista de gatilhos + pacote de cadastro das Slash Commands + seção “Como usar para gerar plano”.
- Conferir que `.trae/memoria/contexto.md` está vazio.
- Validar que o “pacote de cadastro” está no formato aceito pela UI (nomes minúsculos e sem caracteres inválidos).
