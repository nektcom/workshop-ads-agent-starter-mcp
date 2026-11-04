# Quem mexe em quê

> ⚠️ EXEMPLO. Troque pelos seus agentes, rotinas e pessoas.
>
> Com um agente só, isso parece burocracia. Com dois, vira obrigatório: dois agentes mexendo na mesma campanha (um pausa, o outro reativa) é pior que nenhum. A regra é simples: **cada coisa tem um dono**. Quem não é dono, lê e sugere ao dono.

---

## Donos

| Área | Dono (escreve) | Pode ler | Quem aprova escrita |
|---|---|---|---|
| Campanhas Google / Meta / LinkedIn | `ads-agent` | todos | humano responsável por mídia |
| Verba diária | `ads-agent` (só dentro de `decisoes.md`) | todos | humano, acima do degrau |
| Leads → CRM → atendimento | rotina de leads | `ads-agent` | humano de vendas |
| Regras do negócio (`brain/`) | humano dono das regras | todos | ele mesmo |
| Placar do time (dashboard) | camada semântica na Nekt | todos | dono das regras |

## Regras de convivência

1. **Antes de escrever, confira o dono.** Se não é seu, deixe a sugestão pro dono (arquivo, thread, comentário), não execute.
2. **Toda escrita deixa rastro**: o que mudou, por quê, qual regra disparou. Vai pro `knowledge/ads-history.md`.
3. **Um coordenador decide conflito.** Quando dois agentes querem mexer no mesmo lugar, quem decide é o humano dono da área (ou um agente coordenador, se você tiver).
4. **O agente sabe o que o outro fez** lendo o histórico, não adivinhando. Antes de agir numa campanha, ele lê as últimas entradas dela no `ads-history.md`.
5. **Mensagem pra fora (cliente, lead, público) é sempre rascunho.** Quem envia é gente, depois de ler.
