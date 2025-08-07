Fluxo de Dados

1. Ingestão
- `pipeline-ingest` coleta dados (CSV/JSON/APIs) e grava em `query-engine/data`.

2. Tradução NL → Query
- `nl-query` recebe pergunta em linguagem natural e retorna uma query (SQL ou AST).

3. Execução
- `query-engine` executa a query sobre os dados locais (DuckDB) e retorna resultados.

4. Visualização
- `askadb-ui` renderiza tabelas e gráficos; `dashboard-core` pode sugerir visualizações.

