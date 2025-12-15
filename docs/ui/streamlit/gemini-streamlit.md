# Gemini Streamlit UI

```mermaid
flowchart LR
 In[Input] --> UI[("Streamlit UI")]
 VDB[(Vector DB)] --> Agent
 Gemini[("Gemini")] --> Agent
 UI --> Agent[("Knowledge Agent")]
 Agent --> |Query| VDB
 Agent --> Out[Output]
 Out --> UI

 style In fill:#8B0000,color:#fff
 style UI fill:#FF4B4B,color:#fff,shape:circle
 style Gemini fill:#4169E1,color:#fff,shape:circle
 style Agent fill:#2E8B57,color:#fff,shape:circle
 style VDB fill:#4169E1,color:#fff,shape:cylinder
 style Out fill:#8B0000,color:#fff
```

## Prerequisites

## Code

```python
import streamlit as st
from praisonaiagents import Agent

st.title("Gemini 2.0 Thinking AI Agent")

# Initialize the agent

@st.cache_resource
def get_agent():
 llm_config = {
 "model": "gemini/gemini-2.0-flash-thinking-exp-01-21",
 "response_format": {"type": "text"}
 }

 return Agent(
 instructions="You are a helpful assistant",
 llm=llm_config
 )

agent = get_agent()

# Create text area input field

user_question = st.text_area("Ask your question:", height=150)

# Add ask button

if st.button("Ask"):
 if user_question:
 with st.spinner('Thinking...'):
 result = agent.start(user_question)
 st.write("### Answer")

 st.write(result)
 else:
 st.warning("Please enter a question")
```

## Features