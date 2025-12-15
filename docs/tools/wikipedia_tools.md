# Wikipedia Agent

## Wikipedia Tools

Use Wikipedia Tools to retrieve and analyze Wikipedia content with AI agents.

## Understanding Wikipedia Tools

## Key Components

## Examples

### Basic Wikipedia Research Agent

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
from praisonaiagents.tools import wiki_search, wiki_summary, wiki_page, wiki_random, wiki_language

# Create Wikipedia agent

wiki_agent = Agent(
 name="WikiExpert",
 role="Research Specialist",
 goal="Research topics efficiently and accurately.",
 backstory="Expert in information gathering and analysis.",
 tools=[wiki_search, wiki_summary, wiki_page, wiki_random, wiki_language],
 self_reflect=False
)

# Define research task

research_task = Task(
 description="Research scientific discoveries and breakthroughs.",
 expected_output="Detailed research report with references.",
 agent=wiki_agent,
 name="science_research"
)

# Run agent

agents = PraisonAIAgents(
 agents=[wiki_agent],
 tasks=[research_task],
 process="sequential"
)
agents.start()

```

### Advanced Research with Multiple Agents

```python
# Create research agent

researcher_agent = Agent(
 name="Researcher",
 role="Content Researcher",
 goal="Research topics systematically.",
 tools=[wiki_search, wiki_summary, wiki_page],
 self_reflect=False
)

# Create analysis agent

analysis_agent = Agent(
 name="Analyzer",
 role="Content Analyst",
 goal="Analyze and summarize research findings.",
 backstory="Expert in content analysis and synthesis.",
 tools=[wiki_summary, wiki_page],
 self_reflect=False
)

# Define tasks

research_task = Task(
 description="Research technological advancements.",
 agent=researcher_agent,
 name="tech_research"
)

analysis_task = Task(
 description="Analyze and synthesize research findings.",
 agent=analysis_agent,
 name="content_analysis"
)

# Run agents

agents = PraisonAIAgents(
 agents=[researcher_agent, analysis_agent],
 tasks=[research_task, analysis_task],
 process="sequential"
)
agents.start()
```

## Best Practices

## Common Patterns

### Research Pipeline

```python
# Research agent

researcher = Agent(
 name="Researcher",
 role="Wiki Researcher",
 tools=[wiki_search, wiki_summary, wiki_page]
)

# Analysis agent

analyzer = Agent(
 name="Analyzer",
 role="Content Analyzer",
 tools=[wiki_summary, wiki_page]
)

# Define tasks

research_task = Task(
 description="Research topic",
 agent=researcher
)

analyze_task = Task(
 description="Analyze findings",
 agent=analyzer
)

# Run workflow

agents = PraisonAIAgents(
 agents=[researcher, analyzer],
 tasks=[research_task, analyze_task]
)