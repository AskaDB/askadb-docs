# Guia de Desenvolvimento - Askadb Database

## 🎯 **Como Começar a Desenvolver o Banco de Dados**

### **1. Decisões Técnicas Críticas**

#### **Linguagem para o Core do Banco**
**Recomendação: Rust**
- **Performance**: C-like performance com segurança de memória
- **Concorrência**: Async/await nativo
- **Ecosystem**: Crates para databases, networking, serialization
- **Learning Curve**: Moderado, mas vale a pena para um banco de dados

**Alternativa: Go**
- **Simplicidade**: Mais fácil de aprender
- **Performance**: Boa, mas não tão boa quanto Rust
- **Garbage Collection**: Pode ser um problema para latência crítica

#### **Stack Recomendado por Componente**

| Componente | Linguagem | Justificativa |
|------------|-----------|---------------|
| **query-engine** | Rust | Performance crítica, concorrência |
| **storage-engine** | Rust | Gerenciamento de memória, I/O |
| **index-engine** | Rust | Algoritmos complexos, performance |
| **nl-query** | Python | Ecosystem de IA/ML, OpenAI SDK direto |
| **orchestrator-api** | Python | FastAPI, integração fácil |
| **ui** | TypeScript | React ecosystem, DX |
| **dashboard-core** | TypeScript | Charts, visualizações |
| **pipeline-ingest** | Python | Data processing, ETL |
| **ai-agent-sdk** | Python/TS | Multi-language support |

### **2. Arquitetura do Storage Engine**

#### **Estrutura de Dados Híbrida**
```rust
// Exemplo conceitual em Rust
pub struct HybridStorage {
    // Relacional
    relational: BTreeMap<String, Table>,
    
    // Vetorial
    vector_store: VectorStore,
    
    // Semi-estruturado
    document_store: DocumentStore,
}
```

### **3. Implementação do Query Engine (MVP)**
- MVP atual usa SQLite (rusqlite) como storage relacional;
- API HTTP exposta via Axum 0.6 (POST /execute, POST /health);
- Dados de exemplo carregados em `data/askadb.db`.

### **4. Implementação do NL2Query (MVP)**

#### **Integração com LLMs (OpenAI SDK direto)**
```python
# askadb-nl-query/app/services/llm_service.py
from openai import OpenAI

class LLMService:
    def __init__(self):
        self.client = OpenAI()
        self.model = "gpt-4"

    def generate_query(self, request):
        # monta prompt com schema + exemplos e retorna SQL + sugestões
        ...
```

### **5. Dashboard Core (Auto-charting)**
- Lib TypeScript que analisa colunas e sugere 2-3 gráficos ideais;
- Endpoint /suggest no servidor (Express) para uso pelo Orchestrator.

### **6. Comandos para Começar**

#### **Infra e serviços**
```bash
# subir tudo
cd askadb-infra
make up

# derrubar
make down
```

#### **Repositórios (local)**
```bash
# query-engine (Rust)
cd askadb-query-engine
cargo build && cargo test && cargo run

# nl-query (Python)
cd askadb-nl-query
python -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8001

# dashboard-core (Node)
cd askadb-dashboard-core
npm i
npm run dev:server

# ui (React)
cd askadb-ui
npm i
npm run dev
```
