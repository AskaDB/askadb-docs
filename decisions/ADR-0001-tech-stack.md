ADR-0001: Stack Tecnológico

Contexto
- Serviços Python com FastAPI para velocidade e simplicidade.
- DuckDB como engine analítico local.
- React + Vite para UI moderna.
- Docker Compose para orquestração local.

Decisão
- Manter serviços desacoplados com comunicação REST.
- Padronizar Makefiles e .gitignore por repositório.
- Fornecer `launch.json` para DX.

Consequências
- Escala horizontal por serviço é simples.
- Fácil troca de provedores LLM no `nl-query`.

