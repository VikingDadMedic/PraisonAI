# Reasoning Agents

```mermaid
flowchart LR
 In[In] --> Agent[AI Agent]
 Agent --> Think[Think]
 Think --> Decide[Decide]
 Decide --> Act[Act]
 Act --> Out[Out]

 style In fill:#8B0000,color:#fff
 style Agent fill:#2E8B57,color:#fff
 style Think fill:#2E8B57,color:#fff
 style Decide fill:#2E8B57,color:#fff
 style Act fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

Learn how to create AI agents with advanced reasoning capabilities for complex problem-solving.

## Quick Start

## Understanding Reasoning

## Features

## Multi-Agent Reasoning

### Configuration Options

```python
# Create an agent with reasoning configuration

agent = Agent(
 role="Problem Solver",
 goal="Solve complex problems",
 backstory="Expert in problem-solving",
 tools=[Tools.internet_search],
 verbose=True, # Enable detailed logging

 llm="gpt-4o" # Language model to use

)

# Task with reasoning requirements

task = Task(
 description="Solve complex problem",
 expected_output="Detailed solution",
 agent=agent
)
```

## Troubleshooting

## Next Steps