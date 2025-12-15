# AI Agents with Tools

```mermaid
flowchart TB
 subgraph Tools
 direction TB
 T3[Internet Search]
 T1[Code Execution]
 T2[Formatting]
 end

 Input[Input] ---> Agents
 subgraph Agents
 direction LR
 A1[Agent 1]
 A2[Agent 2]
 A3[Agent 3]
 end
 Agents ---> Output[Output]

 T3 --> A1
 T1 --> A2
 T2 --> A3

 style Tools fill:#189AB4,color:#fff
 style Agents fill:#8B0000,color:#fff
 style Input fill:#8B0000,color:#fff
 style Output fill:#8B0000,color:#fff
```

| Feature | [Knowledge](/concepts/knowledge) | [Tools](/concepts/tools) |
|---------|--------------------------------|---------------------------|
| Purpose | Static reference information | Dynamic interaction capabilities |
| Access | Read-only reference | Execute actions and commands |
| Updates | Manual through files | Real-time through tool calls |
| Storage | Knowledge base | Assigned to specific agents |
| Persistence | Permanent until changed | Available during agent execution |

## Quick Start

Tools are functions that agents can use to interact with external systems and perform actions. They are essential for creating agents that can do more than just process text.

## Creating Custom Tool

## Creating Custom Tool with Detailed Instructions

## In-build Tools in PraisonAI

## Tools Overview

## Advanced Tool Features

### Tool Configuration

### Tool Chaining

### Tool Categories

## Tool Integration

### Adding Tools to Agents

### Tool Dependencies

## Tool Guidelines

## Best Practices Summary