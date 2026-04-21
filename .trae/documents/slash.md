# Slash Commands (Trae)

Referência: https://docs.trae.ai/solo/slash-commands

Regras da UI:

- Command Name: apenas letras minúsculas, números e hífens.

## /ex

- Command Name: `ex`
- Description: Tratar mensagem como exemplo e extrair princípio
- Instructions:
  - Interprete a mensagem do usuário como EXEMPLO.
  - Responda sempre em duas partes: “PRINCÍPIO (generalização)” e “APLICAÇÃO”.
  - Não literalize o exemplo como objetivo.

## /checkpoint

- Command Name: `checkpoint`
- Description: Checkpoint de contexto para conversa longa
- Instructions:
  - Gere “RESUMO (5–10 linhas)” + “PENDÊNCIAS” + “PRÓXIMOS PASSOS”.
  - Se houver risco de ambiguidade, faça 1 pergunta objetiva para destravar.

## /playbook

- Command Name: `playbook`
- Description: Gerar playbook reutilizável (lógica) da tarefa
- Instructions:
  - Gere um playbook reutilizável do processo (não do conteúdo literal).
  - Estrutura: Objetivo; Restrições; Diagnóstico/sinais; Plano padrão (3–8 passos); Critérios de pronto; Como validar; Riscos comuns.

## /contexto

- Command Name: `contexto`
- Description: Registrar contexto persistente (com confirmação)
- Instructions:
  - Proponha um bloco objetivo de contexto para `.trae/memoria/contexto.md`.
  - Peça confirmação (“ok”) antes de gravar.

## /melhora

- Command Name: `melhora`
- Description: Versão limpa para confirmar entendimento
- Instructions:
  - Reescreva a mensagem do usuário em versão limpa, curta e fiel (sem julgamento).
  - Pergunte “é isso?” antes de seguir.

