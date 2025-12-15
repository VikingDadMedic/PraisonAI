# Single Agent

```mermaid
flowchart LR
 In[Input] --> Agent[Single Agent]
 Agent --> Out[Output]

 style In fill:#8B0000,color:#fff
 style Agent fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow demonstrating how to create and use a simple single-purpose agent.

## Quick Start

## Understanding Single Agent

The Single Agent is the simplest form of PraisonAI agent:
1. **Simple Setup**: Minimal configuration required
2. **Single Purpose**: Focused on one specific task
3. **Direct Execution**: Straightforward input-output flow
4. **No Dependencies**: No external tools required

## Features

## Example Usage

```python
# Example: Create a content generation agent

from praisonaiagents import Agent

# Create content agent

agent = Agent(
 instructions="You are a Content Generation Agent, create engaging content"
)

# Generate content

response = agent.start("""
 Write a short story about:
- A future world
- With advanced AI
- And human collaboration
""")

# Save the story

with open('ai_story.txt', 'w') as f:
 f.write(response)
```

## Next Steps

- Learn about [Prompt Chaining](/features/promptchaining) for more complex workflows
- Explore [Evaluator Optimizer](/features/evaluator-optimiser) for improving output quality
- Check out other specialized agents like the [Markdown Agent](/agents/markdown) for specific use cases