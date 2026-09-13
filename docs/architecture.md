# 🛠️ Architecture / SSD

**Projeto:** [nome]
**Versão:** 0.0.0 · esqueleto — preencha via `/utf-architecture`
**Última atualização:** [data]

> 🤖 **O `prd.md` responde _o quê_ o produto faz. Este responde _onde as coisas
> moram e como se chamam_.** Detalhe de tela — rota, componente, contrato —
> **não** se decide aqui: isso é trabalho da spec de cada história.
> (No texto oficial da disciplina este documento é o `ssd.md` — ID31.)
>
> ✍️ **Não preencha na mão:** rode `/utf-architecture` (depois do `/utf-prd`).
> A entrevista decide com você cada seção e garante o que o `/utf-setup` exige:
> **framework do frontend, o BaaS, a estrutura do projeto e como rodar os
> testes.**

---

## 🤖 1. Fontes de Contexto para a IA

> Onde a IDE agêntica busca a verdade. **Isto é o índice; a configuração mora
> nos arquivos** — documento não configura ferramenta.

| Fonte | Onde configurar | Serve para |
| :---- | :-------------- | :--------- |
| Constituição da IA | `.agents/rules/utf-rules.md` (via `CLAUDE.md`) | Regras inegociáveis: fases do SDD, 2 rodadas, revisores distintos, Gitflow |
| Fluxos da IA | `.agents/workflows/` | PRD, backlog, design, architecture, setup, ciclo por Issue, ciclo por tarefa, tutor |
| Agentes (subagentes) | `.agents/agents/` (cascas em `.claude/`, `.cursor/`, `.opencode/`) | Implementador, revisores, auditor final e tutor |
| Ficha da disciplina | `docs/checklist.md` | Regras do projeto, IDs e entregas |
| Protótipo (Stitch/Figma) | [link público] | Telas, jornadas e hierarquia visual (ID1) |
| MCPs da IDE | [ex.: Figma, Supabase, Context7] | Contexto exato do projeto para o agente (ID32) |

---

## 📦 2. Stack Tecnológica

> Definição **estrita**: nenhuma dependência entra sem aparecer aqui. Esta
> seção e o `package.json` contam a mesma história, ou o projeto já se perdeu.
> O que a ficha fixa entra como está; o que ela deixa livre é decidido na
> entrevista.

- **Frontend:** Angular [versão 20+] — standalone, signals, zoneless [conforme a versão].
- **Padrões de código exigidos** (a ficha cobra cada um — IDs 4, 6, 7, 9, 10–15):
  `@if`/`@switch`, `@for` com `track`, `@defer`, signals (writable/computed) como
  fonte de estado, `model()` para two-way, `effect()` para efeitos colaterais,
  `input()`/`output()`, `inject()`, Pipes para formatação.
- **Framework CSS:** [Tailwind, PrimeNG, …] (ID5)
- **Dados (em duas fases):** **json-server** no MVP (E2) → **[Supabase, PocketBase, …]** na E3, com autenticação (JWT) e CRUD reais (IDs 21–22). A troca atinge só os Services (§2.1).
- **PWA:** `manifest.webmanifest` — ícones, cores de tema, splash, standalone, offline (ID3)
- **Testes e lint:** [ferramenta do gerador] + comandos exatos de suíte e lint (ID33). O linter não vem no `ng new`: o setup instala o oficial (`ng add angular-eslint`), mais Prettier e `eslint-config-prettier` na raiz.

### 🌐 2.1. Camada de dados — regras estruturais

> Declaradas uma a uma na entrevista, percorrendo os IDs da ficha.

- **Componente não fala com o servidor:** todo acesso a dados passa por
  **Services** injetados via `inject()` (ID15) — mudança de contrato mexe só
  neles, nunca nas telas. **É esta regra que paga a migração da E3:** trocar o
  json-server pelo BaaS reescreve os Services, e nenhuma tela.
- **Autenticação e sessão (JWT):** [fluxo com o serviço de identidade do BaaS — ID21]
- **Interceptors funcionais:** token injetado globalmente + tratamento
  centralizado de erros (ID23).
- **Assíncrono ↔ reativo:** `toSignal()`/`toObservable()` na fronteira entre
  RxJS e signals (ID25).
- **Formulários Reativos** com validação, mensagens claras e submit
  desabilitado quando inválido (ID24).

---

## 🗂️ 3. Estrutura do Projeto

