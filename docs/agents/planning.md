# Planning Agent

```mermaid
flowchart LR
 In[Request] --> Search[Web Search]
 Search --> Analyzer[Options Analyzer]
 Analyzer --> Generator[Plan Generator]
 Generator --> Out[Itinerary]

 style In fill:#8B0000,color:#fff
 style Search fill:#2E8B57,color:#fff
 style Analyzer fill:#2E8B57,color:#fff
 style Generator fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow demonstrating how the Planning Agent can search for travel options and create detailed itineraries.

## Quick Start

## Understanding Travel Planning

The Planning Agent combines multiple capabilities to create comprehensive travel plans:
1. **Web Search**: Uses DuckDuckGo to find current travel options
2. **Options Analysis**: Evaluates and compares different choices
3. **Plan Generation**: Creates detailed itineraries
4. **Recommendations**: Provides personalized suggestions

## Features

## Example Usage

```python
# Example: Create a weekend trip plan

from praisonaiagents import Agent, Tools
from praisonaiagents.tools import duckduckgo

agent = Agent(instructions="You are a Planning Agent", tools=[duckduckgo])
agent.start("I want to go London next week, find me a good hotel and flight")
```

## Next Steps

- Learn about [Prompt Chaining](/features/promptchaining) for complex travel planning
- Explore [Evaluator Optimizer](/features/evaluator-optimiser) for improving recommendations
- Check out the [Research Agent](/agents/research) for detailed destination research