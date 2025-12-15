# LangChain Agent

## Quick Start

## Understanding LangChain Integration

## Features

## LangChain Tools with Wrappers

### Example Wrapper Usage

```bash
pip install langchain-community google-search-results
export SERPAPI_API_KEY=your_api_key_here
export OPENAI_API_KEY=your_api_key_here
```

```python
from praisonaiagents import Agent, PraisonAIAgents
from langchain_community.utilities import SerpAPIWrapper

data_agent = Agent(instructions="Search about AI job trends in 2025", tools=[SerpAPIWrapper])
editor_agent = Agent(instructions="Write a blog article")

agents = PraisonAIAgents(agents=[data_agent, editor_agent])
agents.start()
```

### Example without Wrapper Usage

```bash
pip install langchain-community
export BRAVE_SEARCH_API=your_api_key_here
export OPENAI_API_KEY=your_api_key_here
```

```python
from praisonaiagents import Agent, PraisonAIAgents
from langchain_community.tools import BraveSearch
import os

def search_brave(query: str):
 """Searches using BraveSearch and returns results."""
 api_key = os.environ['BRAVE_SEARCH_API']
 tool = BraveSearch.from_api_key(api_key=api_key, )
 return tool.run(query)

data_agent = Agent(instructions="Search about AI job trends in 2025", tools=[search_brave])
editor_agent = Agent(instructions="Write a blog article")
agents = PraisonAIAgents(agents=[data_agent, editor_agent])
agents.start()
```

## Next Steps

## With More Detailed Instructions