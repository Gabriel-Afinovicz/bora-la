# 📄 Product Requirements Document (PRD)

**Projeto:** Bora Lá UTFPR

**Versão:** 1.0.0

**Última atualização:** 2026-09-15

> Este documento é a fonte da verdade sobre o que o produto faz. As decisões
> técnicas pertencem ao `docs/architecture.md` e aos futuros documentos de
> especificação de cada história.

---

## 🎯 1. Visão Geral e Objetivo

**O problema:** Segundo a equipe, conseguir carona para chegar à UTFPR ou voltar
para casa depende hoje de iniciativas individuais, amizades e contatos pessoais.
Muitos alunos utilizam ônibus, que ficam superlotados, e alguns acabam perdendo o
transporte. Há também pessoas com dificuldade para arcar com o deslocamento diário.

**A solução:** O Bora Lá UTFPR conecta pessoas que precisam de carona a outras
pessoas que já fazem o trajeto de ida ao campus Guarapuava ou de volta para casa.
Motoristas anunciam suas viagens; passageiros pesquisam rotas próximas, solicitam
uma vaga e, com o aceite do motorista, combinam os detalhes por mensagens.

**Como saberemos que deu certo:** Pessoas conseguem combinar e realizar caronas
que não encontrariam apenas por seus contatos, ampliando a ajuda coletiva e
beneficiando também quem tem dificuldade para custear o transporte diário.

---

## 📖 2. Glossário Ubíquo

| Termo | Significa | Não confundir com |
| :---- | :-------- | :---------------- |
| **Viagem ou corrida** | Um trajeto anunciado pelo motorista, com data, horário e vagas. | A programação que gera várias viagens. |
| **Carona** | A participação gratuita de um passageiro em uma viagem. | Transporte pago ou ajuda de custo. |
| **Passageiro** | Pessoa que solicita ou utiliza uma carona; é o papel inicial de toda conta. | Papel exclusivo: a mesma pessoa também pode ser motorista. |
| **Motorista** | Pessoa que cadastrou um veículo e pode oferecer viagens. | Motorista cuja CNH ou vínculo com a UTFPR foi verificado pela plataforma. |
| **Veículo** | Automóvel cadastrado com placa, cor e capacidade máxima de passageiros. | A viagem realizada com esse veículo. |
| **Solicitação** | Pedido de carona que aguarda decisão do motorista. | Reserva: uma solicitação pendente ainda não ocupa vaga. |
| **Reserva** | Solicitação aceita e confirmada, que ocupa uma vaga na viagem. | Pedido pendente. |
| **Programação recorrente** | Regra que gera viagens repetidas nos dias e horários escolhidos. | Uma ocorrência individual dessa programação. |
| **Ocorrência** | Uma viagem específica gerada por uma programação recorrente. | A programação completa. |
| **Cancelamento tardio** | Cancelamento feito pela própria pessoa em uma carona confirmada a menos de 15 minutos da saída. | Cancelamento automático ou encerramento de solicitação pendente. |
| **Infração** | Registro gerado por um cancelamento tardio e usado para aplicar bloqueios. | Cancelamento sem punição. |
| **Bloqueio** | Suspensão temporária do direito de solicitar e oferecer caronas. | Exclusão permanente da conta. |
| **Sequência de recuperação** | Contagem de viagens realizadas sem cancelamento nos últimos 30 minutos, usada para zerar infrações após 30 viagens. | Quantidade total de viagens no histórico. |

---

## 👤 3. Atores e Permissões

| Ator ou condição | Quem é | Pode | Não pode |
| :--------------- | :----- | :--- | :------- |
| **Visitante** | Pessoa sem uma sessão iniciada. | Consultar as viagens disponíveis. | Solicitar vaga, trocar mensagens ou oferecer viagens. |
| **Passageiro** | Papel inicial de toda pessoa cadastrada. | Consultar viagens, solicitar caronas, confirmar sua participação após a aprovação do motorista, cancelar solicitações e reservas e trocar mensagens. | Aprovar seu pedido no lugar do motorista ou administrar viagens de outras pessoas. |
| **Motorista** | Pessoa que cadastrou ao menos um veículo; continua podendo atuar como passageiro. | Publicar e administrar suas viagens, definir vagas e repetições, aceitar ou recusar pedidos, trocar mensagens e concluir viagens. | Aceitar pessoas além das vagas disponíveis ou alterar livremente uma viagem com passageiros aceitos. |
| **Pessoa com e-mail pendente** | Pessoa cadastrada que ainda não confirmou o endereço de e-mail. | Entrar na conta, consultar viagens e solicitar novo link de confirmação. | Solicitar ou oferecer caronas. |
| **Pessoa bloqueada** | Pessoa temporariamente suspensa por cancelamentos tardios repetidos. | Entrar na conta e consultar viagens. | Solicitar caronas ou oferecer viagens durante o bloqueio. |

