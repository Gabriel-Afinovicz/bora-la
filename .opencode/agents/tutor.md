---
description: Ensina o aluno — explica a tarefa antes, o diff depois (inteiro ou arquivo por arquivo), os documentos da Fase 0, o scaffold, a spec e o simulado pré-PR. Não escreve código, não corrige, não opina sobre qualidade.
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
---
Leia `.agents/agents/tutor.md` e siga-o integralmente antes de qualquer outra ação.

> A lista de comandos liberados acima cobre git e os testes mais comuns.
> **Ajuste-a à stack declarada no `docs/architecture.md`** — comando de teste
> que não estiver liberado simplesmente não roda, e o parecer sai incompleto.
