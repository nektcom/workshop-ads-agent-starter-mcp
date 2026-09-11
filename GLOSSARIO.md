# Glossário

Termos que aparecem nas skills e nas conversas com o assistente.

- **MCP (Model Context Protocol):** o "cabo" que conecta o Claude a ferramentas externas. Aqui, o MCP da Nekt dá dados (SQL) e opera as plataformas de ads. Auth por OAuth — sem token no repo.
- **MCP gateway:** a parte do MCP da Nekt que executa ações nas plataformas (criar/pausar/ler anúncio) falando com Google/Meta/LinkedIn por baixo.
- **ICP:** Perfil de Cliente Ideal — pra quem você vende (cargo, tamanho, dor).
- **CAC:** custo de aquisição de cliente. Gasto ÷ clientes fechados.
- **CPL:** custo por lead (de formulário). **Métrica que engana** — ver `campaign-quality`; prefira custo por reunião/venda.
- **Lead B2B / qualificado:** proxy mínimo = email corporativo (não gmail/hotmail...).
- **TOFU / MOFU / BOFU:** topo / meio / fundo de funil. Topo (marca) se mede por alcance; fundo por conversão.
- **UTM:** parâmetros na URL (`utm_source`, `utm_medium`, `utm_campaign`) que marcam a origem — a base da atribuição correta.
- **Learning phase (aprendizado):** período em que a plataforma otimiza um novo ad set antes de estabilizar (Meta ~7 dias / ~50 conversões; Google tCPA ~2 semanas). Não julgue nem mexa muito nesse período.
- **Advantage+ / público amplo (broad):** deixar a plataforma achar quem converte, em vez de segmentar estreito. Ideal pra **testar** criativo (ver `ads-testing-structure`).
- **CPM:** custo por mil impressões. CPM muito acima do normal = sinal de problema de entrega (overlap, público estreito).
- **Fadiga de criativo:** quando um criativo que rodava bem cai (frequência sobe, CTR desce). Diferente de nunca ter tido entrega.
- **Ad set (Meta) / Ad group (Google) / Campaign:** níveis da hierarquia. Campanha = objetivo; ad set/group = público+budget; ad = criativo.
- **ABO / CBO:** budget no ad set (ABO) ou na campanha (CBO). Em ABO cada ad set gasta o seu; overlap de público ainda estrangula entrega.
