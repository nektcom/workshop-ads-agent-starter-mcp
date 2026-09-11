<div align="center">

# 🎯 ads-agent-starter (MCP-first)

**Monte seu próprio assistente de mídia paga com o Claude — sem gerenciar uma credencial sequer.**

Dados e operação de anúncios acontecem tudo via **MCP da Nekt**: SQL sobre suas fontes pra consumir, e um **gateway** pra criar/pausar/ler campanhas. Auth por OAuth. Zero token no repositório.

[![Built with Claude](https://img.shields.io/badge/Built%20with-Claude-D97757)](https://claude.ai/code)
[![MCP](https://img.shields.io/badge/MCP-Nekt-000000)](https://nekt.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-2ea44f)](./LICENSE)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-blue)](#contribuindo)

</div>

---

Não é uma ferramenta pronta. É um **ponto de partida**: você baixa, conecta o MCP da Nekt, troca o contexto pelo seu produto, e o assistente fica mais inteligente conforme você o alimenta.

Destilado de meses operando os próprios ads da [Nekt](https://nekt.com) — as lições que custaram dinheiro já vêm embutidas nas skills.

## Índice
- [Por que MCP-first](#por-que-mcp-first-e-não-scripts-de-api)
- [Qual caminho escolher](#qual-caminho-escolher)
- [O que tem aqui](#o-que-tem-aqui)
- [Começando em 3 passos](#começando-em-3-passos)
- [As lições embutidas](#as-lições-embutidas-o-diferencial)
- [Segurança](#segurança)
- [Créditos](#créditos)

---

## Por que MCP-first (e não scripts de API)

A versão "clássica" desse starter pedia um `.env` com token de Google Ads, Meta e LinkedIn — cada um com aprovação, expiração e dor de cabeça. Aqui **não tem isso**:

| | Clássico (scripts de API) | **Este (MCP-first)** |
|---|---|---|
| Consumir dados | export de CSV / SDK por plataforma | **SQL via MCP da Nekt** |
| Operar anúncios | scripts com token no `.env` | **MCP gateway** (OAuth) |
| Credenciais no repo | vários tokens | **nenhuma** |
| Trocar de produto | reconfigurar tudo | trocar só `knowledge/` |

Resultado: **nada pra vazar**, e o mesmo assistente serve pra qualquer empresa.

## Qual caminho escolher

### Caminho 1 — Claude Cowork (sem terminal, pra time não-técnico)
[Cowork](https://claude.com/product/cowork) é o app desktop. Arraste esta pasta pra dentro, conecte o MCP da Nekt em Configurações → Conectores, e pronto. O Claude detecta o agente e as skills sozinho. **Ideal se** você é de Growth/RevOps/Marketing e quer usar sem virar dev.

### Caminho 2 — Claude Code (terminal, pra dev)
Clone o repo, o `.mcp.json` já vem configurado, abra o Claude na pasta e rode `/mcp`. **Ideal se** você já vive no terminal.

Passo a passo em **[`SETUP.md`](./SETUP.md)**.

## O que tem aqui

```
.claude/
  agents/ads-agent.md         # o assistente (personalidade + regras de operação)
  skills/                     # 12 módulos que ensinam o assistente a fazer cada coisa
    nekt-mcp-data/            # puxar dado real via MCP (SQL pronto)
    metrics-foundation/       # tabelas consolidadas + camada semântica + regras (não achismo)
    ads-gateway-publish/      # criar/operar anúncio via MCP gateway (sem credencial)
    ads-testing-structure/    # testar criativo sem queimar dinheiro (a lição mais cara)
    campaign-quality/         # medir a métrica CERTA (custo/reunião, não CPL)
    ads-attribution/          # de qual canal veio o cliente (sem se enganar)
    copy-rules/               # copy que não parece robô
    creative-fatigue/         # quando o criativo cansou
    icp-discovery/            # descobrir pra quem anunciar via dado real
    positioning/ product-context/ search-terms-pruning/
knowledge/                    # o contexto do SEU produto (vem com exemplo fictício "AcmeRH")
.mcp.json                     # config do MCP (sem segredo — auth é OAuth)
SETUP.md  SECURITY.md  GLOSSARIO.md  LICENSE
```

## Começando em 3 passos

1. **Conecte o MCP da Nekt** (Cowork: Conectores; Code: `/mcp`) — [`SETUP.md`](./SETUP.md).
2. **Preencha o `knowledge/`** com o seu produto (vem com o fictício AcmeRH — troque pelo seu).
3. **Abra o Claude na pasta** e chame `@ads-agent`. Peça algo real:
   > _"Olha meus últimos 30 dias de ads e me diz onde estou queimando dinheiro."_

O assistente lê o contexto, puxa o dado via MCP e responde com base na **sua** realidade — não em achismo. E só sobe/pausa nada com a sua aprovação explícita.

## As lições embutidas (o diferencial)

Isto não é conselho genérico de internet. Cada skill carrega uma lição que custou dinheiro real na operação da Nekt:

- **Testar criativo = concentrar, não espalhar.** 1 conjunto amplo, poucos criativos por vez — não 1 ad set por vídeo (isso estrangula a entrega e você não aprende nada). → `ads-testing-structure`
- **Meça custo por reunião/venda, não CPL de formulário.** Lead barato com email pessoal infla número e não vira pipeline. → `campaign-quality`
- **Atribua por UTM + deal, nunca pelo campo de origem do CRM** (ele mente). → `ads-attribution`
- **Não reinvente a conta a cada pergunta:** tabela consolidada + camada semântica + regra determinística > SQL solto e julgamento no feeling. → `metrics-foundation`
- **Não julgue sem entrega.** Criativo com poucas impressões não é "ruim", é *sem dado*.

## Segurança

Projetado pra **não ter credencial nenhuma no repo** (auth é OAuth do MCP). Antes de forkar/publicar, rode o checklist de [`SECURITY.md`](./SECURITY.md) — inclui um `git grep` que caça segredos e um lembrete de revisar `knowledge/` (dado de negócio, nunca token/PII).

## Contribuindo

PRs bem-vindos. Ideias: mais skills (novos canais, novas lições), exemplos de `knowledge/` pra outros tipos de produto, um `COWORK_SETUP.md` dedicado. Abra uma issue ou mande um PR.

## Créditos

Criado por **[Luana Pereira (Lupe)](https://nekt.com)** — GTM Engineer na [Nekt](https://nekt.com), que usa este mesmo esqueleto pra operar a própria mídia paga. As skills são a destilação de meses testando, errando e aprendendo em Google, Meta e LinkedIn Ads.

Construído com [Claude Code](https://claude.ai/code) + [Claude Cowork](https://claude.com/product/cowork) e o [MCP da Nekt](https://nekt.com).

Se este starter te ajudou, deixa uma ⭐ — e conta pra gente o que você construiu em cima dele.

---

<div align="center">
<sub>Feito com ☕ e budget de ads de verdade · <a href="https://nekt.com">nekt.com</a></sub>
</div>

## Aviso

Este assistente opera contas que gastam **dinheiro real**. Ele foi instruído a nunca subir/pausar/alterar nada sem sua aprovação explícita — mas a responsabilidade pelo que sobe é sua. Leia [`SECURITY.md`](./SECURITY.md).
