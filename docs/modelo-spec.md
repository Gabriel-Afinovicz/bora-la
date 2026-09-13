---
issue: 00
status: rascunho   # rascunho | aprovada
---

<!-- 📐 MODELO — não é uma spec de verdade, e por isso mora em `docs/`, e não em
     `specs/` (lá só existem specs reais, uma pasta por história).
     O `/utf-issue <n>` copia esta estrutura para `specs/<numero-da-issue>-<slug>/spec.md`.
     Leia os comentários: eles dizem por que cada seção existe, e são apagados na cópia. -->

# US00 — [título da história]

## O problema

<!-- Um parágrafo: quem sofre o quê hoje, e por que isso importa. Sem solução ainda. -->

## A história

**Como** [perfil], **eu quero** [ação] **para que** [objetivo].

<!-- O mesmo texto da story no `docs/prd.md`. Se divergir, o PRD é quem manda. -->

## Critérios de aceite

<!-- ✅ O teste de cada critério: você consegue imaginar um teste automatizado que o prove?
     Se não, ele está vago demais. "A tela deve funcionar bem" não é critério.
     Numere-os: o `plan.md` e os pareceres de revisão citam esses números. -->

- [ ] **CA1 — Dado** [contexto], **quando** [ação], **então** [resultado verificável].
- [ ] **CA2 — Dado** [o caminho triste: erro, lista vazia, dado inválido], **quando** …, **então** …
- [ ] **CA3 — Dado** [abandono: fechou a aba, sessão expirou, rede caiu], **quando** …, **então** …

<!-- ⚠️ Todo caso de abandono da seção abaixo precisa reaparecer aqui como critério.
     Só o critério vira teste; o que fica só na prosa não é verificado por ninguém. -->

## Fora de escopo

<!-- O que esta história deliberadamente NÃO faz. É isto que impede a spec de inchar
     no meio do caminho, e é contra isto que o auditor final confere o diff. -->

-

## Abandono no meio

<!-- O raciocínio, em prosa, que gerou os critérios de abandono acima.
     As telas desta história estão no protótipo e os pontos de desistência estão
     registrados no `docs/prd.md`: cada um deles entra aqui.
     Perguntas que costumam achar o buraco: o que acontece se a pessoa fechar a aba
     agora? se a conexão cair no meio do envio? se a sessão do BaaS expirar? se a
     resposta nunca chegar e a tela ficar carregando para sempre? -->

## Assume que

<!-- Premissas que ainda NÃO são verdade — o "seguir com substituto" do guia.
     Cada linha precisa de: (1) o que é mentira, (2) a Issue que a desfaz, e
     (3) um `// TODO #<issue>` no código. O auditor final confere as três.
     O caso mais comum aqui: a coleção ainda vive no json-server e só migra
     para o BaaS na E3. -->

| Premissa | Issue que fecha |
| --- | --- |
| | |

## Depende de

<!-- Issues que precisam estar na `develop` antes desta. Se estiver vazio, apague a seção.
     Se o que falta não pode ser demonstrado para alguém que não programa, não é
     dependência: é fatia horizontal, e as duas histórias deveriam ser uma só. -->

## Dúvidas em aberto

<!-- 🚪 Esta seção precisa estar VAZIA antes de você aprovar.
     Dúvida aberta aqui vira decisão inventada pelo implementador depois. -->

| # | Dúvida | Quem responde |
| --- | --- | --- |