O cadastro é aberto e não exige comprovação de vínculo com a UTFPR. A plataforma
também não verifica a CNH do motorista.

---

## 📝 4. Escopo Funcional (User Stories)

### US01 — Criar e confirmar uma conta · `Must Have` · `M` · Status: `Draft`

**Como** pessoa interessada em caronas, **eu quero** criar uma conta e confirmar
meu e-mail **para que** eu possa participar das viagens.

**Critérios de aceite:**

- [ ] **CA1 — Dado** nome, sobrenome, e-mail válido ainda não cadastrado e senha, **quando** eu concluir o cadastro, **então** minha conta será criada como passageiro e ficará com o e-mail pendente de confirmação.
- [ ] **CA2 — Dado** que minha conta foi criada, **quando** o envio ocorrer com sucesso, **então** receberei um link de confirmação no endereço informado.
- [ ] **CA3 — Dado** que meu e-mail está pendente, **quando** eu utilizar o sistema, **então** poderei consultar viagens, mas não solicitar nem oferecer caronas.
- [ ] **CA4 — Dado** um link válido, **quando** eu o utilizar, **então** meu e-mail será confirmado e poderei participar conforme as permissões do meu perfil.
- [ ] **CA5 — Dado** que a mensagem não chegou ou o link expirou, **quando** eu solicitar o reenvio, **então** o sistema tentará enviar um novo link e informará o resultado.
- [ ] **CA6 — Dado** que faltam informações obrigatórias ou o e-mail tem formato inválido, **quando** eu tentar me cadastrar, **então** o sistema indicará o que corrigir.
- [ ] **CA7 — Dado** um e-mail já cadastrado, **quando** eu tentar criar outra conta com ele, **então** o sistema impedirá a duplicação e orientará o acesso à conta existente.
- [ ] **CA8 — Dado** que comecei a preencher o cadastro, **quando** eu abandonar o fluxo antes do envio, **então** nenhuma conta será criada.

**Regras relacionadas:** RN02, RN03.

### US02 — Entrar e sair da conta · `Must Have` · `M` · Status: `Draft`

**Como** pessoa cadastrada, **eu quero** entrar e sair da minha conta, escolhendo
se desejo permanecer conectada, **para que** eu possa acessar minhas caronas e
viagens.

**Critérios de aceite:**

- [ ] **CA1 — Dado** um e-mail e uma senha corretos, **quando** eu entrar, **então** acessarei a conta com as permissões correspondentes.
- [ ] **CA2 — Dado** que marquei “Lembrar de mim”, **quando** eu fechar e reabrir a aplicação, **então** continuarei conectado.
- [ ] **CA3 — Dado** que não marquei “Lembrar de mim”, **quando** eu fechar e reabrir a aplicação, **então** precisarei entrar novamente.
- [ ] **CA4 — Dado** que estou conectado, **quando** eu escolher sair, **então** minha sessão será encerrada mesmo que eu tenha marcado “Lembrar de mim”.
- [ ] **CA5 — Dado** um e-mail ou uma senha incorretos, **quando** eu tentar entrar, **então** receberei uma mensagem de erro e poderei tentar novamente.
- [ ] **CA6 — Dado** que meu e-mail está pendente ou minha conta está bloqueada, **quando** eu entrar, **então** continuarei limitado à consulta de viagens.
- [ ] **CA7 — Dado** que ocorreu uma falha no serviço de acesso, **quando** eu tentar entrar, **então** o sistema informará a falha e permitirá uma nova tentativa.

**Regras relacionadas:** RN03, RN18.

### US03 — Redefinir a senha · `Must Have` · `S` · Status: `Draft`

**Como** pessoa cadastrada que esqueceu a senha, **eu quero** receber um link no
meu e-mail **para que** eu possa definir uma nova senha e recuperar o acesso.

**Critérios de aceite:**

- [ ] **CA1 — Dado** que possuo uma conta, **quando** eu solicitar a recuperação informando meu e-mail, **então** o sistema enviará um link para redefinir a senha.
- [ ] **CA2 — Dado** um link válido, **quando** eu informar e confirmar uma nova senha, **então** o sistema atualizará minha senha e informará o sucesso.
- [ ] **CA3 — Dado** que redefini a senha, **quando** eu entrar novamente, **então** a nova senha permitirá o acesso e a anterior será recusada.
- [ ] **CA4 — Dado** um link inválido, expirado ou já utilizado, **quando** eu tentar redefinir a senha, **então** o sistema impedirá a alteração e orientará a solicitação de outro link.
- [ ] **CA5 — Dado** um e-mail sem conta, **quando** eu solicitar a recuperação, **então** nenhuma conta será criada e a mensagem não revelará se o endereço está cadastrado.
- [ ] **CA6 — Dado** que ocorreu uma falha no envio ou na redefinição, **quando** a operação falhar, **então** o sistema informará o problema, preservará a senha anterior e permitirá tentar novamente.

