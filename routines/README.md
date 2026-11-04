# Rotinas: o cérebro trabalhando sem você

Um cérebro que só pensa quando você abre o chat é uma ferramenta. Estas rotinas fazem ele rodar **sozinho, no horário, na nuvem, com log**. Cada arquivo aqui é o prompt de uma rotina, pronto pra agendar.

| Rotina | Quando | O que faz | Escreve? |
|---|---|---|---|
| [`audit-diario.md`](./audit-diario.md) | todo dia, 8h | lê gasto + CRM, aplica `brain/decisoes.md`, manda resumo | não, só propõe |
| [`vigia-de-gasto.md`](./vigia-de-gasto.md) | a cada 1h, horário comercial | entrega parada, saldo, lead que não chegou | não, só alarma |
| [`revisao-semanal.md`](./revisao-semanal.md) | segunda, 9h | o que ganhou/perdeu na semana, atualiza `ads-history.md` | só o histórico |

Comece por **uma** (o audit diário). Deixe rodar uma semana, corrija as regras em `brain/` onde ele errou, e só então ligue a próxima.

---

## Como agendar

### Opção A: rotina na nuvem do Claude Code (mais simples)
No Claude Code, dentro desta pasta:

```
/schedule
```

Diga o horário e cole o conteúdo do arquivo da rotina como instrução. A rotina roda na nuvem da Anthropic, com o seu MCP da Nekt conectado na sua conta (OAuth, sem token no repo), mesmo com o seu computador desligado.

### Opção B: GitHub Actions
Se você prefere tudo versionado no repo, rode o Claude Code em modo headless num workflow agendado (`on: schedule`). Guarde credenciais **só** em GitHub Secrets, nunca em arquivo.

---

## Regras de toda rotina

1. **Lê antes de agir**: `CLAUDE.md`, `brain/` e as últimas entradas de `knowledge/ads-history.md`.
2. **Dado ao vivo** via MCP. Se o MCP falhar, a rotina avisa que falhou. Nunca responde com número velho ou inventado.
3. **Propõe, não executa.** Escrita (pausar, verba, criar) só com ok humano. A rotina deixa a proposta pronta com o comando.
4. **Deixa log.** Cada execução termina com uma linha no `ads-history.md` (ou no canal do time): data, o que viu, o que propôs.
5. **Silêncio é sinal.** Se não tem nada fora da regra, a rotina diz "nada fora da regra" em 1 linha. Não inventa assunto.
6. **Mensagem curta.** Conclusão e ação no topo, em 1 a 3 linhas. O detalhe fica no log, pra quem quiser.
