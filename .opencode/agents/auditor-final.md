---
description: Compara o diff inteiro da branch contra o spec.md original, antes do Pull Request. Somente leitura.
mode: subagent
tools:
  write: false
  edit: false
  patch: false
permission:
  edit: deny
  bash:
    "*": deny
    "git diff*": allow
    "git log*": allow
    "git status*": allow
    "ls*": allow
    "cat*": allow
    "npm test*": allow
    "npm run test*": allow
    "npx vitest*": allow
    "npx jest*": allow
---
Leia `.agents/agents/auditor-final.md` e siga-o integralmente antes de qualquer outra ação.

> A lista de comandos liberados acima cobre git e os testes mais comuns.
> **Ajuste-a à stack declarada no `docs/architecture.md`** — comando de teste
> que não estiver liberado simplesmente não roda, e o parecer sai incompleto.