**Regras relacionadas:** RN03.

### US04 — Cadastrar e remover veículos · `Must Have` · `M` · Status: `Draft`

**Como** pessoa cadastrada, **eu quero** gerenciar meus veículos **para que** eu
possa escolher com qual deles oferecerei caronas.

**Critérios de aceite:**

- [ ] **CA1 — Dado** que estou conectado, confirmei meu e-mail e tenho menos de três veículos, **quando** eu informar cor, placa e capacidade máxima de passageiros, **então** poderei cadastrar o veículo e atuar como motorista.
- [ ] **CA2 — Dado** que já tenho três veículos, **quando** eu tentar cadastrar outro, **então** o sistema impedirá o cadastro e explicará o limite.
- [ ] **CA3 — Dado** que vou criar uma viagem, **quando** eu escolher o veículo, **então** somente meus veículos previamente cadastrados estarão disponíveis.
- [ ] **CA4 — Dado** que não tenho veículo cadastrado, **quando** eu tentar oferecer uma viagem, **então** o sistema orientará o cadastro de um veículo primeiro.
- [ ] **CA5 — Dado** um veículo sem viagens futuras vinculadas, **quando** eu o excluir, **então** ele deixará de estar disponível e liberará espaço para outro cadastro.
- [ ] **CA6 — Dado** um veículo com viagem futura vinculada, **quando** eu tentar excluí-lo, **então** o sistema exigirá que eu troque o veículo dessas viagens ou as cancele primeiro.
- [ ] **CA7 — Dado** que faltam campos obrigatórios ou a capacidade não é um número inteiro positivo, **quando** eu tentar cadastrar, **então** o sistema indicará o que corrigir; a capacidade considera somente passageiros.

**Regras relacionadas:** RN04, RN05.

### US05 — Publicar uma viagem avulsa · `Must Have` · `M` · Status: `Draft`

**Como** motorista, **eu quero** anunciar uma viagem com trajeto, horário e vagas
**para que** eu possa receber solicitações de carona.

**Critérios de aceite:**

- [ ] **CA1 — Dado** que tenho e-mail confirmado, não estou bloqueado e possuo um veículo cadastrado, **quando** eu informar sentido, local fora do campus, data, horário de saída e vagas, **então** o sistema calculará a rota.
- [ ] **CA2 — Dado** que estou anunciando uma viagem, **quando** eu definir seu sentido, **então** o campus Guarapuava será obrigatoriamente a origem ou o destino.
- [ ] **CA3 — Dado** o veículo selecionado, **quando** eu definir as vagas, **então** poderei oferecer de uma vaga até a capacidade máxima de passageiros desse veículo.
- [ ] **CA4 — Dado** que os dados estão completos e a rota foi calculada, **quando** eu publicar, **então** a viagem ficará disponível para consulta, inclusive por visitantes.
- [ ] **CA5 — Dado** que faltam informações, as vagas são inválidas ou a saída está no passado, **quando** eu tentar publicar, **então** o sistema indicará o que corrigir e impedirá a publicação.
- [ ] **CA6 — Dado** que não foi possível calcular a rota, **quando** a tentativa falhar, **então** o sistema informará o problema e permitirá corrigir o local ou tentar novamente, sem publicar uma viagem incompleta.

**Regras relacionadas:** RN03, RN05, RN06, RN07.

### US06 — Publicar uma programação recorrente · `Must Have` · `M` · Status: `Draft`

**Como** motorista, **eu quero** programar viagens que se repetem durante a semana
**para que** eu possa oferecer caronas na minha rotina sem cadastrar cada ocorrência
manualmente.

**Critérios de aceite:**

- [ ] **CA1 — Dado** que cumpro os requisitos para oferecer uma viagem, **quando** eu definir trajeto, veículo, vagas, data inicial, dias da semana e horários, **então** o sistema publicará as ocorrências dessa programação durante seis meses.
- [ ] **CA2 — Dado** que estou configurando a repetição, **quando** eu cadastrar os dias individualmente, **então** poderei definir um horário diferente para cada dia.
- [ ] **CA3 — Dado** que utilizo o mesmo horário nos dias selecionados, **quando** eu aplicar essa configuração, **então** todos eles receberão esse horário.
- [ ] **CA4 — Dado** que a programação foi publicada, **quando** uma pessoa consultar as viagens, **então** encontrará as ocorrências nas respectivas datas e horários.
- [ ] **CA5 — Dado** que um passageiro foi aceito em uma ocorrência, **quando** a reserva for confirmada, **então** a vaga será ocupada somente naquela viagem.
- [ ] **CA6 — Dado** que faltam dias, horários ou informações obrigatórias, **quando** eu tentar publicar a programação, **então** o sistema indicará o que corrigir e impedirá a publicação.
- [ ] **CA7 — Dado** uma programação válida, **quando** ocorrer uma falha que impeça sua publicação completa, **então** nenhuma de suas viagens será publicada, o sistema informará o problema e manterá os dados preenchidos para uma nova tentativa.
- [ ] **CA8 — Dado** que estou repetindo uma tentativa de publicação da mesma programação após uma falha, **quando** a publicação for concluída, **então** todas as ocorrências serão disponibilizadas uma única vez, sem duplicar viagens.

