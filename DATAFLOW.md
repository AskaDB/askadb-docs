Fluxo de Dados

1. Ingestão
- `askadb-pipeline-ingest` coleta dados (CSV/JSON/APIs) e grava em `askadb-query-engine/data`.

2. Tradução NL → Query
- `askadb-nl-query` recebe pergunta em linguagem natural e retorna uma query (SQL ou AST).

3. Execução
- `askadb-query-engine` executa a query sobre os dados locais (DuckDB) e retorna resultados.

4. Visualização
- `askadb-ui` renderiza tabelas e gráficos; `askadb-dashboard-core` pode sugerir visualizações.

