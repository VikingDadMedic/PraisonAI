# XML Agent

## XML Tools

Use XML Tools to read, write, and manipulate XML data with AI agents.

## Understanding XML Tools

## Key Components

## Examples

### Basic XML Processing Agent

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
from praisonaiagents.tools import read_xml, write_xml, transform_xml, validate_xml, xml_to_dict, dict_to_xml

# Create XML agent

xml_agent = Agent(
 name="XMLExpert",
 role="XML Processing Specialist",
 goal="Process XML files efficiently and accurately.",
 backstory="Expert in XML file handling and validation.",
 tools=[read_xml, write_xml, transform_xml, validate_xml, xml_to_dict, dict_to_xml],
 self_reflect=False
)

# Define XML task

xml_task = Task(
 description="Parse and validate XML configurations.",
 expected_output="Processed and validated XML data.",
 agent=xml_agent,
 name="config_processing"
)

# Run agent

agents = PraisonAIAgents(
 agents=[xml_agent],
 tasks=[xml_task],
 process="sequential"
)
agents.start()
```

### Advanced XML Processing with Multiple Agents

```python
# Create XML processing agent

processor_agent = Agent(
 name="Processor",
 role="XML Processor",
 goal="Process XML files systematically.",
 tools=[read_xml, write_xml, transform_xml],
 self_reflect=False
)

# Create validation agent

validator_agent = Agent(
 name="Validator",
 role="Data Validator",
 goal="Validate XML structure and content.",
 backstory="Expert in data validation and verification.",
 tools=[validate_xml, xml_to_dict, dict_to_xml],
 self_reflect=False
)

# Define tasks

processing_task = Task(
 description="Process and transform XML configurations.",
 agent=processor_agent,
 name="xml_processing"
)

validation_task = Task(
 description="Validate processed XML data.",
 agent=validator_agent,
 name="data_validation"
)

# Run agents

agents = PraisonAIAgents(
 agents=[processor_agent, validator_agent],
 tasks=[processing_task, validation_task],
 process="sequential"
)
agents.start()
```

## Best Practices

## Common Patterns

### XML Processing Pipeline

```python
# Processing agent

processor = Agent(
 name="Processor",
 role="XML Processor",
 tools=[read_xml, write_xml, transform_xml]
)

# Validation agent

validator = Agent(
 name="Validator",
 role="Data Validator",
 tools=[validate_xml, xml_to_dict, dict_to_xml]
)

# Define tasks

process_task = Task(
 description="Process XML files",
 agent=processor
)

validate_task = Task(
 description="Validate processed data",
 agent=validator
)

# Run workflow

agents = PraisonAIAgents(
 agents=[processor, validator],
 tasks=[process_task, validate_task]
)