**Regras relacionadas:** RN06, RN07, RN11.

### US07 — Encontrar caronas próximas · `Must Have` · `M` · Status: `Draft`

**Como** pessoa interessada em caronas, **eu quero** pesquisar viagens por local,
sentido e data **para que** eu possa encontrar uma opção adequada ao meu
deslocamento.

**Critérios de aceite:**

- [ ] **CA1 — Dado** que sou visitante ou tenho uma conta, **quando** eu informar local, sentido e data, **então** poderei consultar as viagens disponíveis.
- [ ] **CA2 — Dado** que procuro uma ida à UTFPR, **quando** eu pesquisar, **então** a proximidade será calculada pelo meu local de embarque; na volta, pelo local desejado de desembarque.
- [ ] **CA3 — Dado** que existem resultados, **quando** eles forem apresentados, **então** as rotas a até 100 metros aparecerão primeiro, seguidas das opções mais distantes.
- [ ] **CA4 — Dado** um resultado, **quando** eu consultá-lo, **então** poderei conhecer o trajeto anunciado, a data, o horário de saída e as vagas disponíveis, sem visualizar placa ou cor do veículo.
- [ ] **CA5 — Dado** que uma viagem está lotada, cancelada ou já passou do horário de saída, **quando** eu pesquisar, **então** ela não será oferecida nos resultados.
- [ ] **CA6 — Dado** que não existem viagens correspondentes, **quando** eu pesquisar, **então** o sistema informará que não encontrou opções e permitirá alterar a busca.
- [ ] **CA7 — Dado** que ocorre uma falha na consulta, **quando** eu pesquisar, **então** o sistema informará o problema e permitirá tentar novamente.
- [ ] **CA8 — Dado** que sou visitante, tenho e-mail pendente ou estou bloqueado, **quando** eu tentar solicitar uma carona, **então** o sistema impedirá o pedido e explicará o requisito correspondente.

**Regras relacionadas:** RN03, RN18, RN26, RN27, RN28.

### US08 — Solicitar uma carona e receber uma decisão · `Must Have` · `M` · Status: `Draft`

**Como** passageiro, **eu quero** solicitar uma vaga e receber a decisão do
motorista **para que** eu possa confirmar minha participação na viagem.

**Critérios de aceite:**

- [ ] **CA1 — Dado** que tenho e-mail confirmado e não estou bloqueado, **quando** eu solicitar uma carona disponível, **então** o motorista receberá meu pedido, que ficará pendente sem ocupar vaga.
- [ ] **CA2 — Dado** que ainda não tenho reserva confirmada naquele horário, **quando** eu solicitar outras viagens, **então** poderei manter vários pedidos pendentes.
- [ ] **CA3 — Dado** que faltam mais de 15 minutos para a saída, **quando** o motorista aceitar meu pedido, **então** a reserva será confirmada e ocupará uma vaga.
- [ ] **CA4 — Dado** que faltam 15 minutos ou menos, **quando** o motorista aceitar, **então** receberei um pedido de confirmação e a vaga somente será ocupada se eu aceitar e ainda houver disponibilidade; essa confirmação também valerá como confirmação final de presença, sem solicitar outra.
- [ ] **CA5 — Dado** que uma reserva foi confirmada, **quando** a confirmação ocorrer, **então** meus outros pedidos pendentes para o mesmo horário serão cancelados automaticamente, sem punição.
- [ ] **CA6 — Dado** que o motorista recusa meu pedido ou eu recuso uma confirmação tardia, **quando** a decisão ocorrer, **então** a solicitação será encerrada e a outra pessoa será informada, sem punição.
- [ ] **CA7 — Dado** que a viagem ficou lotada ou foi cancelada, **quando** alguém tentar confirmar uma vaga, **então** o sistema impedirá a confirmação e informará o motivo.
- [ ] **CA8 — Dado** um pedido ainda pendente, inclusive aguardando minha confirmação, **quando** chegar o horário de saída, **então** ele será encerrado automaticamente, sem punição.
- [ ] **CA9 — Dado** que já tenho uma solicitação ativa ou reserva naquela viagem, **quando** eu tentar solicitar novamente, **então** o sistema impedirá a duplicação.
- [ ] **CA10 — Dado** que minha reserva foi confirmada, **quando** eu consultar a viagem antes de seu encerramento ou cancelamento, **então** poderei visualizar a placa completa e a cor do veículo.

