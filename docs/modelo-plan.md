<!-- 📐 MODELO — não é um plano de verdade, e por isso mora em `docs/`, e não em `specs/`.
     O `/utf-issue <n>` gera `specs/<numero-da-issue>-<slug>/plan.md` com esta forma,
     depois de você aprovar a spec. O `/utf-task` lê as caixas `- [ ]` daqui para saber
     qual é a próxima tarefa, e marca `- [x]` quando você autoriza o commit. -->

# Plano — Issue #00

## Decisões técnicas

<!-- O que precisa estar decidido antes da primeira linha de código: componentes e
     services novos, rotas afetadas, signals de estado, e o contrato com a fonte de
     dados (quais campos a coleção do json-server ou a tabela do BaaS precisa ter).
     Nada de regra de negócio (isso é a spec) nem de padrão global (isso é o
     `docs/architecture.md`). -->

-

## Tarefas

<!-- 📏 O tamanho certo: cada tarefa é UM critério de aceite inteiro, ponta a ponta,
     ou um passo técnico que sozinho não prova nada mas destrava o próximo.
     Se ao descrever você usa "e" duas vezes, são duas tarefas.
     Mais de 10 tarefas: a história é grande demais, divida em duas Issues.
     Toda tarefa começa pelo teste (RED), então toda tarefa cita o critério que prova. -->

- [ ] **Tarefa 1 — [título curto, um verbo]**
  - Cobre: CA1
  - Teste primeiro: [o que o `.spec.ts` vai provar, e por que ele falha antes do código]
  - Arquivos previstos: [caminhos]

- [ ] **Tarefa 2 — [título]**
  - Cobre: CA2
  - Teste primeiro:
  - Arquivos previstos:

<!-- ⚠️ Confira antes de aprovar: TODO critério de aceite da spec aparece em alguma
     tarefa? Critério sem tarefa passa em todas as revisões — nenhuma delas está
     olhando para o que ninguém implementou. O auditor final pega isso, mas tarde. -->

## Critérios sem tarefa

<!-- Precisa ficar vazio. Se sobrou critério aqui, falta tarefa. -->

-
