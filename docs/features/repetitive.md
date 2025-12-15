# Repetitive Agents

```mermaid
flowchart LR
 In[Input] --> LoopAgent[("Looping Agent")]
 LoopAgent --> Task[Task]
 Task --> |Next iteration| LoopAgent
 Task --> |Done| Out[Output]

 style In fill:#8B0000,color:#fff
 style LoopAgent fill:#2E8B57,color:#fff,shape:circle
 style Task fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow optimization pattern where agents handle repetitive tasks through automated loops, processing multiple instances efficiently while maintaining consistency.

## Quick Start

## Understanding Repetitive Agents

## Features

## Loop Tasks with File Processing

Loop tasks can automatically process CSV and text files to create dynamic subtasks. This powerful feature enables batch processing of data without manual task creation.

### How File Processing Works

When a loop task has an `input_file` parameter:
1. The system reads the CSV file using Python's csv.reader
2. Each row becomes a separate subtask
3. Subtasks inherit properties from the parent loop task
4. Tasks are executed sequentially with proper context passing

### CSV File Format

Loop tasks support multiple CSV formats:

#### Simple Format

```csv
name,issue
John,Billing problem
Jane,Technical issue
Sarah,Password reset
```

#### Q&A Format

```csv
question,answer
What is 2+2?,4
What is the capital of France?,Paris
```

#### Single Column Format

```csv
task
Analyze customer feedback
Process refund request
Update user profile
```

### Complete Example: Customer Support System

```python
from praisonaiagents import Agent, Task, PraisonAIAgents

# Create a CSV file with customer issues

with open("customers.csv", "w") as f:
 f.write("name,issue\n")
 f.write("John,Billing problem with subscription\n")
 f.write("Jane,Technical issue with login\n")
 f.write("Sarah,Request for feature enhancement\n")

# Create specialized support agent

support_agent = Agent(
 name="Support Agent",
 role="Customer support specialist",
 goal="Resolve customer issues efficiently",
 backstory="Expert support agent with years of experience",
 llm="gpt-4o-mini" # Specify the LLM model

)

# Loop task automatically creates subtasks from CSV

loop_task = Task(
 name="Process all customers",
 description="Handle each customer issue",
 expected_output="Resolution for the customer issue",
 agent=support_agent,
 task_type="loop",
 input_file="customers.csv" # Automatically processes each row

)

# Use PraisonAIAgents with workflow process

agents = PraisonAIAgents(
 agents=[support_agent],
 tasks=[loop_task],
 process="workflow", # Required for loop tasks

 max_iter=10 # Prevent infinite loops

)

# Start processing

results = agents.start()

# Each row will be processed as:

# - Subtask 1: "Handle each customer issue: John,Billing problem with subscription"

# - Subtask 2: "Handle each customer issue: Jane,Technical issue with login"

# - Subtask 3: "Handle each customer issue: Sarah,Request for feature enhancement"

```

### Processing Text Files

Loop tasks can also process text files line by line:

```python
# Create a text file with URLs

with open("urls.txt", "w") as f:
 f.write("https://example.com\n")
 f.write("https://test.com\n")
 f.write("https://demo.com\n")

# Create URL analyzer agent

url_agent = Agent(
 name="URL Analyzer",
 role="Website analyzer",
 goal="Analyze websites for SEO and performance"
)

# Process each URL

url_task = Task(
 name="Analyze URLs",
 description="Analyze each website",
 expected_output="SEO and performance report",
 agent=url_agent,
 task_type="loop",
 input_file="urls.txt" # Each line becomes a subtask

)
```

### Advanced Features

#### Subtask Inheritance

Subtasks automatically inherit from the parent loop task:
- Agent assignment
- Expected output format
- Callbacks and hooks
- Task configuration

#### Context Passing

Each subtask receives:
- The specific row data
- Parent task context
- Previous subtask results (when sequential)

#### Error Handling

```python
# Loop tasks handle errors gracefully

loop_task = Task(
 name="Process data",
 description="Process each data entry",
 expected_output="Processed result",
 agent=agent,
 task_type="loop",
 input_file="data.csv",
 on_failure="continue" # Continue processing even if one row fails

)
```

### Best Practices

### Common Use Cases

1. **Customer Support**: Process support tickets from CSV
2. **Data Analysis**: Analyze multiple datasets sequentially
3. **Content Generation**: Create content for multiple topics
4. **URL Processing**: Analyze or scrape multiple websites
5. **Bulk Operations**: Update multiple records or entities

### Important Notes

=======
## Loop Tasks with File Input

Process batches of tasks from CSV or other structured files:

```python
from praisonaiagents import Agent, Task, PraisonAIAgents

# Create agent for processing questions

qa_agent = Agent(
 name="QA Bot",
 role="Answer questions",
 goal="Provide accurate answers to user questions"
)

# Create loop task that processes questions from CSV

loop_task = Task(
 name="process_questions",
 description="Answer each question",
 expected_output="Answer for each question",
 agent=qa_agent,
 task_type="loop",
 input_file="questions.csv" # Each row becomes a subtask

)

# Create workflow

agents = PraisonAIAgents(
 agents=[qa_agent],
 tasks=[loop_task],
 process="workflow" # Use workflow for loop tasks

)

# Run the batch processing

result = agents.start()
```

### CSV File Format

The input CSV file should have headers that correspond to task parameters:

```csv
question,context,priority
"What is Python?","Programming language context","high"
"Explain machine learning","AI and ML context","medium"
"How does Docker work?","Container technology context","high"
```

### Advanced File Processing

#### Processing with Multiple Columns

```python
# Agent that uses multiple CSV columns