**Regras relacionadas:** RN03, RN08, RN09, RN10, RN28.

### US09 — Trocar mensagens sobre uma carona · `Must Have` · `M` · Status: `Draft`

**Como** passageiro ou motorista, **eu quero** trocar mensagens sobre uma
solicitação de carona **para que** eu possa combinar horário e ponto de encontro.

**Critérios de aceite:**

- [ ] **CA1 — Dado** que o passageiro enviou uma solicitação, **quando** o pedido for criado, **então** será aberto um chat entre ele e o motorista.
- [ ] **CA2 — Dado** um chat ativo, **quando** um participante enviar uma mensagem, **então** ela ficará disponível para o outro participante.
- [ ] **CA3 — Dado** que os participantes combinaram uma parada diferente, **quando** conversarem pelo chat, **então** esse combinado não alterará a rota anunciada no sistema.
- [ ] **CA4 — Dado** que o pedido foi recusado ou a carona foi cancelada, **quando** isso ocorrer, **então** o chat correspondente será encerrado imediatamente.
- [ ] **CA5 — Dado** que o motorista encerrou a viagem, **quando** ele confirmar a conclusão, **então** todos os chats daquela viagem serão encerrados.
- [ ] **CA6 — Dado** que a viagem não foi encerrada manualmente, **quando** chegar seu horário marcado, **então** seus chats serão encerrados automaticamente.
- [ ] **CA7 — Dado** um chat encerrado, **quando** um participante consultá-lo, **então** poderá ler o histórico, mas não enviar novas mensagens.
- [ ] **CA8 — Dado** que uma pessoa possui mais de dez conversas encerradas, **quando** uma nova conversa entrar no histórico, **então** a conversa encerrada mais antiga será removida para essa pessoa; chats ativos serão preservados.
- [ ] **CA9 — Dado** que um participante apaga uma conversa, **quando** ele confirmar a exclusão, **então** ela desaparecerá somente do histórico dele.
- [ ] **CA10 — Dado** que uma mensagem não pôde ser enviada, **quando** ocorrer a falha, **então** o sistema informará o problema e permitirá tentar novamente.

**Regras relacionadas:** RN06, RN24, RN25.

### US10 — Alterar uma viagem ou programação · `Must Have` · `M` · Status: `Draft`

**Como** motorista, **eu quero** alterar minhas viagens e programações **para que**
eu possa adaptar a oferta quando minha rotina ou meu veículo mudar.

**Critérios de aceite:**

- [ ] **CA1 — Dado** uma viagem sem passageiros aceitos, **quando** eu alterar horário, vagas, local ou veículo, **então** os novos dados serão aplicados.
- [ ] **CA2 — Dado** uma ocorrência de programação semanal, **quando** eu selecionar “apenas hoje” e alterá-la, **então** somente aquela ocorrência será modificada.
- [ ] **CA3 — Dado** uma ocorrência recorrente, **quando** eu alterar sem selecionar “apenas hoje”, **então** a mudança será aplicada às próximas ocorrências que ainda não tenham passageiros aceitos.
- [ ] **CA4 — Dado** que uma ocorrência futura possui passageiros aceitos, **quando** uma alteração geral alcançar sua data, **então** ela não será modificada e eu precisarei mantê-la ou cancelá-la com justificativa.
- [ ] **CA5 — Dado** uma viagem com passageiros aceitos, **quando** eu trocar para um veículo com capacidade suficiente, **então** a troca será permitida sem cancelar reservas.
- [ ] **CA6 — Dado** uma viagem com passageiros aceitos, **quando** eu tentar trocar para um veículo sem capacidade suficiente, **então** o sistema impedirá a troca e informará quantas reservas precisam ser canceladas.
- [ ] **CA7 — Dado** que cancelei reservas até os passageiros restantes caberem no novo veículo, **quando** eu tentar a troca novamente, **então** o sistema permitirá selecionar o veículo.
- [ ] **CA8 — Dado** que o novo número de vagas é menor que o total de passageiros aceitos, **quando** eu tentar salvar, **então** o sistema impedirá a alteração.
- [ ] **CA9 — Dado** que os novos dados são inválidos ou a rota não pode ser recalculada, **quando** eu tentar salvar, **então** o sistema preservará os dados anteriores e indicará o que corrigir.

**Regras relacionadas:** RN07, RN12, RN13, RN14.

### US11 — Cancelar pedidos, reservas e viagens · `Must Have` · `M` · Status: `Draft`

**Como** participante de uma carona, **eu quero** cancelar um compromisso informando
o motivo **para que** eu possa avisar as outras pessoas e liberar a viagem ou a vaga.

**Critérios de aceite:**

