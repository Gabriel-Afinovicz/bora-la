---
description: Gera a estrutura inicial do projeto a partir do docs/architecture.md — apps via geradores oficiais, scripts da raiz, template de PR, Portão de Entendimento e índice de specs. Roda UMA vez, antes da primeira Issue. É uma Task de manutenção — sem spec.
---

# Setup do projeto

Você gera a fundação sobre a qual o ciclo SDD vai rodar. Isso é uma **Task técnica**,
não uma história: não tem `spec.md`, e o PR dela recebe a etiqueta `manutencao`.

A fonte da verdade é o `docs/architecture.md`. Você **não decide stack** — você lê a
que foi decidida. O que não estiver escrito lá, você pergunta; não escolhe.

---

## Passo 0 — Pré-condições (PARE se qualquer uma falhar)

0. **Os documentos da Fase 0 estão commitados.** Rode `git status --porcelain docs/`:
   se a saída **não** estiver vazia, **PARE** e peça o commit. O scaffold vai nascer a
   partir do `architecture.md`; se ele ainda não está no histórico, o repositório não tem
   como provar qual decisão gerou qual arquivo — e é essa rastreabilidade que a avaliação
   cobra.
1. `docs/prd.md`, `docs/architecture.md` e `docs/design-tokens.md` existem. O
   `architecture.md` declara quatro coisas: o framework do frontend (versão e
   padrões), a fonte de dados de cada fase, a estrutura de pastas e como rodar os
   testes; o `design-tokens.md` traz os valores que o tema vai receber.
   Se algum desses quatro estiver ausente ou ambíguo, **PARE** e diga o que falta —
   setup com stack adivinhada é retrabalho garantido.
2. As pastas de app previstas no `architecture.md` (ex.: `apps/web`)
   **não existem** ou estão vazias — o `.gitkeep` de `apps/web/` e o `README.md`
   de reserva de `apps/api/`, que vêm do template, não contam como conteúdo.
   Se já existirem com conteúdo de verdade, **PARE**: o setup
   roda uma vez, e rodá-lo de novo por cima é destrutivo.
3. Você está na `develop`, limpa e atualizada (se a `develop` ainda não existe, crie-a a partir da `main` e publique: `git switch -c develop && git push -u origin develop` — o Gitflow da ficha exige as duas).
4. O `gh` está autenticado (`gh auth status`) **ou** o MCP do GitHub está
   disponível — sem um dos dois, a etiqueta e o Pull Request do fim não saem.
   Se faltar, **PARE** e oriente: instalar o `gh`, `gh auth login`, com os
   escopos `repo` e `workflow`.

## Passo 1 — Branch

```
git switch -c chore/setup-projeto
```

Nenhum arquivo é criado antes da branch existir. No Gitflow, `main` e `develop` são bloqueadas — a branch do setup nasce da `develop` e volta para ela por PR.

## Passo 2 — O app, pelo gerador oficial

Gere o app com o gerador oficial da stack declarada no `architecture.md`
(`ng new` para Angular, mais `ng add` para o que o documento declarar — PWA, framework CSS),
dentro da estrutura de pastas que o documento descreve.

- **A casca do monorepo é só estrutura.** Se o documento prevê `apps/web` e
  `apps/api`, gere o app em `apps/web` (o `.gitkeep` que veio do template pode
  ser removido junto) e **confira `apps/api/`**: ela vem do template com um
  `README.md` de uma linha dizendo que está reservada para uma API própria,
  se um dia existir — crie-a assim se faltar. **Não gere backend nenhum** — nem scaffold, nem
  `package.json`, nem dependência. Pasta reservada é lugar guardado; scaffold
  morto é código que ninguém mantém e que o agente lê como se existisse.

- **Antes de rodar, confirme na documentação atual** (MCP Context7, se
  disponível) a versão corrente de cada CLI e sua compatibilidade com o Node
  instalado — gerador desatualizado ou incompatível descoberto no meio do passo
  é retrabalho.
