---
description: Leva as user stories do prd.md aprovado para o GitHub — uma Issue por story Ready, com a descrição apontando só para o PRD, e o Kanban no GitHub Projects. Roda depois do /utf-prd, com o aceite do professor. Exige MCP do GitHub ou gh autenticado.
---

# Gerar o backlog no GitHub

Você leva o backlog do papel para o quadro. A fonte da verdade **continua sendo
o `docs/prd.md`** — a Issue é só o cartão que aponta para ela. Issue com regra
de negócio copiada é a receita da divergência: alguém atualiza o PRD, a Issue
fica velha, e a IA lê a versão errada.

## Passo 0 — Pré-condições (PARE se qualquer uma falhar)

1. Acesso ao GitHub: MCP do GitHub disponível **ou** `gh` autenticado
   (`gh auth status`). Sem um dos dois, **PARE** e oriente: instalar o `gh`,
   `gh auth login`, escopos `repo` e `project`.
2. `docs/prd.md` preenchido e **commitado pelo aluno**, com o tema já
   **aceito pelo professor**. Confira o commit com
   `git status --porcelain docs/prd.md`: saída não vazia significa que ainda há
   alteração pendente — **PARE** e peça o commit antes de criar Issue nenhuma.
3. Existe ao menos uma story com `Status: Ready`. Story `Draft` não vira
   Issue — regra indefinida não entra na fila de implementação.

## Passo 1 — Conferir o que já existe

Liste as Issues abertas do repositório. Se alguma story já tem Issue, **não
duplique** — relate e pule. Este fluxo pode rodar de novo a cada leva de
stories promovidas a `Ready`.

## Passo 2 — Uma Issue por story `Ready`

Para cada story `Ready` ainda sem Issue, crie a Issue com:

- **Título:** `USnn — <título da story>`
- **Descrição:** APENAS o link para a story no `docs/prd.md` (ex.:
  `docs/prd.md` seção US03) e o link do critério da ficha, se houver.
  **NUNCA copie regras de negócio ou critérios de aceite para a Issue** —
  constituição, regra 6.
- **Labels:** crie/aplique `story` e a prioridade (`must-have`, `should-have`,
  `could-have`), refletindo o MoSCoW do PRD.

Apresente a lista do que vai criar **antes** de criar, e espere o OK do
usuário — Issue criada aparece para a turma e para o professor.

## Passo 3 — O Kanban (GitHub Projects)

A criação do board é **manual** (a interface do Projects muda rápido e o
aluno precisa conhecê-la): oriente-o a criar um Project no repositório com
cinco colunas, e diga o que cada uma quer dizer — coluna sem regra de entrada
e de saída vira depósito:

| Coluna | O que fica nela | Sai quando |
| --- | --- | --- |
| `Backlog` | Toda Issue que ainda não começou, ordenada por prioridade | A equipe escolhe a próxima história |
| `In Progress` | A história em que se está trabalhando **agora** | O trabalho termina ou trava |
| `In Review` | O Pull Request está aberto e **espera a revisão dos colegas** antes do merge | O PR é mesclado |
| `Blocked` | A história parada à espera de outra coisa (guia, seção *Quando o ciclo não é linear*) | O impedimento é resolvido |
| `Done` | PR mesclado e story `Live` no `prd.md` | Nunca |

Adicione as Issues recém-criadas ao `Backlog` — `Must Have` primeiro, no topo.

> **Não crie uma coluna `Ready`.** Ela colidiria com o status `Ready` do
> `prd.md`, e todo cartão do quadro já é `Ready` por definição — só story
> `Ready` vira Issue. Se a equipe quiser marcar quais histórias pertencem a
> qual entrega, isso é **milestone ou etiqueta na Issue**, não coluna: escopo é
> atributo do cartão e precisa viajar com ele por todas as colunas.

Se o MCP/`gh` da sessão conseguir adicionar as Issues ao Project, ofereça
fazer isso; se não conseguir, não é erro — siga com a orientação manual.

## Passo 4 — Entrega

Relate: Issues criadas (número e título), stories puladas (e por quê), e o
estado do Kanban. Lembre o fluxo: se `/utf-design`, `/utf-architecture` e `/utf-setup` ainda
não rodaram, eles vêm antes; então a implementação de cada Issue começa por
`/utf-issue <n>`, **uma por vez**, começando pelos `Must Have`.

## Proibições

- Copiar regra de negócio, critério de aceite ou texto da story para a Issue.
- Criar Issue de story `Draft`, ou de coisa que não é story (bug e task de
  manutenção nascem direto no GitHub, sem passar por aqui).
- Criar Issues sem o OK do usuário sobre a lista.
- Marcar story como `Ready` para ela poder virar Issue — a promoção é do aluno.
