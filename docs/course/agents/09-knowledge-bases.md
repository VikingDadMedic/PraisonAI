# Knowledge Bases

Knowledge bases provide agents with specialized information beyond their training data. They help agents answer specific questions, follow guidelines, or work within defined domains.

## What is a Knowledge Base?

Think of a knowledge base as a specialized reference library for your agent.

## Why Use Knowledge Bases?

## Types of Knowledge Bases

### 1. Document Collections

Text documents, articles, guides, or manuals that agents can reference.

```mermaid
graph TD
 A[Query] --> B[Search Documents]
 B --> C[Find Relevant Content]
 C --> D[Generate Response]
```

### 2. Structured Data

Databases, tables, or other structured formats that organize information systematically.

### 3. Vector Databases

Special databases that store information as numerical representations (vectors) for semantic search.

```mermaid
graph TD
 A[User Question] --> B[Convert to Vector]
 B --> C[Find Similar Vectors]
 C --> D[Retrieve Related Information]
 D --> E[Generate Answer]
```

## Implementing Knowledge Bases in PraisonAI

Here's an example of how to use a knowledge base with an agent:

```python
from praisonaiagents import Agent

agent = Agent(
 name="Knowledge Agent",
 instructions="You answer questions based on the provided knowledge.",
 knowledge=["small.pdf"]
)

agent.start("What is KAG in one line?")
```

## Advanced Configuration

For more control over the knowledge base, you can specify a configuration:

```python
from praisonaiagents import Agent

config = {
 "vector_store": {
 "provider": "chroma",
 "config": {
 "collection_name": "custom_knowledge",
 "path": ".praison",
 }
 }
}

agent = Agent(
 name="Knowledge Agent",
 instructions="You answer questions based on the provided knowledge.",
 knowledge=["small.pdf"],
 knowledge_config=config
)

agent.start("What is KAG in one line?")
```

## Knowledge Retrieval Process

When an agent uses a knowledge base, this typical process occurs:

```mermaid
graph LR
 A[User Question] --> B[Query Processing]
 B --> C[Search Knowledge Base]
 C --> D[Rank Results]
 D --> E[Synthesize Answer]
```
1. **Query Processing**: The user's question is analyzed
2. **Search**: The system searches the knowledge base for relevant information
3. **Ranking**: Results are ranked by relevance
4. **Synthesis**: The agent creates an answer using the retrieved information

## Best Practices for Knowledge Bases

## When to Use Knowledge Bases

Knowledge bases are particularly valuable when:
1. Your agent needs to reference specific information that may change over time
2. You need to ensure consistent answers to common questions
3. Your agent needs to follow specific guidelines or protocols
4. You want to provide expertise in specialized domains

## Creating a Simple Knowledge Base

For beginners, you can start with a simple text-based knowledge base:

```
# Company FAQ Knowledge Base

## Return Policy

Our return policy allows customers to return products within 30 days of purchase for a full refund.

## Shipping Information

Standard shipping takes 3-5 business days. Express shipping takes 1-2 business days.

## Product Warranty

All products come with a 1-year limited warranty covering manufacturing defects.
```

Save this as a text file and add it to your knowledge base:

```python
kb = KnowledgeBase()
kb.add_document("company_faq.txt")
```

In the next lesson, we'll explore how agents handle tasks and the task management process.