# arXiv Agent

## arXiv Tools

Use arXiv Tools to search and analyze research papers with AI agents.

## Understanding arXiv Tools

## Available Functions

```python
from praisonaiagents.tools import search_arxiv
from praisonaiagents.tools import get_arxiv_paper
from praisonaiagents.tools import get_papers_by_author
from praisonaiagents.tools import get_papers_by_category
```

## Function Details

### search_arxiv(query: str, max_results: int = 10, sort_by: str = "relevance", sort_order: str = "descending", include_fields: Optional[List[str]] = None)

Search arXiv for papers:
- Flexible query support
- Customizable results
- Multiple sorting options
- Field selection
- Error handling

```python
# Basic search

papers = search_arxiv("quantum computing")

# Advanced search

papers = search_arxiv(
 query="quantum computing",
 max_results=5,
 sort_by="submittedDate",
 sort_order="descending",
 include_fields=["title", "authors", "summary"]
)

# Returns: List[Dict[str, Any]]

# Example output:

# [

# {

# "arxiv_id": "2401.00123",

# "title": "Quantum Computing: A Survey",

# "authors": ["John Doe", "Jane Smith"],

# "summary": "This paper reviews...",

# "published": "2024-01-01T00:00:00",

# "updated": "2024-01-02T00:00:00"

# },

# ...

# ]

```

### get_arxiv_paper(paper_id: str, include_fields: Optional[List[str]] = None)

Get specific paper details:
- Direct ID lookup
- Full paper metadata
- Customizable fields
- PDF/Abstract links

```python
# Get paper by ID

paper = get_arxiv_paper("2401.00123")

# Get specific fields

paper = get_arxiv_paper(
 paper_id="2401.00123",
 include_fields=["title", "authors", "pdf_url"]
)

# Returns: Dict[str, Any]

# Example output:

# {

# "arxiv_id": "2401.00123",

# "title": "Quantum Computing: A Survey",

# "authors": ["John Doe", "Jane Smith"],

# "pdf_url": "https://arxiv.org/pdf/2401.00123",

# "abstract_url": "https://arxiv.org/abs/2401.00123",

# "published": "2024-01-01T00:00:00",

# "updated": "2024-01-02T00:00:00"

# }

```

### get_papers_by_author(author: str, max_results: int = 10, sort_by: str = "submittedDate", sort_order: str = "descending", include_fields: Optional[List[str]] = None)

Search papers by author:
- Author-specific search
- Publication timeline
- Sort options
- Field selection

```python
# Get author's papers

papers = get_papers_by_author("Yoshua Bengio")

# Recent papers first

papers = get_papers_by_author(
 author="Yoshua Bengio",
 max_results=5,
 sort_by="submittedDate",
 sort_order="descending"
)

# Returns: List[Dict[str, Any]]

# Example output:

# [

# {

# "arxiv_id": "2401.00124",

# "title": "Deep Learning Advances",

# "authors": ["Yoshua Bengio", "Other Author"],

# "published": "2024-01-01T00:00:00"

# },

# ...

# ]

```

### get_papers_by_category(category: str, max_results: int = 10, sort_by: str = "submittedDate", sort_order: str = "descending", include_fields: Optional[List[str]] = None)

Search papers by category:
- Category-specific search
- Latest publications
- Sort options
- Field selection

```python
# Get papers in category

papers = get_papers_by_category("cs.AI")

# Recent machine learning papers

papers = get_papers_by_category(
 category="cs.LG",
 max_results=5,
 sort_by="submittedDate",
 sort_order="descending"
)

# Returns: List[Dict[str, Any]]

# Example output:

# [

# {

# "arxiv_id": "2401.00125",

# "title": "New ML Algorithm",

# "categories": ["cs.LG", "cs.AI"],

# "published": "2024-01-01T00:00:00"

# },

# ...

# ]

```

## Dependencies

The arXiv tools require the following package:
- arxiv: For accessing the arXiv API

