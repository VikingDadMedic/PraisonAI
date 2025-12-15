# Video Agent

```mermaid
flowchart LR
 In[Video Input] --> Analyzer[Content Analyzer]
 Analyzer --> Detector[Object Detector]
 Detector --> Transcriber[Text Transcriber]
 Transcriber --> Out[Analysis Report]

 style In fill:#8B0000,color:#fff
 style Analyzer fill:#2E8B57,color:#fff
 style Detector fill:#2E8B57,color:#fff
 style Transcriber fill:#2E8B57,color:#fff
 style Out fill:#8B0000,color:#fff
```

A workflow demonstrating how the Video Agent can analyze video content, detect objects, and extract meaningful information.

## Quick Start

## Understanding Video Analysis

The Video Agent combines multiple capabilities for comprehensive video understanding:
1. **Content Analysis**: Analyzes video scenes and events
2. **Object Detection**: Identifies objects and people
3. **Text Extraction**: Captures text shown in videos
4. **Context Understanding**: Interprets settings and situations

## Features

## Example Usage

```python
# Example: Analyze a presentation video

from praisonaiagents import Agent, Task, PraisonAIAgents

video_agent = Agent(
 name="VideoAnalyst",
 role="Video Analysis Specialist",
 goal="Extract information from presentation videos",
 llm="gpt-4o-mini"
)

# Create presentation analysis task

presentation_task = Task(
 name="analyze_presentation",
 description="""Analyze this presentation video:
1. Extract key points
2. Capture slide content
3. Note speaker's main arguments
4. Summarize Q&A session""",
 expected_output="Detailed presentation summary",
 agent=video_agent,
 images=["presentation.mp4"]
)

# Run analysis

agents = PraisonAIAgents(
 agents=[video_agent],
 tasks=[presentation_task],
 process="sequential"
)
agents.start()
```

## Next Steps

- Learn about [Prompt Chaining](/features/promptchaining) for complex video analysis
- Explore [Evaluator Optimizer](/features/evaluator-optimiser) for improving analysis accuracy
- Check out the [Image Agent](/agents/image) for still image analysis