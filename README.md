# 🎓 UTF Angular Template

Template da disciplina de **Angular** (UTFPR): uma aplicação web com **Angular
20+ e BaaS** (sem backend próprio), desenvolvida em equipe com **Gitflow**. Ele
traz o método completo — comandos, agentes, workflows e guias — para você
desenvolver seu projeto com **UTF-SDD**, um **SDD por Portões** (*Gated
Spec-Driven Development*): a IA escreve o código; **você decide nos portões**,
quem revisa nunca é quem escreveu, e todo artefato é evidência para a
apresentação final.

> 📖 **Entenda o método antes de começar:**
> **[utfpr-gp.github.io/utf-angular-template](https://utfpr-gp.github.io/utf-angular-template/)**
> — o ciclo, os portões, os comandos e os papéis, explicados passo a passo.

## 🚀 Como começar

1. Clique em **Use this template → Create a new repository** (não faça fork).
   O repositório criado é seu.
2. Clone o seu repositório e abra-o na sua IDE agêntica. **Claude Code, Cursor,
   Antigravity e OpenCode já vêm configurados** — inclusive a trava que impede os
   revisores de editar arquivo. Veja *Um método, quatro ferramentas*, abaixo.
3. Leia `docs/checklist.md` — é a **ficha da disciplina**: as regras do projeto,
   os Indicadores de Desempenho (IDs) e as entregas.
4. Siga o fluxo, um comando por fase:

| Fase | Comando | Produz |
| --- | --- | --- |
| Requisitos | `/utf-prd` | `docs/prd.md` — o QUE o produto faz |
| Backlog | `/utf-backlog` | Issues no GitHub + Kanban no Projects |
| Design | `/utf-design` | `docs/design-tokens.md` + protótipo navegável — tokens, Mobile-First e identidade PWA |
| Arquitetura | `/utf-architecture` | `docs/architecture.md` — onde as coisas moram |
| Scaffold | `/utf-setup` | o app Angular, nascendo verde |
| Cada história | `/utf-issue <n>` → `/utf-task` | spec, plano e código, tarefa a tarefa |
| Aprender | `/utf-tutor` | a explicação didática de cada passo |

Bug e tarefa técnica não entram nessa tabela: eles nascem como Issue direto no GitHub
(há um modelo para cada em `.github/ISSUE_TEMPLATE/`), não têm `spec.md` e o PR leva a
etiqueta `manutencao`. O tutorial tem o passo a passo. Os modelos comentados de
`spec.md` e `plan.md` ficam em `docs/modelo-spec.md` e `docs/modelo-plan.md`.

### O primeiro prompt

Abra o chat da sua IDE e **cole o texto abaixo**. Ele confirma que o método carregou,
diz em que ponto o projeto está e qual é o próximo passo — serve no primeiro dia e em
qualquer volta depois de dias sem mexer.

```text
Sou aluno da disciplina e este repositório usa o método UTF-SDD.

Antes de qualquer outra coisa:

1. Leia `.agents/rules/utf-rules.md` — é a constituição deste repositório e vale
   para tudo o que fizermos daqui em diante.
2. Leia `docs/checklist.md` — é a ficha da disciplina: regras, indicadores e entregas.
3. Olhe `docs/`, `specs/` e o `git log`, e me responda:
   - as regras inegociáveis, uma linha cada;
   - em que fase o projeto está agora, e como você chegou a essa conclusão;
   - qual é o próximo comando que eu devo rodar, o que ele vai me perguntar e o que
     eu vou ter que decidir nele;
   - o que precisa estar commitado antes de eu rodá-lo.

Explique como se eu nunca tivesse visto este método. Não escreva código nem crie
nenhum arquivo nesta resposta.
```

Se o agente não souber recitar as regras, **elas não carregaram** — confira, na tabela
abaixo, se as pastas da sua ferramenta são mesmo as que ele lê. E, a qualquer momento,
`/utf-tutor` explica o que estiver na sua frente: `prd`, `design`, `architecture` e
`setup` na Fase 0; `spec`, `antes <n>`, `passo <n>`, `<n>` e `prova` durante as
histórias.

### Um método, quatro ferramentas

O conteúdo de verdade — constituição, fluxos e subagentes — vive uma vez só, em
`.agents/`. Cada ferramenta tem apenas uma casca de poucas linhas que aponta para
lá, com a sintaxe de permissão dela. Trocar de ferramenta no meio do semestre não
reescreve nada: o miolo é o mesmo.

| Ferramenta | Regras | Comandos | Subagentes |
| --- | --- | --- | --- |
| Claude Code | `CLAUDE.md` | `.claude/commands/` | `.claude/agents/` |
| Cursor | `.cursor/rules/` | `.cursor/commands/` | `.cursor/agents/` |
| Antigravity | `.agents/rules/` | `.agents/workflows/` | `.agents/agents/` |
| OpenCode | `AGENTS.md` | `.opencode/command/` | `.opencode/agents/` |

Nas quatro, os revisores e o tutor nascem **sem poder de escrita**; só o
implementador escreve. A trava tem forças diferentes, e vale saber qual você tem:

- **Claude Code, Cursor e Antigravity** negam a ferramenta de edição, mas precisam
  liberar o terminal para o revisor rodar `git diff`. Quem tem terminal poderia, em
  tese, escrever com `sed` ou redirecionamento — o que fecha isso ali é a proibição
  escrita no prompt do agente.
- **O OpenCode fecha por configuração:** é o único que libera comandos específicos em
  vez de ligar ou desligar o terminal inteiro. E **funciona com modelos gratuitos**,
  o que faz dele o caminho de custo zero mais completo da disciplina. Ajuste a lista
  de comandos de teste em `.opencode/agents/` à stack do seu `docs/architecture.md`:
  comando que não estiver liberado não roda, e o parecer sai incompleto sem avisar.

> Se o seu OpenCode não listar os agentes ou os comandos, é diferença de versão nos
> nomes das pastas: renomeie `.opencode/agents/` para `.opencode/agent/` e
> `.opencode/command/` para `.opencode/commands/`. O conteúdo é o mesmo.

O passo a passo detalhado está em [`docs/tutorial-sdd.md`](docs/tutorial-sdd.md);
o porquê de cada regra, em [`docs/guia-sdd.md`](docs/guia-sdd.md).
A versão navegável dos dois é o
[site do método](https://utfpr-gp.github.io/utf-angular-template/).

Pré-requisitos das integrações: **`gh` autenticado (`gh auth login`, escopos
`repo`, `workflow` e `project`) ou MCP do GitHub** — sem isso, backlog, etiquetas
e PRs não saem. **MCPs recomendados** (ID32 — configure na sua IDE): **Figma**
(o protótipo vira contexto do agente), **Supabase** (na E3) e **Context7**
(versões atuais de ferramentas antes de decidir).

---

> ✂️ **Daqui para baixo é a vitrine do SEU projeto.** Apague tudo acima desta
> linha (incluindo ela) quando o projeto tiver nome, e preencha o que segue.

# [Nome da aplicação]

[Breve descrição: o tema do semestre e o escopo/identidade da SUA equipe.]

## Autores

- [Nome completo — GitHub]

## Documentação Técnica

- [PRD](docs/prd.md) · [Architecture/SSD](docs/architecture.md) · [Checklist](docs/checklist.md)
- **Protótipo (Stitch/Figma):** [link público]

## Modelagem de Dados (Diagrama ER)

```mermaid
erDiagram
```

## Stack

- **Frontend:** Angular [versão]
- **Framework CSS:** [Tailwind, PrimeNG, …]
- **Dados:** json-server (MVP/E2) → [Supabase, PocketBase, …] (E3)
- **Bibliotecas:** [lista]

## Em produção

- **Aplicação:** [URL no Vercel/Render]

## Instruções de Execução

[Passos para configurar e rodar localmente — gerado/refinado no `/utf-setup`.]

## Telas da Aplicação

[Imagens de algumas telas.]
