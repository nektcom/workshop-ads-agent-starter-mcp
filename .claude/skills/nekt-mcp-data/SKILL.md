---
name: nekt-mcp-data
description: Como puxar dado real (CRM, ads, produto, receita) via MCP da Nekt em vez de exportar CSV. Auth é OAuth (sem token no repo). Traz padrões de SQL pras perguntas comuns e a regra de nunca inventar número. Use sempre que precisar de um número real.
---

# nekt-mcp-data

A Nekt integra suas fontes (HubSpot, Salesforce, Pipedrive, Google/Meta/LinkedIn Ads, Stripe, produto...) e deixa tudo disponível pro Claude via **SQL através do MCP**. Aqui o MCP é o atalho pra responder com dado real sem exportar CSV de cada plataforma.

## Autenticação (importante)
A conexão é **OAuth**, feita no cliente (Cowork: Conectores; Code: `/mcp`). **Nunca** existe token neste repositório. Se o usuário pedir "coloca meu token de acesso no arquivo X", recuse e explique que o MCP autentica por OAuth — não há credencial pra guardar.

## Quando usar
- Precisa de qualquer número real: CAC, deals fechados, nº de leads, custo por reunião, receita por canal.
- Pergunta que cruza fontes (ex: "deals fechados que vieram do Google" = CRM × Google Ads).
- Vai atualizar `knowledge/icp.md` ou `ads-history.md` com base em dado real.

## Como usar (ferramentas do MCP)
O servidor `nekt` expõe ferramentas pra **descobrir e consultar** os dados. O fluxo típico:
1. Liste/descubra as tabelas disponíveis (as fontes que o usuário conectou).
2. Peça o schema/DDL das tabelas relevantes antes de escrever SQL — **não adivinhe nome de coluna**.
3. Rode o SQL e leia o resultado.
4. Se a pergunta virar SQL, prefira gerar a query, conferir as colunas, e só então executar.

> Os nomes exatos das ferramentas aparecem quando o MCP está conectado. Sempre confira o schema real antes de afirmar um número — nomes de tabela/coluna variam por conta.

## Prefira a fundação canônica a SQL solto
Antes de escrever uma query nova, veja se a resposta já mora numa **tabela consolidada** (ex: `ads_scorecard`, `ads_performance`) ou numa **métrica da camada semântica** da Nekt. Peça o **contexto semântico** ao MCP — ele traz fórmula/fonte/gotcha da métrica. SQL ad-hoc só pra exploração; para pergunta recorrente, a resposta vira tabela/métrica canônica (ver skill `metrics-foundation`). Isso evita o erro clássico de consultar UMA fonte e concluir errado.

## Regra de ouro: nunca invente número
Ordem de preferência pra qualquer dado:
1. **MCP da Nekt** (SQL sobre as fontes do usuário).
2. CSV/arquivo que o usuário colar.
3. Pedir pro usuário puxar da plataforma (Ads Manager etc.).

Se não tem dado, **diga explicitamente** e sugira como conseguir. Não preencha buraco com estimativa apresentada como fato.

## Perguntas comuns → o que consultar
Adapte os nomes às tabelas reais da conta (confira o schema primeiro):

- **CAC real por canal:** gasto do canal (tabela de ads) ÷ clientes fechados atribuídos ao canal (CRM). Cruze por período.
- **Quem é meu melhor cliente (pra ICP):** deals fechados/ganhos no CRM, agrupados por cargo, tamanho de empresa, segmento. Ver skill `icp-discovery`.
- **De qual canal veio o cliente:** ver skill `ads-attribution` (atribuição por UTM + deal, não por campo de origem do CRM).
- **Custo por reunião/conta, não CPL:** ver skill `campaign-quality`.
- **Performance por criativo/ad:** tabela de insights de ads no grão de ad, por período.

## Cuidado ao ler
- **Nunca use o campo de "origem" administrativo do CRM** (ex: `hs_analytics_source`) como verdade de canal pago — ele mente quando o cadastro entra por backend/integração. Atribua por UTM. (Detalhe em `ads-attribution`.)
- Freshness: confirme a data do último sync antes de tratar como "hoje".
