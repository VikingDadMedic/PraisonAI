# YFinance Agent

## YFinance Tools

Use YFinance Tools to retrieve and analyze financial data with AI agents.

## Understanding YFinance Tools

## Key Components

## Examples

### Basic Financial Data Agent

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
from praisonaiagents.tools import get_stock_price, get_stock_info, get_historical_data

# Create finance agent

finance_agent = Agent(
 name="MarketAnalyst",
 role="Financial Data Specialist",
 goal="Analyze market data efficiently and accurately.",
 backstory="Expert in financial analysis and market research.",
 tools=[get_stock_price, get_stock_info, get_historical_data],
 self_reflect=False
)

# Define finance task

finance_task = Task(
 description="Analyze tech sector performance and trends.",
 expected_output="Comprehensive market analysis report.",
 agent=finance_agent,
 name="sector_analysis"
)

# Run agent

agents = PraisonAIAgents(
 agents=[finance_agent],
 tasks=[finance_task],
 process="sequential"
)
agents.start()
```

### Advanced Market Analysis with Multiple Agents

```python
# Create data retrieval agent

data_agent = Agent(
 name="DataCollector",
 role="Market Data Collector",
 goal="Retrieve financial data systematically.",
 tools=[get_stock_price, get_historical_data],
 self_reflect=False
)

# Create analysis agent

analysis_agent = Agent(
 name="Analyst",
 role="Market Analyst",
 goal="Analyze market trends and patterns.",
 backstory="Expert in financial market analysis.",
 tools=[get_stock_info],
 self_reflect=False
)

# Define tasks

data_task = Task(
 description="Collect historical market data for analysis.",
 agent=data_agent,
 name="data_collection"
)

analysis_task = Task(
 description="Analyze collected market data for insights.",
 agent=analysis_agent,
 name="trend_analysis"
)

# Run agents

agents = PraisonAIAgents(
 agents=[data_agent, analysis_agent],
 tasks=[data_task, analysis_task],
 process="sequential"
)
agents.start()
```

## Best Practices

## Common Patterns

### Market Analysis Pipeline

```python
# Data agent

collector = Agent(
 name="Collector",
 role="Data Collector",
 tools=[get_stock_price, get_historical_data]
)

# Analysis agent

analyst = Agent(
 name="Analyst",
 role="Market Analyst",
 tools=[get_stock_info]
)

# Define tasks

collect_task = Task(
 description="Collect market data",
 agent=collector
)

analyze_task = Task(
 description="Analyze market trends",
 agent=analyst
)

# Run workflow

agents = PraisonAIAgents(
 agents=[collector, analyst],
 tasks=[collect_task, analyze_task]
)