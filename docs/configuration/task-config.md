# Task Configuration

This page provides comprehensive documentation for task configuration parameters, including task types, conditional execution, task chaining, and workflow control.

## Core Task Parameters

### Task Type Configuration

#### `task_type`

- **Type**: `str`
- **Default**: `"normal"`
- **Options**: `"normal"`, `"decision"`, `"loop"`, `"parallel"`, `"sequential"`
- **Description**: Defines the execution behavior and flow control of the task

The `task_type` parameter determines how a task is executed within the workflow and how it interacts with other tasks.

```python
from praisonaiagents import Task

# Normal task - standard execution

task = Task(
 description="Analyze data",
 task_type="normal",
 expected_output="Data analysis report"
)

# Decision task - conditional branching

decision_task = Task(
 description="Evaluate results",
 task_type="decision",
 ,
 next_tasks=["success_task", "retry_task"]
)
```

### Task Type Details

#### Normal Tasks

Standard tasks that execute sequentially with defined inputs and outputs.

```python
normal_task = Task(
 name="data_processing",
 description="Process customer data",
 task_type="normal",
 agent=data_agent,
 expected_output="Processed data summary"
)
```

#### Decision Tasks

Tasks that evaluate conditions and determine workflow paths.

```python
decision_task = Task(
 name="quality_check",
 description="Check data quality",
 task_type="decision",
 ,
 {"field": "accuracy", "operator": ">=", "value": 0.98}
 ]
 },

)
```

#### Loop Tasks

Tasks that repeat based on conditions or iterations.

```python
loop_task = Task(
 name="iterative_improvement",
 description="Improve results iteratively",
 task_type="loop",
 ,
 "increment_field": "iteration_count"
 }
)
```

### Workflow Control Parameters

#### `condition`

- **Type**: `Dict[str, Any]`
- **Default**: `None`
- **Description**: Defines conditions for task execution or branching

Complex condition structures for advanced workflow control:

```python
# Simple condition

simple_condition = {
 "field": "status",
 "operator": "==",
 "value": "completed"
}

# Compound condition with AND/OR logic

compound_condition = {
 "type": "compound",
 "operator": "OR",
 "conditions": [
 {
 "type": "compound",
 "operator": "AND",
 "conditions": [
 {"field": "priority", "operator": "==", "value": "high"},
 {"field": "deadline", "operator": "", "value": 0.01}
 ]
 }
 },
 "updates": {
 "iteration": {"operation": "increment", "value": 1},
 "history": {"operation": "append", "field": "current_score"},
 "best_score": {"operation": "max", "field": "current_score"}
 }
 }
)
```

## Configuration Best Practices

### Task Naming Conventions

```python
# Good naming practices

task = Task(
 name="validate_user_input", # Descriptive, action-oriented

 description="Validate and sanitize user-provided data",

)
```

### Performance Optimization

```python
# Optimized task configuration

optimized_task = Task(
 name="batch_processor",

)
```

## Environment Variables

Task behavior can be configured via environment variables:

```bash
# Task execution settings

export PRAISONAI_TASK_DEFAULT_TIMEOUT=300
export PRAISONAI_TASK_MAX_RETRIES=3
export PRAISONAI_TASK_RETRY_DELAY=5

# Workflow settings

export PRAISONAI_WORKFLOW_MAX_DEPTH=10
export PRAISONAI_WORKFLOW_PARALLEL_LIMIT=5

# Debug settings

export PRAISONAI_TASK_DEBUG=true
export PRAISONAI_TASK_TRACE_EXECUTION=true
```

## Validation Rules

### Parameter Constraints

| Parameter | Validation | Constraints |
|-----------|------------|-------------|
| `task_type` | Must be valid type | One of: normal, decision, loop, parallel, sequential |
| `condition` | Valid structure | Must have field, operator, value or be compound |
| `next_tasks` | Valid task names | Tasks must exist in workflow |
| `is_start` | Boolean | At least one task must be marked as start |

### Condition Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `==` | Equals | `{"field": "status", "operator": "==", "value": "done"}` |
| `!=` | Not equals | `{"field": "type", "operator": "!=", "value": "error"}` |
| `>`, `", "value": 0.8}` |
| `>=`, `=", "value": 10}` |
| `in` | Contains | `{"field": "category", "operator": "in", "value": ["A", "B"]}` |
| `regex` | Pattern match | `{"field": "email", "operator": "regex", "value": ".*@company.com"}` |

## See Also

- [Agent Configuration](/configuration/agent-config) - Agent parameter reference
- [Workflow Patterns](/concepts/process) - Advanced workflow design
- [Best Practices](/configuration/best-practices) - Configuration guidelines