# Data Analyst Agent

```mermaid
flowchart LR
 In[Data Source] --> Reader[Data Reader]
 Reader --> Analyzer[Data Analyzer]
 Analyzer --> Generator[Insights Generator]
 Generator --> Out[Output]

 style In fill:#8B0000,color:#fff
 style Reader fill:#2E8B57,color:#fff
 style Analyzer fill:#2E8B57,color:#fff
 style Generator fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow demonstrating how the Data Analyst Agent can read data from various sources, analyze it, and generate insights.

## Quick Start

## Understanding Data Analysis Workflow

The Data Analyst Agent is designed to perform comprehensive data analysis tasks using a suite of specialized tools. Here's how it works:
1. **Data Reading**: The agent can read data from various sources:
- CSV files using `read_csv`
- Excel files using `read_excel`
2. **Data Analysis**: Multiple analysis tools are available:
- `filter_data`: Filter datasets based on conditions
- `get_summary`: Generate statistical summaries
- `group_by`: Group data by specific columns
- `pivot_table`: Create pivot tables for analysis
3. **Data Export**: Results can be exported to:
- CSV format using `write_csv`
- Excel format using `write_excel`

## Features

## Example Usage

```python
# Example: Analyzing stock data

agent.start("""
1. Read 'stock_data.csv'
2. Filter data for the last 30 days
3. Calculate daily returns
4. Generate summary statistics
5. Export results to 'analysis_results.xlsx'
""")
```

## Next Steps

- Learn about [Prompt Chaining](/features/promptchaining) for complex analysis workflows
- Explore [Evaluator Optimizer](/features/evaluator-optimiser) for improving analysis accuracy
- Check out other specialized agents like the [Finance Agent](/agents/finance) for specific use cases