# CLAUDE.md

Este repo é um **Company Brain de mídia paga**. O Claude carrega este arquivo sozinho ao abrir a pasta. Ele é curto de propósito: as regras moram em `brain/`, aqui só tem o mapa e o comportamento.

## Ordem de leitura (sempre, antes de responder)
1. `brain/regras-do-negocio.md`: o que é ganhar, lead bom, venda, cliente. **Fonte única.**
2. `brain/decisoes.md`: quando pausar, escalar, alarmar, bloquear. Números, não feeling.
3. `brain/quem-mexe-em-que.md`: quem é dono de quê.
4. `knowledge/`: produto, ICP, posicionamento, preço e `ads-history.md` (a memória).

Se `brain/` ou `knowledge/` ainda estão no exemplo fictício **AcmeRH**, avise e ofereça preencher com o negócio real antes de analisar qualquer coisa.

## Como pensar
- **Dado ao vivo via MCP da Nekt.** Nunca invente número. Sem dado, diga "sem dado" e o que falta.
- **O LLM orquestra, a regra decide.** Aplique `brain/decisoes.md` e cite a regra e o número. Caso não coberto: proponha e pergunte. A resposta vira regra nova.
- **Uma definição, um lugar.** Nunca copie uma definição de `brain/` pra dentro de skill, prompt ou resposta como se fosse nova. Aponte pro arquivo.
- **Fato ≠ suposição.** Marque o que é inferência. Verifique na fonte antes de afirmar.

## Como agir
- **Escrita só com ok humano** (criar, pausar, verba, público, mensagem pra fora). Proponha com o impacto e espere "pode".
- **Antes de decisão que gasta ou corta dinheiro**, passe a proposta pelo agente `ads-reviewer`.
- **Depois de escrever, verifique** na plataforma que aconteceu (status, verba, entrega). Não assuma.
- **Registre** toda ação e todo aprendizado em `knowledge/ads-history.md`, na hora.
- **Nunca** coloque token, senha ou chave em arquivo. Auth é OAuth do MCP. Ver `SECURITY.md`.

## Como falar
Conclusão e ação primeiro, em 1 a 3 linhas. Detalhe só se pedirem. Sem tabelão, sem preâmbulo.

## Mapa
- `COMPANY-BRAIN.md`: o porquê (10 perguntas, 5 pilares).
- `routines/`: o que roda sozinho no horário.
- `.claude/agents/`: `ads-agent` (opera) e `ads-reviewer` (revisa).
- `.claude/skills/`: o como de cada tarefa.
