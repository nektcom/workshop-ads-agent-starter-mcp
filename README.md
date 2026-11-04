<div align="center">

# 🎯 ads-agent-starter (MCP-first)

**Monte seu próprio assistente de mídia paga com o Claude — sem gerenciar uma credencial sequer.**

Dados e operação de anúncios acontecem tudo via **MCP da Nekt**: SQL sobre suas fontes pra consumir, e um **gateway** pra criar/pausar/ler campanhas. Auth por OAuth. Zero token no repositório.

[![Built with Claude](https://img.shields.io/badge/Built%20with-Claude-D97757)](https://claude.ai/code)
[![MCP](https://img.shields.io/badge/MCP-Nekt-000000)](https://nekt.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-2ea44f)](./LICENSE)
[![Use this template](https://img.shields.io/badge/Use%20this-template-8250df)](https://github.com/nektcom/workshop-ads-agent-starter-mcp/generate)

</div>

---

> 🧠 **Novo: camada de Company Brain.** Um cérebro que roda sozinho na nuvem, sabe o que é ganhar, tem as regras escritas num lugar só e descobre quando erra. Comece por **[`COMPANY-BRAIN.md`](./COMPANY-BRAIN.md)**: 10 perguntas pra saber se o seu é cérebro ou chat caro.

Não é uma ferramenta pronta. É um **ponto de partida**: você baixa, conecta o MCP da Nekt, troca o contexto pelo seu produto, e o assistente fica mais inteligente conforme você o alimenta.

Destilado de meses operando os próprios ads da [Nekt](https://nekt.com) — as lições que custaram dinheiro já vêm embutidas nas skills.

## Índice
- [Company Brain: cérebro ou chat caro?](#company-brain-cérebro-ou-chat-caro)
- [Por que MCP-first](#por-que-mcp-first-e-não-scripts-de-api)
- [Qual caminho escolher](#qual-caminho-escolher)
- [O que tem aqui](#o-que-tem-aqui)
- [Começando em 3 passos](#começando-em-3-passos)
- [As lições embutidas](#as-lições-embutidas-o-diferencial)
- [Segurança](#segurança)
- [É seu — faça o fork](#é-seu--faça-o-fork)
- [Créditos](#créditos)

---

## Company Brain: cérebro ou chat caro?

A maioria dos "Company Brain" é um chat com acesso a uns arquivos: para quando você sai, cada prompt tem uma versão da regra, e quem descobre o erro é o cliente. Este repo vem com as 5 peças que separam um do outro:

| Pilar | O que faz | Onde |
|---|---|---|
| **Não desliga quando você sai** | rotinas na nuvem, no horário, com log (audit diário, vigia de gasto e de lead, revisão semanal) | [`routines/`](./routines) |
| **Sabe o que é ganhar** | julga anúncio por custo por reunião lendo gasto + CRM ao vivo, ignora CPL de form | [`brain/regras-do-negocio.md`](./brain/regras-do-negocio.md) |
| **Tem memória escrita** | venda, lead bom e cliente definidos uma vez; skills apontam, não copiam | [`brain/`](./brain) + [`knowledge/ads-history.md`](./knowledge/ads-history.md) |
| **Pessoas e agentes com o mesmo cérebro** | mesmo número no dashboard e no agente (camada semântica), dono definido pra cada área | [`metrics-foundation`](./.claude/skills/metrics-foundation/SKILL.md) + [`brain/quem-mexe-em-que.md`](./brain/quem-mexe-em-que.md) |
| **Sabe quando está errado** | regra determinística, revisor independente, verificação depois de agir | [`brain/decisoes.md`](./brain/decisoes.md) + [`ads-reviewer`](./.claude/agents/ads-reviewer.md) |

Quer o diagnóstico do seu setup? Abra o Claude na pasta e peça: _"roda o company-brain check"_.

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
CLAUDE.md                     # carregado sozinho pelo Claude: mapa + como pensar/agir
COMPANY-BRAIN.md              # o porquê: 10 perguntas, 5 pilares, ordem de montagem
brain/                        # a constituição (FONTE ÚNICA, skills apontam pra cá)
  regras-do-negocio.md        # o que é ganhar, lead bom, venda, cliente
  decisoes.md                 # quando pausar/escalar/alarmar/bloquear, com números
  quem-mexe-em-que.md         # dono de cada área (pessoas e agentes)
routines/                     # prompts pra agendar na nuvem: audit diário, vigia, revisão semanal
.claude/
  agents/ads-agent.md         # o assistente (personalidade + regras de operação)
  agents/ads-reviewer.md      # revisor independente: barra decisão ruim antes de gastar
  skills/                     # 13 módulos que ensinam o assistente a fazer cada coisa
    company-brain/            # diagnóstico: seu setup é cérebro ou chat caro?
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

## O que você pode pedir (exemplos)

Você conversa em português normal — o assistente traduz pro MCP e executa (escrita só com o seu ok):

**Ver / entender**
- "Como foram minhas campanhas essa semana?"
- "Qual campanha está cara e sem resultado?"
- "Quais leads caíram hoje no formulário?"

**Operar (ele propõe, você confirma, ele faz)**
- "Pausa a campanha X."
- "Sobe o budget da Y em 20%."
- "Negativa o termo 'grátis' no Google."
- "Cria um teste com esses 3 vídeos no Meta." (sobe PAUSED e te mostra antes de ativar)

Hoje o assistente **opera Google Ads, Meta/Facebook e LinkedIn** via MCP (ciclo completo: ver, criar, pausar, budget, keywords, criativo, leads). Toda escrita sobe PAUSED e espera sua aprovação antes de ativar.

## A base de dados: consolide com transformadas, defina na camada semântica

O assistente é tão bom quanto o dado que ele enxerga. Antes de pedir análise séria, monte a **fundação** na Nekt — é o que separa "número que bate sempre" de "cada resposta uma conta diferente". São 3 passos, e você faz **conversando com o assistente** (ele usa as ferramentas da Nekt via MCP):

**1. Conecte suas fontes na Nekt — e prefira EXTRAIR (Source), não só ler ao vivo.**
Tem dois jeitos de o dado chegar: o MCP lê **ao vivo** (ótimo pra "o que está acontecendo agora" e pra operar), e a **Source** (conector de extração da Nekt) **puxa e GUARDA** os dados no seu lakehouse. Pra fundação de análise, use a **Source** — ela te dá vantagens que o ao-vivo não dá:
- **Histórico completo e durável.** As APIs de anúncio limitam o quanto você olha pra trás (o Facebook, por exemplo, corta boa parte do detalhe em ~90 dias). A Source extrai todo dia e **acumula** — você mantém anos de histórico, mesmo do que a plataforma já não devolve.
- **Rápido e sem estourar limite.** Consultar uma tabela no seu lakehouse é instantâneo e não bate no rate limit da plataforma a cada pergunta.
- **Base estável pras transformadas.** A consolidação (passo 2) roda sobre dado que não muda debaixo dos pés.

Regra prática: **Source pra histórico e análise; MCP ao vivo pra estado atual e operação.** Elas se somam.

**2. Consolide com TRANSFORMADAS — é o passo que mais importa.**
Uma *transformada* é uma receita que pega esses dados crus e espalhados e monta uma **tabela limpa e única**. Exemplo: juntar o gasto do Google + Meta + LinkedIn com os deals fechados do CRM numa só tabela `ads_performance` (investimento, leads, reuniões, vendas, custo por reunião — por campanha, por dia).
Por que importa: **sem isso, o assistente junta as fontes na mão a cada pergunta — e erra nos cantos** (consulta UM formulário e perde metade dos leads, soma lead duplicado, atribui ao canal errado). Com a transformada, a conta é feita **uma vez, certa, e roda todo dia sozinha**; o assistente só **lê a tabela pronta**.
Regra: **pergunta que se repete vira transformada, não query nova.** Peça ao assistente: *"monta uma transformada que consolida meu gasto de ads com os deals do CRM"* — ele usa as ferramentas de transformação da Nekt pra criar e rodar.

**3. Defina os números na CAMADA SEMÂNTICA.**
Com a tabela pronta, defina cada métrica **uma vez, canonicamente**: o que É um "lead B2B" (email corporativo), uma "reunião" (agendada menos no-show), a "atribuição" (por UTM do toque pago, nunca pelo campo de origem do CRM) — fórmula + fonte + o "pega-ratão" de cada uma. Isso vira a **camada semântica** da Nekt.
Por que importa: o número **não muda dependendo de quem perguntou**. O assistente **puxa a definição** em vez de inventar a conta; humano e agente usam a mesma régua; e cada "pega-ratão" novo que você descobre vira parte da camada — não folclore que se perde no Slack.

**O resultado:** fontes cruas → **transformada consolida** → **camada semântica define** → o assistente responde rápido, barato e **sempre com o mesmo número certo**. O passo a passo pro assistente está na skill [`metrics-foundation`](./.claude/skills/metrics-foundation/SKILL.md).

## Começando em 3 passos

1. **Conecte o MCP da Nekt** (Cowork: Conectores; Code: `/mcp`) — [`SETUP.md`](./SETUP.md).
2. **Preencha `brain/` e `knowledge/`** com o seu negócio (vem com o fictício AcmeRH — troque pelo seu). Comece por `brain/regras-do-negocio.md`: é o arquivo mais importante.
3. **Abra o Claude na pasta** e chame `@ads-agent`. Peça algo real:
   > _"Olha meus últimos 30 dias de ads e me diz onde estou queimando dinheiro."_
4. **Ligue a primeira rotina** ([`routines/audit-diario.md`](./routines/audit-diario.md)) pra ele trabalhar sem você abrir nada.

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

## É seu — faça o fork

Este é um **starter pra você clonar e adaptar**, não um projeto pra contribuir de volta. Clique em **[Use this template](https://github.com/nektcom/workshop-ads-agent-starter-mcp/generate)** (ou dê um fork), troque o `knowledge/` pelo seu produto, ajuste as skills pro seu contexto e deixe do seu jeito. O objetivo é ele virar **o SEU assistente**, não uma base compartilhada.

## Créditos

Criado por **[Luana Pereira (Lupe)](https://nekt.com)** — GTM Engineer na [Nekt](https://nekt.com), que usa este mesmo esqueleto pra operar a própria mídia paga. As skills são a destilação de meses testando, errando e aprendendo em Google, Meta e LinkedIn Ads.

Construído com [Claude Code](https://claude.ai/code) + [Claude Cowork](https://claude.com/product/cowork) e o [MCP da Nekt](https://nekt.com).

Se você virar cliente da Nekt, dá pra montar o seu Company Brain junto com a gente.

Se este starter te ajudou, deixa uma ⭐ — e conta pra gente o que você construiu em cima dele.

---

<div align="center">
<sub>Feito com ☕ e budget de ads de verdade · <a href="https://nekt.com">nekt.com</a></sub>
</div>

## Aviso

Este assistente opera contas que gastam **dinheiro real**. Ele foi instruído a nunca subir/pausar/alterar nada sem sua aprovação explícita — mas a responsabilidade pelo que sobe é sua. Leia [`SECURITY.md`](./SECURITY.md).
