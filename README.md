# Askadb - Banco de Dados Revolucionário

## 🎯 **Visão**

O **Askadb** é um **banco de dados híbrido revolucionário** que combina:
- **Linguagem natural nativa** para queries
- **Armazenamento híbrido** (relacional + vetorial)
- **Dashboards automáticos**
- **RAG nativo** para IA

**Objetivo**: Revolucionar o mercado para PMs, data engineers e startups de IA.

## 🏗️ **Arquitetura**

```
github.com/askadb
├── query-engine       → Core do banco (Rust) - Storage + Query Engine
├── nl-query           → Camada de linguagem natural (Python) - NL2Query
├── dashboard-core     → Dashboards automáticos (TypeScript) - Auto-charting
├── pipeline-ingest    → Ingestão de dados (Python) - ETL Pipeline
├── ui                 → Interface do usuário (React) - Chat-like UI
├── ai-agent-sdk       → SDK para agentes LLM (Python/TS) - RAG Integration
├── orchestrator-api   → Orquestração entre serviços (Python) - API Gateway
├── infra              → Infraestrutura (Docker) - Orquestrador central
└── docs               → Documentação e visão de produto
```

## 🚀 **Diferenciais Revolucionários**

### 1. **Query por Linguagem Natural Nativa**
```
❌ Competidores: "SELECT * FROM sales WHERE region = 'north' AND month = 'may'"
✅ Askadb: "Quero vendas por região no mês de maio"
```

### 2. **Armazenamento Híbrido**
- **Relacional**: Dados estruturados tradicionais
- **Vetorial**: Embeddings para busca semântica
- **Semi-estruturado**: JSON, documentos flexíveis

### 3. **Dashboards Automáticos**
- Geração automática de visualizações
- Sugestões inteligentes de gráficos
- Templates personalizáveis

### 4. **RAG Nativo**
- Chunking automático
- Embeddings automáticos
- Busca semântica nativa

## 📚 **Documentação**

### **Arquitetura e Design**
- [Arquitetura Detalhada](ARCHITECTURE.md) - Visão técnica completa
- [Guia de Desenvolvimento](DEVELOPMENT_GUIDE.md) - Como implementar
- [Posicionamento no Mercado](MARKET_POSITIONING.md) - Análise competitiva

### **Serviços e Fluxos**
- [Fluxo de Dados](DATAFLOW.md) - Como os dados fluem
- [Serviços e Portas](SERVICES.md) - Endpoints e configurações
- [Decisões Arquiteturais](decisions/) - ADRs e decisões técnicas

## 🎯 **Target Markets**

### **Product Managers (PMs)**
- **Problema**: Precisam de insights rápidos, mas dependem de data engineers
- **Solução**: Queries em linguagem natural + dashboards automáticos

### **Data Engineers**
- **Problema**: Gastam 80% do tempo criando dashboards básicos
- **Solução**: Pipeline automatizado + queries simplificadas

### **Startups de IA**
- **Problema**: Precisam de RAG e vetores, mas não têm expertise
- **Solução**: RAG nativo + SDK para agentes

### **Analistas de Dados**
- **Problema**: SQL é complexo e demorado
- **Solução**: Interface intuitiva + queries em linguagem natural

## 🏁 **vs. Competidores**

| Aspecto | PostgreSQL | ChromaDB | MotherDuck | Weaviate | **Askadb** |
|---------|------------|----------|------------|----------|------------|
| **NL2Query** | ❌ | ❌ | ❌ | ⚠️ | ✅ **Nativo** |
| **Vetores** | ⚠️ | ✅ | ❌ | ✅ | ✅ **Nativo** |
| **Relacional** | ✅ | ❌ | ✅ | ❌ | ✅ **Nativo** |
| **Dashboards** | ❌ | ❌ | ⚠️ | ❌ | ✅ **Automático** |
| **RAG** | ❌ | ✅ | ❌ | ✅ | ✅ **Nativo** |
| **Facilidade** | ❌ | ⚠️ | ⚠️ | ❌ | ✅ **Intuitivo** |

## 🚀 **Roadmap**

### **Fase 1: MVP (3-6 meses)**
- Storage Engine básico (Rust)
- NL2Query simples (Python)
- UI mínima (React)
- Pipeline de ingestão (CSV/JSON)

### **Fase 2: Híbrido (6-12 meses)**
- Storage vetorial (HNSW)
- Query Engine avançado
- Dashboards automáticos
- SDK para agentes

### **Fase 3: Revolucionário (12-18 meses)**
- RAG nativo
- Otimizador inteligente
- Plugins ecosystem
- Enterprise features

## 🎯 **Próximos Passos**

1. **Definir MVP**: Escopo mínimo viável
2. **Implementar Storage**: B-tree básico em Rust
3. **Integrar LLM**: OpenAI para NL2Query
4. **Criar UI**: Interface básica para testes
5. **Testar com Usuários**: Validar com PMs reais

## 💰 **Modelo de Negócio**

- **Starter**: $0/mês - 1GB, 100 queries
- **Professional**: $50/mês - 10GB, 1000 queries
- **Business**: $200/mês - 100GB, 10000 queries
- **Enterprise**: $1000/mês - Ilimitado

## 🏆 **Conclusão**

O Askadb tem o potencial de **revolucionar o mercado de bancos de dados** ao:

1. **Democratizar dados**: Linguagem natural para todos
2. **Unificar tecnologias**: Relacional + vetorial + RAG
3. **Automatizar insights**: Dashboards automáticos
4. **Acelerar IA**: RAG nativo para startups

**O momento é agora** - o mercado está pronto para um banco de dados que combine simplicidade, performance e inteligência artificial.

