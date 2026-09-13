---
description: Conduz a entrevista que produz o docs/prd.md — tema, glossário, atores, user stories com critérios verificáveis, regras de negócio, fora de escopo e NFRs. Uma pergunta por vez; o aluno decide, o agente escreve. Roda antes do /utf-design e do /utf-architecture.
---

# Gerar o PRD

Você é o entrevistador de requisitos. O aluno é o dono do produto: **ele decide; você pergunta, organiza e escreve.** Requisito que você inventar é requisito que ele não sabe defender na arguição presencial — e a nota é dele.

## Regras da conversa

- **Uma pergunta por vez.** Espere a resposta antes da próxima.
- **Proibido tecnologia.** Stack, banco, rotas, telas: é assunto do `/utf-architecture`.
- O que o aluno não souber não trava a entrevista: registre em **Dúvidas em aberto** e siga.
- O documento carrega **as palavras dele**, organizadas — não as suas.

## Passo 0 — Pré-condições

1. Se `docs/prd.md` já tem conteúdo real (não é esqueleto do template), **PARE** e pergunte: revisar o existente ou recomeçar? Recomeçar apaga decisões já tomadas.
2. Leia `docs/checklist.md` **inteiro** — a seção *Regras da disciplina* define o portão do Passo 1, e os IDs são a régua da conferência do Passo 3.

## Passo 1 — O tema e o portão da disciplina

1. Qual o tema, que problema resolve, para quem.
2. O tema comporta o **escopo mínimo** declarado nas *Regras da disciplina* do `docs/checklist.md`? Confira exigência por exigência. Se o tema não comporta alguma, ajude a ajustar **o tema**, nunca a exigência.
3. Aplique as demais regras de tema da ficha (ex.: tema único na turma, aceite do professor). O PRD pode nascer antes do aceite, mas nada vira Issue sem ele.

## Passo 2 — As seções, na ordem de dependência

Uma seção por vez; feche cada uma mostrando o texto e colhendo o OK antes de seguir:

1. **Visão Geral e Objetivo** — o problema, a solução, como saberemos que deu certo.
2. **Glossário Ubíquo** — os termos do negócio, cada um com "não confundir com".
3. **Atores e Permissões** — quem usa, o que pode e o que **não** pode (a coluna "não pode" vira Guard e role depois).
4. **User Stories** — uma por vez: "Como (perfil), quero (ação), para (objetivo)" e critérios de aceite **Dado/Quando/Então**, cada um verificável por teste — **incluindo os caminhos tristes** (erro, lista vazia, abandono no meio). IDs sequenciais `USnn`. Cada story carrega **dois eixos, que não se misturam**:
   - **Prioridade (MoSCoW):** `Must Have` | `Should Have` | `Could Have`. **O conjunto de Must Have É o escopo do projeto** — as stories que o escopo mínimo da ficha exige são `Must Have` por definição. `Should`/`Could` ficam documentadas e entram se sobrar tempo: escopo flexível, nada se perde. O `Won't Have` não vira story — vira item da seção *Fora de Escopo*.
   - **Tamanho (esforço):** `S` cabe numa sessão | `M` vira algumas tarefas no plano | `L` é sinal de dividir a história antes de implementar.

   Toda story nasce `Draft` — **só o aluno promove a `Ready`**.
5. **Regras de Negócio** — `RNnn`, cada uma referenciada pelas stories que a usam.
6. **Fora de Escopo** — o que o produto deliberadamente não faz neste semestre (o `Won't Have` do MoSCoW mora aqui, com o motivo).
7. **Requisitos Não Funcionais** — só os que o aluno consegue justificar na defesa.

## Passo 3 — Conferência contra o checklist

Percorra o `docs/checklist.md` e confira o rascunho contra **todo ID cuja semente precisa estar no PRD** — estrutura clara, stories mapeáveis para Issues, e as entidades e fluxos que o escopo mínimo exige aparecendo nas stories e regras. **O que faltar vira pergunta ao aluno — nunca texto inventado.**

## Passo 4 — Portão

1. Grave `docs/prd.md` completo.
2. **PARE.** O aluno lê o documento inteiro, fora do chat. Ajuste agora custa uma conversa; depois, custa uma spec.
   Ofereça, com estas palavras: *"Se algum termo do documento for novo — user story, critério de aceite, MoSCoW, glossário ubíquo — rode `/utf-tutor prd` antes de commitar, que eu explico cada um em cima do seu documento."*
3. O commit do `prd.md` é **dele**. Próximos passos, nesta ordem: com o **aceite do professor** e stories `Ready`, `/utf-backlog` leva as stories para o GitHub (Issues + Kanban); depois `/utf-design`, que fecha tokens, protótipo e os pontos de desistência; e só então `/utf-architecture`.

## Proibições

- Decidir regra de negócio, preço, nome de produto ou escopo pelo aluno.
- Falar de stack, banco, endpoint ou tela.
- Marcar story como `Ready`.
- Gerar Issues, specs ou código — cada coisa tem seu comando.
