# 🎮 Tutorial: operando o método SDD

Este é o passo a passo **operacional**: o que digitar e o que fazer em cada pausa.
O *porquê* de cada regra está no [guia da disciplina](./guia-sdd.md); as normas de
Git e de PR estão no [CONTRIBUTING](../CONTRIBUTING.md).

> A regra que resume tudo (é o que dá nome ao método — **UTF-SDD, um SDD por
> Portões**): **a IA escreve o código; você decide nos portões.**
> São quatro portões por história — aprovar a spec, aceitar a explicação do
> tutor antes de cada tarefa, fazer a triagem dos apontamentos e **autorizar
> cada commit** — mais o PR no fim.

---

## Fase 0 — Iniciar o projeto (uma vez por projeto)

Cinco comandos, nesta ordem, cada um fechando num portão seu:

| # | Comando | O que sai | 🚪 Você faz o quê |
| --- | --- | --- | --- |
| 1 | `/utf-prd` | `docs/prd.md` — entrevista de requisitos | Lê o documento inteiro, ajusta e **commita**; leva o tema ao professor |
| 2 | `/utf-backlog` | Issues (uma por story `Ready`) + Kanban no Projects | **Aprova a lista** antes de as Issues serem criadas |
| 3 | `/utf-design` | `docs/design-tokens.md` + protótipo navegável — tokens, Mobile-First, identidade PWA | Decide o que acontece em cada ponto de desistência (vai para o PRD) e **commita** |
| 4 | `/utf-architecture` | `docs/architecture.md` — entrevista técnica | Lê e **commita** |
| 5 | `/utf-setup` | o app Angular nascendo com testes verdes | Ratifica as decisões relatadas e abre o **1º PR** (`manutencao`) |

Pré-requisito dos passos 2 e 5: `gh` autenticado ou MCP do GitHub. O `/utf-backlog`
pode rodar de novo mais tarde, a cada leva de stories promovidas a `Ready`.

> 🎓 **O tutor também vale na Fase 0.** Cada documento é decisão sua, e decisão que
> você não sabe explicar não sobrevive à arguição. Antes de commitar, rode
> `/utf-tutor prd`, `design` ou `architecture` — ele explica os conceitos em cima do
> **seu** documento, não em exemplo genérico. Depois do `/utf-setup` você não precisa
> pedir: o fluxo chama o tutor sozinho, porque ali é o único momento em que você
> recebe dezenas de arquivos que não escreveu.

---

## Antes de começar (uma vez por história)

1. A história está no [prd.md](./prd.md) com status `🟡 Ready` (regras definidas).
2. Existe a Issue no GitHub Projects apontando para ela — criada pelo
   `/utf-backlog`, com a descrição só linkando o PRD, nunca copiando regra de
   negócio.

---

## Passo 1 — Iniciar a Issue

```
/utf-issue 12
```

O agente vira orquestrador: lê a Issue e o PRD e **faz perguntas** sobre casos de
borda e caminhos tristes (brainstorming). Ele cria a branch da história a partir
da `develop` (Gitflow) e da conversa sai
`specs/12-<slug>/spec.md` com `status: rascunho`, commitado na branch — e ele **para**.

## Passo 2 — 🚪 Aprovar a spec (fora do chat)

Leia o arquivo **inteiro**. Em dúvida sobre alguma decisão técnica, rode
`/utf-tutor spec` antes.

A aprovação é **você** trocar `status: rascunho` por `status: aprovada` no
frontmatter e **commitar essa linha na branch da história** — ela fica no
`git log`, com o seu nome. Nenhum agente altera esse campo.

## Passo 3 — Aprovar o plano

Avise que aprovou; o agente gera o `plan.md` (uma tarefa por critério de aceite,
ou um passo técnico que destrava o próximo — mais de 10, a história é grande
demais e ele propõe dividir). Você lê, dá o OK na conversa, e ele commita o
plano na branch da história (criada no Passo 1).

## Passo 4 — Implementar, uma tarefa por vez

```
/utf-task 1
```

Dentro do comando acontece o ciclo completo, com as suas paradas:

| Etapa | Quem age | Você faz o quê |
| --- | --- | --- |
| Tutor explica a tarefa, bem mastigado | tutor (contexto limpo) | **🚪 aceita** ("pode implementar") ou pergunta |
| Implementação com TDD | implementador novo | acompanha |
| Revisão em paralelo | revisor-conformidade + revisor-codigo | nada — quem despacha é o fluxo |
| Pareceres gravados em `reviews/` | orquestrador | nada |
| **Triagem** (se houve apontamentos) | orquestrador apresenta a lista | **🚪 aceita ou recusa cada um** — recusa exige justificativa, registrada em `reviews/tarefa-NN-decisoes-rN.md` |
| Commit `tarefa 1: ...` | orquestrador apresenta o diff e os pareceres | **🚪 confere o diff na IDE e autoriza** ("pode commitar"); `/utf-tutor passo 1` destrincha o diff arquivo por arquivo, e `/utf-tutor 1` dá a aula depois do commit |

