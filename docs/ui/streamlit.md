# Streamlit Agent

```mermaid
flowchart LR
 In[User Input] --> UI[Streamlit UI]
 UI --> Agent[AI Agent]
 Agent --> Out[Results Display]

 style In fill:#8B0000,color:#fff
 style UI fill:#2E8B57,color:#fff
 style Agent fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

## Quick Start

## Features

## Understanding the Code

The example demonstrates a simple research assistant with these key components:
1. **UI Setup**:
- Title and description using `st.title()` and `st.write()`
- Input field with `st.text_input()`
- Search button with `st.button()`
2. **Agent Integration**:
- Initialize the AI agent with specific instructions
- Connect the agent to the UI components
- Handle user input and display results
3. **User Experience**:
- Loading spinner during processing
- Input validation and error messages
- Clean result display

## Customization

You can enhance the UI with additional Streamlit components:

```python
# Add sidebar options

st.sidebar.title("Settings")
model = st.sidebar.selectbox("Select Model", ["GPT-3", "GPT-4"])

# Add multiple input types

text_input = st.text_area("Long Query", height=100)
file_input = st.file_uploader("Upload File")

# Display results with formatting

st.markdown("### Results")

st.json(structured_data)
st.dataframe(tabular_data)
```

## Next Steps

- Learn about [Prompt Chaining](/features/promptchaining) for complex UI workflows
- Explore [Evaluator Optimizer](/features/evaluator-optimiser) for improving responses
- Check out [Gradio Integration](/ui/gradio) for an alternative UI framework