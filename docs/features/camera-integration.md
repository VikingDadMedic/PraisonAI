# Camera Integration

PraisonAI supports camera integration for real-time visual analysis through multimodal agents. While there's no built-in camera capture, you can easily integrate camera feeds by capturing frames or videos and passing them to vision agents.

## Overview

Camera integration works by:
1. **Capturing frames/videos** from camera using OpenCV
2. **Saving temporarily** to disk
3. **Passing file paths** to agents via the `images` parameter
4. **Cleaning up** temporary files after analysis

## Quick Start

### Prerequisites

```bash
pip install praisonaiagents opencv-python
export OPENAI_API_KEY=your_openai_api_key
```

### Basic Camera Capture

```python
import cv2
from praisonaiagents import Agent, Task, PraisonAIAgents

def capture_and_analyze():
 # Create vision agent

 vision_agent = Agent(
 name="CameraAnalyst",
 role="Camera Feed Analyzer",
 goal="Analyze camera captures in real-time",
 backstory="Expert in real-time visual analysis",
 llm="gpt-4o-mini"
 )

 # Capture from camera

 cap = cv2.VideoCapture(0) # 0 for default camera

 ret, frame = cap.read()

 if ret:
 # Save frame temporarily

 cv2.imwrite("temp_capture.jpg", frame)
 cap.release()

 # Create analysis task

 task = Task(
 description="Analyze what you see in this camera feed",
 expected_output="Detailed analysis of camera content",
 agent=vision_agent,
 images=["temp_capture.jpg"]
 )

 # Run analysis

 agents = PraisonAIAgents(
 agents=[vision_agent],
 tasks=[task]
 )

 return agents.start()

# Run analysis

result = capture_and_analyze()
```

## Integration Patterns

### 1. Single Frame Analysis

Perfect for quick snapshots and one-time analysis:

```python
def single_frame_analysis():
 cap = cv2.VideoCapture(0)
 ret, frame = cap.read()

 if ret:
 image_path = "snapshot.jpg"
 cv2.imwrite(image_path, frame)

 # Analyze with your agent

 # ... (agent setup and task creation)

 cap.release()
```

### 2. Continuous Monitoring

Ideal for security systems and real-time monitoring:

```python
def continuous_monitoring(interval=10):
 vision_agent = Agent(
 name="SecurityMonitor",
 role="Security Camera Analyst",
 goal="Monitor for security events",
 backstory="Expert security analyst",
 llm="gpt-4o-mini"
 )

 cap = cv2.VideoCapture(0)

 while True:
 ret, frame = cap.read()
 if ret:
 timestamp = int(time.time())
 filename = f"capture_{timestamp}.jpg"
 cv2.imwrite(filename, frame)

 # Analyze frame

 task = Task(
 description="Monitor for unusual activities",
 agent=vision_agent,
 images=[filename]
 )

 agents = PraisonAIAgents(
 agents=[vision_agent],
 tasks=[task]
 )

 result = agents.start()
 print(f"Analysis: {result}")

 time.sleep(interval)
```

### 3. Multi-Agent Analysis

Use multiple specialized agents for comprehensive analysis:

```python
def multi_agent_camera_analysis():
 # Create specialized agents

 security_agent = Agent(
 name="SecurityExpert",
 role="Security Specialist",
 goal="Identify security threats",
 backstory="Expert in surveillance and threat detection",
 llm="gpt-4o-mini"
 )

 object_detector = Agent(
 name="ObjectDetector",
 role="Object Recognition Specialist",
 goal="Identify and catalog objects",
 backstory="Computer vision expert",
 llm="gpt-4o-mini"
 )

 scene_analyst = Agent(
 name="SceneAnalyst",
 role="Scene Understanding Expert",
 goal="Provide comprehensive scene analysis",
 backstory="Environmental analysis expert",
 llm="gpt-4o-mini"
 )

 # Capture frame

 cap = cv2.VideoCapture(0)
 ret, frame = cap.read()
 cap.release()

 if ret:
 cv2.imwrite("analysis_frame.jpg", frame)

 # Create specialized tasks

 security_task = Task(
 description="Analyze for security threats",
 agent=security_agent,
 images=["analysis_frame.jpg"]
 )

 object_task = Task(
 description="Identify all objects in scene",
 agent=object_detector,
 images=["analysis_frame.jpg"]
 )

 scene_task = Task(
 description="Provide comprehensive scene analysis",
 agent=scene_analyst,
 images=["analysis_frame.jpg"]
 )

 # Run parallel analysis

 agents = PraisonAIAgents(
 agents=[security_agent, object_detector, scene_analyst],
 tasks=[security_task, object_task, scene_task],
 process="parallel"
 )

 return agents.start()
```

### 4. Video Recording & Analysis

Analyze temporal events and activities:

```python
def record_and_analyze_video(duration=10):
 cap = cv2.VideoCapture(0)

 # Setup video writer

 fps = int(cap.get(cv2.CAP_PROP_FPS)) or 30
 width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
 height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))

 fourcc = cv2.VideoWriter_fourcc(*'mp4v')
 out = cv2.VideoWriter('recording.mp4', fourcc, fps, (width, height))

 # Record video

 start_time = time.time()
 while (time.time() - start_time) Security & Privacy

**Windows:**
- Check camera privacy settings in Windows Settings

### Performance Issues

```python
# Reduce frame size

frame = cv2.resize(frame, (320, 240))

# Lower analysis frequency

time.sleep(10) # Analyze every 10 seconds

# Use threading for non-blocking capture

import threading
```

## Examples

Complete working examples are available in the repository:
- [`camera-basic.py`](../../examples/python/camera/camera-basic.py) - Basic single frame capture
- [`camera-continuous.py`](../../examples/python/camera/camera-continuous.py) - Continuous monitoring
- [`camera-multi-agent.py`](../../examples/python/camera/camera-multi-agent.py) - Multi-agent analysis
- [`camera-video-analysis.py`](../../examples/python/camera/camera-video-analysis.py) - Video recording & analysis

## Related Features

- [Multimodal Agents](./multimodal) - Core multimodal capabilities
- [Image Generation](./image-generation) - Creating images with AI
- [Agent Memory](../concepts/memory) - Persistent agent memory
- [Task Management](../concepts/tasks) - Task configuration and execution