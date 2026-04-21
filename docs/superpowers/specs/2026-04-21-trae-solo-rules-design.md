# Design — Rulebook de Rules para Trae Solo Web

## Objetivo

Criar um conjunto de rules para uso no Trae Solo Web que funcione em repositórios multistack, reduzindo ambiguidade e retrabalho por meio de um workflow consistente (plan → confirmação → execução incremental → validação).

## Requisitos

- Ter um arquivo versionável no repo com regras completas.
- Ter um snippet compacto para colar no Trae Solo Web.
- Ser “balanceado”: regras fortes, com exceção explícita para tarefas triviais.
- Ser multistack com detecção por heurísticas; quando incerto, perguntar antes.
- Cobrir Python como default (sem depender de um framework específico).

## Entregáveis

- Rulebook completo: `.trae/rules.md`
- Snippet compacto: `.trae/snippet.txt`

## Decisões de design

- Modularidade por detecção de stack: regras base sempre e blocos condicionais ativados por sinais do repo (arquivos e dependências).
- Segurança como invariável: nunca expor segredos e nunca executar ações destrutivas sem confirmação explícita.
- Dependências: não adicionar libs novas sem pedir aprovação.
- Verificação: sempre propor comandos de lint/test quando existirem; caso contrário, validação mínima reproduzível.

## Critério de pronto

- O snippet é suficiente para orientar o Trae a trabalhar corretamente em um repo desconhecido.
- O arquivo `.trae/rules.md` é claro, sem contradições e com instruções acionáveis.

