# Newspaper Agent

## Newspaper Tools

Use Newspaper Tools to extract and analyze news articles with AI agents.

## Understanding Newspaper Tools

## Key Components

## Available Functions

```python
from praisonaiagents.tools import get_article
from praisonaiagents.tools import get_news_sources
from praisonaiagents.tools import get_articles_from_source
from praisonaiagents.tools import get_trending_topics
```

## Function Details

### get_article(url: str, language: str = 'en')

Extracts and parses news articles:
- Full text extraction
- Author detection
- Date parsing
- Image extraction
- NLP processing

```python
# Basic usage

article = get_article("https://example.com/article")

# With language specification

article = get_article(
 "https://lemonde.fr/article",
 language="fr"
)

# Returns: Dict[str, Any]

# Example output:

# {

# "url": "https://example.com/article",

# "title": "Breaking News Story",

# "text": "Full article text...",

# "authors": ["John Doe", "Jane Smith"],

# "publish_date": "2025-01-07T05:31:38",

# "top_image": "https://example.com/image.jpg",

# "images": ["https://example.com/image1.jpg", ...],

# "movies": ["https://example.com/video1.mp4", ...],

# "source_domain": "example.com",

# "keywords": ["technology", "ai", ...],

# "summary": "Article summary..."

# }

```

### get_news_sources(category: Optional[str] = None, language: str = 'en', country: Optional[str] = None)

Gets news sources by category:
- Category filtering
- Language support
- Country filtering
- Domain information

```python
# Get all sources

sources = get_news_sources()

# Get technology news sources

tech_sources = get_news_sources(
 category="technology",
 language="en",
 country="us"
)

# Returns: List[Dict[str, str]]

# Example output:

# [

# {

# "url": "https://techcrunch.com",

# "domain": "techcrunch.com",

# "name": "TechCrunch",

# "category": "technology"

# },

# ...

# ]

```

### get_articles_from_source(source_url: str, limit: int = 10, language: str = 'en')

Gets recent articles from a source:
- Configurable limit
- Language support
- Full article parsing
- Error handling

```python
# Get latest articles

articles = get_articles_from_source(
 "https://techcrunch.com",
 limit=5
)

# Get articles in specific language

articles = get_articles_from_source(
 "https://lemonde.fr",
 limit=10,
 language="fr"
)

# Returns: List[Dict[str, Any]]

# Each article has the same structure as get_article()

```

### get_trending_topics(sources: Optional[List[str]] = None, limit: int = 10, language: str = 'en')

Analyzes trending topics:
- Cross-source analysis
- Keyword extraction
- Customizable sources
- Topic ranking

```python
# Get trending topics from default sources

topics = get_trending_topics(limit=5)

# Get topics from specific sources

topics = get_trending_topics(
 sources=[
 "https://techcrunch.com",
 "https://wired.com"
 ],
 limit=10,
 language="en"
)

# Returns: List[str]

# Example: ["artificial intelligence", "climate change", ...]

```

## Examples

### Basic News Agent

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
from praisonaiagents.tools import get_article, get_news_sources, get_articles_from_source, get_trending_topics

# Create news agent

news_agent = Agent(
 name="NewsExpert",
 role="News Analyst",
 goal="Gather and analyze news content efficiently.",
 backstory="Expert in news analysis and content curation.",
 tools=[get_article, get_news_sources, get_articles_from_source, get_trending_topics],
 self_reflect=False
)

# Define news task

news_task = Task(
 description="Analyze tech news trends.",
 expected_output="Tech news analysis report.",
 agent=news_agent,
 name="tech_news_analysis"
)

# Run agent

agents = PraisonAIAgents(
 agents=[news_agent],
 tasks=[news_task],
 process="sequential"
)
agents.start()
```

### Advanced News Analysis with Multiple Agents

```python
# Create news collection agent

collector_agent = Agent(
 name="Collector",
 role="News Collector",
 goal="Gather comprehensive news coverage.",
 tools=[get_article, get_news_sources, get_articles_from_source, get_trending_topics],
 self_reflect=False
)

# Create analysis agent

analysis_agent = Agent(
 name="Analyzer",
 role="Content Analyst",
 goal="Analyze news content and identify trends.",
 backstory="Expert in news analysis and trend identification.",
 self_reflect=False
)

# Define tasks

collection_task = Task(
 description="Collect news articles about renewable energy developments.",
 agent=collector_agent,
 name="energy_news"
)

analysis_task = Task(
 description="Analyze the articles and identify key trends and breakthroughs.",
 agent=analysis_agent,
 name="news_analysis"
)

# Run agents

agents = PraisonAIAgents(
 agents=[collector_agent, analysis_agent],
 tasks=[collection_task, analysis_task],
 process="sequential"
)
agents.start()
```

## Best Practices

## Common Patterns

### News Monitoring

```python
# News monitor agent

monitor = Agent(
 name="Monitor",
 role="News Monitor",
 tools=[get_article, get_news_sources, get_articles_from_source, get_trending_topics]
)

# Analysis agent

analyst = Agent(
 name="Analyst",
 role="News Analyst"
)

# Define tasks

monitor_task = Task(
 description="Monitor tech news sources",
 agent=monitor
)

analysis_task = Task(
 description="Analyze news trends",
 agent=analyst
)

# Run workflow

agents = PraisonAIAgents(
 agents=[monitor, analyst],
 tasks=[monitor_task, analysis_task]
)

```

## Example Agent Configuration

```python
from praisonaiagents import Agent
from praisonaiagents.tools import (
 get_article, get_news_sources,
 get_articles_from_source, get_trending_topics
)

agent = Agent(
 name="NewsAnalyzer",
 description="An agent that analyzes news articles",
 tools=[
 get_article, get_news_sources,
 get_articles_from_source, get_trending_topics
 ]
)
```

## Dependencies

The newspaper tools require the following Python packages:
- newspaper3k: For article extraction and parsing
- nltk: For NLP processing (automatically installed with newspaper3k)

These will be automatically installed when needed.

## Error Handling

All functions include comprehensive error handling:
- Network errors
- Parsing errors
- Language errors
- Source availability errors

Errors are handled consistently:
- Success cases return the expected data type
- Error cases return a dict with an "error" key
- All errors are logged for debugging

## Common Use Cases

1. News Monitoring:
```python
# Monitor tech news

sources = get_news_sources(category="technology")
for source in sources:
 articles = get_articles_from_source(
 source["url"],
 limit=5
 )
 for article in articles:
 if "ai" in article.get("keywords", []):
 print(f"AI article found: {article['title']}")
```
2. Trend Analysis:
```python
# Analyze trending topics

topics = get_trending_topics(limit=10)
print("Current hot topics:")
for i, topic in enumerate(topics, 1):
 print(f"{i}. {topic}")
```
3. Content Aggregation:
```python
# Aggregate news from multiple sources

def aggregate_news(categories):
 all_articles = []
 for category in categories:
 sources = get_news_sources(category=category)
 for source in sources[:3]: # Top 3 sources per category

 articles = get_articles_from_source(
 source["url"],
 limit=3
 )
 all_articles.extend(articles)
 return all_articles

news = aggregate_news(["technology", "business", "science"])