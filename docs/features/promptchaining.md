# Agentic Prompt Chaining

```mermaid
flowchart LR
 In[In] --> LLM1[LLM Call 1] --> Gate{Gate}
 Gate -->|Pass| LLM2[LLM Call 2] -->|Output 2| LLM3[LLM Call 3] --> Out[Out]
 Gate -->|Fail| Exit[Exit]

 style In fill:#8B0000,color:#fff
 style LLM1 fill:#2E8B57,color:#fff
 style LLM2 fill:#2E8B57,color:#fff
 style LLM3 fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
 style Exit fill:#8B0000,color:#fff
```

A workflow where the output of one LLM call becomes the input for the next. This sequential design allows for structured reasoning and step-by-step task completion.

## Quick Start

## Understanding Prompt Chaining

## Features

## Configuration Options to exit the chain

```python
# Task with chaining configuration

task = Task(
 name="time_check",
 description="Check time and make decision",
 expected_output="Time check result",
 agent=agent,
 is_start=True,
 task_type="decision",
 next_tasks=["next_step"],

)
```

```python
task = Task(
 name="time_check",
 description="Check time and make decision",
 expected_output="Time check result",
 agent=agent,
 is_start=True,
 task_type="decision",
 next_tasks=["next_step"],

)
```

```python
task = Task(
 name="time_check",
 description="Check time and make decision",
 expected_output="Time check result",
 agent=agent,
 is_start=True,
 task_type="decision",
 next_tasks=["next_step"],

)
```

```python
task = Task(
 name="time_check",
 description="Check time and make decision",
 expected_output="Time check result",
 agent=agent,
 is_start=True,
 task_type="decision",
 next_tasks=["next_step"],

)
```

## Troubleshooting

## Next Steps