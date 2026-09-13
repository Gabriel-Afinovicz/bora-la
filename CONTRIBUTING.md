# Guia de Contribuição e Método de Trabalho (UTF-SDD)

Bem-vindo ao repositório! Este documento explica como trabalhamos aqui. Leia antes de iniciar sua primeira entrega e volte a ele sempre que tiver dúvidas sobre "como eu deveria estar fazendo isso".

## 🥇 A Regra de Ouro

Pedir código para uma Inteligência Artificial é fácil. O problema aparece semanas depois, quando ninguém lembra o porquê de uma regra existir e alterar o sistema vira um caos. Para resolver isso, adotamos o **UTF-SDD**, um **SDD por Portões** (*Gated Spec-Driven Development*): nada avança sem um portão de decisão humana registrada, e quem revisa nunca é quem escreveu.

A regra número um deste repositório é:

> **A IA escreve o código. Você continua responsável por cada decisão.**

Você é o Engenheiro e o Arquiteto; a IA é a sua equipe de execução.

---

## 🌳 Fluxo Git e Proteção da Produção (Gitflow)

> As regras desta seção valem para **o projeto que a equipe constrói**. Este
> repositório é o template da disciplina: ele não tem produção nem release, então vive
> só na `main`, e a manutenção dele entra por Pull Request de branch curta direto para
> ela. A `develop` do projeto de vocês é criada pelo `/utf-setup`, no repositório novo.

- **Duas branches permanentes e bloqueadas:** a `main` reflete a produção; a `develop` integra o trabalho da equipe. Commit direto em qualquer uma das duas é proibido.
- **Trabalho:** Crie uma branch curta **a partir da `develop`** para cada Issue, nomeada `feature/<numero-da-issue>-<slug>`. Manutenção (bug, tarefa técnica, setup) usa `chore/<slug>`.
- **Integração:** Ao finalizar, abra um Pull Request **para a `develop`** com `Closes #<n>`. O CI (testes + lint) precisa passar antes do merge.
- **Release:** quando a `develop` está estável, um PR de `develop` → `main` publica a versão (é o que o deploy em produção acompanha).
- **O que este projeto não usa:** o Gitflow original também prevê branches `release/*` e `hotfix/*`. Aqui não há trem de release nem correção de emergência em produção separada — a publicação é o próprio PR de `develop` → `main`. Se vocês encontrarem esses nomes em tutoriais, não é algo que ficou faltando.
- **Em equipe (ID27):** todo PR precisa da aprovação de **um colega** antes do merge — quem abre a story não mergeia o próprio PR. Os portões da história (spec, triagem, commit) são do **dono da história**; a revisão do PR é do colega.

---

## 🗺️ Onde as coisas vivem (Artefatos)

Nada é duplicado neste projeto. Informação repetida diverge.

| Artefato          | Onde fica               | O que responde                                                         |
| :---------------- | :---------------------- | :--------------------------------------------------------------------- |
| **Vitrine**       | `README.md` na raiz     | O que é o projeto e como rodar.                                        |
| **Produto**       | `docs/prd.md`           | O que o sistema faz (Glossário, Atores, Histórias).                    |
| **Arquitetura**   | `docs/architecture.md`  | Onde as coisas estão (estrutura, entidades, contratos).                |
| **Ficha**         | `docs/checklist.md`     | As regras da disciplina, os IDs e as entregas — a régua dos workflows. |
| **Design**        | `docs/design-tokens.md` | Tokens, breakpoints, identidade PWA e o link do protótipo navegável.   |
| **Especificação** | `specs/<issue>-<slug>/` | O `spec.md` (o que fazer), o `plan.md` (tarefas técnicas) e `reviews/` (pareceres e triagem). |
| **Modelos**       | `docs/modelo-spec.md` e `docs/modelo-plan.md` | A forma do `spec.md` e do `plan.md`, comentada. Não são specs — são a régua. |
| **Leis da IA**    | `.agents/`              | `rules/utf-rules.md` (constituição, carregada via `CLAUDE.md`), `workflows/` (ciclos) e `agents/` (prompts dos subagentes). |

---

## 🔄 O Ciclo de Trabalho

Todo trabalho que altera o comportamento do sistema segue o ciclo UTF-SDD, orquestrado pelos agentes do repositório: `/utf-issue <n>` inicia (spec → plano), `/utf-task <n>` executa cada tarefa com tutor, implementador de contexto limpo e dois revisores distintos, e o auditor final confere o diff inteiro antes do PR.

- **O passo a passo operacional** (comandos e o que fazer em cada pausa) está no [Tutorial do Método](./docs/tutorial-sdd.md).
- **O porquê de cada regra** está no [Guia da Disciplina](./docs/guia-sdd.md).

O que é **norma inegociável** deste repositório são os cinco portões humanos — nenhum agente passa por eles em seu lugar:

1. **🚪 Spec:** só você aprova, trocando `status: rascunho` → `status: aprovada` num commit seu.
2. **🚪 Explicação do tutor:** cada tarefa só é implementada depois do seu "pode implementar" — dúvida agora custa cinco minutos; depois do diff, custa uma rodada.
3. **🚪 Triagem:** só você aceita ou recusa apontamentos de revisão (recusa exige justificativa, registrada em `specs/<issue>-<slug>/reviews/tarefa-NN-decisoes-rN.md`).
4. **🚪 Commit:** revisores aprovarem não basta — o orquestrador apresenta o diff e os pareceres e só commita com o seu "pode commitar".
5. **🚪 Pull Request:** só você escreve a explicação, com as suas palavras, listando os apontamentos aceitos e recusados — e um **colega** aprova antes do merge.

---

## 🛑 O Portão de Entendimento (Regras de Pull Request)

Se o Pull Request for a primeira vez que você olha o código, o método falhou. Todo PR passa por uma verificação automática antes de ser mesclado, e ela confere **uma regra só**:

**O PR será REPROVADO se** a descrição não contiver a seção _"O que este PR faz e por quê"_ preenchida por você com pelo menos **250 caracteres** (não cole o _diff_ nem a saída da IA; explique com suas palavras). Vale para **todos** os PRs, inclusive os de manutenção.

A documentação anda junto do código, mas quem cobra isso não é o CI: PR de história que muda comportamento atualiza `docs/` e `specs/` no mesmo commit — é o **auditor-final** (antes do PR) e o colega que revisa que conferem.

**Exceção (Manutenção puramente técnica):**
Se a mudança não afeta o produto (ex: atualizar versão, refatorar código, arrumar formatação), você não precisa criar um `spec.md`. Abra o PR direto e aplique a etiqueta `manutencao` — a explicação de 250 caracteres continua valendo.

---

## ⚠️ Erros Comuns e Como Evitá-los

- **Inchar a Spec no meio do caminho:** Se descobrir um problema novo durante a implementação, não remende o plano em andamento. Pause, registre o problema e abra uma nova Issue focada apenas naquilo.
- **Fatiar na Horizontal:** "Criar tabela de usuários" não é uma história demonstrável. Fatie por valor: "Fazer login de usuário".
- **A Janela de Contexto Suja:** Se o agente travar, errar repetidamente ou defender ideias descartadas, descarte a sessão do chat. Comece uma nova entregando apenas o `spec.md` e o `plan.md` aprovados. O contexto limpo resolve 90% dos problemas.
- **Aceitar todos os apontamentos do revisor:** Revela que você não leu. Recusar com justificativa registrada vale mais do que aceitar tudo — e é o que a defesa presencial cobra.
