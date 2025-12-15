# API Reference

# Module praisonaiagents

## Sub-modules

* praisonaiagents.agent - Agent module for defining individual AI agents
* praisonaiagents.agents - Agents module for managing multiple agents
* praisonaiagents.task - Task module for defining and managing tasks
* praisonaiagents.process - Process module for handling task execution flows
* praisonaiagents.main - Main module containing utility functions

## Functions

### clean_triple_backticks(text: str) → str

Clean triple backticks from text output.

### display_error(message: str, console=None)

Display error messages in the console.

### display_generating(content: str = '', start_time: float | None = None)

Display generation status with optional timing information.

### display_instruction(message: str, console=None)

Display instruction messages in the console.

### display_interaction(message, response, markdown=True, generation_time=None, console=None)

Display interaction between user and agent.

### display_self_reflection(message: str, console=None)

Display agent self-reflection messages.

### display_tool_call(message: str, console=None)

Display tool call messages.