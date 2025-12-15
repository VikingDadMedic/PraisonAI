# AI Agents

PraisonAI provides a diverse set of specialized agents for various tasks. Each agent is designed with specific capabilities and tools to handle different types of tasks effectively.

```mermaid
graph LR
 %% Define the main flow
 Start([▶ Start]) --> Agent1
 Agent1 --> Process[⚙ Process]
 Process --> Agent2
 Agent2 --> Output([✓ Output])
 Process -.-> Agent1

 %% Define subgraphs for agents and their tasks
 subgraph Agent1[ ]
 Task1[📋 Task]
 AgentIcon1[🤖 AI Agent]
 Tools1[🔧 Tools]

 Task1 --- AgentIcon1
 AgentIcon1 --- Tools1
 end

 subgraph Agent2[ ]
 Task2[📋 Task]
 AgentIcon2[🤖 AI Agent]
 Tools2[🔧 Tools]

 Task2 --- AgentIcon2
 AgentIcon2 --- Tools2
 end

 classDef input fill:#8B0000,stroke:#7C90A0,color:#fff
 classDef process fill:#189AB4,stroke:#7C90A0,color:#fff
 classDef tools fill:#2E8B57,stroke:#7C90A0,color:#fff
 classDef transparent fill:none,stroke:none

 class Start,Output,Task1,Task2 input
 class Process,AgentIcon1,AgentIcon2 process
 class Tools1,Tools2 tools
 class Agent1,Agent2 transparent
```

## Data & Analysis

## Media & Content

## Search & Recommendations

## Development

## Getting Started

Each agent can be easily initialized and customized for your specific needs. Here's a basic example:

```python
from praisonaiagents import Agent

# Create an agent with specific instructions

agent = Agent(instructions="Your task-specific instructions")

# Start the agent with a task

response = agent.start("Your task description")
```

For more detailed information about each agent, click on the respective cards above.