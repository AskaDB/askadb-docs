Fluxo de Dados

1. Ingestão
- `askadb-pipeline-ingest` coleta dados (CSV/JSON/APIs) e grava em `askadb-query-engine/data` (SQLite).

2. Tradução NL → Query
- `askadb-nl-query` recebe pergunta em linguagem natural e retorna SQL (via OpenAI SDK).

3. Orquestração e Execução
- `askadb-orchestrator-api` chama `askadb-nl-query` (gera SQL) e depois `askadb-query-engine` (executa em SQLite) e agrega metadados.

4. Visualização
- `askadb-dashboard-core` sugere visualizações (auto-charting) e retorna sugestões ao Orchestrator.
- `askadb-ui` (porta 3000) renderiza resultados, dashboard sugerido e sugestões de próximas análises.

