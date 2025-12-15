# Quality Checking

Quality checking in PraisonAI automatically evaluates task outputs, calculates quality scores, and stores high-quality results in long-term memory for future reference.

## Quick Start

## How Quality Checking Works

When `quality_check=True` is set on a task:
1. **Output Generation**: Agent completes the task normally
2. **Quality Assessment**: System automatically evaluates the output
3. **Score Calculation**: Assigns a quality score (0.0 to 1.0)
4. **Memory Storage**: High-quality outputs (score > 0.7) are stored in long-term memory
5. **Metadata Tracking**: Quality metrics are saved with the result

## Quality Metrics

The quality assessment evaluates multiple factors:

## Complete Examples

### Example 1: Blog Post with Quality Tracking

```python
from praisonaiagents import Agent, Task, PraisonAIAgents, Memory

# Create writer with memory for quality tracking

blog_writer = Agent(
 name="Blog Writer",
 role="Professional blogger",
 goal="Create engaging, high-quality blog posts",
 backstory="Expert writer with years of experience",
 memory=Memory()
)

# Define quality task

blog_task = Task(
 name="Write blog post",
 description="Write a 1000-word blog post about 'The Future of Remote Work'",
 expected_output="""A complete blog post with:
- Catchy title
- Introduction (150-200 words)
- 3-4 main sections with subheadings
- Conclusion (150-200 words)
- Call to action""",
 agent=blog_writer,
 quality_check=True
)

# Run task

workflow = PraisonAIAgents(
 agents=[blog_writer],
 tasks=[blog_task]
)

result = workflow.start()

# Access quality information

task_result = result['task_results']['Write blog post']
print(f"Quality Score: {task_result.metadata.get('quality_score', 0):.2f}")
print(f"Stored in Memory: {task_result.metadata.get('stored_in_memory', False)}")

# High-quality outputs can be retrieved later

if task_result.metadata.get('stored_in_memory'):
 # Search for similar high-quality content

 similar_posts = blog_writer.memory.search_long_term(
 "blog posts about remote work",
 relevance_cutoff=0.7
 )
 print(f"Found {len(similar_posts)} similar high-quality posts")
```

### Example 2: Code Generation with Quality Metrics

```python
from praisonaiagents import Agent, Task, PraisonAIAgents, Memory

# Code generator with quality tracking

code_agent = Agent(
 name="Code Generator",
 role="Senior developer",
 goal="Write clean, efficient, well-documented code",
 llm="gpt-4", # Better model for code generation

 memory=Memory(provider="chroma")
)

# Multiple tasks with quality checking

tasks = [
 Task(
 name="API endpoint",
 description="Create a REST API endpoint for user authentication",
 expected_output="Complete Python code with Flask, including error handling and documentation",
 agent=code_agent,
 quality_check=True
 ),
 Task(
 name="Unit tests",
 description="Write comprehensive unit tests for the authentication endpoint",
 expected_output="Pytest test cases covering all scenarios",
 agent=code_agent,
 quality_check=True
 ),
 Task(
 name="Documentation",
 description="Write API documentation for the authentication endpoint",
 expected_output="OpenAPI/Swagger documentation with examples",
 agent=code_agent,
 quality_check=True
 )
]

# Run all tasks

workflow = PraisonAIAgents(
 agents=[code_agent],
 tasks=tasks,
 process="sequential"
)

results = workflow.start()

# Analyze quality across tasks

quality_report = {}
for task_name, result in results['task_results'].items():
 quality_report[task_name] = {
 'score': result.metadata.get('quality_score', 0),
 'stored': result.metadata.get('stored_in_memory', False)
 }

print("Quality Report:")
for task, metrics in quality_report.items():
 print(f"- {task}: , ")

# Calculate average quality

avg_quality = sum(m['score'] for m in quality_report.values()) / len(quality_report)
print(f"\nAverage Quality Score: {avg_quality:.2f}")
```

### Example 3: Research with Quality Filtering

```python
from praisonaiagents import Agent, Task, PraisonAIAgents, Memory
from typing import List, Dict

# Research agent with quality memory

researcher = Agent(
 name="Research Analyst",
 role="Research specialist",
 goal="Conduct thorough research with verified sources",
 tools=["web_search", "pdf_reader"],
 memory=Memory(provider="chroma")
)

# Custom quality checker for research

def research_quality_check(output: str) -> float:
 """Custom quality scoring for research outputs"""
 score = 0.0

 # Check for citations

 if "References:" in output or "[1]" in output:
 score += 0.3

 # Check for multiple sources

 source_indicators = ["according to", "study shows", "research indicates"]
 sources_found = sum(1 for indicator in source_indicators if indicator in output.lower())
 score += min(0.3, sources_found * 0.1)

 # Check for balanced perspective

 if "however" in output.lower() or "on the other hand" in output.lower():
 score += 0.2

 # Check for conclusion

 if "conclusion" in output.lower() or "in summary" in output.lower():
 score += 0.2

 return min(1.0, score)

# Research task with custom quality

research_task = Task(
 name="Market research",
 description="Research the electric vehicle market trends for 2024-2025",
 expected_output="Comprehensive research report with sources",
 agent=researcher,
 quality_check=True,
 quality_checker=research_quality_check # Custom quality function

)

# Execute research

workflow = PraisonAIAgents(
 agents=[researcher],
 tasks=[research_task]
)

result = workflow.start()

# Only high-quality research is stored

if result['task_results']['Market research'].metadata.get('stored_in_memory'):
 print("High-quality research stored for future reference")

 # Retrieve high-quality research on similar topics

 related_research = researcher.memory.search_long_term(
 "electric vehicle market analysis",
 relevance_cutoff=0.8,
 limit=5
 )

 print(f"\nFound {len(related_research)} related high-quality research pieces")
```

