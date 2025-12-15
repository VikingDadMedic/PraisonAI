# Understanding Process Types

Process types in PraisonAI define how tasks are executed and how agents collaborate. Each process type offers different patterns for task execution and agent coordination.

## Process Types Overview

## Sequential Process

```mermaid
graph LR
 Input[Input] --> A1
 subgraph Agents
 direction LR
 A1[Agent 1] --> A2[Agent 2] --> A3[Agent 3]
 end
 A3 --> Output[Output]

 classDef input fill:#8B0000,stroke:#7C90A0,color:#fff
 classDef process fill:#189AB4,stroke:#7C90A0,color:#fff
 classDef transparent fill:none,stroke:none

 class Input,Output input
 class A1,A2,A3 process
 class Agents transparent
```

The simplest form of task execution where tasks are performed one after another.

## Hierarchical Process

```mermaid
graph TB
 Input[Input] --> Manager

 subgraph Agents
 Manager[Manager Agent]

 subgraph Workers
 direction LR
 W1[Worker 1]
 W2[Worker 2]
 W3[Worker 3]
 end

 Manager --> W1
 Manager --> W2
 Manager --> W3
 end

 W1 --> Manager
 W2 --> Manager
 W3 --> Manager
 Manager --> Output[Output]

 classDef input fill:#8B0000,stroke:#7C90A0,color:#fff
 classDef process fill:#189AB4,stroke:#7C90A0,color:#fff
 classDef transparent fill:none,stroke:none

 class Input,Output input
 class Manager,W1,W2,W3 process
 class Agents,Workers transparent
```

Uses a manager agent to coordinate task execution and agent assignments.

## Workflow Process

```mermaid
graph LR
 Input[Input] --> Start

 subgraph Workflow
 direction LR
 Start[Start] --> C1{Condition}
 C1 --> |Yes| A1[Agent 1]
 C1 --> |No| A2[Agent 2]
 A1 --> Join
 A2 --> Join
 Join --> A3[Agent 3]
 end

 A3 --> Output[Output]

 classDef input fill:#8B0000,stroke:#7C90A0,color:#fff
 classDef process fill:#189AB4,stroke:#7C90A0,color:#fff
 classDef decision fill:#2E8B57,stroke:#7C90A0,color:#fff
 classDef transparent fill:none,stroke:none

 class Input,Output input
 class Start,A1,A2,A3,Join process
 class C1 decision
 class Workflow transparent
```

Advanced process type supporting complex task relationships and conditional execution.

## Getting Started

## Advanced Features

## Async Processing

### Core Async Methods

### Process-Specific Features

### Key Benefits