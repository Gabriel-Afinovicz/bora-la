# ✅ Checklist — a ficha da disciplina

> Este arquivo é **a única fonte do que a disciplina exige**: as regras do
> projeto, os Indicadores de Desempenho (IDs) e as entregas. Os workflows
> `/utf-prd` e `/utf-architecture` o usam como régua de conferência — trocar
> de disciplina (ou de semestre) é trocar este arquivo, sem mexer no framework.
> Marque um item **somente quando conseguir explicá-lo** — item preenchido
> sem o aluno saber explicar é penalizado com a retirada da nota **deste e de
> mais um item feito**. Não altere os enunciados.
> ➡️ O detalhamento do checklist e das entregas está no Guia do Projeto no
> Notion: [link no Moodle].

## 📐 Regras da disciplina

- **Tema unificado do semestre** *(este bullet muda a cada semestre)* —
  **2026/2: gestão de Caronas na UTFPR** — uma plataforma web funcional,
  intuitiva e moderna para organizar caronas, gerenciar participantes e
  automatizar resultados. O tema é o mesmo para toda a turma; cada equipe
  imprime a própria **identidade visual, fluxo de UX e funcionalidades
  extras**. O app integra o portal **UTFApps**, como legado para a
  comunidade.
- **Equipes:** 2 ou 3 integrantes.
- **Stack fixa:** **Angular 20+** — arquitetura **standalone** (sem NgModules),
  **Signals** para estado, sintaxe moderna (`@if`/`@for`/`@switch`/`@defer`,
  `input()`/`output()`/`model()`, `inject()`).
- **Sem backend próprio — e em duas fases:** no MVP (Entrega 2) os dados vêm
  de um **json-server** (API fake local); na Entrega 3 a aplicação troca para
  um **BaaS** (ex.: Supabase, PocketBase) com autenticação (JWT) e CRUD reais.
  A escolha do BaaS é da equipe, registrada no documento técnico — e a troca
  deve atingir **só os Services**: planeje a camada de dados para isso desde o
  início (é a regra "componente não fala com o servidor" valendo dinheiro).
- **Framework CSS moderno** à escolha (ex.: Tailwind CSS, PrimeNG), com
  **Design System** próprio da equipe (tokens em `docs/design-tokens.md`).
- **UI/UX:** protótipo navegável (Stitch/Figma) com link público no
  repositório; **Mobile-First** responsivo; experiência **PWA**
  (`manifest.webmanifest`, ícones, tema, splash, standalone, estados offline).
- **Fluxo de trabalho: Gitflow** — branches `main` e `develop`; features em
  branches curtas a partir da `develop`, integradas via Pull Request;
  a `main` reflete produção. (Atenção: **não** é GitHub Flow.)
- **Deploy** em produção: Vercel, Render ou similar.
- **Desenvolvimento progressivo:** as atividades semanais são entregas
  parciais do projeto — pratique o conteúdo da semana no próprio app.
- **Documento técnico:** `docs/architecture.md` (no texto oficial da
  disciplina ele é chamado de `ssd.md` — é o mesmo documento).
- **README.md** com a estrutura exigida: título/nome do app, autores,
  descrição, links para `docs/prd.md`, `docs/architecture.md` e
  `docs/checklist.md`, **diagrama ER em Mermaid renderizado no próprio
  README**, link do protótipo (Stitch/Figma), stack (Angular, framework CSS,
  BaaS, bibliotecas), link do site em produção, instruções de execução e
  imagens de telas.

## RA1 — Design e Experiência do Usuário (UI/UX) com IA

- [ ] **ID1:** Desenvolver protótipos navegáveis (ex: gerados via Stitch e refinados no Figma) que demonstram compreensão das diretrizes de usabilidade, com link público disponibilizado no repositório.
- [ ] **ID2:** Projetar interfaces responsivas com a abordagem Mobile-First, garantindo que o layout se adapte perfeitamente a diferentes resoluções (celulares, tablets e desktops).
- [ ] **ID3:** Projetar a experiência de aplicativo nativo (PWA), configurando o manifest.webmanifest (ícones, cores de tema, splash screen e modo de exibição standalone) e prevendo o comportamento visual da interface em estados offline.

## RA2 — Componentização e UI Declarativa Moderna

- [ ] **ID4:** Desenvolver componentes utilizando estritamente a arquitetura Standalone (sem o uso de NgModules).
- [ ] **ID5:** Incorporar e customizar componentes utilizando um Framework CSS moderno (ex: Tailwind CSS, PrimeNG).
- [ ] **ID6:** Aplicar a nova sintaxe de fluxo de controle (@if / @switch) para exibição condicional de elementos.
- [ ] **ID7:** Utilizar a nova sintaxe de fluxo de controle @for com a propriedade track obrigatória para a renderização dinâmica e otimizada de coleções.
- [ ] **ID8:** Aplicar Pipes (nativos ou customizados) para formatar a apresentação de dados na interface.
- [ ] **ID9:** Implementar Deferrable Views (@defer) para otimizar a performance, carregando componentes pesados apenas sob demanda.

## RA3 — Reatividade e Gerenciamento de Estado (Signals)

