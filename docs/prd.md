# 📄 Product Requirements Document (PRD)

**Projeto:** [nome]
**Versão:** 0.0.0 · esqueleto — preencha via `/utf-prd`
**Última atualização:** [data]

> 🤖 **Este documento é a fonte da verdade sobre o QUE o produto faz.** Regra de
> negócio que não estiver aqui não existe — nem para a equipe, nem para a IA.
> Tecnologia **não** se discute aqui: isso é assunto do `architecture.md`.
>
> ✍️ **Não preencha na mão:** rode `/utf-prd` — a entrevista percorre as seções
> abaixo, na ordem, e confere o resultado contra a ficha da disciplina
> (`docs/checklist.md`). As respostas são suas; o agente só organiza.

---

## 🎯 1. Visão Geral e Objetivo

**O problema:** [quem sofre o quê, hoje]

**A solução:** [o que o produto faz a respeito, em um parágrafo]

**Como saberemos que deu certo:** [comportamento observável, não métrica inventada]

---

## 📖 2. Glossário Ubíquo

> Os termos do negócio, como o cliente fala. É daqui que o `architecture.md`
> deriva os nomes das entidades.

| Termo | Significa | Não confundir com |
| :---- | :-------- | :---------------- |
| | | |

---

## 👤 3. Atores e Permissões

> ⚠️ A coluna **"Não pode"** vira Guard na rota e regra de acesso no BaaS
> (ex.: RLS no Supabase).

| Ator | Quem é | Pode | Não pode |
| :--- | :----- | :--- | :------- |
| | | | |

---

## 📝 4. Escopo Funcional (User Stories)

> Uma story por vez, no formato do modelo abaixo. Cada uma carrega dois eixos:
> **Prioridade (MoSCoW)** — `Must Have` é o escopo comprometido do projeto
> (o escopo mínimo da ficha é `Must Have` por definição); `Should`/`Could`
> entram se sobrar tempo, mas ficam documentadas — nada se perde; o
> `Won't Have` vira item da seção *Fora de Escopo* — e **Tamanho (esforço)** —
> `S` cabe numa sessão, `M` vira algumas tarefas no plano, `L` pede divisão.
> O status percorre `Draft` → `Ready` → `Live`: toda story nasce `Draft` —
> **só você promove a `Ready`** — e vira `Live` quando o PR dela é mesclado
> (o auditor final cobra essa atualização; o commit é seu).

### US01 — [título] · `Must|Should|Could Have` · `S|M|L` · Status: `Draft`

**Como** [perfil], **eu quero** [ação] **para que** [objetivo].

**Critérios de aceite:**

- [ ] **Dado** [contexto], **quando** [ação], **então** [resultado verificável].
- [ ] **Dado** [o caminho triste: erro, vazio, abandono], **quando** …, **então** …

**Regras relacionadas:** RNnn

---

## 🛡️ 5. Regras de Negócio (Constraints)

| ID | Regra |
| :-- | :---- |
| RN01 | |

---

## 🚫 6. Fora de Escopo (Non-goals)

> O que o produto deliberadamente **não** faz neste semestre — o `Won't Have`
> do MoSCoW, com o motivo de cada corte.

-

---

## ⚙️ 7. Requisitos Não Funcionais (Qualidade)

> Só os que você consegue justificar na defesa.

-

---

## 🛠️ 8. Histórico

| Data | Versão | O que mudou |
| :--- | :----- | :---------- |
| | 1.0.0 | Versão inicial via `/utf-prd` |
