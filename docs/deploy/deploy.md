# Deploying PraisonAI Agents as APIs

PraisonAI agents can be easily deployed as RESTful APIs, allowing you to integrate them into various applications and services. This guide covers how to deploy both single and multiple agents as APIs.

## Quick Start

## Making API Requests

Once your agent is deployed, you can make POST requests to interact with it:

```bash
curl -X POST http://localhost:3030/ask \
 -H "Content-Type: application/json" \
 -d '{"message": "What is artificial intelligence?"}'
```

The response will be in JSON format:

```json
{
 "response": "Artificial intelligence (AI) refers to the simulation of human intelligence in machines that are programmed to think and learn like humans. It encompasses various technologies including machine learning, natural language processing, computer vision, and robotics. AI systems can perform tasks that typically require human intelligence such as understanding natural language, recognizing patterns, solving problems, and making decisions."
}
```

## Multi-Agent API Deployment

You can deploy multiple agents on the same server, each with its own endpoint:

```python
from praisonaiagents import Agent

weather_agent = Agent(
 instructions="""You are a weather agent that can provide weather information for a given city.""",
 llm="gpt-4o-mini"
)

stock_agent = Agent(
 instructions="""You are a stock market agent that can provide information about stock prices and market trends.""",
 llm="gpt-4o-mini"
)

travel_agent = Agent(
 instructions="""You are a travel agent that can provide recommendations for destinations, hotels, and activities.""",
 llm="gpt-4o-mini"
)

weather_agent.launch(path="/weather", port=3030)
stock_agent.launch(path="/stock", port=3030)
travel_agent.launch(path="/travel", port=3030)
```

With this setup, you can access:
- Weather agent at `http://localhost:3030/weather`
- Stock agent at `http://localhost:3030/stock`
- Travel agent at `http://localhost:3030/travel`

## Production Deployment Options

For production environments, consider the following deployment options:

### Docker Deployment

### Cloud Deployment

#### Deploying to AWS

#### Deploying to Google Cloud Run

## API Configuration Options

When launching your agent as an API, you can customize various parameters:

```python
agent.launch(
 path="/custom-endpoint", # API endpoint path

 port=8080, # Port number

 host="0.0.0.0", # Host address (0.0.0.0 for external access)

 debug=True, # Enable debug mode

 cors_origins=["*"], # CORS configuration

 api_key="your-api-key" # Optional API key for authentication

)
```

## Securing Your API

For production deployments, consider implementing:
1. **API Key Authentication**: Require API keys for all requests
2. **Rate Limiting**: Limit the number of requests per client
3. **HTTPS**: Use SSL/TLS certificates for encrypted communication
4. **Input Validation**: Validate all input data before processing

## Monitoring and Scaling

For production environments, consider:
1. **Load Balancing**: Distribute traffic across multiple instances
2. **Auto-Scaling**: Automatically adjust resources based on demand
3. **Logging**: Implement comprehensive logging for debugging
4. **Monitoring**: Set up alerts for errors and performance issues

## Features