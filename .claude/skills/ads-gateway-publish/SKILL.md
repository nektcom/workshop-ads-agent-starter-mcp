---
name: ads-gateway-publish
description: Como LER e OPERAR anúncios via MCP gateway da Nekt — sem credencial, auth por OAuth. Google Ads e Meta/Facebook operam ciclo completo; LinkedIn já LÊ via MCP (escrita liberando). Traz o mapa das ferramentas disponíveis hoje, como descobrir os nomes exatos (são por-org), os defaults seguros (PAUSED primeiro, sem signup) e a regra de só executar escrita com aprovação explícita. Use quando o usuário quiser ver, subir, pausar ou alterar algo.
---

# ads-gateway-publish

Ver performance, criar campanha, subir vídeo, pausar, mexer em budget, negativar keyword — **tudo vai pelo MCP gateway da Nekt**, que fala com Google Ads e Meta por baixo e resolve a autenticação por OAuth. **Você não usa SDK nem token de plataforma.** Não há credencial neste projeto.

## Regra nº 1: escrita só com aprovação explícita
Toda ação que gasta ou muda entrega é uma **proposta** primeiro:
1. Diga **o que** vai fazer (ação concreta + valores).
2. Diga **por quê** (citando o número real que puxou via MCP, não achismo).
3. **Espere o usuário dizer "pode subir/pausar/mudar".** Sem isso, não execute a ferramenta de escrita.

Nunca "já subi pra adiantar". O gate é do usuário.

## Como achar as ferramentas (os nomes são por-org)
As ferramentas de ads NÃO aparecem na sua lista de tools fixa — elas são **ao vivo**, por conector que você ligou na Nekt. E o nome tem um **prefixo da sua org** no começo. Ex: a ação "pausar campanha no Google" pode se chamar `google_ads_ab12_update_campaign` na sua conta e `google_ads_xy99_update_campaign` em outra.

Então o fluxo é sempre:
1. **`discover_live_tools`** → lista os conectores que VOCÊ ligou e o nome exato de cada ferramenta.
2. **`run_live_tool(tool_name="<o nome exato>", arguments={...})`** → executa.
3. Antes de uma escrita, **leia a `description` da ferramenta** (via `discover_live_tools(tool_name=...)`): é lá que estão as regras reais (qual campo é obrigatório, unidade de valor, qual id vem de onde). O `input_schema` sozinho não conta isso.

Se uma ferramenta que você espera não aparecer, rode `discover_live_tools` UMA vez pra atualizar (pode ser conector ligado agora). Se ainda não aparecer, o conector não está ligado na Nekt — fale pro usuário conectar em nekt.com.

## O que o gateway faz hoje (mapa de capacidades)

### Google Ads — ciclo completo
- **Ver:** listar campanhas / grupos / anúncios / keywords / listas de negativas; performance por campanha; rodar **GAQL** (query livre do Google Ads); achar ids de localização; listar contas acessíveis.
- **Criar:** campanha (nasce PAUSED), budget diário, grupo de anúncios, **anúncio de busca responsivo (RSA)**, lista de negativas (shared set).
- **Operar:** ativar/pausar campanha, grupo ou anúncio; mudar budget, estratégia de lance, geo, idiomas, agendamento (dias/horas), modificador de lance por device, redes (Search/Partners); adicionar/remover keywords e negativas; anexar lista de negativas a campanha; remover objeto.

### Meta / Facebook — ciclo completo (gaps antigos já fechados)
- **Ver:** listar contas / campanhas / conjuntos / anúncios / lead forms; performance (por campanha e por anúncio); **pegar os leads** submetidos num Lead Ads form.
- **Criar:** campanha, conjunto, anúncio de **imagem / link / vídeo**, **lead form**; upload de imagem e de vídeo pra biblioteca.
- **Operar:** ativar/pausar campanha, conjunto ou anúncio; atualizar conjunto; **atualizar o criativo** de um anúncio; **duplicar** campanha / conjunto / anúncio / lead form; deletar objeto.