### Example 4: Iterative Quality Improvement

```python
from praisonaiagents import Agent, Task, PraisonAIAgents, Memory

# Agent that improves based on quality feedback

learning_agent = Agent(
 name="Learning Writer",
 role="Adaptive content creator",
 goal="Continuously improve content quality",
 memory=Memory(provider="chroma")
)

# Task that uses previous high-quality examples

def create_improved_task(topic: str, previous_score: float = 0.0):
 instruction = f"Write about {topic}."

 if previous_score > 0:
 instruction += f" Previous attempt scored {previous_score:.2f}."

 # Retrieve high-quality examples

 examples = learning_agent.memory.search_long_term(
 f"high quality content about {topic}",
 relevance_cutoff=0.8,
 limit=2
 )

 if examples:
 instruction += " Use these high-quality examples as reference:"
 for ex in examples:
 instruction += f"\n- {ex['memory'][:100]}..."

 return Task(
 name=f"Write about {topic}",
 description=instruction,
 expected_output="High-quality article",
 agent=learning_agent,
 quality_check=True
 )

# Iterative improvement loop

topics = ["AI Ethics", "Quantum Computing", "Climate Technology"]
quality_history = {}

for topic in topics:
 print(f"\nWorking on: {topic}")

 # Get previous score if exists

 prev_score = quality_history.get(topic, 0.0)

 # Create task with context

 task = create_improved_task(topic, prev_score)

 # Execute

 workflow = PraisonAIAgents(
 agents=[learning_agent],
 tasks=[task]
 )

 result = workflow.start()

 # Track quality

 score = result['task_results'][f'Write about {topic}'].metadata.get('quality_score', 0)
 quality_history[topic] = score

 print(f"Quality Score: {score:.2f}")

 # Retry if quality is low

 if score threshold
)
```

### Memory Configuration for Quality

```python
# Configure memory for quality-based storage

memory = Memory()
```

## Quality Score Calculation

The default quality scoring considers:
1. **Task Completion** (40%)
- Does output match expected format?
- Are requirements met?
2. **Content Quality** (30%)
- Grammar and coherence
- Logical flow
- Appropriate length
3. **Relevance** (20%)
- On-topic content
- Addresses the prompt
4. **Creativity/Insight** (10%)
- Novel approaches
- Valuable insights

### Custom Quality Functions

```python
def custom_quality_scorer(task_output) -> float:
 """
 Custom quality scoring logic
 Returns: float between 0.0 and 1.0
 """
 score = 0.0
 content = task_output.raw

 # Your custom scoring logic

 if len(content) > 1000:
 score += 0.2

 if "conclusion" in content.lower():
 score += 0.1

 # ... more criteria

 return min(1.0, score)

# Use in task

task = Task(
 name="Custom quality task",
 quality_check=True,
 quality_checker=custom_quality_scorer
)
```

## Best Practices

## Quality Metrics Dashboard

```python
from datetime import datetime, timedelta
import pandas as pd

def analyze_quality_trends(agent: Agent, days: int = 7):
 """Analyze quality trends over time"""

 # Retrieve recent tasks with quality scores

 end_date = datetime.now()
 start_date = end_date - timedelta(days=days)

 # Get quality data from memory

 quality_data = []

 # Search for recent outputs

 recent_outputs = agent.memory.search_long_term(
 f"outputs from {agent.name}",
 limit=100
 )

 for output in recent_outputs:
 metadata = output.get('metadata', {})
 if 'quality_score' in metadata:
 quality_data.append({
 'timestamp': metadata.get('timestamp', ''),
 'task': metadata.get('task_name', ''),
 'score': metadata['quality_score'],
 'stored': metadata.get('stored_in_memory', False)
 })

 # Create DataFrame for analysis

 df = pd.DataFrame(quality_data)

 if not df.empty:
 print(f"Quality Analysis for {agent.name}")
 print("-" * 50)
 print(f"Average Quality Score: {df['score'].mean():.2f}")
 print(f"Highest Score: {df['score'].max():.2f}")
 print(f"Lowest Score: {df['score'].min():.2f}")
 print(f"Storage Rate: {df['stored'].mean()*100:.1f}%")

 # Group by task

 task_quality = df.groupby('task')['score'].agg(['mean', 'count'])
 print("\nQuality by Task:")
 print(task_quality.sort_values('mean', ascending=False))

 return df
```

## Troubleshooting

## Next Steps