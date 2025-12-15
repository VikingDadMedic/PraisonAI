# AI Agents with Memory

```mermaid
flowchart TB
 subgraph Memory
 direction TB
 STM[Short Term]
 LTM[Long Term]
 end

 subgraph Store
 direction TB
 DB[(Vector DB)]
 end

 Input[Input] ---> Agents
 subgraph Agents
 direction LR
 A1[Agent 1]
 A2[Agent 2]
 A3[Agent 3]
 end
 Agents ---> Output[Output]

 Memory Store
 Store A1
 Store A2
 Store A3

 style Memory fill:#189AB4,color:#fff
 style Store fill:#2E8B57,color:#fff
 style Agents fill:#8B0000,color:#fff
 style Input fill:#8B0000,color:#fff
 style Output fill:#8B0000,color:#fff
```

| Feature | [Knowledge](/concepts/knowledge) | [Memory](/concepts/memory) |
|---------|--------------------------------|---------------------------|
| When Used | Pre-loaded before agent execution | Created and updated during runtime |
| Purpose | Provide static reference information | Store dynamic context and interactions |
| Storage | Read-only knowledge base | Read-write memory store |
| Persistence | Permanent until explicitly changed | Can be temporary (STM) or persistent (LTM) |
| Updates | Manual updates through knowledge files | Automatic updates during agent execution |

## Quick Start

## Understanding Memory

## Features

## Multi-Agent Memory

### Configuration Options

```python
# Create an agent with memory configuration

agent = Agent(
 role="Research Analyst",
 goal="Research and retain information",
 backstory="Expert in research and analysis",
 tools=[duckduckgo],
 verbose=True, # Enable detailed logging

 llm="gpt-4o" # Language model to use

)

# Create agents with memory options

agents = PraisonAIAgents(
 agents=[agent],
 tasks=[task],
 memory=True, # Enable memory

)
```

## Troubleshooting

## Next Steps

### Memory Configuration Options

The memory system in PraisonAI supports various configuration options to customize how agents store and retrieve information:

```python
memory_config = {
 # Memory Provider

 "provider": "rag", # Options: "rag", "mem0", "none"

 "use_embedding": True, # Enable semantic search with embeddings

 # Storage Paths

 "short_db": ".praison/short_term.db", # Short-term memory SQLite DB

 "long_db": ".praison/long_term.db", # Long-term memory SQLite DB

 "rag_db_path": ".praison/chroma_db", # Vector database path

 # Memory Settings

 "ttl": 3600, # Time to live for memory items (in seconds)

 # Optional Mem0 Config (if using mem0 provider)

 "config": {
 "api_key": "...", # Mem0 API key

 "org_id": "...", # Organization ID

 "project_id": "..." # Project ID

 }
}

# Create agents with memory configuration

agents = PraisonAIAgents(
 agents=[agent],
 tasks=[task],
 memory=True,
 memory_config=memory_config
)
```

### Memory Types

PraisonAI's memory system includes several types of memory:

### Memory Quality Control

PraisonAI includes built-in quality control for memory storage:

```python
# Example of storing with quality metrics

agents.memory.store_long_term(
 text="Important information to remember",
 ,
 completeness=0.9, # How complete is the information

 relevance=0.85, # How relevant to the task

 clarity=0.95, # How clear and well-structured

 accuracy=0.9, # How accurate is the information

)

# Search with quality filter

results = agents.memory.search_long_term(
 query="search query",
 min_quality=0.8, # Only return high-quality matches

 limit=5 # Maximum number of results

)