# DeepSeek Agents

```mermaid
flowchart LR
 In[Input] --> Ollama[("Ollama")]
 Ollama --> DS[("Deepseek")]
 DS --> Basic[Basic Agent]
 DS --> RAG[RAG Agent]
 DS --> UI[UI Agent]
 Basic --> Out1[Output]
 RAG --> Out2[Output]
 UI --> Out3[Output]

 style In fill:#8B0000,color:#fff
 style Ollama fill:#4169E1,color:#fff,shape:circle
 style DS fill:#2E8B57,color:#fff,shape:circle
 style Basic fill:#2E8B57,color:#fff
 style RAG fill:#2E8B57,color:#fff
 style UI fill:#2E8B57,color:#fff
 style Out1 fill:#8B0000,color:#fff
 style Out2 fill:#8B0000,color:#fff
 style Out3 fill:#8B0000,color:#fff
```

Learn how to use Deepseek models with PraisonAI Agents through Ollama integration for basic queries, RAG applications, and interactive UI implementations.

## Prerequisites

## Basic Usage

The simplest way to use Deepseek with PraisonAI Agents:

```python
from praisonaiagents import Agent

agent = Agent(instructions="You are helpful Assisant", llm="deepseek-r1")

agent.start("Why sky is Blue?")
```

## RAG Implementation

Use Deepseek with RAG capabilities for knowledge-based interactions:

```python
from praisonaiagents import Agent

config = {
 "vector_store": {
 "provider": "chroma",
 "config": {
 "collection_name": "praison",
 "path": ".praison"
 }
 },
 "llm": {
 "provider": "ollama",
 "config": {
 "model": "deepseek-r1:latest",
 "temperature": 0,
 "max_tokens": 8000,
 "ollama_base_url": "http://localhost:11434",
 },
 },
 "embedder": {
 "provider": "ollama",
 "config": {
 "model": "nomic-embed-text:latest",
 "ollama_base_url": "http://localhost:11434",
 "embedding_dims": 1536
 },
 },
}

agent = Agent(
 name="Knowledge Agent",
 instructions="You answer questions based on the provided knowledge.",
 knowledge=["kag-research-paper.pdf"], # Indexing

 knowledge_config=config,
 user_id="user1",
 llm="deepseek-r1"
)

agent.start("What is KAG in one line?") # Retrieval

```

## Interactive UI with Streamlit

Create an interactive chat interface using Streamlit:

```python
import streamlit as st
from praisonaiagents import Agent

def init_agent():
 config = {
 "vector_store": {
 "provider": "chroma",
 "config": {
 "collection_name": "praison",
 "path": ".praison"
 }
 },
 "llm": {
 "provider": "ollama",
 "config": {
 "model": "deepseek-r1:latest",
 "temperature": 0,
 "max_tokens": 8000,
 "ollama_base_url": "http://localhost:11434",
 },
 },
 "embedder": {
 "provider": "ollama",
 "config": {
 "model": "nomic-embed-text:latest",
 "ollama_base_url": "http://localhost:11434",
 "embedding_dims": 1536
 },
 },
 }

 return Agent(
 name="Knowledge Agent",
 instructions="You answer questions based on the provided knowledge.",
 knowledge=["kag-research-paper.pdf"],
 knowledge_config=config,
 user_id="user1",
 llm="deepseek-r1"
 )

st.title("Knowledge Agent Chat")

if "agent" not in st.session_state:
 st.session_state.agent = init_agent()
 st.session_state.messages = []

if "messages" in st.session_state:
 for message in st.session_state.messages:
 with st.chat_message(message["role"]):
 st.markdown(message["content"])

prompt = st.chat_input("Ask a question...")

if prompt:
 st.session_state.messages.append({"role": "user", "content": prompt})
 with st.chat_message("user"):
 st.markdown(prompt)

 with st.chat_message("assistant"):
 response = st.session_state.agent.start(prompt)
 st.markdown(response)
 st.session_state.messages.append({"role": "assistant", "content": response})
```

## Running the UI

## Features

## Troubleshooting