- Desative o `git init` interno do gerador — o repositório é um só, na raiz.
- **Gere com a instalação de dependências desligada** (`--skip-install` no
  `ng new`). O `package.json` da raiz, que declara os workspaces, só nasce no
  Passo 3 — e um `npm install` disparado antes dele cria um `node_modules`
  próprio dentro de `apps/web`, sem hoisting. A instalação acontece uma vez só,
  na raiz, no fim do Passo 3. Se algum `ng add` precisar rodar antes disso,
  faça a instalação da raiz primeiro.
- Aceite os padrões do gerador. Não adicione biblioteca que o `architecture.md`
  não menciona.
- **Os tokens do design viram CSS.** Instalado o framework, o
  `docs/design-tokens.md` precisa chegar ao arquivo de estilo global do app — é
  lá que o Tailwind lê. Enquanto isso não acontece, o documento é decoração: o
  aluno decidiu a paleta e o app continua com as cores de fábrica.
  **Pergunte antes de escrever:** *"escrevo o bloco de tema a partir do
  `design-tokens.md`, ou você prefere escrever?"*
  - Se ele escrever, espere e confira ao fim contra o documento.
  - Se você escrever: transcreva **só o que está no documento** — cor,
    tipografia, espaçamento, arredondamento, breakpoints — para os namespaces do
    Tailwind (`--color-*`, `--font-*`, `--text-*`, `--radius-*`, `--spacing`,
    `--breakpoint-*`) dentro de `@theme`, **mantendo os nomes semânticos que a
    equipe deu** (`--color-primary`, nunca `--color-azul-2`). Se o documento
    declara uma biblioteca de componentes com mecanismo próprio de tema
    (daisyUI, por exemplo), use o dela em vez do `@theme` cru — dois lugares
    declarando cor é a duplicação que o método existe para evitar.
  - **Valor que não estiver no documento, você não inventa** — pergunte. Cor
    escolhida por modelo de linguagem é exatamente o que o `/utf-design` existe
    para impedir; refazer isso aqui anula a atividade inteira.

  Nos dois casos, **só o tema**: nenhuma tela, componente ou classe de negócio.

- **O linter não vem no `ng new`.** Se o `architecture.md` declara um comando de
  lint (e ele declara — §2 exige os comandos exatos de suíte e lint), instale o
  oficial: `ng add angular-eslint`. Ele gera o `eslint.config.js` e cria o alvo
  `lint` no `angular.json`. Sem isso, a prova de vida do Passo 5 roda um comando
  que não existe.
- Se o `architecture.md` declara ferramenta de teste diferente do padrão do gerador,
  siga o documento; se não declara, fique com o padrão do gerador e **relate isso no
  fim** como decisão que o usuário precisa ratificar no `architecture.md`.

## Passo 3 — A raiz

1. `.gitignore` da raiz cobrindo `node_modules/`, artefatos de build (`dist/`,
   `build/`, `.angular/`, `coverage/`) e `.env` — **antes do primeiro
   `git add`**. Confira o que os geradores deixaram: com `--skip-git`, alguns
   não criam `.gitignore` próprio — não é esquecimento, o da raiz cobre o projeto. Valide com `git status --short`: se aparecerem
   milhares de arquivos, o ignore não cobriu algo. (A IDE mostrar uma
   avalanche de untracked **entre** a geração e este passo é normal — ela
   some aqui.)
2. `.gitattributes` com:

   ```
   * text=auto eol=lf
   ```

   Sem isso, um repositório tocado em Windows e Linux reescreve todos os arquivos a
   cada troca de máquina, e o diff de qualquer PR vira ruído.

