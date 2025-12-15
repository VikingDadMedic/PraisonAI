# Mini AI Agents

## Quick Start

Create multiple AI agents that can work together in just a few lines of code!

## Understanding Mini AI Agents

## Key Components

## Available Tools

## Custom Instructions

## Best Practices

## Common Patterns

### Research and Analysis

```python
# Research agent

researcher = Agent(
 instructions="Research latest developments in quantum computing",
 tools=[Tools.internet_search]
)

# Analysis agent

analyst = Agent(
 instructions="Analyze and explain the research findings in simple terms"
)

agents = Agents(agents=[researcher, analyst])
```

### Information Processing

```python
# Data collector

collector = Agent(
 instructions="Collect information about renewable energy",
 tools=[Tools.internet_search]
)

# Summarizer

summarizer = Agent(
 instructions="Create a concise summary of the collected information"
)

# Report writer

writer = Agent(
 instructions="Write a detailed report based on the summary"
)

agents = Agents(agents=[collector, summarizer, writer])
```

## Troubleshooting

## Next Steps