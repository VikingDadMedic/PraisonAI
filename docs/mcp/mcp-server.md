# Creating MCP Servers

This guide demonstrates how to create Model Context Protocol (MCP) servers using PraisonAI agents. MCP is a protocol that enables AI models to use tools and communicate with external systems in a standardized way.

## Single Agent MCP Server

The simplest way to create an MCP server is with a single agent. This approach is ideal for specialized tasks where you need just one agent with a specific capability.

## Multi-Agent MCP Server with Custom Tools

For more complex scenarios, you can create an MCP server with multiple agents and custom tools. This approach allows for collaborative problem-solving and specialized capabilities.

## Multi-Agent MCP Server (Simple)

For scenarios where you need multiple agents to collaborate without custom tools, you can create a simpler multi-agent MCP server:

```python
from praisonaiagents import Agent, Agents

agent = Agent(instructions="You Search the internet for information")
agent2 = Agent(instructions="You Summarise the information")

agents = Agents(agents=[agent, agent2])
agents.launch(port=8080, protocol="mcp")
```

This approach is ideal for cases where you want agents with different specializations to work together using their built-in capabilities.

## Connecting to MCP Servers

You can connect to MCP servers using various clients:

### Using PraisonAI Agents

```python
from praisonaiagents import Agent, MCP

client_agent = Agent(
 instructions="Use the MCP server to complete tasks",
 llm="gpt-4o-mini",
 tools=MCP("http://localhost:8080")
)

response = client_agent.start("Create a tweet about artificial intelligence")
print(response)
```

### Using JavaScript/TypeScript

```typescript
import { MCPClient } from '@modelcontextprotocol/client';

async function main() {
 const client = new MCPClient('http://localhost:8080');

 const response = await client.chat([
 { role: 'user', content: 'Create a tweet about artificial intelligence' }
 ]);

 console.log(response.choices[0].message.content);
}

main();
```

## Advanced Configuration

### Custom Port and Host

```python
agent.launch(port=9000, host="0.0.0.0", protocol="mcp")
```

### Authentication

```python
agent.launch(port=8080, protocol="mcp", api_key="your-secret-key")
```

### CORS Configuration

```python
agent.launch(port=8080, protocol="mcp", cors_origins=["https://yourdomain.com"])
```

## Deployment Options

For production deployments, consider:
1. **Docker Containerization**:
 ```dockerfile
 FROM python:3.11-slim

 WORKDIR /app

 COPY requirements.txt .
 RUN pip install --no-cache-dir -r requirements.txt

 COPY . .

 EXPOSE 8080

 CMD ["python", "simple-mcp-server.py"]
 ```
2. **Cloud Deployment**: Deploy to AWS, Google Cloud, or Azure using their container services.
3. **Kubernetes**: For scalable deployments, use Kubernetes to manage your MCP server containers.

## Security Considerations

1. **API Authentication**: Always use API keys in production
2. **Rate Limiting**: Implement rate limiting to prevent abuse
3. **Input Validation**: Validate all incoming requests
4. **HTTPS**: Use SSL/TLS for all production deployments
5. **Tool Permissions**: Limit what custom tools can access

## Features and Benefits

## Best Practices

1. **Agent Instructions**: Provide clear, specific instructions for each agent
2. **Tool Documentation**: Document your custom tools thoroughly
3. **Error Handling**: Implement robust error handling in your tools
4. **Monitoring**: Set up logging and monitoring for your MCP servers
5. **Testing**: Test your MCP servers thoroughly before deployment