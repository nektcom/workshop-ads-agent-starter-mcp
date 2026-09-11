# ads-agent-starter (MCP-first)

Esqueleto pra você montar seu próprio assistente de ads com o Claude — **sem gerenciar credencial nenhuma**. Consumir dados e subir anúncios acontecem tudo pelo **MCP da Nekt**: os dados do seu CRM/ads/produto vêm via SQL, e as ações nas plataformas (criar campanha, pausar, ler performance) vão por um **MCP gateway** que já cuida da autenticação.

Não é uma ferramenta pronta. É um ponto de partida. Você baixa, conecta o MCP da Nekt, adapta o contexto pro seu produto, e o assistente vai ficando mais inteligente conforme você alimenta o contexto certo.

Feito pela [Nekt](https://nekt.com), que usa esse mesmo esqueleto (destilado de meses rodando os próprios ads) pra operar mídia paga.

---

## Por que MCP-first (e não scripts de API)

A versão "clássica" desse starter pedia um `.env` com token de Google Ads, Meta e LinkedIn — cada um com seu processo de aprovação, expiração e dor de cabeça. Aqui **não tem isso**:

- **Consumir dados** (CRM, ads, produto, receita) → SQL via **MCP da Nekt** sobre as fontes que você já conectou lá.
- **Subir/operar anúncios** (criar campanha, ad set, criativo, pausar, ler insights) → **MCP gateway** da Nekt, que fala com Google/Meta/LinkedIn por baixo e resolve a auth por OAuth.

Resultado: **nenhuma chave neste repositório**, nada pra vazar, e o mesmo assistente funciona pro seu produto trocando só o contexto em `knowledge/`.

---

## Qual caminho escolher

### Caminho 1: Claude Cowork (sem terminal, recomendado pra time não-técnico)

[Cowork](https://claude.com/product/cowork) é o app desktop do Claude. Você arrasta esta pasta pra dentro dele, conecta o MCP da Nekt em Configurações → Conectores, e pronto. Sem terminal, sem `git`. O Claude detecta o assistente (`.claude/agents/`) e as habilidades (`.claude/skills/`) sozinho.

**Ideal se:** você é de Growth, RevOps, Marketing, e quer usar sem virar dev.

### Caminho 2: Claude Code (terminal, recomendado pra dev)

[Claude Code](https://claude.ai/code) é a versão de linha de comando. Clona o repo, adiciona o MCP da Nekt no `.mcp.json` (já vem um exemplo aqui), abre o Claude na pasta.

Passo a passo dos dois em [`SETUP.md`](./SETUP.md).

---

## O que tem aqui

```
.claude/
  agents/ads-agent.md         # o assistente de ads (personalidade + regras de operação)
  skills/                     # módulos que ensinam o assistente a fazer cada coisa
    nekt-mcp-data/            # como puxar dado real via MCP (SQL pronto pras perguntas comuns)
    ads-gateway-publish/      # como criar/operar anúncio via MCP gateway (sem credencial)
    ads-testing-structure/    # COMO testar criativo sem queimar dinheiro (a lição mais cara)
    ads-attribution/          # de qual canal veio o cliente (sem se enganar com last-click)
    campaign-quality/         # medir a métrica CERTA (custo/reunião, não CPL de form)
    copy-rules/               # regras de copy que não fazem o anúncio parecer robô
    creative-fatigue/         # quando o criativo cansou
    icp-discovery/            # descobrir pra quem anunciar a partir de quem já compra
    positioning/              # como falar do produto (e o que NÃO falar)
    product-context/          # manter o contexto do produto vivo
    search-terms-pruning/     # cortar desperdício de search terms
knowledge/                    # o contexto do SEU produto (vem com exemplo fictício "AcmeRH")
  product.md  icp.md  positioning.md  pricing.md  ads-history.md
.mcp.json                     # config do MCP da Nekt (sem segredo — auth é por OAuth)
SETUP.md  SECURITY.md  GLOSSARIO.md
```

---

## Começando em 3 passos

1. **Conecta o MCP da Nekt** (Cowork: Conectores; Code: `.mcp.json`) — veja `SETUP.md`.
2. **Preenche o `knowledge/`** com o seu produto (os arquivos vêm com o exemplo fictício AcmeRH — troca pelo seu).
3. **Abre o Claude na pasta** e chama `@ads-agent`. Pede algo real: _"olha meus últimos 30 dias de ads e me diz onde estou queimando dinheiro"_.

O assistente lê o contexto, puxa o dado via MCP, e responde com base na sua realidade — não em achismo.

---

## Aviso

Esse assistente opera sobre contas que gastam **dinheiro real**. Ele foi instruído a nunca subir/pausar/alterar nada sem você aprovar explicitamente. Leia `SECURITY.md`. A responsabilidade pelo que sobe é sua.
