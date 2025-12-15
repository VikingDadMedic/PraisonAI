# CSV Agent

## CSV Tools

Use CSV Tools to read, write, and manipulate CSV files with AI agents.

## Understanding CSV Tools

## Key Components

## Available Functions

```python
from praisonaiagents.tools import read_csv
from praisonaiagents.tools import write_csv
from praisonaiagents.tools import merge_csv
```

## Function Details

### read_csv(filepath: str, encoding: str = 'utf-8', delimiter: str = ',', header: Union[int, List[int], None] = 0, usecols: Optional[List[str]] = None, dtype: Optional[Dict[str, str]] = None, parse_dates: Optional[List[str]] = None, na_values: Optional[List[str]] = None, nrows: Optional[int] = None)

Reads a CSV file with advanced options:
- Supports multiple encodings (utf-8, ascii, etc.)
- Configurable column delimiter
- Flexible header row handling
- Column selection and data type specification
- Date parsing
- Custom NA/NaN value handling
- Row limit option

```python
# Basic usage

data = read_csv("data.csv")

# Advanced usage with options

data = read_csv(
 "data.csv",
 encoding='utf-8',
 delimiter=',',
 header=0,
 usecols=['name', 'age', 'city'],
 ,
 parse_dates=['birth_date'],
 na_values=['N/A', 'missing'],
 nrows=1000
)
# Returns: List[Dict[str, Any]]

# Example: [{"name": "Alice", "age": 25, "city": "New York"}, ...]

```

### write_csv(filepath: str, data: List[Dict[str, Any]], encoding: str = 'utf-8', delimiter: str = ',', index: bool = False, header: bool = True, float_format: Optional[str] = None, date_format: Optional[str] = None, mode: str = 'w')

Writes data to a CSV file with formatting options:
- Supports different encodings
- Configurable delimiter
- Optional row indices
- Optional headers
- Custom float and date formatting
- Append mode support

```python
# Basic usage

data = [
 {"name": "Alice", "age": 25, "city": "New York"},
 {"name": "Bob", "age": 30, "city": "San Francisco"}
]
success = write_csv("output.csv", data)

# Advanced usage with formatting

success = write_csv(
 "output.csv",
 data,
 encoding='utf-8',
 delimiter=',',
 index=False,
 header=True,
 float_format='%.2f',
 date_format='%Y-%m-%d',
 mode='w'
)
# Returns: bool (True if successful)

```

### merge_csv(files: List[str], output_file: str, how: str = 'inner', on: Optional[Union[str, List[str]]] = None, suffixes: Optional[tuple] = None)

Merges multiple CSV files with flexible join options:
- Supports different join types (inner, outer, left, right)
- Merge on single or multiple columns
- Custom suffixes for overlapping columns
- Automatic handling of column name conflicts

```python
# Merge two files on a common column

success = merge_csv(
 files=["employees.csv", "salaries.csv"],
 output_file="merged.csv",
 how='inner',
 on='employee_id'
)

# Advanced merge with multiple columns and custom suffixes

success = merge_csv(
 files=["data1.csv", "data2.csv", "data3.csv"],
 output_file="merged.csv",
 how='outer',
 on=['id', 'department'],
 suffixes=('_2022', '_2023')
)
# Returns: bool (True if successful)

```

## Dependencies

The CSV tools require the following Python packages:
- pandas: For advanced CSV operations and data manipulation

These will be automatically installed when needed.

## Example Agent Configuration

```python
from praisonaiagents import Agent
from praisonaiagents.tools import read_csv, write_csv, merge_csv

agent = Agent(
 name="CSVProcessor",
 description="An agent that processes CSV files",
 tools=[read_csv, write_csv, merge_csv]
)
```

## Error Handling

All functions include comprehensive error handling:
- File not found errors
- Permission errors
- Encoding errors
- Data format errors
- Missing dependency errors

Errors are logged and returned in a consistent format:
- Success cases return the expected data type
- Error cases return a dict with an "error" key containing the error message

## Examples

### Basic CSV Processing Agent

```python
from praisonaiagents import Agent, Task, PraisonAIAgents
from praisonaiagents.tools import read_csv, write_csv, merge_csv

# Create CSV agent

csv_agent = Agent(
 name="CSVExpert",
 role="CSV Processing Specialist",
 goal="Process CSV files efficiently and accurately.",
 backstory="Expert in data processing and analysis.",
 tools=[read_csv, write_csv, merge_csv],
 self_reflect=False
)

# Define CSV task

csv_task = Task(
 description="Process and analyze customer data.",
 expected_output="Customer analysis report.",
 agent=csv_agent,
 name="customer_analysis"
)

# Run agent

agents = PraisonAIAgents(
 agents=[csv_agent],
 tasks=[csv_task],
 process="sequential"
)
agents.start()
```

### Advanced CSV Processing with Multiple Agents

```python
# Create data processing agent

processor_agent = Agent(
 name="Processor",
 role="Data Processor",
 goal="Process CSV data systematically.",
 tools=[read_csv, write_csv, merge_csv],
 self_reflect=False
)

# Create analysis agent

analysis_agent = Agent(
 name="Analyzer",
 role="Data Analyst",
 goal="Analyze processed CSV data.",
 backstory="Expert in data analysis and reporting.",
 self_reflect=False
)

# Define tasks

processing_task = Task(
 description="Process customer data files.",
 agent=processor_agent,
 name="data_processing"
)

analysis_task = Task(
 description="Analyze customer trends and patterns.",
 agent=analysis_agent,
 name="data_analysis"
)

# Run agents

agents = PraisonAIAgents(
 agents=[processor_agent, analysis_agent],
 tasks=[processing_task, analysis_task],
 process="sequential"
)
agents.start()
```

## Best Practices

## Common Patterns

### CSV Processing Pipeline

```python
# Processing agent

processor = Agent(
 name="Processor",
 role="CSV Processor",
 tools=[read_csv, write_csv, merge_csv]
)

# Analysis agent

analyzer = Agent(
 name="Analyzer",
 role="Data Analyst"
)

# Define tasks

process_task = Task(
 description="Process CSV files",
 agent=processor
)

analyze_task = Task(
 description="Analyze processed data",
 agent=analyzer
)

# Run workflow

agents = PraisonAIAgents(
 agents=[processor, analyzer],
 tasks=[process_task, analyze_task]
)