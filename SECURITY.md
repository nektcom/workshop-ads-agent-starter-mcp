# Segurança

Este starter foi desenhado pra que **não exista credencial nenhuma dentro do repositório**. Leia antes de usar ou publicar um fork.

## Por que (quase) não há o que vazar

Diferente de um setup com `.env` cheio de tokens de Google/Meta/LinkedIn, aqui:

- **Toda autenticação é do MCP da Nekt, via OAuth.** O login acontece no seu cliente (Claude Code ou Cowork) e o token fica **no cliente, nunca no repositório**.
- **Nenhuma chave de plataforma** (Google Ads developer token, Meta system-user token, LinkedIn token) mora aqui. O MCP gateway resolve isso do lado da Nekt.
- O `.mcp.json` só tem a **URL pública** `https://mcp.nekt.com/mcp` — não é segredo.

## Regras (valem pra você e pro assistente)

1. **Nunca cole token/senha/chave em nenhum arquivo do repo** — nem em `knowledge/`, nem em skill, nem em comentário. Se precisa de auth, é OAuth do MCP.
2. **`knowledge/` é dado de negócio, não credencial.** Pode ter ICP, posicionamento, histórico de campanha. NÃO pode ter token, export de CRM com PII em massa, nem lista de clientes com dado pessoal. Se for publicar o fork, revise `knowledge/`.
3. **Não commite `.env`, `*_token.*`, `service-account*.json`** — o `.gitignore` já bloqueia, mas confira.
4. O assistente foi instruído a **recusar** pedidos de "coloca meu token no arquivo X" e a apontar o caminho OAuth.

## Antes de publicar um fork (checklist)

```bash
# 1. Nenhum segredo rastreado? (procura padrões comuns de credencial)
git grep -nEi 'EAA[A-Za-z0-9]{20,}|AKIA[0-9A-Z]{16}|(secret|token|password|api[_-]?key)\s*[:=]\s*["'\''][^"'\'' ]{12,}' || echo "  OK — nenhum segredo aparente rastreado"

# 2. Nenhum arquivo de credencial rastreado?
git ls-files | grep -Ei '\.env$|token|secret|credential|service.account' || echo "  OK — nenhum arquivo de credencial rastreado"

# 3. knowledge/ sem PII em massa? (revisão manual — abra os arquivos)
```

Se algo aparecer, **remova do histórico** (`git filter-repo` ou recrie o repo) antes de tornar público. Um segredo commitado uma vez fica no histórico mesmo se você deletar depois.

## Escopo de acesso do MCP

O MCP da Nekt acessa só as fontes que **você conectou** na sua conta Nekt, e opera só as contas de ads que você autorizou. Revise, na Nekt, quais fontes/contas estão ligadas antes de dar ao assistente autonomia de leitura/escrita.