- [ ] **CA1 — Dado** um pedido pendente, **quando** o passageiro informar um motivo com pelo menos três palavras e cancelar, **então** o pedido e seu chat serão encerrados, sem punição.
- [ ] **CA2 — Dado** uma reserva confirmada, **quando** o passageiro informar um motivo com pelo menos três palavras e cancelar, **então** a vaga será liberada e o motorista será notificado.
- [ ] **CA3 — Dado** uma viagem publicada, **quando** o motorista informar um motivo com pelo menos três palavras e cancelar, **então** a viagem será cancelada e todos os passageiros envolvidos serão notificados.
- [ ] **CA4 — Dado** uma ocorrência recorrente, **quando** o motorista cancelar com “apenas hoje”, **então** somente aquela ocorrência será cancelada.
- [ ] **CA5 — Dado** uma programação recorrente, **quando** o motorista cancelar sem selecionar “apenas hoje”, **então** todas as próximas ocorrências serão canceladas.
- [ ] **CA6 — Dado** um motivo com menos de três palavras, **quando** alguém tentar cancelar, **então** o sistema impedirá a operação e solicitará uma justificativa válida.
- [ ] **CA7 — Dado** que uma reserva ou viagem confirmada foi cancelada pela própria pessoa faltando menos de 15 minutos, **quando** o cancelamento for concluído, **então** será registrada uma infração.
- [ ] **CA8 — Dado** que o motorista cancela uma viagem com vários passageiros, **quando** a infração for registrada, **então** ela contará apenas uma vez para aquela viagem.
- [ ] **CA9 — Dado** um cancelamento automático causado por confirmação de outra carona, expiração de pedido ou bloqueio, **quando** ele ocorrer, **então** não será registrada uma infração.
- [ ] **CA10 — Dado** que o cancelamento não pôde ser concluído, **quando** ocorrer a falha, **então** o compromisso anterior será preservado e a pessoa poderá tentar novamente.

**Regras relacionadas:** RN12, RN14, RN15, RN16, RN19.

### US12 — Receber avisos e bloqueios · `Must Have` · `M` · Status: `Draft`

**Como** participante da comunidade, **eu quero** ser avisado sobre as consequências
dos cancelamentos tardios **para que** eu possa acompanhar minha situação e evitar
novos bloqueios.

**Critérios de aceite:**

- [ ] **CA1 — Dado** que uma carona confirmada foi cancelada pela própria pessoa faltando menos de 15 minutos, **quando** a infração for registrada, **então** ela receberá um aviso informando quantas faltam para o próximo bloqueio.
- [ ] **CA2 — Dado** que a pessoa nunca foi bloqueada ou recuperou sua situação, **quando** acumular duas infrações, **então** ficará bloqueada por cinco dias.
- [ ] **CA3 — Dado** que a pessoa já cumpriu um bloqueio e ainda não recuperou sua situação, **quando** acumular três novas infrações, **então** receberá outro bloqueio.
- [ ] **CA4 — Dado** cada novo bloqueio sem recuperação, **quando** ele for aplicado, **então** sua duração aumentará cinco dias em relação ao anterior.
- [ ] **CA5 — Dado** que o bloqueio começou, **quando** o sistema aplicá-lo, **então** cancelará automaticamente as viagens futuras e reservas da pessoa, liberará as vagas e notificará os envolvidos.
- [ ] **CA6 — Dado** um cancelamento provocado automaticamente pelo bloqueio, **quando** ele ocorrer, **então** não contará como nova infração.
- [ ] **CA7 — Dado** que a pessoa está bloqueada, **quando** utilizar o sistema, **então** poderá consultar viagens, mas não solicitar nem oferecer caronas.
- [ ] **CA8 — Dado** que o período de bloqueio terminou, **quando** a pessoa voltar a utilizar o sistema, **então** poderá novamente solicitar e oferecer caronas.
- [ ] **CA9 — Dado** que a pessoa completa uma viagem como motorista ou passageiro sem cancelar nos últimos 30 minutos, **quando** a viagem for concluída, **então** avançará na sequência de recuperação.
- [ ] **CA10 — Dado** que a pessoa cancela uma carona confirmada faltando menos de 30 minutos, **quando** isso ocorrer, **então** sua sequência voltará a zero; se faltarem menos de 15 minutos, também será registrada a infração.
- [ ] **CA11 — Dado** que a pessoa completa 30 viagens consecutivas válidas, **quando** a trigésima for concluída, **então** suas infrações serão zeradas e um futuro bloqueio voltará ao patamar inicial de duas infrações e cinco dias.
- [ ] **CA12 — Dado** que a mesma pessoa atua como motorista e passageiro, **quando** houver cancelamentos ou viagens concluídas, **então** ambos os papéis alimentarão os mesmos contadores pessoais.

**Regras relacionadas:** RN15, RN16, RN17, RN18, RN19, RN20, RN21.

### US13 — Confirmar presença e concluir uma viagem · `Must Have` · `M` · Status: `Draft`

