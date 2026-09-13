# 🎨 Tokens de Design

**Projeto:** [nome]
**Versão:** 0.0.0 · esqueleto — preencha via `/utf-design`
**Última atualização:** [data]

> 🤖 **Este documento existe para a IA parar de inventar um botão diferente a cada
> tela.** Não é um design system — é o mínimo que dá à prototipagem assistida algo a
> que obedecer.
>
> ✍️ **Não preencha na mão:** rode `/utf-design`.

---

## Paleta

Nome semântico, nunca `azul-2` — a cor muda, o papel dela não.

| Token | Valor | Onde se usa |
| --- | --- | --- |
| `primaria` | | ação principal |
| `superficie` | | fundo de card e painel |
| `texto` | | texto padrão |
| `texto-suave` | | legenda, apoio |
| `perigo` | | erro, exclusão |
| `sucesso` | | confirmação |
| `desabilitado` | | controle inativo |

## Escala de espaçamento

Uma progressão só, usada em tudo.

| Token | Valor |
| --- | --- |
| `xs` / `sm` / `md` / `lg` / `xl` | |

## Tipografia

| Token | Família · tamanho · peso | Papel |
| --- | --- | --- |

## Estados de botão

| Estado | Aparência |
| --- | --- |
| normal | |
| hover | |
| foco (teclado) | |
| desabilitado | |
| carregando | |

## Breakpoints (Mobile-First)

O design nasce para a menor tela e cresce (ID2). Toda tela do protótipo tem
versão mobile antes da versão desktop.

| Token | Largura mínima | Vale para |
| --- | --- | --- |
| `sm` | | |
| `md` | | |
| `lg` | | |

## Identidade PWA

Os valores abaixo alimentam o `manifest.webmanifest` no `/utf-setup` (ID3).

| Campo | Valor |
| --- | --- |
| Nome curto (`short_name`) | |
| Cor de tema (`theme_color`) | |
| Cor de fundo (`background_color`) | |
| Ícone | |
| Modo de exibição (`display`) | `standalone` |
| Comportamento visual offline | [o que a pessoa vê sem rede] |

## Protótipo

**Link:** [Figma / Stitch / equivalente]
**Telas:** [3 a 5 telas das jornadas principais]
