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
| **nl-query** | Python | Ecosystem de IA/ML, LangChain |
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

pub struct Table {
    schema: Schema,
    data: Vec<Row>,
    indexes: Vec<Index>,
}

pub struct VectorStore {
    embeddings: Vec<f32>,
    index: HNSWIndex,
}
```

#### **Index Engine**
```rust
// Índices híbridos
pub enum Index {
    BTree(BTreeIndex),      // Para dados relacionais
    HNSW(HNSWIndex),        // Para vetores
    FullText(FullTextIndex), // Para texto
}
```

### **3. Implementação do Query Engine**

#### **Parser de Linguagem Natural**
```python
# askadb-nl-query/app/services/nl_parser.py
class NLParser:
    def __init__(self, llm_provider: str = "openai"):
        self.llm = self._setup_llm(llm_provider)
        self.schema = self._load_schema()
    
    def parse_nl_to_ast(self, query: str) -> AST:
        """Converte linguagem natural em AST"""
        prompt = self._build_prompt(query, self.schema)
        response = self.llm.generate(prompt)
        return self._parse_response_to_ast(response)
    
    def _build_prompt(self, query: str, schema: Schema) -> str:
        return f"""
        Schema: {schema}
        Query: {query}
        
        Converta a query em linguagem natural para uma AST (Abstract Syntax Tree).
        Formato JSON esperado:
        {{
            "type": "SELECT",
            "columns": ["column1", "column2"],
            "from": "table_name",
            "where": {{"column": "value", "operator": "="}},
            "order_by": ["column1"],
            "limit": 10
        }}
        """
```

#### **Query Executor**
```rust
// askadb-query-engine/src/executor.rs
pub struct QueryExecutor {
    storage: HybridStorage,
    optimizer: QueryOptimizer,
}

impl QueryExecutor {
    pub async fn execute(&self, ast: AST) -> Result<QueryResult, Error> {
        // 1. Otimizar AST
        let optimized_ast = self.optimizer.optimize(ast)?;
        
        // 2. Executar query
        match optimized_ast {
            AST::Select(select) => self.execute_select(select).await,
            AST::Insert(insert) => self.execute_insert(insert).await,
            AST::Update(update) => self.execute_update(update).await,
            AST::Delete(delete) => self.execute_delete(delete).await,
        }
    }
    
    async fn execute_select(&self, select: SelectAST) -> Result<QueryResult, Error> {
        // Implementação do SELECT
        let table = self.storage.get_table(&select.from)?;
        let filtered_data = self.apply_filters(table, &select.where_clause)?;
        let projected_data = self.apply_projection(filtered_data, &select.columns)?;
        let ordered_data = self.apply_ordering(projected_data, &select.order_by)?;
        let limited_data = self.apply_limit(ordered_data, select.limit)?;
        
        Ok(QueryResult::new(limited_data))
    }
}
```

### **4. Implementação do NL2Query**

#### **Integração com LLMs**
```python
# askadb-nl-query/app/services/llm_service.py
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

class LLMService:
    def __init__(self):
        self.llm = OpenAI(temperature=0)
        self.prompt_template = PromptTemplate(
            input_variables=["query", "schema", "examples"],
            template="""
            Você é um assistente especializado em converter linguagem natural em queries SQL.
            
            Schema do banco:
            {schema}
            
            Exemplos de conversão:
            {examples}
            
            Query em linguagem natural: {query}
            
            Converta para SQL:
            """
        )
        self.chain = LLMChain(llm=self.llm, prompt=self.prompt_template)
    
    async def translate_to_sql(self, query: str, schema: dict) -> str:
        examples = self._get_examples()
        result = await self.chain.arun(
            query=query,
            schema=schema,
            examples=examples
        )
        return self._clean_sql(result)
```

### **5. Implementação do Dashboard Core**

#### **Auto-charting Engine**
```typescript
// askadb-dashboard-core/src/auto-charting.ts
export class AutoChartingEngine {
    private chartSuggestions: ChartSuggestion[] = [];
    
    suggestChart(data: QueryResult): ChartSuggestion[] {
        const suggestions: ChartSuggestion[] = [];
        
        // Analisar estrutura dos dados
        const dataStructure = this.analyzeDataStructure(data);
        
        // Sugerir gráficos baseado no tipo de dados
        if (dataStructure.hasTimeSeries) {
            suggestions.push(new TimeSeriesChart(data));
        }
        
        if (dataStructure.hasCategories) {
            suggestions.push(new BarChart(data));
        }
        
        if (dataStructure.hasNumericalComparison) {
            suggestions.push(new ScatterPlot(data));
        }
        
        return suggestions;
    }
    
    private analyzeDataStructure(data: QueryResult): DataStructure {
        // Implementar análise automática da estrutura dos dados
        return {
            hasTimeSeries: this.detectTimeSeries(data),
            hasCategories: this.detectCategories(data),
            hasNumericalComparison: this.detectNumericalComparison(data),
        };
    }
}
```

### **6. Roadmap de Implementação**

#### **Fase 1: MVP (Meses 1-3)**
1. **Storage Engine Básico** (Rust)
   - Implementar B-tree para dados relacionais
   - Suporte a CSV/JSON import
   - Queries básicas (SELECT, WHERE, ORDER BY)

2. **NL2Query Simples** (Python)
   - Integração com OpenAI GPT
   - Conversão básica NL → SQL
   - Suporte a queries simples

3. **UI Mínima** (React)
   - Interface para queries em NL
   - Visualização de resultados em tabela
   - Upload de dados

#### **Fase 2: Híbrido (Meses 4-8)**
1. **Storage Vetorial** (Rust)
   - Implementar HNSW index
   - Suporte a embeddings
   - Queries vetoriais

2. **Query Engine Avançado** (Rust)
   - Otimizador de queries
   - Cache inteligente
   - Queries híbridas (relacional + vetorial)

3. **Dashboards Automáticos** (TypeScript)
   - Auto-charting engine
   - Templates de dashboards
   - Sugestões inteligentes

#### **Fase 3: Revolucionário (Meses 9-12)**
1. **RAG Nativo**
   - Chunking automático
   - Embeddings automáticos
   - Busca semântica

2. **SDK para Agentes**
   - Integração com LangChain
   - Plugins para LlamaIndex
   - API para agentes LLM

### **7. Comandos para Começar**

#### **Setup do Ambiente**
```bash
# 1. Instalar Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# 2. Instalar Python 3.11+
# 3. Instalar Node.js 18+

# 4. Clonar e setup
git clone https://github.com/askadb/askadb.git
cd askadb

# 5. Setup infra
cd askadb-infra
make up
```

#### **Desenvolvimento Local**
```bash
# Desenvolver query-engine (Rust)
cd askadb-query-engine
cargo build
cargo test

# Desenvolver nl-query (Python)
cd askadb-nl-query
make install
make run

# Desenvolver ui (React)
cd askadb-ui
make install
make dev
```

### **8. Próximos Passos**

1. **Definir Schema**: Decidir estrutura de dados inicial
2. **Implementar Storage**: Começar com B-tree em Rust
3. **Integrar LLM**: Conectar OpenAI para NL2Query
4. **Criar UI**: Interface básica para testes
5. **Testar com Dados Reais**: Validar com PMs e data engineers

### **9. Recursos Úteis**

- **Rust Database Development**: https://github.com/rust-lang/rust
- **LangChain Documentation**: https://python.langchain.com/
- **React + TypeScript**: https://react.dev/
- **Database Design Patterns**: https://martinfowler.com/
