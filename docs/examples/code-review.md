# Code Review

```mermaid
flowchart LR
 In[In] --> Analyzer[Code Analyzer]
 Analyzer --> Suggester[Fix Suggester]
 Suggester --> Applier[Fix Applier]
 Applier -->|fixed| Out[Out]
 Applier -->|manual_review| Suggester

 style In fill:#8B0000,color:#fff
 style Analyzer fill:#2E8B57,color:#fff
 style Suggester fill:#2E8B57,color:#fff
 style Applier fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow demonstrating how AI agents can automate code review, from analysis through fix suggestion and application.

## Quick Start

## Understanding Code Review

## Features

## Next Steps