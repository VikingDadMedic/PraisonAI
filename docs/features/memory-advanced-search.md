# Memory Advanced Search

Memory search in PraisonAI Agents provides advanced parameters for better control over search results, including reranking for improved relevance and cutoff thresholds for quality control.

## Quick Start

## Advanced Search Parameters

The `search_long_term` method supports several advanced parameters:

### Method Signature

```python
def search_long_term(
 self,
 query: str,
 limit: int = 5,
 relevance_cutoff: float = 0.0,
 min_quality: float = 0.0,
 rerank: bool = False,
 **kwargs
) -> List[Dict[str, Any]]:
```

### Parameter Details

## Provider-Specific Features

### ChromaDB (Local Storage)

ChromaDB is the default local storage provider that supports relevance filtering:

```python
from praisonaiagents import Memory

# Initialize ChromaDB memory

memory = Memory()

# Store memories with metadata

memory.store_long_term(
 "The Eiffel Tower is 330 meters tall",

)

memory.store_long_term(
 "The Statue of Liberty is 93 meters tall",

)

# Search with relevance cutoff

results = memory.search_long_term(
 "How tall is the Eiffel Tower?",
 relevance_cutoff=0.6, # Filter out low-relevance results

 limit=10
)

# ChromaDB calculates score as: 1.0 - distance

# Higher scores mean better relevance

```

### Mem0 (Cloud Provider)

Mem0 is a cloud-based provider that supports both relevance filtering and reranking:

```python
from praisonaiagents import Memory

# Initialize Mem0 memory

mem0_memory = Memory(
})

# Search with reranking enabled

results = mem0_memory.search(
 query="What are the key features of our product?",
 agent_id="agent-123", # Required for Mem0

 rerank=True, # Enable reranking for better results

 limit=5
)

# Reranking adds 150-200ms latency but improves result quality

```

## Relevance Scoring

### How Relevance Scores Work

### Setting Appropriate Cutoffs

```python
# Conservative cutoff - only very relevant results

high_quality_results = memory.search_long_term(
 "important company policies",
 relevance_cutoff=0.8
)

# Moderate cutoff - balanced results

balanced_results = memory.search_long_term(
 "product features",
 relevance_cutoff=0.6
)

# Low cutoff - more inclusive results

inclusive_results = memory.search_long_term(
 "general information",
 relevance_cutoff=0.3
)
```

## Complete Examples

### Example 1: Knowledge Base Search

```python
from praisonaiagents import Memory

# Create a knowledge base

knowledge_memory = Memory()

# Store various facts

facts = [
 "Python was created by Guido van Rossum in 1991",
 "JavaScript was created by Brendan Eich in 1995",
 "Java was created by James Gosling in 1995",
 "C++ was created by Bjarne Stroustrup in 1985",
 "Ruby was created by Yukihiro Matsumoto in 1995"
]

for fact in facts:
 knowledge_memory.store_long_term(fact)

# Search with different relevance thresholds

query = "Who created Python?"

# High relevance - only direct matches

strict_results = knowledge_memory.search_long_term(
 query,
 relevance_cutoff=0.8,
 limit=3
)
print(f"Strict search found {len(strict_results)} results")

# Medium relevance - related programming languages

related_results = knowledge_memory.search_long_term(
 query,
 relevance_cutoff=0.5,
 limit=5
)
print(f"Related search found {len(related_results)} results")
```

### Example 2: Agent Memory with Quality Tracking

```python
from praisonaiagents import Agent, Memory, Task, PraisonAIAgents

# Create agent with memory

agent = Agent(
 name="Research Assistant",
 role="Information specialist",
 goal="Provide accurate information from memory",
 backstory="An AI with perfect recall and organization skills"
)

# Create memory instance

memory = Memory()

# Task that stores high-quality information

def research_and_store(topic: str):
 # Simulate research with quality score

 research_data = f"Comprehensive research on {topic}"
 quality_score = 0.85 # High quality

 # Store with quality metadata

 memory.store_long_term(
 research_data,

 )
 return f"Stored research on {topic}"

# Search with quality filtering

def search_quality_info(query: str):
 results = memory.search_long_term(
 query,
 relevance_cutoff=0.6,
 min_quality=0.8, # Only high-quality results

 limit=3
 )
 return results

# Create tasks

store_task = Task(
 description="Research and store information about artificial intelligence",
 expected_output="Confirmation of stored research",
 agent=agent,
 execute_function=lambda: research_and_store("artificial intelligence")
)

search_task = Task(
 description="Find high-quality information about AI",
 expected_output="Top quality search results",
 agent=agent,
 execute_function=lambda: search_quality_info("artificial intelligence")
)

# Run workflow

workflow = PraisonAIAgents(
 agents=[agent],
 tasks=[store_task, search_task],
 process="sequential"
)

results = workflow.start()
```

### Example 3: Multi-Provider Setup

```python
from praisonaiagents import Memory
import os

# Setup both providers

local_memory = Memory()

cloud_memory = Memory(
})

# Function to search both providers

def search_all_memory(query: str, use_rerank: bool = True):
 # Search local memory with relevance cutoff

 local_results = local_memory.search_long_term(
 query,
 relevance_cutoff=0.6,
 limit=5
 )

 # Search cloud memory with reranking

 cloud_results = cloud_memory.search(
 query=query,
 agent_id="global",
 rerank=use_rerank, # Only works with Mem0

 limit=5
 )

 # Combine and deduplicate results

 all_results = []
 seen_content = set()

 for result in local_results + cloud_results:
 content = result.get('memory', '')
 if content not in seen_content:
 seen_content.add(content)
 all_results.append(result)

 # Sort by relevance score

 all_results.sort(
 key=lambda x: x.get('score', 0),
 reverse=True
 )

 return all_results[:10] # Top 10 results

# Use the multi-provider search

results = search_all_memory(
 "What are the main features of our product?",
 use_rerank=True
)

for i, result in enumerate(results, 1):
 print(f"{i}. {result['memory']}")
 print(f" Score: {result.get('score', 'N/A')}")
 print(f" Provider: {result.get('provider', 'unknown')}")
```

## Performance Considerations

## Best Practices

1. **Choose the Right Provider**
- Use ChromaDB for local, fast searches
- Use Mem0 for cloud-based with reranking needs
2. **Set Appropriate Cutoffs**
- Start with 0.6-0.7 for general searches
- Use 0.8+ for precise matching
- Use 0.3-0.5 for exploratory searches
3. **Optimize for Your Use Case**
 ```python
 # Fast, local search for UI

 quick_results = memory.search_long_term(
 query,
 relevance_cutoff=0.7,
 limit=3
 )

 # Comprehensive search for analysis

 detailed_results = cloud_memory.search(
 query=query,
 agent_id=agent_id,
 rerank=True,
 limit=20
 )
 ```

## Troubleshooting

## Next Steps