3. `package.json` da raiz — **a raiz do npm workspace**, no formato do §3.1 do
   `architecture.md`: `"private": true`, `"workspaces": ["apps/*"]` e os scripts de
   orquestração (`start`, `build`, `test`, `lint`, `api`). É ele que dá um
   `npm install` único para o repositório inteiro e poupa o aluno de entrar em
   `apps/web` a cada comando — a flag `-w apps/web` faz isso por ele. Se o documento
   traz os scripts prontos, copie-os literalmente.

   **Depois de criá-lo, rode `npm install` na raiz** — é esta instalação que junta as
   dependências num `node_modules` só. Se `apps/web/node_modules` existir (geração
   feita sem `--skip-install`), remova antes de instalar.

   Dois casos desta disciplina:
   - **json-server declarado para o MVP:** adicione a dependência, o script
     (`"api": "json-server db.json"` ou equivalente) e um `db.json` **vazio de
     negócio** (`{}`) — as entidades chegam pelas histórias, nunca pelo setup.
   - **PWA declarado:** rode `ng add @angular/pwa` e preencha o
     `manifest.webmanifest` com a identidade decidida no `/utf-design`
     (nome curto, cores, ícones) — sem inventar valores.

4. **Formatação, na raiz — regra do projeto, não preferência de quem digita.**

   Na raiz (sem `-w`, para ficarem como dependência da raiz e hoisted para todos
   os apps):

   ```
   npm install -D prettier eslint-config-prettier
   ```

   Mais um `.prettierrc` (pode nascer `{}` — o padrão do Prettier serve) e um
   `.editorconfig` com `indent_style`, `indent_size`, `end_of_line = lf` e
   `insert_final_newline`. Acrescente ao `package.json` da raiz:
   `"format": "prettier --write ."`.

   O `eslint-config-prettier` desliga as regras de estilo do ESLint que brigariam
   com o Prettier — a config recomendada do angular-eslint inclui
   `tseslint.configs.stylistic`, então o conflito é real, não teórico. Aplique-o
   **por último** no `eslint.config.js`.

   Estes arquivos são o que o agente lê para saber como escrever: sem eles, cada
   integrante formata de um jeito e todo Pull Request vira ruído de espaço em
   branco. **Não instale extensão de IDE por eles** — extensão é da máquina de
   cada um; aqui o que se versiona é a regra.

## Passo 4 — As ferramentas do método

O `.github/` **já vem no template**: `pull_request_template.md` e
`workflows/portao-de-entendimento.yml` estão na `main` desde o primeiro commit.
Você não os gera — você **confere** que existem. Copiar YAML de dentro da prosa
do guia é exatamente como o Portão nasce parafraseado e sem efeito.

1. Confira que `.github/pull_request_template.md` e
   `.github/workflows/portao-de-entendimento.yml` existem. Se faltar algum,
   **PARE** e avise: o repositório não veio do template, e os dois são
   pré-requisito do método — não subproduto do setup.
2. A etiqueta de manutenção no GitHub (é ela que marca PRs sem spec, a começar
   pelo deste setup):

   ```
   gh label create manutencao --description "PR tecnico, sem spec" --color FBCA04
   ```

