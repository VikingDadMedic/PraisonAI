# Configuration Reference

This section provides comprehensive documentation for all configuration options in PraisonAI. Each component has its own detailed configuration guide with examples, best practices, and advanced options.

## Configuration Categories

## Quick Reference

### Essential Configuration Parameters

| Component | Parameter | Type | Description |
|-----------|-----------|------|-------------|
| Agent | `max_iter` | int | Maximum iterations for task completion |
| Agent | `max_retry_limit` | int | Maximum retries for failed operations |
| Task | `task_type` | str | Type of task execution |
| Memory | `quality_threshold` | float | Minimum quality score for memory retrieval |
| LLM | `timeout` | int | Request timeout in seconds |
| Tool | `execution_timeout` | int | Tool execution timeout |

## Environment Variables

PraisonAI supports configuration through environment variables for sensitive settings:

```bash
# LLM Configuration

export OPENAI_API_KEY="your-api-key"
export OPENAI_MODEL="gpt-4o"

# Memory Configuration

export PRAISONAI_MEMORY_PROVIDER="rag"
export PRAISONAI_DB_PATH="/path/to/db"

# Performance Settings

export PRAISONAI_MAX_WORKERS=4
export PRAISONAI_TIMEOUT=300
```

## Configuration Files

PraisonAI supports YAML configuration files for complex setups:

```yaml
# config.yaml

agents:
 default:
 max_iter: 15
 max_retry_limit: 3
 context_length: 128000
 markdown: true

memory:
 provider: rag
 quality_threshold: 0.7
 graph_enabled: true

llm:
 timeout: 60
 max_retries: 3
 temperature: 0.7
```

## Getting Started

1. Start with the [Agent Configuration](/configuration/agent-config) to set up your agents
2. Configure [Tasks](/configuration/task-config) for your workflow
3. Set up [Memory](/configuration/memory-config) for persistent storage
4. Fine-tune [LLM settings](/configuration/llm-config) for optimal performance

## Need Help?

- Check our [Best Practices](/configuration/best-practices) guide
- See [Examples](/examples) for real-world configurations
- Visit our [GitHub](https://github.com/MervinPraison/PraisonAI) for issues and discussions