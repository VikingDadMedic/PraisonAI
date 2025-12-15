# Agentic Autonomous Workflow

```mermaid
flowchart LR
 Human[Human] LLM[LLM Call]
 LLM -->|ACTION| Environment[Environment]
 Environment -->|FEEDBACK| LLM
 LLM --> Stop[Stop]

 style Human fill:#8B0000,color:#fff
 style LLM fill:#2E8B57,color:#fff
 style Environment fill:#8B0000,color:#fff
 style Stop fill:#333,color:#fff
```

An agent-based workflow where LLMs act autonomously within a loop, interacting with their environment and receiving feedback to refine their actions and decisions.

## Quick Start

## Understanding Autonomous Workflow

## Features

## Configuration Options

```python
# Create a monitor agent

monitor = Agent(
 name="Environment Monitor",
 role="State analyzer",
 goal="Monitor and analyze state",
 tools=[get_environment_state], # Environment monitoring tools

 verbose=True # Enable detailed logging

)

# Create an action agent

action = Agent(
 name="Action Executor",
 role="Action performer",
 goal="Execute appropriate actions",
 tools=[perform_action] # Action execution tools

)

# Create monitoring task

monitor_task = Task(
 name="monitor_environment",
 description="Monitor environment state",
 agent=monitor,
 is_start=True,
 task_type="decision",

)

# Create feedback loop task

feedback_task = Task(
 name="process_feedback",
 description="Process and adapt",
 agent=feedback_agent,
 next_tasks=["monitor_environment"], # Create feedback loop

 context=[monitor_task, action_task] # Access history

)
```

## Troubleshooting

## Next Steps