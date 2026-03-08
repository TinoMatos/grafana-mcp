# Relatorio de Erros - Pasta `docs`

**Data:** 2026-03-08  
**Escopo analisado:** `exemplo-09-grafana-mcp/alumnus/docs`

## Resumo Executivo
Os erros na documentacao da pasta `docs` sao causados por inconsistencias entre o que o markdown descreve e o estado atual do codigo/fixos esperados do cenario `db-leaky-connections`.

## Evidencias
1. Referencias de linha desatualizadas em `prompt.md`:
- Arquivo: `docs/prompt.md`
- Trecho atual: `main.ts:51` e `main.ts:80`
- Codigo real aponta para:
  - `pool.connect()` em `src/scenarios/db-leaky-connections/main.ts:52`
  - chamada no handler em `src/scenarios/db-leaky-connections/main.ts:84`

2. Link quebrado para relatorio esperado:
- Arquivo: `docs/prompt.md`
- Link presente: `./incident-report-db-leaky-connections-2025-12-16.md`
- Problema: o arquivo nao existe atualmente em `docs/`

3. Inconsistencia textual no resumo:
- Arquivo: `docs/prompt.md`
- Secao `Summary` menciona stack trace em `main.ts:80`, mas o ponto atual no codigo e `main.ts:84`.

## Causa Raiz
A documentacao foi revertida/ficou defasada em relacao ao codigo atual do cenario. Isso gera:
- referencias incorretas de stacktrace,
- expectativa de um arquivo de relatorio que nao existe,
- ruido na investigacao (quem segue o guia chega em linhas erradas).

## Impacto
- Investigacoes com Grafana MCP ficam menos confiaveis.
- Pessoas do time podem perder tempo validando linhas erradas.
- O link para o "Expected AI Report" falha e prejudica onboarding/treinamento.

## Correcoes Recomendadas
1. Atualizar `docs/prompt.md`:
- `main.ts:51` -> `main.ts:52`
- `main.ts:80` -> `main.ts:84`

2. Criar o arquivo referenciado no link:
- `docs/incident-report-db-leaky-connections-2025-12-16.md`
- Conteudo minimo: correlacao de metricas, logs, traces, causa raiz e fix.

3. (Opcional) Adicionar um check de links quebrados no CI para `docs/`.

## Checklist de Validacao
- [ ] Link `./incident-report-db-leaky-connections-2025-12-16.md` abre sem erro.
- [ ] Todos os pontos de stacktrace no `prompt.md` batem com `src/scenarios/db-leaky-connections/main.ts`.
- [ ] Passo a passo de investigacao gera conclusao coerente com o bug de leak.

## Conclusao
Os "erros" em `docs` sao de consistencia documental, nao de logica do cenario em si. Ajustando as linhas e recriando o relatorio esperado, a pasta volta a servir como guia confiavel para diagnostico do problema de conexoes vazando.
