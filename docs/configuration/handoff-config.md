# Handoff Configuration

This page provides comprehensive documentation for configuring handoffs in PraisonAI, including handoff filters, delegation strategies, routing rules, and advanced orchestration patterns.

## Handoff System Overview

The handoff system enables agents to delegate tasks to other agents based on expertise, availability, or specific conditions. This creates flexible multi-agent workflows with intelligent task routing.

## Basic Handoff Configuration

```python
from praisonaiagents import Agent, Handoff

# Basic handoff configuration

handoff = Handoff(
 name="research_handoff",
 target="research_agent",
 description="Hand off research tasks to specialized researcher",
 condition="task requires research"
)

agent = Agent(
 name="Coordinator",
 handoffs=[handoff],

)
```

## Handoff Filters

### Filter Types and Configuration

```python
# Expertise-based filter

expertise_filter = {
 "type": "expertise",
 "required_skills": ["python", "data_analysis"],
 "minimum_skill_level": 0.8,
 "match_all": True # Require all skills

}

# Availability filter

availability_filter = {
 "type": "availability",
 "max_queue_size": 5,
 "max_active_tasks": 3,
 "estimated_duration": 300, # 5 minutes

 "check_schedule": True
}

# Performance filter

performance_filter = {
 "type": "performance",
 "min_success_rate": 0.9,
 "max_avg_duration": 120,
 "recent_window": 100, # Last 100 tasks

 "exclude_outliers": True
}

# Resource filter

resource_filter = {
 "type": "resource",
 "required_memory": 2048, # MB

 "required_cpu": 2, # cores

 "required_gpu": False,
 "check_availability": True
}

# Custom filter

custom_filter = {
 "type": "custom",
 "function": "evaluate_agent_compatibility",
 "parameters": {
 "task_complexity": "high",
 "domain": "finance"
 }
}
```

### Composite Filters

```python
# Combine multiple filters with AND/OR logic

composite_filter = {
 "type": "composite",
 "operator": "AND",
 "filters": [
 {
 "type": "expertise",
 "required_skills": ["machine_learning"],
 "minimum_skill_level": 0.7
 },
 {
 "type": "composite",
 "operator": "OR",
 "filters": [
 {
 "type": "availability",
 "max_queue_size": 3
 },
 {
 "type": "performance",
 "min_success_rate": 0.95
 }
 ]
 }
 ]
}
```

### Dynamic Filter Functions

```python
def dynamic_agent_filter(agent, task, context):
 """Custom filter function for complex logic"""
 # Check agent capabilities

 if not agent.has_capability(task.required_capability):
 return False

 # Check workload

 if agent.current_load > 0.8:
 return False

 # Check domain expertise

 domain_score = agent.get_domain_score(task.domain)
 if domain_score 0.6 else None

dynamic_routing_config = {
 "router_function": dynamic_router,
 "refresh_interval": 30, # Re-evaluate every 30 seconds

 "cache_decisions": True,
 "cache_ttl": 60
}
```

### Rule-Based Routing

```python
rule_based_routing = {
 "rules": [
 {
 "name": "vip_customer_rule",
 "condition": "customer.tier == 'VIP'",
 "route": "senior_support",
 "priority": 1
 },
 {
 "name": "technical_issue_rule",
 "condition": "issue.category == 'technical' and issue.severity > 7",
 "route": "tech_specialist",
 "priority": 2
 },
 {
 "name": "language_rule",
 "condition": "customer.language != 'english'",
 "route": "multilingual_support",
 "priority": 3
 }
 ],
 "evaluation_order": "priority", # or "sequential", "parallel"

 "stop_on_match": True
}
```

## Handoff Security and Validation

