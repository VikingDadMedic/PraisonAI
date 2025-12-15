# JavaScript AI Agents Framework

PraisonAI is a production-ready Multi AI Agents framework for JavaScript, designed to create AI Agents to automate and solve problems ranging from simple tasks to complex challenges. It provides a low-code solution to streamline the building and management of multi-agent LLM systems, emphasising simplicity, customisation, and effective human-agent collaboration.

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

## Usage Examples

## Running the Examples