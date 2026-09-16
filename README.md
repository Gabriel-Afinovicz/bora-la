# Bora Lá UTFPR

Plataforma comunitária de caronas para estudantes e servidores da UTFPR —
campus Guarapuava. O projeto aproxima pessoas que já farão um trajeto de ida ou
volta do campus de quem precisa de transporte, facilitando a organização de
viagens gratuitas e reduzindo a dependência do transporte coletivo.

## Autores

- [Gabriel Afinovicz](https://github.com/Gabriel-Afinovicz)
- [Péricles Expedito Andrade](https://github.com/Pericles3322)
- [Eder Murilo de Morais](https://github.com/ederzin13)

## Documentação

- [Requisitos do produto](docs/prd.md)
- [Arquitetura e especificação técnica](docs/architecture.md)
- [Design System](docs/design-tokens.md)
- [Checklist da disciplina](docs/checklist.md)

O protótipo navegável será produzido na etapa de design e seu link público será
adicionado aqui após a aprovação da equipe.

## Funcionalidades planejadas

- Cadastro e confirmação de contas por e-mail.
- Cadastro de veículos e publicação de viagens avulsas ou recorrentes.
- Busca de caronas próximas ao local informado.
- Solicitação de vaga e aprovação pelo motorista.
- Conversas entre motorista e passageiro.
- Confirmação de presença, cancelamento e conclusão das viagens.
- Avisos e bloqueios progressivos por cancelamentos tardios repetidos.

As regras completas e os critérios de aceite estão descritos no
[PRD](docs/prd.md). As histórias permanecem em `Draft` enquanto aguardam a
leitura e a aprovação de todos os integrantes.

## Modelagem de dados

O diagrama de entidades e relacionamentos será definido na etapa de arquitetura,
depois da aprovação do PRD. Ele será renderizado nesta seção em Mermaid.

## Tecnologias

- **Frontend:** Angular 20+ com componentes standalone.
- **Estado da interface:** Signals.
- **Dados do MVP:** json-server.
- **Dados da aplicação final:** BaaS a ser escolhido e registrado na arquitetura.
- **Interface:** framework CSS a ser escolhido na etapa de design.
- **Distribuição:** aplicação web responsiva e instalável como PWA.

## Aplicação em produção

O endereço público será incluído após a configuração do deploy em uma etapa
posterior do projeto.

## Execução local

O projeto ainda está na fase de concepção e planejamento. As instruções para
instalação e execução serão adicionadas quando a estrutura Angular for criada
pelo fluxo `/utf-setup`.

## Telas da aplicação

As imagens serão adicionadas depois da criação e aprovação do protótipo e da
implementação das primeiras interfaces.