```python
security_config = {
 "authentication": {
 "enabled": True,
 "method": "token", # or "certificate", "mutual_tls"

 "token_ttl": 3600
 },

 "authorization": {
 "enabled": True,
 "check_permissions": True,
 "role_based": True,
 "resource_based": True
 },

 "data_handling": {
 "encrypt_in_transit": True,
 "sanitize_sensitive": True,
 "audit_trail": True,
 "data_retention": 90 # days

 },

 "validation": {
 "validate_schema": True,
 "validate_constraints": True,
 "max_payload_size": 1048576 # 1MB

 }
}
```

## Performance Optimization

### Caching and Optimization

```python
optimization_config = {
 "caching": {
 "cache_routing_decisions": True,
 "cache_ttl": 300,
 "cache_size": 1000,
 "invalidation_strategy": "ttl" # or "lru", "event_based"

 },

 "batching": {
 "enable_batching": True,
 "batch_size": 10,
 "batch_timeout": 1.0,
 "group_by": ["target_agent", "priority"]
 },

 "connection_pooling": {
 "pool_size": 20,
 "max_overflow": 10,
 "pool_timeout": 30,
 "recycle": 3600
 },

 "monitoring": {
 "track_latency": True,
 "track_success_rate": True,
 "alert_thresholds": {
 "latency_p95": 1000, # ms

 "error_rate": 0.05
 }
 }
}
```

## Complete Handoff Configuration Example

```python
from praisonaiagents import Agent, Handoff

# Create comprehensive handoff configuration

coordinator = Agent(
 name="MasterCoordinator",
 handoffs=[
 Handoff(
 name="research_handoff",
 target="research_team",
 description="Complex research tasks",
 filters=[
 {
 "type": "expertise",
 "required_skills": ["research", "analysis"],
 "minimum_skill_level": 0.8
 }
 ]
 ),
 Handoff(
 name="urgent_handoff",
 target="emergency_team",
 description="High-priority urgent tasks",
 ,
 priority=1
 )
 ],

 ],

 # Routing configuration

 "routing": {
 "type": "hybrid",
 "static_routes": {
 "research": ["researcher_1", "researcher_2"],
 "analysis": ["analyst_1", "analyst_2"]
 },
 "dynamic_router": "smart_router_function"
 },

 # Performance settings

 "timeout": 60,
 "max_retries": 2,
 "retry_delay": 5,

 # Security settings

 "secure_handoffs": True,
 "encrypt_data": True,

 # Monitoring

 "track_metrics": True,
 "log_handoffs": True
 }
)
```

## Environment Variables

```bash
# Handoff strategy

export PRAISONAI_HANDOFF_STRATEGY="best_match"
export PRAISONAI_HANDOFF_TIMEOUT="60"

# Filter settings

export PRAISONAI_HANDOFF_MIN_SCORE="0.7"
export PRAISONAI_HANDOFF_CHECK_AVAILABILITY="true"

# Routing settings

export PRAISONAI_HANDOFF_ROUTING="dynamic"
export PRAISONAI_HANDOFF_CACHE_ROUTES="true"

# Performance settings

export PRAISONAI_HANDOFF_BATCH_SIZE="10"
export PRAISONAI_HANDOFF_MAX_RETRIES="3"

# Security settings

export PRAISONAI_HANDOFF_SECURE="true"
export PRAISONAI_HANDOFF_ENCRYPT="true"
```

## Best Practices

1. **Use appropriate filters** to ensure tasks are routed to capable agents
2. **Implement fallback mechanisms** for handling failures
3. **Monitor handoff performance** and adjust strategies accordingly
4. **Cache routing decisions** for frequently occurring patterns
5. **Set reasonable timeouts** to prevent indefinite waiting
6. **Implement circuit breakers** for unreliable agents
7. **Use batching** for high-volume scenarios
8. **Maintain audit trails** for compliance and debugging

## See Also

- [Handoff Concepts](/concepts/handoffs) - Understanding handoff patterns
- [Agent Configuration](/configuration/agent-config) - Agent handoff settings
- [Best Practices](/configuration/best-practices) - Configuration guidelines