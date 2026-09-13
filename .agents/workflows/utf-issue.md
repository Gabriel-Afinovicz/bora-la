---
description: Define o fluxo operacional obrigatório da IA. Exige a criação e aprovação do spec.md e plan.md antes de qualquer código. Garante a execução em micro-tarefas com TDD, revisão por agentes distintos, trava erros em 2 rodadas de revisão e prepara o Pull Request.
---

# Fluxo de Trabalho de Implementação (SOP)

Sempre que o usuário pedir para trabalhar em uma Issue (Feature), você atuará como o orquestrador do método SDD seguindo ESTA ORDEM rigorosa:

**Passo 0: Pré-condições**
- Acesso ao GitHub: MCP do GitHub disponível **ou** `gh` autenticado (`gh auth status`). Sem um dos dois, **PARE** — este fluxo lê Issues e prepara PR.
- A Issue existe no GitHub. Se as Issues das stories ainda não foram criadas, mande rodar `/utf-backlog` primeiro.
- **Retomada.** Se `specs/<numero-da-issue>-<slug>/` já existe, **não recomece**: descubra o estado e entre no passo certo — spec `rascunho` → a pausa do Passo 1, esperando a aprovação; `aprovada` sem `plan.md` → Passo 2; plano com tarefa pendente → Passo 3; todas as tarefas feitas → Passo 4. É assim que `/utf-issue <n>` fecha a Issue depois da última tarefa, e é o que impede reescrever uma spec que o usuário já aprovou.

**Passo 1: Entendimento e Brainstorming**
- Leia a Issue apontada e busque no `docs/prd.md` os critérios e o Glossário Ubíquo.
- Faça perguntas ao usuário de forma proativa. Questione sobre casos de borda, caminhos tristes (ex: falhas de rede, dados inválidos) e como validar os critérios de aceite.
- Após sanar as dúvidas, **crie a branch da história a partir da `develop`** (`git switch develop && git pull && git switch -c feature/<numero-da-issue>-<slug>`). Ela nasce agora, antes da aprovação, porque no Gitflow `main` e `develop` são bloqueadas — e o commit de aprovação do usuário precisa de um lugar para viver.
- Redija o documento e salve no caminho `specs/<numero-da-issue>-<slug>/spec.md`, commitando o rascunho na branch. **A estrutura é a de `docs/modelo-spec.md`** — copie-a e preencha; os comentários dela explicam cada seção e são apagados no caminho. Não invente seções novas nem pule as existentes. O frontmatter:

```yaml
---
issue: 27
status: rascunho   # rascunho | aprovada
---
```

- **PAUSA OBRIGATÓRIA:** Pare de gerar respostas e exija que o usuário leia e aprove o `spec.md`. Ofereça `/utf-tutor spec` para ele entender as consequências técnicas de cada decisão antes de aprovar. A aprovação é o **próprio usuário** trocar `status: rascunho` por `status: aprovada` e commitar essa linha **na branch da história** — assim a aprovação fica no `git log`, com o nome dele. Você não altera esse campo em hipótese nenhuma.

**Passo 2: Planejamento**
- Com o `spec.md` aprovado, quebre o trabalho em tarefas curtas e encadeadas — cada uma prova **um critério de aceite inteiro**, ou é um passo técnico que sozinho não prova nada mas destrava o próximo.
- Cada tarefa deve prever a criação de testes primeiro (TDD).
- Se o plano passar de **10 tarefas**, pare: a história é grande demais. Proponha dividi-la em duas Issues antes de continuar.
- Salve o resultado no caminho `specs/<numero-da-issue>-<slug>/plan.md`, na forma de `docs/modelo-plan.md`: checklist `- [ ] **Tarefa N — <título>**`, cada uma citando o critério de aceite que cobre e o teste que nasce primeiro. Tarefa feita vira `- [x]`: é essa marcação que o `/utf-task` sem número lê. A seção *Critérios sem tarefa* precisa terminar vazia.
- **PAUSA OBRIGATÓRIA:** Peça a aprovação do usuário para o plano. Com o OK, commite o `plan.md` na branch da história.

**Passo 3: Execução (uma tarefa por vez)**
- A branch da história existe desde o Passo 1. Antes do primeiro código, confira que o `spec.md` (aprovado) e o `plan.md` estão commitados nela — é esse `git log` que prova que a especificação veio antes do código.
- Execute **uma tarefa por vez** através do fluxo `utf-task` (`.agents/workflows/utf-task.md`), que despacha o subagente **implementador** com contexto limpo e, depois dele, dois revisores distintos e somente-leitura: **revisor-conformidade** (diff × critérios de aceite da `spec.md`) e **revisor-codigo** (diff × `docs/architecture.md`).
- **Você nunca revisa o código que você mesmo despachou.** Revisor é sempre outro agente, sem permissão de escrita. Auto-auditoria não conta como revisão: quem escreveu carrega os mesmos pontos cegos.
- Ao fim de cada tarefa, pare e devolva o controle ao usuário. Ele pede a próxima.

**Passo 4: Auditoria final e Pull Request**
- Terminadas todas as tarefas, atualize **primeiro** a documentação: o status da história no `docs/prd.md`, os diagramas do `docs/architecture.md` que mudaram, e a linha da spec no `specs/README.md`. Proponha o commit e faça-o **só com o "pode commitar" do usuário** — o portão do commit vale aqui como em cada tarefa.
- Despache **então** o subagente **auditor-final**, que compara o diff **inteiro** contra o `spec.md` original — nunca contra o `plan.md` — e confere a documentação que acabou de ser atualizada. Auditar antes de atualizar os documentos é auditar um repositório que ainda não é o que vai para o PR. Se o veredito for NÃO PRONTO, cada pendência vira tarefa nova no `plan.md` (com o OK do usuário) e passa pelo `/utf-task`; depois o auditor roda de novo.
- Antes de o usuário escrever o PR, sugira `/utf-tutor prova` — o simulado interativo sobre o diff inteiro, que é o ensaio da defesa presencial.
- Prepare as alterações (commit) e lembre o usuário de abrir o Pull Request com `Closes #<n>`.
- A seção **"O que este PR faz e por quê"** é escrita **pelo usuário, com as palavras dele**. Ofereça os fatos do diff; não ofereça o texto pronto.
- No corpo do PR devem constar os apontamentos **aceitos e recusados**, com o motivo de cada recusa. A fonte é `specs/<issue>-<slug>/reviews/`: os pareceres (`tarefa-NN-<tipo>-r<N>.md`) e as decisões de triagem (`tarefa-NN-decisoes-r<N>.md`).
