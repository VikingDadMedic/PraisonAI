# Recommendation Agent

```mermaid
flowchart LR
 In[User Preferences] --> Search[Content Search]
 Search --> Analyzer[Preference Analyzer]
 Analyzer --> Generator[Recommendation Generator]
 Generator --> Out[Recommendations]

 style In fill:#8B0000,color:#fff
 style Search fill:#2E8B57,color:#fff
 style Analyzer fill:#2E8B57,color:#fff
 style Generator fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow demonstrating how the Recommendation Agent can analyze preferences and generate personalized recommendations.

## Quick Start

## Understanding Recommendation System

The Recommendation Agent uses multiple approaches to generate personalized suggestions:
1. **Content Search**: Uses DuckDuckGo to find current options
2. **Preference Analysis**: Understands user preferences
3. **Recommendation Generation**: Creates personalized suggestions
4. **Content Filtering**: Filters based on relevance and quality

## Features

## Example Usage

```python
# Example: Get personalized recommendations

from praisonaiagents import Agent, Tools
from praisonaiagents.tools import duckduckgo

agent = Agent(instructions="You are a Recommendation Agent", tools=[duckduckgo])
agent.start("Recommend me a good movie to watch in 2025")
```

## Next Steps

- Learn about [Prompt Chaining](/features/promptchaining) for complex recommendation systems
- Explore [Evaluator Optimizer](/features/evaluator-optimiser) for improving recommendation accuracy
- Check out the [Research Agent](/agents/research) for detailed content research