3. **Proteção das branches `main` e `develop`.** Sem isso, *"a `main` é sagrada"* é a única
   regra da constituição sem mecanismo nenhum — e proteção de branch **não é
   herdada de repositório template**, então cada aluno precisa ligar a dele.
   Proponha ao usuário e, com o OK, rode:

   ```
   gh api -X PUT repos/{owner}/{repo}/branches/main/protection --input - <<'JSON'
   {
     "required_status_checks": { "strict": false, "checks": [ { "context": "explicacao" } ] },
     "enforce_admins": true,
     "required_pull_request_reviews": { "required_approving_review_count": 1 },
     "restrictions": null,
     "allow_force_pushes": false,
     "allow_deletions": false
   }
   JSON
   ```

   `required_approving_review_count: 1` exige **Pull Request aprovado por um
   colega** — nesta disciplina o projeto é em equipe (2–3), e a revisão entre
   colegas com resolução de conflitos é cobrada pelo ID27: quem abre a story
   não mergeia o próprio PR. O bloco `required_status_checks` exige que o check
   `explicacao` (o job do Portão de Entendimento) **passe antes do merge** — sem
   ele, o Portão reprovaria mas não bloquearia nada.
   `enforce_admins: true` faz a regra valer também para o
   dono do repositório: sem isso, o aluno é justamente quem fura a regra sem
   perceber. Para destravar uma emergência ele desliga a proteção
   conscientemente, e isso fica registrado no log do repositório.

   **Repita o mesmo comando trocando `main` por `develop`** — no Gitflow as
   duas são protegidas: features entram na `develop` por PR, e a `main` só
   recebe releases vindas da `develop`.

   **Se a API recusar**, quase sempre é conta gratuita com repositório privado —
   proteção de branch exige repositório público ou plano pago. Relate e siga;
   não insista.
4. `specs/README.md` — o índice de specs, com a tabela vazia:

   ```markdown
   # Índice de specs

   | Issue | Spec | Estado | Observação |
   | --- | --- | --- | --- |
   ```

## Passo 5 — Prova de vida

Rode a suíte de testes de **cada** app e o lint, com os comandos da raiz.

O scaffold precisa nascer **verde**. É esse verde que dá sentido ao RED do TDD a
partir da primeira tarefa: um teste que falha só é informação num repositório onde
os testes comprovadamente rodam.

**Mesmo princípio das 2 rodadas:** se um gerador ou a suíte falhar duas vezes pelo mesmo
motivo, **PARE** e relate. Não tente uma terceira abordagem.

## Passo 6 — Entrega

1. Commits pequenos e nomeados por passo (apps, raiz, ferramentas do método) —
   **cada um proposto ao usuário antes** ("commit do passo X: <mensagem>?"),
   nenhum sem o OK dele.
2. **Despache o tutor em modo `setup`, antes do PR.** Este é o único momento do
   semestre em que o aluno recebe um monte de arquivos que ele não escreveu e não
   viu nascer — se ninguém explicar, ele abre o primeiro PR sem saber o que tem
   dentro do próprio repositório. Não pergunte se ele quer: despache, apresente a
   explicação na íntegra e só então siga. O despacho leva `docs/architecture.md`,
   `docs/design-tokens.md`, a lista de arquivos gerados (incluindo o arquivo de
   estilo global, com o tema) e a saída dos testes.
3. Relate ao usuário: o que foi gerado, a saída dos testes, e as decisões que o
   `architecture.md` não cobria (Passo 2) para ele ratificar no documento.
   **Ratificação aprovada pelo usuário = atualize o `architecture.md` na mesma
   branch**, antes do PR — documento e scaffold entram juntos, contando a mesma
   história.
4. Instrua o usuário a abrir o PR com a etiqueta **`manutencao`** — setup é Task,
   não história. O corpo já vem preenchido pelo
   `.github/pull_request_template.md`, que está na `main` desde o template.
   Explique o detalhe que ninguém adivinha:
   - **`Closes #<n>` no corpo do PR** liga o PR à Issue e a fecha no merge — é
     esse elo que fecha a rastreabilidade Issue → spec → código exigida na
     avaliação. O PR do setup não fecha Issue nenhuma, então não leva
     `Closes` — mas todo PR de história leva.

---

## Proibições

- **Nenhuma entidade, endpoint, tela, modelo ou migration de negócio.** Se um nome
  do glossário do PRD aparecer em código gerado por você, você passou do ponto — o
  negócio começa na primeira Issue, pelo `/utf-issue`, com spec aprovada.
- Nada de CI além do Portão de Entendimento — esteira de testes é Issue própria.
- Não editar `docs/prd.md` nem `docs/architecture.md`. Se encontrar contradição
  entre eles, relate; não resolva por conta própria.
