# Chat with PDF Agents

```mermaid
flowchart LR
 PDF[PDF Document] --> PDFAgent[("PDF Chat Agent")]
 VDB[(Vector DB)] --> PDFAgent
 PDFAgent --> Chat[Chat Interface]
 Chat --> |Query| VDB
 Chat --> Out[Response]

 style PDF fill:#8B0000,color:#fff
 style PDFAgent fill:#2E8B57,color:#fff,shape:circle
 style VDB fill:#4169E1,color:#fff,shape:cylinder
 style Chat fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A PDF-centric workflow where Chat agents interact with vector databases to store and retrieve information from PDF documents, enabling natural conversations and intelligent question-answering capabilities.

## Quick Start

## PDF Processing and Chat Agents

```mermaid
flowchart LR
 subgraph In[Input]
 PDF[PDF Documents]
 end

 subgraph Router[Vector Store]
 DB[(Vector DB)]
 end

 subgraph Out[Chat Agents]
 A1[Chat Agent 1]
 A2[Chat Agent 2]
 A3[Chat Agent 3]
 end

 In --> Router
 Router --> A1
 Router --> A2
 Router --> A3

 style In fill:#8B0000,color:#fff
 style Router fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

The simplest way to create a PDF chat agent is without any configuration:

```python
from praisonaiagents import Agent

agent = Agent(
 name="PDF Chat Agent",
 instructions="You answer questions based on the provided PDF document.",
 knowledge=["document.pdf"] # PDF Indexing

)

agent.start("What are the key points in this document?") # Chat Query

```

### Advanced Configuration

For more control over the knowledge base, you can specify a configuration:

```python
from praisonaiagents import Agent

config = {
 "vector_store": {
 "provider": "chroma",
 "config": {
 "collection_name": "praison",
 "path": ".praison",
 }
 }
}

agent = Agent(
 name="PDF Chat Agent",
 instructions="You answer questions based on the provided PDF document.",
 knowledge=["document.pdf"], # PDF Indexing

 knowledge_config=config # Configuration

)

agent.start("What is the main topic of this PDF?") # Chat Query

```

### Multi-Agent Knowledge System

For more complex scenarios, you can create a knowledge-based system with multiple agents:

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
import logging
import os

# Configure logging

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)

# Define the configuration for the Knowledge instance

config = {
 "vector_store": {
 "provider": "chroma",
 "config": {
 "collection_name": "praison",
 "path": ".praison",
 }
 }
}

# Create an agent with knowledge capabilities

knowledge_agent = Agent(
 name="KnowledgeAgent",
 role="Information Specialist",
 goal="Store and retrieve knowledge efficiently",
 backstory="Expert in managing and utilizing stored knowledge",
 knowledge=["sample.pdf"], # Indexing

 knowledge_config=config, # Configuration

 verbose=True
)

# Define a task for the agent

knowledge_task = Task(
 name="knowledge_task",
 description="Who is Mervin Praison?",
 expected_output="Answer to the question",
 agent=knowledge_agent # Agent

)

# Create and start the agents

agents = PraisonAIAgents(
 agents=[knowledge_agent],
 tasks=[knowledge_task],
 process="sequential",
 user_id="user1" # User ID

)

# Start execution

result = agents.start() # Retrieval

```

## Understanding PDF Chat Agents

## Features

## Troubleshooting

## Next Steps