```text
.
├── .agents/               # constituição, workflows e prompts dos agentes (§1)
├── CLAUDE.md / AGENTS.md  # cascas por ferramenta (.claude/, .cursor/, .opencode/)
├── README.md              # a vitrine, na estrutura exigida pela ficha
├── docs/                  # prd.md, este arquivo, design-tokens.md, checklist.md e guias
├── specs/                 # uma pasta por história implementada
├── package.json           # a raiz do workspace: scripts de orquestração (§3.1)
└── apps/
    ├── web/               # o app Angular — package.json próprio
    └── api/               # reservada para uma API real, se um dia existir
```

> 📌 **Por que `apps/` com duas pastas se só uma tem código.** Nesta disciplina os
> dados vêm do json-server e depois do BaaS: não há backend para escrever. Mas a
> casca do monorepo custa nada agora e evita mover o projeto inteiro no dia em que
> uma API própria fizer sentido. **`apps/api/` nasce vazia, e continua vazia** — o
> setup não gera backend nenhum; ela só guarda o lugar (e o `db.json` do
> json-server, se o documento assim declarar).

### 📦 3.1. A raiz do monorepo (npm workspaces)

O `package.json` da raiz declara os subprojetos e concentra os comandos:

```json
{
  "name": "[nome-do-projeto]",
  "private": true,
  "workspaces": ["apps/*"],
  "scripts": {
    "start": "npm run start -w apps/web",
    "build": "npm run build -w apps/web",
    "test":  "npm run test -w apps/web",
    "lint":  "npm run lint -w apps/web",
    "format": "prettier --write .",
    "api":   "json-server db.json"
  }
}
```

> 📌 **O que os workspaces resolvem aqui.** Um `npm install` na raiz instala as
> dependências de todos os `apps/*` de uma vez, num `node_modules` só: quem clona o
> repositório roda **um** comando, não um por pasta. A flag `-w` executa um script
> dentro de um subprojeto sem `cd`. O curinga `apps/*` faz qualquer pasta nova ali
> dentro ser reconhecida sem editar este arquivo, e `"private": true` impede a
> publicação acidental no npm. **`apps/api/` não tem `package.json` e é ignorada pelo
> npm** — nada a fazer nela.

### Organização interna do app (`apps/web/src/app/` — feature-driven)

[decidido na entrevista: `core/` (singletons: guards, interceptors, services de
dados), `shared/` (componentes burros, pipes), `features/` (uma pasta por
domínio) — com a regra de dependência: features não importam umas das outras.]

---

## 🧭 4. Roteamento e Navegação

> A ficha cobra a API funcional moderna (IDs 16–19): `provideRouter` com
> `withComponentInputBinding()`, parâmetros de rota via Signal `input()`,
> rotas filhas para hierarquia de layout, Functional Guards e Resolvers.

[mapa inicial de rotas nasce aqui; cada história nova preenche uma linha no §6]

---

## 🗄️ 5. Arquitetura de Dados (BaaS)

### 📖 5.1. Glossário Técnico (Mapeamento)

> A ponte entre o português do negócio (PRD §2) e o inglês do código.
> **Dados e código em inglês, interface em português.**

| Termo PRD (PT-BR) | Entidade/Tabela (EN) | Atributos principais |
| :---------------- | :------------------- | :------------------- |
| | | |

### 📊 5.2. Diagrama ER (Mermaid)

> As tabelas do BaaS e seus relacionamentos — o mesmo diagrama vai renderizado
> no README, como a ficha exige.

```mermaid
erDiagram
```

### 🔒 5.3. Segredos e ambientes

> Chaves do BaaS: a *anon key* pública vive em `environment.ts` (é pública por
> design — a segurança vem das regras de acesso do BaaS, ex.: RLS no Supabase);
> **service keys e segredos nunca entram no repositório**.

| Fase | App roda em | Dados |
| :--- | :--- | :--- |
| **Local (E2/MVP)** | `ng serve` | json-server (`db.json` local) |
| **Local (E3)** | `ng serve` | [BaaS — projeto de dev] |
| **Produção (E3)** | [Vercel/Render] | [BaaS — projeto de produção] |

---

## 🗺️ 6. Mapa de Domínios e Rotas

> **Este índice cresce.** Não é para preencher agora: **uma linha por história
> implementada** — a spec é que define rota e contrato. Aqui fica só o mapa de
> quem já existe.

| Domínio | Rota | Guard | Dados (service) | US |
| :------ | :--- | :---- | :-------------- | :-- |
| | | | | |

---

## 📅 7. Histórico

| Data | Versão | O que mudou |
| :--- | :----- | :---------- |
| | 1.0.0 | Versão inicial via `/utf-architecture` |

---

## 🛑 O que ainda **não** está neste documento

Detalhes de funcionalidade — contratos de uma tela específica, máquinas de
estado de uma história — **não entram aqui**: nascem sob demanda no `spec.md`
de cada história. Este documento guarda só o que vale para o sistema inteiro.
