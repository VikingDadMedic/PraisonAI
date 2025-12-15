# Task Validation & Feedback

Task validation in PraisonAI Agents ensures output quality through guardrails and decision-based feedback systems. When validation fails, tasks automatically retry with contextual feedback about what went wrong.

## Quick Start

## Validation Methods

PraisonAI offers two primary validation approaches:

### 1. Guardrails (Recommended)

Guardrails provide inline validation with automatic retry and feedback mechanisms.

#### Function-Based Guardrails

```python
from typing import Tuple, Any

def validate_output(task_output) -> Tuple[bool, Any]:
 """
 Validate task output
 Returns: (is_valid, feedback_or_output)
 """
 content = task_output.raw

 # Your validation logic

 if len(content) > 100:
 return True, task_output
 else:
 return False, "Content too short, needs at least 100 characters"

task = Task(
 description="Generate detailed analysis",
 agent=agent,
 guardrail=validate_output,
 max_retries=3
)
```

#### LLM-Based Guardrails

For complex validation that requires understanding:

```python
writer = Agent(
 name="Writer",
 role="Content creator",
 goal="Write high-quality content",
 llm="gpt-4" # Required for LLM guardrails

)

task = Task(
 description="Write a technical blog post about quantum computing",
 expected_output="Technical blog post",
 agent=writer,
 guardrail="Validate that the blog post: 1) Is technically accurate, 2) Contains at least 3 code examples, 3) Has proper introduction and conclusion sections",
 max_retries=2
)
```

### 2. Decision-Based Validation Workflows

For complex validation flows with multiple validators:

```python
from praisonaiagents import Agent, Task, PraisonAIAgents

# Create agents

writer = Agent(
 name="Writer",
 role="Content creator",
 goal="Write content"
)

validator = Agent(
 name="Quality Checker",
 role="Content validator",
 goal="Ensure content meets requirements"
)

# Writing task

write_task = Task(
 name="write_article",
 description="Write a 500-word article about AI ethics",
 expected_output="500-word article",
 agent=writer,
 is_start=True,
 next_tasks=["validate_article"]
)

# Validation task (decision type)

validate_task = Task(
 name="validate_article",
 description="Check if article is exactly 500 words and covers key ethical points. Respond with 'valid' if correct, 'retry' with specific feedback if not.",
 expected_output="Validation decision with feedback",
 agent=validator,
 task_type="decision",

)

# Create workflow

workflow = PraisonAIAgents(
 agents=[writer, validator],
 ,
 process="workflow"
)

result = workflow.start()
```

## How Validation Feedback Works

When validation fails, the system automatically:
1. **Captures the validation feedback** including:
- The validation decision (e.g., "retry", "invalid")
- Detailed feedback about what was wrong
- The original output that failed
- Which validator made the decision
2. **Passes feedback to retry task** via context:
 ```python
 # The retry task receives:

 task.validation_feedback = {
 "validation_response": "retry",
 "validation_details": "Article has 450 words, needs exactly 500",
 "rejected_output": "The original article text...",
 "validator_task": "validate_article",
 "validated_task": "write_article"
 }
 ```
3. **Includes feedback in task context** for the next attempt

## Complete Examples

### Example 1: Data Validation Pipeline

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
import json

# Validation function for JSON data

def validate_json_schema(task_output) -> Tuple[bool, Any]:
 try:
 data = json.loads(task_output.raw)

 # Check required fields

 required_fields = ["name", "email", "age"]
 missing_fields = [f for f in required_fields if f not in data]

 if missing_fields:
 return False, f"Missing required fields: {', '.join(missing_fields)}"

 # Validate data types

 if not isinstance(data["age"], int) or data["age"] Tuple[bool, Any]:
 content = task_output.raw
 missing_requirements = []

 for req in self.requirements:
 if req.lower() not in content.lower():
 missing_requirements.append(req)

 if missing_requirements:
 feedback = f"Missing requirements: {', '.join(missing_requirements)}"
 return False, feedback

 return True, task_output

# Create agent

analyst = Agent(
 name="Business Analyst",
 role="Requirements analyst",
 goal="Create comprehensive requirement documents"
)

# Define requirements

requirements = [
 "user authentication",
 "data encryption",
 "audit logging",
 "error handling",
 "performance metrics"
]

# Create validator instance

validator = RequirementsValidator(requirements)

# Task with custom validator

requirements_task = Task(
 description="Write security requirements for the new banking application",
 expected_output="Comprehensive security requirements document",
 agent=analyst,
 guardrail=validator.validate,
 max_retries=3
)

# Run task

agents = PraisonAIAgents(
 agents=[analyst],
 tasks=[requirements_task]
)

result = agents.start()
```

## Validation Feedback in Action

When validation fails, agents receive detailed feedback:

```python
# Example of validation feedback structure

{
 "validation_response": "retry",
 "validation_details": {
 "reason": "Article word count is 450, expected 500",
 "suggestions": "Add 50 more words to meet requirement",
 "failed_criteria": ["word_count"]
 },
 "rejected_output": "The original article content...",
 "validator_task": "validate_article",
 "validated_task": "write_article",
 "retry_count": 1
}
```

## Best Practices

## Advanced Configuration

### Retry Strategies

```python
# Configure retry behavior

task = Task(
 description="Generate report",
 agent=agent,
 guardrail=validate_report,
 max_retries=3,
 retry_strategy="exponential", # or "linear"

 retry_delay=2 # seconds between retries

)
```

### Custom Feedback Formatting

```python
def validate_with_detailed_feedback(task_output) -> Tuple[bool, Any]:
 # Perform validation

 issues = []

 if len(task_output.raw) < 100:
 issues.append("Content too short")

 if "conclusion" not in task_output.raw.lower():
 issues.append("Missing conclusion section")

 if issues:
 feedback = {
 "status": "failed",
 "issues": issues,
 "suggestions": "Please address the above issues",
 "example": "See template for reference"
 }
 return False, json.dumps(feedback)

 return True, task_output
```

## Common Validation Patterns

### Word/Character Count

```python
def validate_length(min_words=100, max_words=500):
 def validator(task_output):
 word_count = len(task_output.raw.split())
 if min_words <= word_count <= max_words:
 return True, task_output
 else:
 return False, f"Word count {word_count} not in range {min_words}-{max_words}"
 return validator

task = Task(
 description="Write article",
 guardrail=validate_length(400, 600)
)
```

### Content Requirements

```python
def validate_content_includes(required_sections):
 def validator(task_output):
 content = task_output.raw.lower()
 missing = [s for s in required_sections if s.lower() not in content]

 if missing:
 return False, f"Missing sections: {', '.join(missing)}"
 return True, task_output
 return validator

task = Task(
 description="Write report",
 guardrail=validate_content_includes([
 "Executive Summary",
 "Methodology",
 "Results",
 "Conclusion"
 ])
)
```

## Troubleshooting

## Next Steps