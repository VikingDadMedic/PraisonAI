# Introduction

## What is PraisonAI?

PraisonAI is a powerful Multi-Agent Framework for building and deploying AI agents that can understand, reason, and execute complex tasks autonomously.

# Core Components

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

## Use Cases

## Getting Started is Easy

## Playground

## Key Features

## Chat with One Agent

## Next Steps