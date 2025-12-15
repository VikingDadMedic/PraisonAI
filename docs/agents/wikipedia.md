# Wikipedia Agent

```mermaid
flowchart LR
 In[Query] --> Search[Wiki Search]
 Search --> Page[Page Retrieval]
 Page --> Summary[Content Summary]
 Summary --> Out[Knowledge Output]

 style In fill:#8B0000,color:#fff
 style Search fill:#2E8B57,color:#fff
 style Page fill:#2E8B57,color:#fff
 style Summary fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow demonstrating how the Wikipedia Agent can search, retrieve, and summarize Wikipedia content.

## Quick Start

## Understanding Wikipedia Research

The Wikipedia Agent provides comprehensive Wikipedia access through multiple tools:
1. **Wiki Search**: `wiki_search` for finding relevant articles
2. **Page Access**: `wiki_page` for retrieving full articles
3. **Summarization**: `wiki_summary` for concise overviews
4. **Random Articles**: `wiki_random` for discovering content
5. **Language Support**: `wiki_language` for multilingual access

## Features

## Example Usage

```python
# Example: Research a scientific topic

from praisonaiagents import Agent
from praisonaiagents.tools import wiki_search, wiki_summary, wiki_page, wiki_random, wiki_language

agent = Agent(
 instructions="You are a Wikipedia Agent",
 tools=[wiki_search, wiki_summary, wiki_page, wiki_random, wiki_language],
 self_reflect=True,
 min_reflect=3,
 max_reflect=5,
)
agent.start(
 "What is the history of AI?"
 "First search the history of AI"
 "Read the page of the history of AI"
 "Get the summary of the page"
)
```

## Next Steps

- Learn about [Prompt Chaining](/features/promptchaining) for complex Wikipedia research
- Explore [Evaluator Optimizer](/features/evaluator-optimiser) for improving research quality
- Check out the [Research Agent](/agents/research) for broader research capabilities