### LinkedIn — LEITURA via MCP ok, OPERAÇÃO (escrita) ainda liberando
Já tem conector de LinkedIn Ads no gateway e a **leitura funciona**: listar contas / campaign groups / campanhas / criativos / conversões, performance por campanha e por criativo, e **resolver URNs de targeting** (`find_targeting_urns`). Dá pra o assistente ANALISAR LinkedIn via MCP hoje.

As ferramentas de **escrita** existem (criar campanha / sponsored content / **Thought Leader Ad** / InMail, set targeting, pausar/ativar campanha e criativo, upload de mídia) **mas podem voltar 403 de permissão** até a conta do cliente ter a escrita liberada no app do conector (Advertising API). Se um write der 403, não é argumento errado — é permissão: avise o cliente a liberar na Nekt, não fique retentando.

### Audiências e sinal de conversão (via destinations da Nekt)
Além do gateway de operação, a Nekt tem **destinations** pra: mandar uma audiência (lista de clientes/leads) pro Facebook Custom Audiences, mandar eventos de conversão de volta (Facebook Conversions API / CAPI — ensina o Meta a buscar quem CONVERTE, não só quem clica), e Customer Match no Google. Use quando o usuário quiser "achar mais gente parecida com quem fecha" ou "dizer pro Facebook quais leads foram bons".

## Você pode pedir ao assistente (exemplos em português)
- "Como foi a campanha X essa semana?" → ele puxa a performance.
- "Quais leads caíram hoje no formulário?" → ele busca os leads do form.
- "Pausa a campanha Y." → ele propõe, você confirma, ele pausa.
- "Sobe o budget da Z em 20%." → ele calcula o valor, você confirma, ele muda.
- "Negativa o termo 'grátis'." → ele adiciona à lista de negativas.
- "Cria um teste com esses 3 vídeos." → ele sobe os vídeos e cria os anúncios **PAUSED**, te mostra o preview, e só ativa com teu ok.

## Defaults seguros (hardcode mental)
- **Sempre criar PAUSED.** Revisar e ativar depois, com o usuário.
- **Destino nunca é /signup.** Home, landing page ou formulário nativo. Cold traffic entende o produto antes de criar conta.
- **Formulário nativo (lead gen):** o agradecimento manda pra agendamento/próximo passo, e capture o contato de forma que vire deal no CRM (senão o lead some do funil).
- **Valores:** confirme moeda e unidade. Meta usa centavos por baixo; leia a `description` da ferramenta, que diz a unidade esperada, e confirme o número em reais/dólares com o usuário.

## Ordem de criação por plataforma
**Meta:** Campaign (objetivo) → Ad Set (público, budget, schedule) → upload do vídeo/imagem → Ad (de vídeo/imagem/link, no Ad Set).
**Google (Search):** Campaign → Budget → Ad Group → Keywords → Responsive Search Ad (maximizar títulos/descrições).

Ao **testar múltiplos criativos**, NÃO crie um conjunto por criativo — leia `ads-testing-structure` (1 conjunto amplo, poucos criativos, budget concentrado). Esse é o erro que mais queima dinheiro. Pra duplicar rápido um conjunto/campanha que já funciona, use as ferramentas de **copy** do Meta.

## Antes de propor a criação, confirme com o contexto
- O público bate com o `knowledge/icp.md`? (Não estourar pra fora do ICP sem o usuário pedir.)
- A copy respeita `copy-rules` e o `positioning.md`?
- O criativo é vídeo/mensagem (não imagem estática)?
- O destino não é signup?
- A métrica de sucesso é a certa (custo/reunião, não CPL — ver `campaign-quality`)?

Se algum item falha, aponte antes de propor — não suba e conserte depois.

## Depois de criar
Verifique via MCP que o objeto ficou no estado esperado (PAUSED, servindo quando ativar, sem reprovação). Não assuma que funcionou — confirme listando de novo.