**Como** participante de uma carona, **eu quero** registrar a confirmação e a
conclusão da viagem **para que** o histórico de viagens realizadas permaneça correto.

**Critérios de aceite:**

- [ ] **CA1 — Dado** um passageiro com reserva confirmada e sem confirmação final de presença, **quando** faltarem 15 minutos para a saída, **então** o sistema solicitará sua confirmação final de presença.
- [ ] **CA2 — Dado** que o passageiro confirma até o horário de saída, **quando** a confirmação ocorrer, **então** sua reserva permanecerá ativa.
- [ ] **CA3 — Dado** que o passageiro não responde até o horário de saída, **quando** esse horário chegar, **então** sua reserva será cancelada automaticamente, sem punição e sem crédito pela viagem.
- [ ] **CA4 — Dado** que a viagem foi realizada, **quando** o motorista encerrá-la, **então** deverá indicar quais passageiros realmente participaram.
- [ ] **CA5 — Dado** que o motorista realizou a viagem, **quando** concluí-la, **então** ela contará para sua sequência de recuperação mesmo que algum ou todos os passageiros tenham cancelado ou não comparecido.
- [ ] **CA6 — Dado** que um passageiro foi indicado como participante, **quando** a viagem for concluída, **então** ela contará para a sequência de recuperação dele.
- [ ] **CA7 — Dado** que um passageiro não participou, **quando** o motorista concluir a viagem, **então** ela não contará para esse passageiro.
- [ ] **CA8 — Dado** que a viagem foi cancelada pelo motorista, **quando** isso ocorrer, **então** ninguém receberá crédito de viagem concluída.
- [ ] **CA9 — Dado** que o motorista ainda não concluiu a viagem, **quando** alguém consultar o histórico, **então** ela permanecerá aguardando conclusão e não contará para a recuperação.
- [ ] **CA10 — Dado** que o motorista aceitou o pedido faltando 15 minutos ou menos e o passageiro confirmou a reserva antes da saída, **quando** o sistema verificar sua presença, **então** essa confirmação já valerá como confirmação final de presença, sem exigir outra resposta.
- [ ] **CA11 — Dado** que a viagem ainda não foi concluída no sistema, **quando** uma falha impedir que o motorista salve a conclusão, **então** o sistema informará o problema, manterá a viagem aguardando conclusão e permitirá tentar novamente, sem contabilizar créditos para ninguém.
- [ ] **CA12 — Dado** que a conclusão da viagem já foi salva, **quando** o motorista repetir a ação de concluir, **então** o sistema informará que a viagem já foi concluída e não contabilizará novamente créditos para nenhum participante.

**Regras relacionadas:** RN10, RN20, RN21, RN22, RN23.

---

## 🛡️ 5. Regras de Negócio (Constraints)