This will be automatically installed when needed.

## Available Fields

When using `include_fields`, you can select from:
- title: Paper title
- authors: List of authors
- summary: Abstract text
- comment: Author comments
- journal_ref: Journal reference
- doi: Digital Object Identifier
- primary_category: Main arXiv category
- categories: All arXiv categories
- links: PDF and abstract URLs

## Common Use Cases

1. Literature Review:
```python
# Search multiple related topics

topics = ["machine learning", "neural networks", "deep learning"]
all_papers = []
for topic in topics:
 papers = search_arxiv(
 topic,
 max_results=5,
 sort_by="relevance"
 )
 all_papers.extend(papers)
```
2. Author Research:
```python
# Get recent papers by multiple authors

authors = ["Yoshua Bengio", "Geoffrey Hinton", "Yann LeCun"]
author_papers = {}
for author in authors:
 papers = get_papers_by_author(
 author,
 max_results=3,
 sort_by="submittedDate"
 )
 author_papers[author] = papers
```
3. Category Monitoring:
```python
# Monitor new papers in AI categories

categories = ["cs.AI", "cs.LG", "cs.CL"]
new_papers = {}
for category in categories:
 papers = get_papers_by_category(
 category,
 max_results=5,
 sort_by="submittedDate"
 )
 new_papers[category] = papers
```

## Examples

### Basic Research Agent

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
from praisonaiagents.tools import search_arxiv, get_arxiv_paper, get_papers_by_author, get_papers_by_category

# Create research agent

research_agent = Agent(
 name="PaperSearcher",
 role="Scientific Literature Specialist",
 goal="Find relevant scientific papers on specified topics.",
 backstory="Expert in academic research and literature analysis.",
 tools=[search_arxiv, get_arxiv_paper, get_papers_by_author, get_papers_by_category],
 self_reflect=False
)

# Define research task

research_task = Task(
 description="Search for papers on 'transformer models in NLP' from the last year.",
 expected_output="List of relevant papers with abstracts and key findings.",
 agent=research_agent,
 name="nlp_research"
)

# Run agent

agents = PraisonAIAgents(
 agents=[research_agent],
 tasks=[research_task],
 process="sequential"
)
agents.start()
```

### Advanced Research with Multiple Agents

```python
# Create research agent

research_agent = Agent(
 name="Researcher",
 role="Literature Specialist",
 goal="Gather comprehensive scientific literature.",
 tools=[search_arxiv, get_arxiv_paper, get_papers_by_author, get_papers_by_category],
 self_reflect=False
)

# Create analysis agent

analysis_agent = Agent(
 name="Analyzer",
 role="Research Analyst",
 goal="Analyze and synthesize research findings.",
 backstory="Expert in scientific literature analysis.",
 self_reflect=False
)

# Define tasks

research_task = Task(
 description="Search for papers on quantum computing applications in cryptography.",
 agent=research_agent,
 name="quantum_research"
)

analysis_task = Task(
 description="Analyze the papers and identify key trends and breakthroughs.",
 agent=analysis_agent,
 name="research_analysis"
)

# Run agents

agents = PraisonAIAgents(
 agents=[research_agent, analysis_agent],
 tasks=[research_task, analysis_task],
 process="sequential"
)
agents.start()
```

## Best Practices

## Common Patterns

### Literature Review

```python
# Literature search agent

searcher = Agent(
 name="Searcher",
 role="Literature Specialist",
 tools=[search_arxiv, get_arxiv_paper, get_papers_by_author, get_papers_by_category]
)

# Review agent

reviewer = Agent(
 name="Reviewer",
 role="Research Reviewer"
)

# Define tasks

search_task = Task(
 description="Find papers on AI ethics",
 agent=searcher
)

review_task = Task(
 description="Review and summarize findings",
 agent=reviewer
)

# Run workflow

agents = PraisonAIAgents(
 agents=[searcher, reviewer],
 tasks=[search_task, review_task]
)