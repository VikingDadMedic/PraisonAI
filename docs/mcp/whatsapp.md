# WhatsApp MCP Integration

## Add WhatsApp Tool to AI Agent

```mermaid
flowchart LR
 In[In] --> Agent[AI Agent]
 Agent --> Tool[WhatsApp MCP]
 Tool --> Agent
 Agent --> Out[Out]

 style In fill:#8B0000,color:#fff
 style Agent fill:#2E8B57,color:#fff
 style Tool fill:#25D366,color:#fff
 style Out fill:#8B0000,color:#fff
```

## Quick Start

## Multi-Agent Integration

You can also combine WhatsApp with other MCP tools, such as Airbnb search:

```python
from praisonaiagents import Agent, Agents, MCP

airbnb_agent = Agent(
 instructions="""Search for Apartments in Paris for 2 nights on Airbnb. 04/28 - 04/30 for 2 adults""",
 llm="gpt-4o-mini",
 tools=MCP("npx -y @openbnb/mcp-server-airbnb --ignore-robots-txt")
)

whatsapp_agent = Agent(
 instructions="""Send AirBnb Search Result to 'Mervin Praison'""",
 llm="gpt-4o-mini",
 tools=MCP("python /path/to/whatsapp-mcp/whatsapp-mcp-server/main.py")
)

agents = Agents(agents=[airbnb_agent, whatsapp_agent])

agents.start()
```

## Alternative LLM Integrations

### Using Groq with WhatsApp

```python
from praisonaiagents import Agent, MCP

whatsapp_agent = Agent(
 instructions="Whatsapp Agent",
 llm="groq/llama-3.2-90b-vision-preview",
 tools=MCP("python /path/to/whatsapp-mcp/whatsapp-mcp-server/main.py")
)

whatsapp_agent.start("Send Hello to Mervin Praison")
```

### Using Ollama with WhatsApp

```python
from praisonaiagents import Agent, MCP

whatsapp_agent = Agent(
 instructions="Whatsapp Agent",
 llm="ollama/llama3.2",
 tools=MCP("python /path/to/whatsapp-mcp/whatsapp-mcp-server/main.py")
)

whatsapp_agent.start("Send Hello to Mervin Praison. Use send_message tool, recipient and message are the required parameters.")
```

## Gradio UI Integration

Create a Gradio UI for your WhatsApp and Airbnb integration:

```python
from praisonaiagents import Agent, Agents, MCP
import gradio as gr

def search_airbnb(query):
 airbnb_agent = Agent(
 instructions=query+" on Airbnb",
 llm="gpt-4o-mini",
 tools=MCP("npx -y @openbnb/mcp-server-airbnb --ignore-robots-txt")
 )

 whatsapp_agent = Agent(
 instructions="""Send AirBnb Search Result to 'Mervin Praison'. Don't include Phone Number in Response, but include the AirBnb Search Result""",
 llm="gpt-4o-mini",
 tools=MCP("python /path/to/whatsapp-mcp/whatsapp-mcp-server/main.py")
 )

 agents = Agents(agents=[airbnb_agent, whatsapp_agent])

 result = agents.start()
 return f"## Airbnb Search Results\n\n{result}"

demo = gr.Interface(
 fn=search_airbnb,
 inputs=gr.Textbox(placeholder="I want to book an apartment in Paris for 2 nights..."),
 outputs=gr.Markdown(),
 title="WhatsApp MCP Agent",
 description="Enter your booking requirements below:"
)

if __name__ == "__main__":
 demo.launch()
```

## Features