| ID | Regra |
| :--- | :---- |
| **RN01** | As caronas são gratuitas nesta versão; ajuda de custo fica fora do escopo atual. |
| **RN02** | O cadastro é aberto a qualquer pessoa, embora o público principal seja a comunidade da UTFPR; não há comprovação de vínculo institucional. |
| **RN03** | Toda conta começa como passageiro, mas somente contas com e-mail confirmado podem solicitar ou oferecer caronas. |
| **RN04** | A plataforma não verifica CNH nem apresenta o motorista como verificado; o direito de oferecer viagens nasce com o cadastro de um veículo. |
| **RN05** | Cada pessoa pode manter até três veículos, previamente cadastrados com placa, cor e capacidade máxima de passageiros. |
| **RN06** | Toda viagem deve ter o campus Guarapuava como origem ou destino. Paradas diferentes podem ser combinadas por mensagem, sem alterar a rota anunciada. |
| **RN07** | O motorista define as vagas de cada viagem sem ultrapassar a capacidade do veículo selecionado. |
| **RN08** | Solicitações pendentes não ocupam vagas; reservas confirmadas ocupam uma vaga cada. |
| **RN09** | O passageiro pode manter vários pedidos para o mesmo horário; ao confirmar uma reserva, os demais são cancelados automaticamente, sem punição. |
| **RN10** | Um aceite feito faltando 15 minutos ou menos depende de confirmação do passageiro e só ocupa vaga depois dessa confirmação, se ainda houver disponibilidade. Essa mesma resposta vale como confirmação final de presença, sem exigir outra. |
| **RN11** | Programações recorrentes duram seis meses a partir da primeira viagem e podem ter horários por dia ou um horário aplicado aos dias selecionados. A publicação disponibiliza todas as ocorrências ou nenhuma; em caso de falha, os dados preenchidos são preservados para uma nova tentativa, sem duplicar viagens. |
| **RN12** | “Apenas hoje” altera ou cancela uma ocorrência; sem essa opção, a ação alcança as próximas ocorrências da programação. |
| **RN13** | Viagens com passageiros aceitos não podem ser alteradas, exceto pela troca para um veículo que comporte todos eles. |
| **RN14** | Motorista e passageiro devem informar um motivo com pelo menos três palavras para cancelar. |
| **RN15** | Só o cancelamento feito pela própria pessoa em uma carona já confirmada, a menos de 15 minutos da saída, gera infração. |
| **RN16** | Vários passageiros afetados pelo cancelamento da mesma viagem geram no máximo uma infração para o motorista. |
| **RN17** | O primeiro bloqueio ocorre após duas infrações; os seguintes, após três novas infrações. As durações são 5, 10, 15 dias e continuam aumentando de cinco em cinco. |
| **RN18** | O bloqueio vale para os papéis de motorista e passageiro, cancela viagens e reservas futuras e permite apenas consultar viagens. |
| **RN19** | Cancelamentos automáticos não geram infração. |
| **RN20** | Trinta viagens realizadas consecutivamente sem cancelamento nos últimos 30 minutos zeram as infrações e fazem o próximo bloqueio voltar ao patamar inicial. |
| **RN21** | O cancelamento de uma carona confirmada feito pela própria pessoa faltando menos de 30 minutos para a saída reinicia sua sequência de recuperação; se faltarem menos de 15 minutos, também gera infração. |
| **RN22** | Quinze minutos antes da saída, passageiros com reserva confirmada e sem confirmação final de presença recebem um pedido para confirmar a presença; a falta de resposta até o horário cancela a reserva sem punição. Para pedidos aceitos nos últimos 15 minutos, a confirmação prevista na RN10 já cumpre essa exigência. |
| **RN23** | O motorista registra a conclusão e os passageiros presentes. O motorista recebe crédito se realizou a viagem; somente passageiros presentes recebem crédito. Se uma falha impedir que a conclusão seja salva, a viagem permanece aguardando conclusão, sem contabilizar créditos, e o motorista pode tentar novamente. Repetir uma conclusão já salva informa que a viagem já foi concluída, sem contabilizar créditos novamente para ninguém. |
| **RN24** | O chat nasce com a solicitação e encerra na recusa, no cancelamento, na conclusão pelo motorista ou automaticamente no horário da viagem. |
| **RN25** | Cada pessoa mantém até dez chats encerrados; os mais antigos são removidos primeiro. Excluir um chat afeta somente quem o excluiu. |
| **RN26** | Na busca, viagens cuja rota passa a até 100 metros do local informado aparecem primeiro; opções mais distantes continuam disponíveis. |
| **RN27** | Viagens lotadas, canceladas ou cujo horário já passou não aparecem como opções disponíveis na busca. |
| **RN28** | Placa completa e cor são exibidas somente ao passageiro com reserva confirmada, desde a confirmação até a viagem ser encerrada ou cancelada. |

---

## 🚫 6. Fora de Escopo (Non-goals)

- Cobrança, divisão de combustível ou qualquer pagamento pela carona.
- Verificação automática ou manual de CNH.
- Garantia de autenticidade dos dados declarados pelo motorista.
- Comprovação de vínculo da pessoa com a UTFPR.
- Atendimento a outros campi além de Guarapuava.
- Alteração da rota anunciada para registrar paradas combinadas pelo chat.
- Rastreamento do veículo em tempo real.
- Administração do transporte público ou controle da lotação dos ônibus.
- Aplicativo nativo distribuído por lojas; a entrega é uma aplicação web com
  experiência PWA.

---

## ⚙️ 7. Requisitos Não Funcionais (Qualidade)

- **Mobile-First e responsividade:** as funções principais devem funcionar em
  celulares e se adaptar a tablets e computadores.
- **Experiência PWA:** a aplicação deve poder ser instalada e apresentar um
  estado visual claro quando estiver sem conexão.
- **Privacidade:** dados pessoais e conversas devem ser acessíveis somente aos
  participantes autorizados; placa e cor seguem a RN28.
- **Segurança de acesso:** ações de passageiro e motorista exigem conta
  autenticada, e senhas não podem ser exibidas nem armazenadas em formato
  legível.
- **Clareza:** formulários devem indicar campos inválidos e explicar como
  corrigir; confirmações, cancelamentos, bloqueios e falhas devem produzir
  mensagens compreensíveis em português.
- **Consistência:** uma falha de conexão não pode deixar vaga, reserva ou
  cancelamento em estado parcialmente atualizado.
- **Acessibilidade básica:** a aplicação deve permitir navegação por teclado,
  apresentar foco visível, contraste legível e identificação textual dos
  controles principais.
- **Compatibilidade:** a aplicação deve funcionar nas versões atuais dos
  navegadores mais usados em celulares e computadores.

---

## 🛠️ 8. Histórico

| Data | Versão | O que mudou |
| :--- | :----- | :---------- |
| 2026-09-15 | 1.0.0 | Versão inicial produzida pela entrevista `/utf-prd`. |
