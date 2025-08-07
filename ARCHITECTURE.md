Arquitetura do Askadb

Visão geral
O Askadb é um banco analítico híbrido (relacional + vetorial) com camada nativa de linguagem natural, orquestração de serviços e UI para exploração.

Diagrama (Mermaid)
```mermaid
flowchart TD
  user[Usuário] -- NL pergunta --> ui[Interface Web]
  ui -- REST --> orchestrator[Orchestrator API]
  orchestrator -- /translate --> nlquery[NL Query]
  orchestrator -- /execute --> engine[Query Engine]
  engine --> storage[(Dados: CSV/Parquet)]
  engine --> indexes[Indexação Vetorial]
  orchestrator --> ui
```

Componentes
- Interface Web: React + Vite
- Orchestrator API: FastAPI
- NL Query: FastAPI + LLM provider
- Query Engine: FastAPI + DuckDB
- Infra: Docker Compose para ambiente local

