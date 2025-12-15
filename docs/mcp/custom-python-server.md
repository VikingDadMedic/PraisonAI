# Custom Python MCP Server

## Custom Python MCP Server

```mermaid
flowchart LR
 In[Query] --> Agent[AI Agent]
 Agent --> Client[Python MCP Client]
 Client --> Server[Python MCP Server]
 Server --> Client
 Client --> Agent
 Agent --> Out[Answer]

 style In fill:#8B0000,color:#fff
 style Agent fill:#2E8B57,color:#fff
 style Client fill:#3776AB,color:#fff
 style Server fill:#3776AB,color:#fff
 style Out fill:#8B0000,color:#fff
```

## Overview

The Custom Python MCP Server is a simple implementation of the Model Context Protocol (MCP) that provides stock price information using the yfinance library. This server can be used with PraisonAI agents to retrieve real-time stock prices.

## Server Implementation

Below is the complete implementation of the custom Python MCP server:

## Quick Start