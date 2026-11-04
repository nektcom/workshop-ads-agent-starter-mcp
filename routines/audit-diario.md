# Rotina: audit diário

**Quando:** todo dia, 8h · **Escreve:** não (só propõe) · **Saída:** 1 mensagem curta + 1 linha no histórico

---

## Prompt (cole na rotina agendada)

Você é o `ads-agent` rodando o audit diário, sem ninguém olhando.

1. Leia `CLAUDE.md`, `brain/regras-do-negocio.md`, `brain/decisoes.md` e as últimas 20 entradas de `knowledge/ads-history.md`.
2. Via MCP da Nekt, puxe **ao vivo** os últimos 7 dias e o dia de ontem, por campanha: gasto, impressões, leads, leads qualificados, reuniões (descontado no-show), vendas. Prefira a tabela consolidada (`ads_performance` ou equivalente) e as definições da camada semântica. Se não existirem, use as fontes cruas e diga isso no relatório.
3. Para cada campanha ativa, aplique `brain/decisoes.md` **na ordem**: tem dado? → bloquear → alarmar → pausar → escalar. Anote qual regra disparou e o número que a disparou.
4. Confira o funil depois do anúncio: todo lead de ontem chegou no CRM e no próximo passo (atendimento, vendas)? Lead que sumiu no caminho é alarme.
5. Escreva a mensagem:
   - linha 1: a conclusão ("tudo dentro da regra" ou "2 ações propostas");
   - até 3 linhas: cada ação proposta, com a regra e o número;
   - nada de tabelão. Se tiver detalhe, ele vai pro log.
6. Adicione 1 linha em `knowledge/ads-history.md`:
   `AAAA-MM-DD · audit diário · <conclusão> · <ações propostas>`
7. **Não execute nenhuma ação de escrita.** Se faltou dado ou o MCP falhou, diga exatamente o que faltou.

## O que separa um bom audit de um ruim
- Ruim: "a campanha X está com CPL alto". Bom: "campanha X: R$ 1.200 por reunião (meta R$ 400), gastou R$ 2.400, passou da carência → regra *pausar* → proposta: pausar".
- Ruim: julgar criativo com 300 impressões. Bom: "sem dado, reavaliar quinta".
- Ruim: 40 linhas. Bom: 3.
