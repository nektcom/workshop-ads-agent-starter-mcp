# Company Brain: de chat caro pra cérebro de verdade

Quase todo "Company Brain" que aparece por aí é um chat com acesso a uns arquivos. Funciona enquanto você está olhando. Quando você sai, ele para. Quando a regra do negócio muda, ninguém sabe em quantos prompts ela estava copiada. E quando ele erra, quem descobre é o cliente.

Este repo é o esqueleto do Company Brain que roda a mídia paga da Nekt. Aqui está o que separa um do outro e onde cada peça mora.

---

## O teste: 10 perguntas

Responda com honestidade sobre o SEU setup hoje. Mais de dois "não" = você tem um chat caro com nome de Company Brain. Todo mundo começa assim. O problema é parar aí.

| # | Pergunta | Onde isso mora neste repo |
|---|---|---|
| 1 | Se você sair de férias, ele continua trabalhando? | [`routines/`](./routines) |
| 2 | O seu time consegue usar, ou ele só existe no seu computador? | [`routines/`](./routines) + MCP por OAuth |
| 3 | Ele sabe qual número precisa mexer? | [`brain/regras-do-negocio.md`](./brain/regras-do-negocio.md) |
| 4 | Ele lê o dado ao vivo, ou alguém exporta e cola pra ele? | MCP da Nekt ([`nekt-mcp-data`](./.claude/skills/nekt-mcp-data/SKILL.md)) |
| 5 | As regras do negócio estão escritas, ou na cabeça de quem montou? | [`brain/`](./brain) |
| 6 | Se o contexto muda, você sai atualizando um monte de skills? | [`brain/`](./brain): skills apontam pra lá, não copiam |
| 7 | Pessoas e agentes operam com as mesmas regras e os mesmos números? | camada semântica ([`metrics-foundation`](./.claude/skills/metrics-foundation/SKILL.md)) |
| 8 | Dois agentes mexendo no mesmo lugar, um sabe do outro? | [`brain/quem-mexe-em-que.md`](./brain/quem-mexe-em-que.md) |
| 9 | Se ele errar, alguém descobre antes do cliente? | [`ads-reviewer`](./.claude/agents/ads-reviewer.md) + verificação pós-ação |
| 10 | Rodando a mesma coisa duas vezes, ele decide igual? | [`brain/decisoes.md`](./brain/decisoes.md) |

Quer que o Claude faça esse diagnóstico pra você? Peça: _"roda o company-brain check no meu setup"_ (skill [`company-brain`](./.claude/skills/company-brain/SKILL.md)).

---

## Os 5 pilares

### 1. Um cérebro não desliga quando você sai
Se ele só trabalha quando você abre o chat, ele é uma ferramenta, não um cérebro. O cérebro roda **sozinho, no horário, na nuvem, com log**: audit diário, vigia de gasto, revisão semanal. Ninguém precisa abrir nada pra ele trabalhar, e o time vê o resultado no canal de sempre.

→ Comece pelas 3 rotinas prontas em [`routines/`](./routines). Na Nekt rodamos dezenas.

### 2. Um cérebro sabe o que é ganhar e perder
Ele sabe qual número importa pro negócio, e não é o mais fácil de medir. Em mídia paga, o agente julga anúncio por **custo por reunião agendada (ou venda)**, lendo gasto e CRM juntos. CPL de formulário ele ignora: lead barato com e-mail pessoal infla o número e não vira pipeline.

E ele lê o dado **ao vivo, direto da fonte** (via MCP). Se alguém precisa exportar CSV e colar no chat, ele depende de você pra pensar.

→ A métrica norte está definida em [`brain/regras-do-negocio.md`](./brain/regras-do-negocio.md).

### 3. Um cérebro tem memória escrita
O que é venda, o que é lead bom, o que é cliente: escrito **uma vez, num lugar só**. As skills e os agentes **apontam** pra essa definição, não copiam. Mudou a regra, muda ali e todo agente já pensa com a nova.

O erro clássico: a definição de "lead qualificado" copiada em 7 prompts diferentes. Você muda em 3, esquece 4, e cada agente passa a contar de um jeito.

→ [`brain/`](./brain) é a constituição. [`knowledge/ads-history.md`](./knowledge/ads-history.md) é a memória do que já foi testado.

### 4. Pessoas e agentes pensam com o mesmo cérebro
O placar que o time vê na TV tem que ser **o mesmo número** que o agente usa pra decidir. Se o dashboard diz uma coisa e o agente calcula outra, vocês vão brigar com dois placares. A saída é a **camada semântica**: cada métrica definida uma vez na Nekt, e humano e agente leem dali.

E quando há mais de um agente, alguém decide **quem mexe em quê**. Dois agentes pausando e despausando a mesma campanha é pior que nenhum.

→ [`metrics-foundation`](./.claude/skills/metrics-foundation/SKILL.md) + [`brain/quem-mexe-em-que.md`](./brain/quem-mexe-em-que.md).

### 5. Um cérebro sabe quando está errado
Ele confere o próprio trabalho: depois de criar ou pausar, verifica na plataforma se aconteceu mesmo. Antes de uma decisão importante, um segundo agente (o **revisor**) confronta a proposta com as regras escritas. Ele separa fato de suposição.

E ele é **determinístico onde importa**: "pausar quando custo por reunião > 2× a meta depois de gastar o mínimo" decide igual hoje e amanhã. "Achei que tá ruim" não.

→ [`brain/decisoes.md`](./brain/decisoes.md) + [`.claude/agents/ads-reviewer.md`](./.claude/agents/ads-reviewer.md).

---

## Como montar o seu (ordem que funciona)

1. **Escreva as regras** (30 min): preencha [`brain/regras-do-negocio.md`](./brain/regras-do-negocio.md) com o que é venda, lead bom e cliente NO SEU negócio. É o arquivo mais importante do repo.
2. **Conecte o dado ao vivo**: MCP da Nekt ([`SETUP.md`](./SETUP.md)). Sem isso, ele pensa com o que você cola.
3. **Defina as regras de decisão**: ajuste os números de [`brain/decisoes.md`](./brain/decisoes.md) pra sua meta.
4. **Ligue 1 rotina**: comece pelo [audit diário](./routines/audit-diario.md). Deixe rodar 1 semana antes de ligar a próxima.
5. **Feche o loop**: toda decisão vai pro [`knowledge/ads-history.md`](./knowledge/ads-history.md). Em um mês ele sabe mais da sua operação do que qualquer pessoa nova no time.
6. **Cresça pelo erro**: cada vez que ele errar, a pergunta é "qual regra escrita faltou?". Escreva a regra em `brain/`, não só corrija a resposta.

> A regra de ouro: **o LLM orquestra, a regra decide.** O que é critério fica escrito e versionado; o modelo aplica, explica e executa.
