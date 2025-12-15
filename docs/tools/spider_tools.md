# Spider Agent

## Spider Tools

Use Spider Tools to crawl and scrape web content with AI agents.

## Understanding Spider Tools

## Key Components

## Examples

### Basic Web Scraping Agent

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
from praisonaiagents.tools import scrape_page, extract_links, crawl, extract_text

# Create search agent

agent = Agent(
 name="AsyncWebCrawler",
 role="Web Scraping Specialist",
 goal="Extract and analyze web content efficiently.",
 backstory="Expert in web scraping and content extraction.",
 tools=[scrape_page, extract_links, crawl, extract_text],
 self_reflect=False
)

# Define task

task = Task(
 description="Scrape and analyze the content from 'https://example.com'",
 expected_output="Extracted content with links and text analysis",
 agent=agent,
 name="web_scraping"
)

# Run agent

agents = PraisonAIAgents(
 agents=[agent],
 tasks=[task],
 process="sequential"
)
agents.start()
```

### Advanced Scraping with Multiple Agents

```python
# Create scraping agent

scraper_agent = Agent(
 name="Scraper",
 role="Content Scraper",
 goal="Extract web content systematically.",
 tools=[scrape_page, crawl_links, parse_html],
 self_reflect=False
)

# Create analysis agent

analysis_agent = Agent(
 name="Analyzer",
 role="Content Analyst",
 goal="Analyze and structure scraped content.",
 backstory="Expert in data analysis and organization.",
 tools=[extract_content, structure_data],
 self_reflect=False
)

# Define tasks

scraping_task = Task(
 description="Scrape product data from multiple pages.",
 agent=scraper_agent,
 name="product_scraping"
)

analysis_task = Task(
 description="Analyze and structure the scraped product data.",
 agent=analysis_agent,
 name="data_analysis"
)

# Run agents

agents = PraisonAIAgents(
 agents=[scraper_agent, analysis_agent],
 tasks=[scraping_task, analysis_task],
 process="sequential"
)
agents.start()
```

## Best Practices

## Common Patterns

### Web Scraping Pipeline

```python
# Scraping agent

scraper = Agent(
 name="Scraper",
 role="Web Scraper",
 tools=[scrape_page, crawl_links, parse_html]
)

# Processing agent

processor = Agent(
 name="Processor",
 role="Data Processor",
 tools=[extract_content, structure_data]
)

# Define tasks

scrape_task = Task(
 description="Scrape website content",
 agent=scraper
)

process_task = Task(
 description="Process scraped content",
 agent=processor
)

# Run workflow

agents = PraisonAIAgents(
 agents=[scraper, processor],
 tasks=[scrape_task, process_task]
)