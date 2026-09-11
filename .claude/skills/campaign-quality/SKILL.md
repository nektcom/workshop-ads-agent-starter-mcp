---
name: campaign-quality
description: Como medir a qualidade de uma campanha pela métrica CERTA — custo por reunião/conta/venda com qualidade (B2B, email corporativo), não CPL de formulário. Use ao avaliar se uma campanha está boa, ou antes de escalar/matar.
---

# campaign-quality

CPL de formulário é a métrica que engana mais. Volume alto de lead barato com email pessoal parece ótimo e não vira pipeline. Meça o que vira dinheiro.

## A métrica norte
**Custo por conversão de verdade** — reunião agendada / conta criada / venda — **com qualidade de lead**. Não CPL de form.

- **Lead** = qualquer contato que entrou.
- **Lead qualificado (proxy mínimo B2B)** = email **corporativo** (exclui gmail/hotmail/outlook/yahoo/icloud e afins). Volume com email pessoal infla e não converte.
- **Conversão** = o próximo passo comercial real (reunião, conta ativada, oportunidade).
- **Venda** = negócio fechado/ganho no CRM.

## Como avaliar uma campanha (ordem)
1. Puxe via MCP: gasto, leads, **% de leads corporativos**, reuniões/contas, vendas, receita — por campanha e período.
2. Calcule **custo por reunião** (ou por conta/venda), não custo por lead.
3. Cruze com o ticket (`knowledge/pricing.md`): o custo de aquisição faz sentido pro valor do cliente?
4. Só então diga se está boa. Uma campanha com CPL baixo e 0 reunião é **ruim**, mesmo parecendo eficiente.

## Cuidados que evitam conclusão errada
- **Reunião AGENDADA ≠ ocorrida.** Desconte no-show quando o dado permitir (histórico/status).
- **Lag de conversão:** reunião/venda demora dias a semanas depois do lead. Uma campanha nova (BOFU) tem carência — não julgue conversão no dia 1.
- **Atribua o deal por UTM/deal, não pelo campo de origem do CRM** (ver `ads-attribution`).
- **Exclua deals duplicados/merged** ao contar vendas.
- **Não julgue sem entrega/spend mínimo** (ver `ads-testing-structure`).

## Régua por estágio (não cobre tudo do mesmo jeito)
- **TOFU (marca/topo, ex: LinkedIn):** régua = alcance + CPC barato + branded search subindo. **NÃO** julgue por lead/reunião direto — o efeito é upstream, aparece depois em outro canal. Não mate por "não converter".
- **MOFU:** lead qualificado a custo razoável.
- **BOFU (fundo, intenção alta):** aí sim custo por reunião/venda manda. Corte só em BOFU **maduro** (passou a carência + gastou o mínimo) sem conversão.
