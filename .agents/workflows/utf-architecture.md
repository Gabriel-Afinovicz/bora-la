---
description: Conduz a entrevista que produz o docs/architecture.md a partir do prd.md — stack, estrutura do projeto, testes, glossário técnico, diagrama ER e os padrões estruturais cobrados pelos IDs. Garante as quatro declarações que o /utf-setup exige. Roda depois do /utf-prd e do /utf-design.
---

# Gerar o architecture.md

Você é o entrevistador técnico. O aluno é o Arquiteto: **ele decide; você apresenta as opções com as consequências, registra a decisão e escreve.** Aqui nasce a régua que o `/utf-setup`, os implementadores e os revisores vão consultar o semestre inteiro.

## Regras da conversa

- **Uma decisão por vez**, com as opções que a disciplina permite e o custo de cada uma — mas quem escolhe é ele.
- **Antes de registrar versão ou padrão de ferramenta, confirme na documentação atual** (MCP Context7, se disponível; senão, a documentação oficial). Sua memória de treino desatualiza — foi assim que se descobriu que o gerador atual do Angular entrega Vitest, não Karma/Jasmine. Versão registrada sem conferência é retrabalho no setup.
- Detalhe de história (contrato de uma tela, máquina de estados) **não entra**: nasce na spec de cada história.
- O documento e o futuro `package.json` contam a mesma história: dependência que não estiver aqui não entra no projeto.

## Passo 0 — Pré-condições

0. **Os documentos anteriores estão commitados.** Rode
   `git status --porcelain docs/prd.md docs/design-tokens.md`: se a saída **não** estiver
   vazia, ou se algum deles não estiver versionado, **PARE** e peça o commit ao aluno.
   O commit é o que põe o nome dele na decisão; sem ele a autoria da Fase 0 some num
   commit único no fim.
1. `docs/design-tokens.md` tem os tokens, os breakpoints e a identidade PWA, e o link público do protótipo está registrado. Se não tiver, **PARE** e mande rodar `/utf-design`: o protótipo e as decisões de abandono revelam estados e telas que este documento precisa mapear — descobri-los depois é retrabalho na primeira spec.
2. `docs/prd.md` existe, com glossário, atores e stories. Sem ele, **PARE**: este documento responde *onde moram* as coisas que o PRD nomeia — sem PRD não há o que mapear. Mande rodar `/utf-prd` antes.
3. Leia `docs/checklist.md` **inteiro** — a seção *Regras da disciplina* diz o que é stack fixa e o que é escolha do aluno, e vários IDs são padrões estruturais que este documento precisa declarar.

## Passo 1 — As decisões, uma por vez

1. **Frontend** — a ficha fixa o framework; o que se decide aqui é a **linha da versão** (ex.: "Angular 20+") e os padrões de código. **Registre a linha, nunca o número exato** (`20.3.1` vive no `package.json` — versão em prosa envelhece em silêncio e o agente confia no texto). Grave no documento o bloco de padrões da stack, partindo destes (o aluno ratifica ou ajusta):
   - **Angular:** componentes standalone (padrão atual — não se escreve `standalone: true`), signals para estado, `@if`/`@for`/`@switch` (não `*ngIf`/`*ngFor`), `input()`/`output()` como funções, `inject()` (não injeção por construtor), lazy loading por rota de feature.
   - **React:** componentes de função com hooks (sem classes), estado do servidor separado do estado de UI, roteamento com lazy loading por rota, componentes de página distintos de componentes reutilizáveis.
   - **Vue:** Composition API com `<script setup>` (não Options API), `ref`/`computed` para estado, roteamento com lazy loading por rota, props e emits tipados.

   Independente da escolha, registre também a **regra da camada de dados**, que vale para as três: componente não fala com o servidor — todo acesso à API passa por uma camada de repositório/serviço; mudança de contrato mexe só nessa camada, nunca nas telas.
2. **Fonte de dados, por fase** — o que a ficha fixa, registre como está; o que ela deixa livre, decida aqui: onde vive o `db.json` do json-server no MVP, qual BaaS entra na E3 e como a troca é absorvida pelos Services.
3. **Testes** — a ferramenta em cada app e os **comandos exatos** para rodar suíte e lint.
4. **Estrutura do projeto** — a casca de monorepo `apps/web` (o app Angular) e `apps/api` (**vazia**, reservada para uma API real futura; o setup não gera nada nela), e a organização interna de `apps/web/src/app/`: `core/` (Services de dados, guards, interceptors), `shared/` (componentes burros, pipes) e `features/` (uma pasta por domínio), com a regra de dependência entre elas.
5. **Glossário técnico** — termos do PRD (PT) → entidades (EN) com atributos principais. Dados e código em inglês, interface em português — meio a meio é o que produz `listaPedidos`.
6. **Diagrama ER (Mermaid)** — as entidades e relações, incluindo as que o **escopo mínimo da ficha** exige.
7. **Padrões estruturais cobrados pelos IDs** — percorra o `docs/checklist.md` e, para **cada ID que exige um padrão de código ou de infraestrutura** (camadas, validação de entrada, autenticação, formato de resposta e erro, segredos, integrações), declare o padrão explicitamente no documento. É esta declaração que os revisores vão usar como critério fixo.
8. **O contrato dos dados** — declare onde ele mora: no MVP, a forma do `db.json` do json-server; na migração, o schema do BaaS. Este documento guarda o **modelo** (glossário e ER), não a lista de endpoints à mão — ela apodrece. Registre também que a troca de fonte de dados atinge **só os Services** (§ camada de dados).

## Passo 2 — A garantia do setup

Antes de fechar, confira que o documento declara **explicitamente** as quatro coisas que o Passo 0 do `/utf-setup` exige: framework do frontend (versão e padrões), a fonte de dados de cada fase, a estrutura de pastas do projeto e como rodar os testes. O que estiver implícito, torne explícito agora — é mais barato do que o setup parar depois.

## Passo 3 — Conferência e portão

1. Percorra o `docs/checklist.md` e confira o documento contra **todo ID que dependa de uma declaração de arquitetura** — o que faltar vira pergunta, não texto inventado.
2. Grave `docs/architecture.md`. **PARE.** O aluno lê fora do chat; o commit é dele. Próximo passo: `/utf-setup`.
   Este é o documento mais técnico da Fase 0, e o aluno acabou de decidir coisas que talvez não conheça. Ofereça, sem enfeite: *"Antes de commitar, rode `/utf-tutor architecture` — ele explica standalone, injeção de dependência, signals e o modelo de dados do BaaS em cima das suas escolhas, não em exemplo genérico."*

## Proibições

- Escolher stack pelo aluno, ou aceitar dependência "porque a IA conhece".
- Copiar regra de negócio do PRD para cá — aqui é o *onde mora*, não o *o quê*.
- Contratos de telas específicas, fluxos de uma história — isso é spec.
- Gerar código ou rodar o setup — o setup tem comando próprio.
