# DuckDuckGo Agent

## DuckDuckGo Tools

Use DuckDuckGo Tools to perform internet searches with AI agents.

## Understanding DuckDuckGo Tools

## Key Components

## Examples

### Basic Search Agent

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
from praisonaiagents.tools import duckduckgo

# Create search agent

search_agent = Agent(
 name="WebSearcher",
 role="Search Specialist",
 goal="Find accurate information about specified topics.",
 backstory="Expert in internet research and data collection.",
 tools=[duckduckgo],
 self_reflect=False
)

# Define search task

search_task = Task(
 description="Search for 'Python programming best practices 2024' and summarize the key points.",
 expected_output="List of best practices with sources.",
 agent=search_agent,
 name="search_python"
)

# Run agent

agents = PraisonAIAgents(
 agents=[search_agent],
 tasks=[search_task],
 process="sequential"
)
agents.start()
```

### Advanced Search with Multiple Agents

```python
# Create search agent

search_agent = Agent(
 name="Researcher",
 role="Search Specialist",
 goal="Gather comprehensive information about topics.",
 tools=[duckduckgo],
 self_reflect=False
)

# Create analysis agent

analysis_agent = Agent(
 name="Analyzer",
 role="Data Analyst",
 goal="Analyze and synthesize search results.",
 backstory="Expert in data analysis and trend identification.",
 self_reflect=False
)

# Define tasks

search_task = Task(
 description="Search for latest AI developments in healthcare.",
 agent=search_agent,
 name="healthcare_search"
)

analysis_task = Task(
 description="Analyze the search results and identify key trends.",
 agent=analysis_agent,
 name="trend_analysis"
)

# Run agents

agents = PraisonAIAgents(
 agents=[search_agent, analysis_agent],
 tasks=[search_task, analysis_task],
 process="sequential"
)
agents.start()
```

## Best Practices

## Common Patterns

### Research and Analysis

```python
# Research agent

researcher = Agent(
 name="Researcher",
 role="Search Specialist",
 tools=[duckduckgo]
)

# Analysis agent

analyst = Agent(
 name="Analyst",
 role="Information Analyst"
)

# Define tasks

research_task = Task(
 description="Research quantum computing advances",
 agent=researcher
)

analysis_task = Task(
 description="Analyze research findings",
 agent=analyst
)

# Run workflow

agents = PraisonAIAgents(
 agents=[researcher, analyst],
 tasks=[research_task, analysis_task]
)