Repita para cada tarefa: `/utf-task 2`, `/utf-task 3`… — ou apenas
`/utf-task`, que pega a próxima pendente do `plan.md` e avisa quando não
houver mais nenhuma. O ciclo devolve
o controle a você ao fim de **cada** tarefa — nunca emenda duas.

## Passo 5 — Fechar a Issue

Com todas as tarefas prontas, o orquestrador despacha o **auditor-final** (diff
inteiro contra a spec, ignorando o plano) e atualiza os docs no mesmo commit.

Antes de escrever o PR:

```
/utf-tutor prova
```

O simulado da defesa: uma pergunta por vez sobre o diff, com correção das suas
respostas e a lista de arquivos para reler.

Então **você** escreve a seção *"O que este PR faz e por quê"* com as suas
palavras, lista os apontamentos aceitos e recusados (saem dos arquivos
`decisoes` em `reviews/`) e abre o PR com `Closes #12`.

---

## Resumo dos comandos

| Comando | Quando usar |
| --- | --- |
| `/utf-prd` | Fase 0, etapa 1 — a entrevista que gera o `docs/prd.md` |
| `/utf-backlog` | Fase 0, etapa 2 — PRD aprovado vira Issues + Kanban (e roda de novo a cada leva de stories `Ready`) |
| `/utf-design` | Fase 0, etapa 3 — framework CSS, tokens, protótipo, Mobile-First e PWA |
| `/utf-architecture` | Fase 0, etapa 4 — a entrevista que gera o `docs/architecture.md` |
| `/utf-setup` | Fase 0, etapa 5 — gera o scaffold do projeto |
| `/utf-issue <n>` | Uma vez, para iniciar o ciclo da Issue (spec → plano) |
| `/utf-task [n]` | Uma vez **por tarefa** do plano — sem número, executa a próxima pendente |
| `/utf-tutor prd` · `design` · `architecture` | Na Fase 0, antes de commitar cada documento |
| `/utf-tutor setup` | Depois do scaffold — o app, a fonte de dados e os arquivos que você não escreveu (o `/utf-setup` já chama sozinho) |
| `/utf-tutor spec` | Antes de aprovar a spec |
| `/utf-tutor passo <n>` | A leitura do diff arquivo por arquivo, no seu ritmo |
| `/utf-tutor <n>` | Depois de uma tarefa, para a aula sobre aquele diff |
| `/utf-tutor antes <n>` | Para reouvir a explicação pré-implementação de uma tarefa |
| `/utf-tutor prova` | Antes de escrever o PR — o ensaio da defesa |

Dizer "vamos trabalhar na Issue 12" em linguagem natural também dispara o fluxo
(`utf-rules.md` §1) — os comandos são só o caminho mais curto.

---

## Fora do ciclo — bug e tarefa técnica

Nem todo trabalho é história. **Bug** (algo que já deveria funcionar e não funciona) e
**tarefa técnica** (subir versão, refatorar, configurar a esteira) não têm `spec.md`
e não passam pelo `/utf-issue`. O caminho é mais curto, e mesmo assim tem regras:

1. **Abra a Issue direto no GitHub**, escolhendo o modelo (🔴 Bug ou 🟡 Tarefa técnica).
   Aqui a descrição é detalhada — passos, erro do console, evidência. É ela que faz o
   papel da spec.
2. **Branch a partir da `develop`**, como sempre no Gitflow.
3. **No bug, o primeiro commit é um teste que reproduz a falha e falha de verdade.**
   Sem esse teste, nada prova que o bug foi embora nem que ele não volta. Só depois vem
   a correção. É o mesmo RED → GREEN do ciclo, sem a papelada.
4. **PR para a `develop` com a etiqueta `manutencao`**, `Closes #<n>` e a explicação de
   250 caracteres. A etiqueta dispensa a spec, **nunca** o Portão de Entendimento.

Se, ao investigar, você descobrir que o `docs/prd.md` nunca disse o que o sistema
deveria fazer ali, então não era bug: é história nova. Feche a Issue, escreva a story
no PRD e volte para o ciclo normal.

---

## Quando algo dá errado

- **Estourou as 2 rodadas de revisão:** o ciclo para sozinho e te chama, com os
  pareceres no disco. Quase sempre a causa é spec ambígua, tarefa grande demais
  ou dependência não declarada. Corrija a spec e **abra uma sessão nova**
  entregando só a spec e o plano — o contexto da conversa velha está sujo.
- **Descobriu um problema novo no meio:** não inche a spec. Registre como
  comentário na Issue e abra uma Issue nova. O escopo do PR é o escopo da spec.
- **Bug ou tarefa técnica (sem história):** não passa por aqui — Issue direto no
  GitHub, PR com a etiqueta `manutencao`, sem `spec.md`.

Os detalhes desses desvios estão no [guia](./guia-sdd.md), seção *"Quando o
ciclo não é linear"*.
