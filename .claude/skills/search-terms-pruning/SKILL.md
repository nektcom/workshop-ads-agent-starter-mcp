---
name: search-terms-pruning
description: Como cortar desperdício de search terms no Google Ads (termos que gastam sem converter) via MCP, negativando com critério — sem matar termo só pelo CPC. Use na auditoria de Google Search.
---

# search-terms-pruning

No Google Search você paga por cliques de buscas que às vezes não têm nada a ver. Cortar isso (negative keywords) é dinheiro recuperado — mas com critério.

## Fluxo
1. Puxe os **search terms** dos últimos dias via MCP (o termo real que a pessoa buscou, gasto, cliques, conversões).
2. Classifique:
   - **Irrelevante ao produto** (buscou outra coisa) → negativar.
   - **Curioso/estudante** (termo conceitual genérico, gasta e não converte) → negativar ou rebaixar.
   - **Relevante mas caro sem conversão** → ver "cuidado com CPC" abaixo antes de matar.
   - **Relevante que converte** → manter/escalar.
3. Proponha a lista de negativas ao usuário; **só aplique com o OK** (ver `ads-gateway-publish`).

## Cuidado: não mate termo só pelo CPC
CPC alto em B2B é normal (keywords competitivas custam caro). O que importa é **resultado**, não o CPC isolado. Descarte um termo quando: **não converte após gasto mínimo** E o CPC está alto. CPC alto + conversão = mantém.

## Match type e exploração
- Broad match explora e acha keywords novas, mas espalha lixo → exige negativação frequente (cadência tipo 2-3x ao dia em conta ativa) e **smart bidding** (nunca broad com lance manual).
- Exact/phrase controla mais, explora menos.

## Registre
Negativas relevantes e migrações de keyword vão pro `ads-history.md` — pra não re-testar o que já provou ser lixo.
