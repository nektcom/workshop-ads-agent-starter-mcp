# Regras do negócio (fonte única)

> ⚠️ EXEMPLO FICTÍCIO (AcmeRH). Troque pelos números e definições do SEU negócio.
>
> Este é o arquivo mais importante do repo. Toda skill e todo agente leem daqui. **Nenhuma definição abaixo deve ser copiada pra dentro de skill ou prompt**: aponte pra este arquivo. Mudou a regra, muda só aqui.

Última revisão: AAAA-MM-DD · dono: _(nome de quem decide estas regras)_

---

## O que é ganhar

**Métrica norte:** custo por **reunião agendada com lead qualificado**.
Mais baixo = melhor. Meta atual: **R$ 400 por reunião**. Teto antes de alarmar: **R$ 800**.

Por quê esta e não outra: é o primeiro ponto do funil que prova intenção de compra e que aparece rápido (dias, não meses). Venda é a verdade final, mas demora demais pra decidir anúncio no dia a dia.

**O que NÃO é ganhar** (métricas que o agente ignora pra decidir):
- CPL de formulário. Lead barato com e-mail pessoal não vira pipeline.
- CTR, alcance, curtida, isoladamente.
- "Lead" que veio do próprio time (teste interno). Conta como lead normal no fluxo, mas não entra na meta.

---

## Definições

| Termo | Definição canônica | Onde medir |
|---|---|---|
| **Lead** | Contato que entrou por qualquer canal | CRM: contato criado |
| **Lead qualificado** | Lead com e-mail corporativo E empresa dentro do ICP (`knowledge/icp.md`) | CRM: domínio do e-mail ≠ gmail/hotmail/outlook/yahoo/icloud |
| **Reunião** | Reunião agendada com lead qualificado, **descontado no-show** | CRM: evento de reunião + status |
| **Oportunidade** | Deal que vendas aceitou como real (passou da 1ª reunião) | CRM: estágio "Oportunidade" do pipeline de vendas |
| **Venda** | Deal no estágio **Ganho** do pipeline. Valor = mensalidade | CRM: estágio Won |
| **Cliente** | Empresa com pelo menos 1 venda ativa | CRM / billing |
| **Canal de origem** | UTM do toque pago + atribuição no **deal**. Nunca o campo "origem" automático do CRM | ver skill `ads-attribution` |

### Pega-ratão (já custaram dinheiro)
- **Reunião agendada ≠ reunião que aconteceu.** Automação de CRM pode mover deal pra "reunião marcada" sem reunião nenhuma. Confira o evento.
- **Lead que volta** (já existia e preencheu outro formulário) precisa seguir o mesmo fluxo de um lead novo. Se a automação só trata o "novo", o lead mais quente fica sem abordagem.
- **Formulário compartilhado**: consultar só um formulário perde metade dos leads. Conte por campanha, não por formulário.

---

## Régua por estágio do funil

| Estágio | Exemplo | Como julgar | Nunca julgar por |
|---|---|---|---|
| **Topo (marca)** | LinkedIn com post de pessoa | alcance + CPC barato + busca pela marca subindo | lead ou reunião direta |
| **Meio** | conteúdo, webinar | lead qualificado a custo razoável | venda no mês |
| **Fundo** | busca com intenção, demo | custo por reunião e venda | CTR |

---

## Limites que o agente nunca cruza sozinho

- Público acima de **500 funcionários**: fora do ICP. Só com ok explícito.
- Anúncio apontando pra página de **signup**: proibido. Vai pra home, landing page ou formulário nativo.
- Qualquer **gasto novo, campanha nova ou aumento de verba**: proposta + ok humano.
- **Mensagem pra cliente** (e-mail, WhatsApp, DM): rascunho, quem envia é gente.
