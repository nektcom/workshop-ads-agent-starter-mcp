---
name: ads-gateway-publish
description: Como criar e operar anúncios (Google, Meta, LinkedIn) via MCP gateway da Nekt — sem credencial, com a auth por OAuth. Traz a ordem de criação por plataforma, os defaults seguros (PAUSED primeiro, sem signup) e a regra de só executar escrita com aprovação explícita. Use quando o usuário quiser subir/pausar/alterar algo.
---

# ads-gateway-publish

Criar campanha, ad set, criativo, pausar, subir budget — tudo isso vai pelo **MCP gateway** da Nekt, que fala com Google/Meta/LinkedIn por baixo e resolve a autenticação por OAuth. **Você não usa SDK nem token de plataforma.** Não há credencial neste projeto.

## Regra nº 1: escrita só com aprovação explícita
Toda ação que gasta ou muda entrega é uma **proposta** primeiro:
1. Diga **o que** vai fazer (ação concreta + valores).
2. Diga **por quê** (citando o número real que puxou via MCP, não achismo).
3. **Espere o usuário dizer "pode subir/pausar/mudar".** Sem isso, não execute a ferramenta de escrita.

Nunca "já subi pra adiantar". O gate é do usuário.

## Defaults seguros (hardcode mental)
- **Sempre criar PAUSED.** Revisar e ativar depois, com o usuário.
- **Destino nunca é /signup.** Home, landing page ou formulário nativo. Cold traffic entende o produto antes de criar conta.
- **Formulário nativo (lead gen):** se usar, o agradecimento manda pra agendamento/próximo passo, e capture o contato de forma que vire deal no CRM (senão o lead some do funil).
- **Valores:** confirme moeda e unidade (Meta usa centavos por baixo; o gateway normaliza, mas confirme o número em reais/dólares com o usuário).

## Ordem de criação por plataforma
**Meta:** Campaign (objetivo) → Ad Set (público, budget, schedule) → Criativo (upload de vídeo/imagem) → Ad (Ad Set + Criativo).
**Google (Search):** Campaign → Ad Group → Keywords → Responsive Search Ad (maximizar títulos/descrições).
**LinkedIn:** Campaign Group → Campaign (público) → Creative.

Ao **testar múltiplos criativos**, NÃO crie um ad set por criativo — leia `ads-testing-structure` (1 conjunto amplo, poucos criativos, budget concentrado). Esse é o erro que mais queima dinheiro.

## Antes de propor a criação, confirme com o contexto
- O público bate com o `knowledge/icp.md`? (Não estourar pra fora do ICP sem o usuário pedir.)
- A copy respeita `copy-rules` e o `positioning.md`?
- O criativo é vídeo/mensagem (não imagem estática)?
- O destino não é signup?
- A métrica de sucesso é a certa (custo/reunião, não CPL — ver `campaign-quality`)?

Se algum item falha, aponte antes de propor — não suba e conserte depois.

## Depois de criar
Verifique via MCP que o objeto ficou no estado esperado (PAUSED, servindo quando ativar, sem reprovação). Não assuma que funcionou — confirme.
