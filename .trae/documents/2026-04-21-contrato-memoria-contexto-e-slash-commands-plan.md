# Plano — Contrato de Memória/Contexto + Slash Commands (com limpeza)

## Summary

Verificar se os contratos estão consistentes e consolidar tudo que é “operacional/persistente” em `.trae/rules.md`, criando um pacote de Slash Commands cadastrável no Trae Solo Web. Ao final, limpar arquivos temporários do projeto.

## Current State Analysis

Arquivos existentes (no repo atual):

- `chat.md`
  - Contém acordos úteis, mas será tratado como arquivo temporário (não é a fonte final).
  - Gap: contém regras que ainda referenciam persistência no próprio `chat.md`.

- `.trae/memoria/contexto.md`
  - Contém o contrato atual, mas o requisito atualizado é: por enquanto, **não manter memória persistente ativa**; o arquivo deve ficar em branco.

- `.trae/rules.md`
  - Já contém regras operacionais gerais (workflow/segurança/stack).
  - Gap: ainda não reflete o contrato de memória/contexto + gatilhos em formato de Slash Commands conforme o requisito atualizado.

- `.trae/snippet.txt` e `boas-ideias.md`
  - Arquivos auxiliares temporários que deverão ser removidos após a consolidação em `.trae/rules.md`.

## Proposed Changes

### 1) Reescrever `.trae/rules.md` como fonte operacional única (inclui contratos e gatilhos)

- **Arquivo**: `.trae/rules.md`
  - Incorporar uma seção “Memória & Contexto (contrato)” com:
    - definição de memória de sessão vs persistência (com o aviso de limite e o conceito de checkpoint)
    - regra de “não pedir pro Eduardo o que ele não pode fazer”
    - regra de “não literalizar EXEMPLO”
  - Incorporar uma seção “Gatilhos (Slash Commands)” com a padronização:
    - `/ex` = gatilho EXEMPLO (responder em PRINCÍPIO + APLICAÇÃO)
    - `/checkpoint` = gatilho CHECKPOINT (resumo + pendências; propor registro se/quando persistência for habilitada)
    - `/playbook` = gatilho PLAYBOOK (método reutilizável)
    - `/contexto` = gatilho CONTEXTO (por enquanto: contexto operacional da sessão; não persistir automaticamente)
    - `/melhora` = gatilho MELHORA (versão limpa do entendimento)
  - Incorporar a seção “Pacote de cadastro na UI do Trae (Commands)” com Name/Description/Instructions (copiar/colar).

### 2) Zerar o arquivo de memória persistente (por enquanto)

- **Arquivo**: `.trae/memoria/contexto.md`
  - Limpar o conteúdo e deixar em branco, conforme requisito: “nenhuma memória persistente por enquanto”.

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

- Por enquanto, **não existe memória persistente ativa**; `.trae/memoria/contexto.md` fica em branco.
- Se/quando persistência for habilitada, ela deve seguir a regra “propor e confirmar” antes de gravar.
- Slash Commands são viáveis no Trae Solo Web e serão cadastradas via UI de Commands.
- O padrão recomendado passa a ser `/...` (Slash Commands), mas a ELIS pode entender versões textuais quando aparecerem.

## Cleanup (após implementação)

Após consolidar tudo em `.trae/rules.md`, remover arquivos temporários:

- Remover: `/.trae/snippet.txt`
- Remover: `/boas-ideias.md`
- Remover: `/chat.md`

Por serem ações destrutivas, o executor pede confirmação explícita imediatamente antes de remover.

## Verification

- Conferir que `.trae/rules.md` contém: contrato de memória/contexto + lista de gatilhos + pacote de cadastro das Slash Commands.
- Conferir que `.trae/memoria/contexto.md` está vazio.
- Validar que o “pacote de cadastro” está no formato aceito pela UI (nomes minúsculos e sem caracteres inválidos).
