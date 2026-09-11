---
name: ads-attribution
description: Como saber de qual canal veio o cliente sem se enganar. Nunca usar o campo de origem administrativo do CRM nem last-click cego; atribuir por UTM do toque pago + tratar o DEAL como fonte da verdade. Use quando o usuário perguntar "de onde vieram os clientes/leads" ou for medir canal.
---

# ads-attribution

Atribuição errada faz você escalar o canal errado e matar o certo. As armadilhas comuns e como escapar.

## Armadilha 1: o campo de origem do CRM mente
Campos automáticos de "origem" do CRM (ex: `hs_analytics_source` no HubSpot) parecem a resposta fácil, mas mentem em casos comuns:
- Cadastro que entra por **backend/integração/produto** aparece como "offline/direct", não como o ads que trouxe a pessoa.
- A **call de vendas** (link de agendamento) frequentemente **sobrescreve** a origem original — e o cliente pago vira "direct".

**Não use esse campo como verdade de canal pago.** Use **UTM**.

## Armadilha 2: last-click cego
Olhar só o último toque subconta canais de **topo** (marca, LinkedIn, conteúdo) que criam a demanda que depois converte por outro canal. Se um canal é upstream (planta a semente), ele não vai aparecer no last-click — não o mate por "não converter".

## O jeito certo
1. **Atribua por UTM do toque pago:** `utm_medium` pago (paid_social, cpc, paid) ou `utm_source` de rede (facebook, meta, instagram, linkedin, google) + `utm_campaign`. Isso vive no contato/deal.
2. **O DEAL é a fonte da verdade.** O que importa é o canal atrelado ao **negócio fechado**, não a um evento solto. Se o time preenche origem manual/parceiro, isso entra no deal.
3. **Multi-toque:** se há **qualquer** toque pago na jornada, dê crédito ao pago (não deixe a call de vendas apagar o canal). Quando der, cruze com o **formulário/histórico** pra recuperar o canal real.
4. **Exclua ruído:** toques de link de agendamento (`utm_campaign` tipo `meeting-*`) não são origem — são etapa interna.

## Como puxar (via MCP)
- Contatos/deals com `utm_medium`/`utm_source` de pago, agrupados por `utm_campaign` e mês.
- Cruze com a tabela de gasto de ads (mesmo `utm_campaign`/nome de campanha) pra ter custo por resultado por canal.
- Confira os nomes reais das colunas no schema antes (ver `nekt-mcp-data`).

## Regra de leitura
Se um número de "canal" depende do campo de origem administrativo do CRM, trate como **suspeito** e recalcule por UTM/deal. Diga ao usuário quando a atribuição estiver frágil (ex: muitos deals pagos sem canal por causa da sobrescrita da call) — não finja precisão que não existe.
