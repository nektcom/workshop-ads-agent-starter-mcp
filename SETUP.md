# Setup

Sem token de plataforma, sem processo de aprovação de API. Você só conecta o **MCP da Nekt** uma vez e preenche o contexto do seu produto. Leva ~15 minutos.

---

## 1. Pré-requisito: conta Nekt com fontes conectadas

1. Conta gratuita em [nekt.com](https://nekt.com).
2. Conecte pelo menos 1 fonte de dado (CRM tipo HubSpot/Pipedrive/Salesforce, e/ou as contas de ads Google/Meta/LinkedIn, e/ou produto/receita).
3. Aguarde o sync inicial (geralmente 1–4h, depende da fonte).

Sem isso, o assistente funciona só como "consultor de estratégia" — sem dado real.

---

## 2. Conectar o MCP

### Claude Cowork (sem terminal)

1. Abra o Cowork e arraste a pasta deste projeto pra dentro.
2. Configurações → **Conectores (MCP)** → **Adicionar conector** → Custom MCP.
3. URL: `https://mcp.nekt.com/mcp`
4. Faça o login (OAuth) na sua conta Nekt. Ícone fica verde quando conecta.
5. Dê permissão de leitura à pasta do projeto pra ele ver `.claude/` e `knowledge/`.

### Claude Code (terminal)

O repo já traz o [`.mcp.json`](./.mcp.json) com o servidor `nekt`. Só abrir o Claude na pasta:

```bash
git clone <seu-fork>.git
cd ads-agent-starter
claude
```

No Claude, autorize o MCP e confirme:

```
/mcp
```

Deve listar `nekt` como conectado (faz o OAuth no primeiro uso). **Não precisa editar nada com token** — a auth é toda pelo fluxo OAuth.

---

## 3. Preencher o contexto do seu produto

Os arquivos em `knowledge/` vêm com um exemplo fictício (**AcmeRH**, um SaaS de RH). Troque pelo seu produto real:

- `knowledge/product.md` — o que é, o que resolve, features, o que **não** faz.
- `knowledge/icp.md` — pra quem você vende (cargo, tamanho de empresa, dor).
- `knowledge/positioning.md` — como falar do produto + **o que NÃO falar** nos ads.
- `knowledge/pricing.md` — planos e ticket (o assistente usa pra julgar se o CAC faz sentido).
- `knowledge/ads-history.md` — o que já testou (vai crescendo conforme você roda).

Dica: peça ajuda ao assistente. _"Vamos preencher o knowledge/icp.md a partir dos meus clientes reais — puxa via MCP quem já fechou e me ajuda a descrever o padrão."_

---

## 4. Usar

```
@ads-agent
```

Exemplos de primeira conversa:
- _"Olha meus últimos 30 dias de ads (via MCP) e me diz onde estou queimando dinheiro."_
- _"Quero testar 4 vídeos novos no Meta. Como estruturo pra conseguir de fato comparar?"_
- _"De qual canal vieram os clientes que fecharam esse mês?"_

O assistente carrega o `.claude/agents/ads-agent.md` e as skills de `.claude/skills/` automaticamente. Ele lê o `knowledge/`, puxa o dado via MCP, e só age (subir/pausar) com a sua aprovação explícita.
