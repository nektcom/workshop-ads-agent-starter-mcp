---
name: icp-discovery
description: Como descobrir pra quem anunciar a partir de quem JÁ compra (dado real do CRM via MCP), não de suposição. Gera/atualiza knowledge/icp.md. Use quando o usuário quiser definir ou refinar público-alvo.
---

# icp-discovery

O melhor público de ads não sai da sua cabeça — sai de quem já fechou com você. Deixe o dado dizer.

## Como fazer (com MCP)
1. Puxe os **clientes fechados/ganhos** do CRM (via `nekt-mcp-data`).
2. Agrupe por padrões: **cargo/função**, **tamanho de empresa** (nº de funcionários), **segmento/indústria**, **como entraram** (canal), **ticket**.
3. Ache a concentração: onde estão os melhores clientes (maior ticket / menor churn / fecham mais rápido)?
4. Escreva isso em `knowledge/icp.md` como o ICP real, com evidência ("X% dos deals ganhos são Y").

## Cuidados
- **Melhor cliente ≠ mais clientes.** Pondere por valor/retenção, não só volume. Um segmento com muitos leads baratos que não retêm não é o ICP.
- **Não infira empresa por domínio de email pessoal** (gmail etc.) nem trate franquias/multi-CNPJ como uma empresa só.
- **Range de tamanho é limite, não sugestão vaga.** Se seu produto nunca vendeu pra empresa >500 pessoas, não expanda o público pra 500+ sem o usuário decidir.

## Usar o ICP nos ads
- No Meta, ICP vira **público** (mas lembre: pra TESTAR criativo use broad — ver `ads-testing-structure`; o ICP casado é mais pra escalar/lookalike).
- No LinkedIn, ICP vira segmentação por cargo/senioridade/indústria — evite funções operacionais quando você quer decisor de marketing/dados (ex: não confunda "operações" de campo com ops de marketing).
- No Google, o ICP informa quais keywords de intenção fazem sentido.

Mantenha `knowledge/icp.md` vivo: quando o CRM mostrar um padrão novo de quem fecha, atualize.