- [ ] **ID10:** Aplicar técnicas de one-way data binding (Interpolação {{ }} e Property Binding [ ]) para exibir e atualizar dados, utilizando estritamente Signals (writable e computed) como fonte de estado.
- [ ] **ID11:** Aplicar técnicas de event binding ( ) para capturar interações do usuário e atualizar o estado da aplicação.
- [ ] **ID12:** Aplicar técnicas de two-way data binding utilizando a função moderna model() para sincronização bidirecional.
- [ ] **ID13:** Utilizar efeitos (effect()) para a manipulação segura de efeitos colaterais reativos.

## RA4 — Arquitetura de Software e Injeção de Dependências

- [ ] **ID14:** Utilizar as funções modernas input() e output() para a comunicação segura e tipada entre componentes em uma hierarquia (pai/filho).
- [ ] **ID15:** Criar comunicação entre componentes não relacionados hierarquicamente extraindo a lógica para Services, utilizando a função inject() em vez de injeção via construtor.

## RA5 — Roteamento e Navegação SPA

- [ ] **ID16:** Configurar rotas dinâmicas utilizando a API funcional moderna (provideRouter) e habilitar a passagem automática de parâmetros ativando a função withComponentInputBinding().
- [ ] **ID17:** Passar e consumir dados entre telas capturando os parâmetros da rota (URL) diretamente através da função moderna Signal input() no componente de destino.
- [ ] **ID18:** Criar uma estrutura de navegação aninhada (rotas filhas) para representar hierarquias de layout.
- [ ] **ID19:** Aplicar Functional Route Guards para controle de acesso (autenticação) e Resolvers para pré-carregamento de dados antes da transição da tela.

## RA6 — Integração de APIs e Assincronismo (BaaS)

- [ ] **ID20:** Realizar requisições assíncronas (GET) a uma API pública.
- [ ] **ID21:** Implementar o fluxo de Autenticação e Gerenciamento de Sessão (JWT) conectando a aplicação aos serviços de identidade do BaaS (ex: Supabase Auth).
- [ ] **ID22:** Realizar o ciclo completo de operações CRUD (GET, POST, PUT, PATCH, DELETE) conectando a aplicação a um Backend-as-a-Service (ex: Supabase, PocketBase).
- [ ] **ID23:** Implementar Functional Interceptors para injetar tokens de autenticação globalmente e tratar erros de forma centralizada.
- [ ] **ID24:** Aplicar validações em Formulários Reativos, exibindo mensagens de erro claras e desabilitando o botão de submit com base na validade do formulário.
- [ ] **ID25:** Fazer a ponte entre o assincronismo e a reatividade utilizando toSignal() e toObservable(), integrando RxJS com o ecossistema de Signals.

## RA7 — Engenharia de Software, Versionamento e DevOps

- [ ] **ID26:** Criar e gerenciar um repositório no GitHub utilizando a estrutura ágil do Gitflow (branches main e develop).
- [ ] **ID27:** Colaborar ativamente realizando integrações via Pull Requests e resolução de conflitos.
- [ ] **ID28:** Planejar, executar o processo de build moderno e realizar o deploy automatizado da aplicação em ambiente de produção (ex: Render, Vercel).

## RA8 — Engenharia de Software Assistida por IA (SDD e Orquestração)

- [ ] **ID29 — Escopo e Gestão Ágil:** Utilizar IA Generativa para a ideação e redação de User Stories. Cadastrar e gerenciar essas histórias como Issues em um Kanban no GitHub Projects.
- [ ] **ID30 — Fundações (PRD):** Apoiar-se na IA para estruturar o Documento de Requisitos do Produto (prd.md).
- [ ] **ID31 — Especificação Técnica:** A partir do PRD, instruir a IA a gerar um documento de especificação rigoroso (ssd.md — aqui, `docs/architecture.md`), detalhando explicitamente a arquitetura dos componentes Standalone e Services antes da geração do código fonte.
- [ ] **ID32 — Orquestração (MCP e Skills):** Configurar a IDE (ex: Antigravity) ativando Servidores MCP (Model Context Protocol, ex: Figma, Supabase) e utilizando Skills de Angular 20+ para que o Agente gere o código com o contexto exato do projeto.
- [ ] **ID33 — Validação e Testes (TDD):** Atuar como revisor técnico da IA. Orientar o agente a gerar testes unitários (.spec.ts) focados nas regras de negócio para validar rigorosamente a implementação gerada.

---

## 📦 As três entregas

| Entrega | O quê |
| --- | --- |
| **E1 — Concepção e Planejamento** | Escopo da equipe sobre o tema do semestre, repositório com Gitflow, README com o checklist, Design System, framework CSS, protótipo navegável no Figma |
| **E2 — Estrutura Funcional (MVP)** | Aplicação com a estrutura funcional mínima, consumindo dados do **json-server** |
| **E3 — Aplicação Completa e Apresentação** | Dados e autenticação migrados para o **BaaS** (ex.: Supabase), app completo em produção (Vercel/Render) + **vídeo** apresentando inspiração, design system, protótipo e o projeto contra o checklist |

> 📅 **As datas de entrega estão no Moodle.** Elas mudam a cada semestre, e
> data em arquivo do repositório envelhece sem ninguém perceber — a do Moodle é
> a que vale.

> 🎥 Se o vídeo for insuficiente, a apresentação é síncrona ao Professor
> (presencial ou remota). O detalhamento de cada entrega está no Guia do
> Projeto no Notion: [link no Moodle].
