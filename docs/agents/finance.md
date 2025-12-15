# Finance Agent

```mermaid
flowchart LR
 In[Request] --> Price[Stock Price]
 Price --> Info[Stock Info]
 Info --> History[Historical Data]
 History --> Out[Investment Advice]

 style In fill:#8B0000,color:#fff
 style Price fill:#2E8B57,color:#fff
 style Info fill:#2E8B57,color:#fff
 style History fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow demonstrating how the Finance Agent can analyze stock market data and provide investment recommendations.

## Quick Start

## Understanding Financial Analysis

The Finance Agent specializes in stock market analysis and investment recommendations using three key components:
1. **Real-time Price Data**: Uses `get_stock_price` to fetch current market prices
2. **Company Information**: Retrieves company details using `get_stock_info`
3. **Historical Analysis**: Analyzes trends with `get_historical_data`

## Features

## Example Usage

```python
# Example: Analyzing specific stocks

agent.start("""
1. Get current prices for AAPL and GOOGL
2. Retrieve company information
3. Analyze 6-month historical data
4. Provide investment recommendations
""")
```

## Next Steps

- Learn about [Prompt Chaining](/features/promptchaining) for complex financial analysis
- Explore [Evaluator Optimizer](/features/evaluator-optimiser) for improving recommendation accuracy
- Check out the [Data Analyst Agent](/agents/data-analyst) for detailed data analysis capabilities