analyzer = Agent(
 name="Data Analyzer",
 role="Analyze data entries",
 goal="Process and analyze each data entry"
)

# Task that uses multiple columns from CSV

analysis_task = Task(
 name="analyze_entries",
 description="Analyze data: {title} with context: {context}",
 expected_output="Analysis report for each entry",
 agent=analyzer,
 task_type="loop",
 input_file="data_entries.csv",
 # Map CSV columns to task parameters

)
```

#### Processing Different File Types

```python
# Define the processor agent

processor = Agent(
 name="DataProcessor",
 role="Data processing specialist",
 goal="Process various data formats efficiently"
)

# JSON file processing

json_task = Task(
 name="process_json_data",
 description="Process JSON entries",
 expected_output="Processed results",
 agent=processor,
 task_type="loop",
 input_file="data.json",
 file_format="json" # Specify file format

)

# Text file processing (one task per line)

text_task = Task(
 name="process_lines",
 description="Process text: {line}",
 expected_output="Processed line",
 agent=processor,
 task_type="loop",
 input_file="tasks.txt",
 file_format="text"
)
```

### Batch Processing Patterns

#### Parallel Processing

```python
# Configure parallel processing for better performance

agents = PraisonAIAgents(
 agents=[qa_agent],
 tasks=[loop_task],
 process="workflow",
 max_workers=5, # Process 5 items in parallel

 batch_size=10 # Process in batches of 10

)
```

#### Sequential Processing with Dependencies

```python
# Define the required agents

extractor = Agent(
 name="DataExtractor",
 role="Data extraction specialist",
 goal="Extract data from various sources"
)

transformer = Agent(
 name="DataTransformer",
 role="Data transformation expert",
 goal="Transform data to required format"
)

# First loop task processes data

extract_task = Task(
 name="extract_data",
 description="Extract data from {source}",
 expected_output="Extracted data",
 agent=extractor,
 task_type="loop",
 input_file="sources.csv"
)

# Second loop task uses results from first

transform_task = Task(
 name="transform_data",
 description="Transform extracted data",
 expected_output="Transformed data",
 agent=transformer,
 task_type="loop",
 depends_on=["extract_data"] # Uses output from extract_data

)
```

### Error Handling and Recovery

```python
# Define processor agent

processor = Agent(
 name="SafeProcessor",
 role="Error-tolerant processor",
 goal="Process items with error recovery"
)

# Configure error handling for batch processing

loop_task = Task(
 name="process_with_recovery",
 description="Process item safely",
 expected_output="Processed result",
 agent=processor,
 task_type="loop",
 input_file="items.csv",

)
```

### Progress Tracking

Monitor batch processing progress:

```python
from praisonaiagents.callbacks import Callback

class BatchProgressTracker(Callback):
 def __init__(self):
 self.processed = 0
 self.total = 0

 def on_task_start(self, task, **kwargs):
 if task.task_type == "loop" and self.total == 0:
 # Count total items

 import csv
 try:
 with open(task.input_file, 'r', encoding='utf-8') as f:
 # More efficient for counting lines in a CSV

 self.total = sum(1 for _ in f) - 1
 except FileNotFoundError:
 print(f"Warning: Input file not found at {task.input_file}. Progress will not be shown.")
 self.total = 0

 def on_subtask_complete(self, subtask, result, **kwargs):
 self.processed += 1
 print(f"Progress: {self.processed}/{self.total} ({self.processed/self.total*100:.1f}%)")

# Use progress tracker

agents = PraisonAIAgents(
 agents=[qa_agent],
 tasks=[loop_task],
 callbacks=[BatchProgressTracker()]
)
```

### Output Aggregation

Collect and aggregate results from loop tasks:

```python
# Define the summarizer agent

summarizer = Agent(
 name="Summarizer",
 role="Results aggregator",
 goal="Create comprehensive summaries from processed data"
)

# Task that aggregates all loop results

summary_task = Task(
 name="summarize_results",
 description="Create summary of all processed items",
 expected_output="Comprehensive summary report",
 agent=summarizer,
 depends_on=["process_questions"], # Depends on loop task

 aggregate_results=True # Receives all loop results

)

# Complete workflow

# Note: Assuming qa_agent and loop_task are defined from previous examples

agents = PraisonAIAgents(
 agents=[qa_agent, summarizer],
 tasks=[loop_task, summary_task],
 process="workflow"
)
```

### Best Practices

1. **File Validation**: Always validate input files before processing
```python
import os
import csv

def validate_input_file(filepath):
 if not os.path.exists(filepath):
 raise FileNotFoundError(f"Input file not found: {filepath}")

 with open(filepath, 'r') as f:
 reader = csv.reader(f)
 headers = next(reader, None)
 if not headers:
 raise ValueError("CSV file is empty or has no headers")

 return True
```
2. **Memory Management**: For large files, use streaming
```python
loop_task = Task(
 name="process_large_file",
 description="Process item",
 expected_output="Result",
 agent=processor,
 task_type="loop",
 input_file="large_data.csv",
 streaming=True, # Process one item at a time

 chunk_size=100 # Read 100 rows at a time

)
```
3. **Result Storage**: Save results progressively
```python
loop_task = Task(
 name="process_and_save",
 description="Process and save",
 expected_output="Saved result",
 agent=processor,
 task_type="loop",
 input_file="data.csv",
 output_file="results.csv", # Save results to file

 append_mode=True # Append results as processed

)
```

## Troubleshooting

## Next Steps