---
name: metrics-foundation
description: Como montar a fundação de análise CERTA — tabelas consolidadas canônicas + camada semântica na Nekt + regras determinísticas de decisão — em vez de gerar SQL solto e julgar no olho toda vez. Use antes de análises recorrentes, ou quando o assistente estiver escrevendo query ad-hoc pra uma pergunta que se repete.
---

# metrics-foundation

A maior fonte de erro em análise de ads não é falta de dado — é **cada análise reinventar a conta**. SQL ad-hoc diferente toda vez → números que não batem, atribuição furada, decisão no achismo. A fundação resolve isso em três camadas. Construa-as **na Nekt**, não na cabeça do assistente.

## 1. Tabelas consolidadas (fonte única)
Em vez de o assistente juntar tabelas cruas a cada pergunta (e errar — ex: consultar UM formulário e perder metade dos leads), monte **tabelas transformadas canônicas** na Nekt, uma por grão:

- **`ads_performance`** (por campanha/mês): investimento, leads, leads B2B, reuniões, vendas, receita, custo/lead, custo/reunião, custo/venda. Roda todo dia.
- **`ads_scorecard`** (fonte única do time): o funil completo + status semáforo por campanha.
- **`ads_por_criativo`** / **`ads_por_conjunto`**: mesmos números no grão de ad/ad set.

Regra: se uma pergunta se repete, ela vira **tabela**, não query nova. O assistente lê a tabela consolidada; não remonta o join. (Isso mata o erro clássico de "consultei uma fonte e concluí errado".)

## 2. Camada semântica na Nekt
Defina cada métrica **UMA vez**, canonicamente, na camada semântica da Nekt — fórmula + fonte + filtros + o "gotcha" (ex: "reunião = agendada menos no-show", "lead B2B = email corporativo", "atribuição = UTM, nunca campo de origem do CRM"). Assim:

- O assistente **puxa a definição** (via o contexto semântico do MCP) em vez de inventar a conta.
- Todo mundo (humano e agente) usa a **mesma** definição — o número não muda dependendo de quem perguntou.
- Novos "gotchas" descobertos viram parte da camada, não folclore que se perde.

Antes de escrever SQL de métrica, **peça o contexto semântico** ao MCP. Se a métrica ainda não existe lá, o certo é **criá-la na camada semântica**, não gerar um SQL solto que só você conhece.

## 3. Regras determinísticas de decisão (não julgamento aberto)
Decisão de ads não deveria depender do "feeling" do assistente naquele dia. Encode as regras — thresholds explícitos, aplicados igual sempre:

- **Pausar:** custo/reunião > 2× a meta **E** gastou o mínimo pra julgar **E** fora da carência (learning) **E** não é canal de topo (marca) → propor pausa.
- **Escalar:** custo/reunião abaixo da meta com volume → propor +budget (em degraus ≤30-50%).
- **Não julgar sem entrega:** impressões abaixo do mínimo → veredito = "sem dado", nunca "ruim".
- **Barrar destino errado:** anúncio pra /signup → bloquear antes de propor.

O LLM **aplica** a regra e explica citando o número da tabela canônica; ele não improvisa o critério. Regra determinística > julgamento solto, porque é consistente, auditável e não erra por cansaço/contexto.

## Como o assistente constrói isso (via MCP, passo a passo)
Você não monta na mão — o assistente usa as ferramentas da Nekt pelo MCP:

1. **Ver o que já existe primeiro.** Liste as tabelas e as camadas (layers) da conta via MCP. Se `ads_performance` (ou equivalente) já existe, **use** — não recrie. Peça o schema real antes de escrever qualquer coisa.
2. **Criar/editar a transformada.** Use as ferramentas de transformação do MCP (criar/atualizar a transformada — é código Python/SQL que a Nekt roda no lakehouse — e rodar o pipeline). Comece pela **`ads_performance`**: uma transformada que junta o gasto por plataforma (Google/Meta/LinkedIn) com os deals do CRM, no grão **campanha × dia**, com merge incremental.
3. **Materializar + conferir.** Rode o pipeline, leia algumas linhas da tabela e confirme que os números batem com a plataforma **antes** de confiar. Não assuma que o join ficou certo.
4. **Definir na camada semântica.** Puxe o **contexto semântico** (a definição canônica) via MCP; se a métrica ainda não existe lá, **crie o documento de contexto** com fórmula + fonte + filtros + o "pega-ratão". Toda métrica que o cliente usa pra DECIDIR vira documento aqui.
5. **Daí pra frente:** o assistente **só lê** a tabela consolidada + a definição semântica. Query crua fica só pra exploração pontual, nunca pra decisão recorrente.

> Comece pequeno: `ads_performance` + a definição de "custo por reunião" na semântica + as 4 regras de decisão acima. Cresça conforme a operação pedir — não precisa montar tudo de uma vez.

## Por quê isso importa (a lição)
Análise ad-hoc mente em transição e em caso de borda (formulário compartilhado, atribuição sobrescrita, criativo sem entrega). Tabela canônica + camada semântica + regra fixa transformam "achismo do dia" em "fato repetível". O assistente fica mais rápido, mais barato e para de errar a mesma conta duas vezes.

> Construa isso incrementalmente: comece pela `ads_performance` (a fundação) + a definição de custo/reunião na camada semântica + as 4 regras acima. O resto cresce conforme a operação pede.
