# Regras de decisão (determinísticas)

> ⚠️ Números de EXEMPLO. Ajuste pra sua meta em `regras-do-negocio.md`.
>
> Objetivo: rodando a mesma análise duas vezes, o agente decide **igual**. O LLM não inventa critério: ele aplica a regra daqui, cita o número que disparou e explica. Se nenhuma regra cobre o caso, ele **propõe** e pergunta, e a resposta vira regra nova aqui.

---

## Antes de qualquer veredito: tem dado?

| Situação | Veredito |
|---|---|
| Menos de **2.000 impressões** OU menos de **R$ 100** gastos | **sem dado**. Nunca "ruim". |
| Campanha com menos de **7 dias** (Meta) ou **14 dias** (Google com lance automático) | **em aprendizado**. Não pausar, não mexer em verba. |
| Campanha de fundo nova com menos de **15 dias** | **carência**. Reunião tem atraso, não julgar por reunião ainda. |

## Pausar (propor)

Propõe pausa só quando **todas** batem:
1. passou da carência e do mínimo de dado acima;
2. gastou pelo menos **2× a meta** de custo por reunião;
3. custo por reunião > **2× a meta** (ou zero reunião);
4. não é campanha de topo (marca). Topo é julgado por alcance e CPC.

Exceção: anúncio que é a **única fonte** de lead qualificado do canal não é pausado sem substituto no ar.

## Cortar criativo (topo / marca)

- CPC acima de **R$ 0,40** com pelo menos **2.500 impressões** → pausa o anúncio (não a campanha).
- Antes de pausar, garante **3 anúncios ativos** no conjunto (sobe o substituto primeiro).

## Escalar (propor)

Propõe aumento quando, nos últimos 3 dias completos:
- custo por reunião (ou por lead qualificado, em campanha de captura) ≤ meta;
- pelo menos **1 lead qualificado**;
- última mudança de verba há **≥ 72h**.

Degrau máximo: **+20%** por vez. Teto: definido em `regras-do-negocio.md`.

## Alarmar (avisar humano, não agir)

- Gasto do dia = **R$ 0** numa campanha ativa depois das 11h → entrega parada.
- Saldo da conta acaba em **≤ 3 dias**.
- Custo por lead subiu **> 30%** contra a semana anterior.
- Lead entrou e **não chegou** no próximo passo (CRM, atendimento) em 1h.

## Bloquear (nunca executar)

- Destino de anúncio = página de signup.
- Público fora do ICP (ver limites em `regras-do-negocio.md`).
- Qualquer ação de escrita sem ok humano registrado na conversa.

---

## Como evoluir este arquivo

Toda vez que você discordar de uma decisão do agente, pergunte: **qual número faltou aqui?** Escreva a regra, com data e o porquê. Exemplo:

```
- 2026-11-03 · Lead que volta por outro formulário também vai pro atendimento.
  Por quê: lead mais quente da semana ficou sem abordagem porque a automação só tratava lead novo.
```
