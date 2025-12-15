# Gemini Internal Tools

## Overview

Google Gemini provides three powerful internal tools that are natively supported by the model without requiring external implementations. These tools can be used directly through PraisonAI's tool system.

## Available Internal Tools

## Quick Start

## Individual Tool Examples

### Google Search Tool

Use Google Search for real-time information retrieval:

```python
from praisonaiagents import Agent

search_agent = Agent(
 instructions="You are a research assistant with web search capabilities",
 llm="gemini/gemini-2.0-flash",
 tools=[{"googleSearch": {}}]
)

response = search_agent.start("What are the latest developments in quantum computing?")
print(response)
```

### URL Context Tool

Analyze content from specific web pages:

```python
from praisonaiagents import Agent

url_agent = Agent(
 instructions="You are a content analyzer that can read and summarize web pages",
 llm="gemini/gemini-2.0-flash",
 tools=[{"urlContext": {}}]
)

response = url_agent.start("Summarize this article: https://example.com/article")
print(response)
```

### Code Execution Tool

Execute Python code for calculations and data analysis:

```python
from praisonaiagents import Agent

code_agent = Agent(
 instructions="You are a data analyst that can execute Python code",
 llm="gemini/gemini-2.0-flash",
 tools=[{"codeExecution": {}}]
)

response = code_agent.start("Calculate the compound interest for $10,000 at 5% annual rate for 10 years")
print(response)
```

## Combined Tools Example

Use multiple internal tools together for complex tasks:

```python
from praisonaiagents import Agent

research_agent = Agent(
 instructions="""You are an advanced research assistant that can:
1. Search the web for information
2. Analyze content from specific URLs
3. Execute Python code for data analysis""",
 llm="gemini/gemini-2.0-flash",
 tools=[
 {"googleSearch": {}},
 {"urlContext": {}},
 {"codeExecution": {}}
 ]
)

# Complex research task

response = research_agent.start("""
Research the current stock price of Apple (AAPL),
find recent news about the company,
and calculate its P/E ratio if the EPS is $6.15
""")
print(response)
```

## Multi-Agent System with Internal Tools

Create a multi-agent system where different agents use different internal tools:

```python
from praisonaiagents import Agent, Task, PraisonAIAgents

# Research agent with Google Search

researcher = Agent(
 name="Researcher",
 role="Web Research Specialist",
 goal="Find accurate and up-to-date information",
 instructions="Search for comprehensive information on topics",
 llm="gemini/gemini-2.0-flash",
 tools=[{"googleSearch": {}}]
)

# Analyst agent with Code Execution

analyst = Agent(
 name="Analyst",
 role="Data Analyst",
 goal="Analyze data and perform calculations",
 instructions="Perform data analysis and calculations",
 llm="gemini/gemini-2.0-flash",
 tools=[{"codeExecution": {}}]
)

# Content agent with URL Context

content_analyzer = Agent(
 name="ContentAnalyzer",
 role="Content Analysis Specialist",
 goal="Extract and summarize content from URLs",
 instructions="Analyze and summarize web content",
 llm="gemini/gemini-2.0-flash",
 tools=[{"urlContext": {}}]
)

# Define tasks

research_task = Task(
 description="Search for information about renewable energy trends in 2024",
 expected_output="List of key trends with sources",
 agent=researcher
)

analysis_task = Task(
 description="Calculate the growth rate of solar energy adoption based on the research",
 expected_output="Growth rate calculation with visualization",
 agent=analyst
)

content_task = Task(
 description="Analyze this article: https://example.com/renewable-energy-report",
 expected_output="Summary of key points from the article",
 agent=content_analyzer
)

# Create and run the multi-agent system

agents = PraisonAIAgents(
 agents=[researcher, analyst, content_analyzer],
 tasks=[research_task, analysis_task, content_task],
 process="sequential"
)

agents.start()
```

## Mixing Internal and External Tools

Combine Gemini's internal tools with custom external tools:

```python
from praisonaiagents import Agent

def custom_calculator(expression: str) -> str:
 """Custom calculator function"""
 import ast
 import operator

 ops = {
 ast.Add: operator.add,
 ast.Sub: operator.sub,
 ast.Mult: operator.mul,
 ast.Div: operator.truediv,
 ast.Pow: operator.pow,
 ast.USub: operator.neg,
 }

 def eval_expr(node):
 if isinstance(node, ast.Constant):
 return node.value
 elif isinstance(node, ast.BinOp):
 return ops[type(node.op)](eval_expr(node.left), eval_expr(node.right))
 elif isinstance(node, ast.UnaryOp):
 return ops[type(node.op)](eval_expr(node.operand))
 else:
 raise TypeError(f"Unsupported operation: {type(node)}")

 try:
 return str(eval_expr(ast.parse(expression, mode='eval').body))
 except Exception as e:
 return f"Error: {e}"

# Agent with both internal and external tools

hybrid_agent = Agent(
 instructions="You are a versatile assistant with multiple capabilities",
 llm="gemini/gemini-2.0-flash",
 tools=[
 {"googleSearch": {}}, # Internal tool

 {"codeExecution": {}}, # Internal tool

 custom_calculator # External tool

 ]
)

response = hybrid_agent.start("Search for the current Bitcoin price and calculate 15% of $10,000")
print(response)
```

## How It Works

### Tool Definition Syntax

Gemini internal tools use a special dictionary syntax:

```python
# Internal tool format

tools=[{"toolName": {}}]

# Multiple internal tools

tools=[
 {"googleSearch": {}},
 {"urlContext": {}},
 {"codeExecution": {}}
]
```

### Integration Flow

1. **Tool Definition**: Define tools using the special internal tool syntax
2. **Pass-Through**: PraisonAI passes these tools directly to LiteLLM
3. **Execution**: LiteLLM sends them to Gemini as internal tool configurations
4. **Results**: Gemini executes the tools natively and returns integrated responses

## Benefits of Internal Tools

## Best Practices

## Troubleshooting

## References

- [Gemini API: Google Search Grounding](https://ai.google.dev/gemini-api/docs/google-search)
- [Gemini API: URL Context](https://ai.google.dev/gemini-api/docs/url-context)
- [Gemini API: Code Execution](https://ai.google.dev/gemini-api/docs/code-execution)
- [LiteLLM Gemini Provider Documentation](https://docs.litellm.ai/docs/providers/gemini)

## Related Documentation