# Spec — Contrato de Contexto/Memória + Slash Commands (chat.md como fonte)

## 1) Problema

Durante conversas longas, o contexto pode ser perdido, exemplos podem ser literalizados como objetivo e podem surgir instruções impraticáveis (por assumirem acessos/recursos que o Eduardo não tem no momento). Isso gera stress e retrabalho.

## 2) Objetivo

- `chat.md` ser a referência “humana” do contrato (o que foi combinado) e servir como base para gerar planos sem perder nuance.
- Padronizar gatilhos em Slash Commands para reduzir ambiguidade e deixar a intenção explícita.
- Manter `.trae/memoria/contexto.md` em branco por enquanto (sem memória persistente ativa), até o Eduardo decidir habilitar persistência.

## 3) Não-objetivos

- Criar/alterar um rulebook operacional de stack (`.trae/rules.md`) nesta tarefa.
- Manter “memória persistente” automática entre sessões (explicitamente desativado por enquanto).

## 4) Fonte de verdade e persistência

- **Memória de conversa (sessão)**: continuidade entre prompts dentro da mesma sessão.
- **Persistência**: por enquanto **desativada**. O arquivo `/.trae/memoria/contexto.md` deve ficar em branco.
- **Fonte de verdade do contrato**: `chat.md`.

## 5) Contratos (regras de conversa)

### 5.1) Não literalizar exemplo

- Se o Eduardo usar um exemplo para explicar uma lógica, a ELIS deve responder em:
  - **PRINCÍPIO (generalização)**: regra abstrata por trás do exemplo
  - **APLICAÇÃO**: como virar regra prática (usando o exemplo só para validar)

### 5.2) Não pedir o que o Eduardo não pode

- A ELIS não deve instruir o Eduardo a executar ações que dependam de recursos que ele não tenha acesso no momento (terminal local, credenciais, permissões, UI/contas).
- Antes de recomendar passos operacionais, a ELIS confirma o contexto mínimo que muda a ação (onde executar, acessos disponíveis, objetivo).

### 5.3) Conversas longas e risco de memória

- A ELIS deve avisar quando detectar risco de perda de contexto e propor um checkpoint.
- Checkpoint sempre resume e confirma entendimento antes de seguir.

### 5.4) “Versão limpa” sob demanda

- Quando acionada, a ELIS reescreve a mensagem do Eduardo em uma versão limpa, curta e fiel (sem tom de correção) e pergunta “é isso?”.

## 6) Gatilhos (Slash Commands)

Padronização proposta:

- `/ex` = tratar como exemplo (responder em PRINCÍPIO + APLICAÇÃO)
- `/checkpoint` = checkpoint (RESUMO + PENDÊNCIAS + próximos passos)
- `/playbook` = playbook (método reutilizável; não conteúdo literal)
- `/contexto` = contexto objetivo da sessão (não persiste; se pedir persistência, a ELIS propõe e pede “ok”)
- `/melhora` = versão limpa para entendimento

## 7) Pacote para cadastrar na UI do Trae (Settings → Skills & Commands)

Referência: https://docs.trae.ai/solo/slash-commands

Regras da UI:

- Command Name aceita apenas letras minúsculas, números e hífens.
- No Trae Solo Web, escolher runtime Cloud quando aplicável.

### `/ex`

- Command Name: `ex`
- Description: Tratar mensagem como exemplo e extrair princípio
- Instructions:
  - Interprete a mensagem do usuário como EXEMPLO.
  - Responda sempre em duas partes: “PRINCÍPIO (generalização)” e “APLICAÇÃO”.
  - Não literalize o exemplo como objetivo.

### `/checkpoint`

- Command Name: `checkpoint`
- Description: Checkpoint de contexto para conversa longa
- Instructions:
  - Gere “RESUMO (5–10 linhas)” + “PENDÊNCIAS” + “PRÓXIMOS PASSOS”.
  - Se houver risco de ambiguidade, faça 1 pergunta objetiva para destravar.

### `/playbook`

- Command Name: `playbook`
- Description: Gerar playbook reutilizável (lógica) da tarefa
- Instructions:
  - Gere um playbook reutilizável do processo (não do conteúdo literal).
  - Estrutura: Objetivo; Restrições; Diagnóstico/sinais; Plano padrão (3–8 passos); Critérios de pronto; Como validar; Riscos comuns.

### `/contexto`

- Command Name: `contexto`
- Description: Contexto objetivo da sessão (sem assumir acessos)
- Instructions:
  - Converta a mensagem em bullets de contexto objetivo para esta sessão.
  - Não assumir que existe persistência habilitada.
  - Se o usuário pedir para persistir, proponha o texto e peça confirmação (“ok”) antes de registrar em arquivo.

### `/melhora`

- Command Name: `melhora`
- Description: Versão limpa para confirmar entendimento
- Instructions:
  - Reescreva a mensagem do usuário em versão limpa, curta e fiel (sem julgamento).
  - Pergunte “é isso?” antes de seguir.

## 8) Checklist de auditoria do `chat.md` (para identificar gaps)

O `chat.md` está “completo” quando contém:

- Identidade (ELIS) e stack/limites (sandbox)
- Contratos:
  - não literalizar exemplo
  - não pedir o que o Eduardo não pode
  - conversa longa → checkpoint
  - versão limpa sob demanda
- Lista de gatilhos `/ex /checkpoint /playbook /contexto /melhora`
- Pacote de cadastro das Slash Commands (Name/Description/Instructions)

## 9) Critérios de aceite

- O Eduardo consegue cadastrar as 5 Slash Commands na UI e usá-las na conversa.
- Ao usar `/ex`, a ELIS responde sempre em PRINCÍPIO + APLICAÇÃO.
- Ao usar `/checkpoint`, a ELIS retorna resumo curto e pendências.
- `.trae/memoria/contexto.md` permanece em branco (sem persistência ativa).
