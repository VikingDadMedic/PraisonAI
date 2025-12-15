# Memory Cleanup for Long-Running Apps

Long-running multi-agent applications can accumulate memory over time, leading to performance degradation and potential crashes. This guide covers best practices for effective memory management.

## Understanding Memory Issues

### Common Memory Problems

1. **Memory Leaks**: Unreleased references to objects
2. **Conversation History Accumulation**: Growing chat histories
3. **Cache Overflow**: Unbounded caching
4. **Circular References**: Objects referencing each other
5. **Resource Handles**: Unclosed files, connections, etc.

## Memory Management Strategies

### 1. Conversation History Management

Implement sliding window or summary-based history management:

```python
from collections import deque
from typing import List, Dict, Any
import hashlib

class MemoryEfficientConversationManager:
 def __init__(self, max_history_length: int = 100, summary_threshold: int = 50):
 self.max_history_length = max_history_length
 self.summary_threshold = summary_threshold
 self.conversation_history = deque(maxlen=max_history_length)
 self.summaries = []

 def add_message(self, message: Dict[str, Any]):
 """Add a message to conversation history with automatic cleanup"""
 self.conversation_history.append(message)

 # Create summary when threshold is reached

 if len(self.conversation_history) >= self.summary_threshold:
 self._create_summary()

 def _create_summary(self):
 """Create a summary of older messages"""
 messages_to_summarize = list(self.conversation_history)[:self.summary_threshold//2]

 # In production, use an LLM to create actual summaries

 summary = {
 "type": "summary",
 "message_count": len(messages_to_summarize),
 "timestamp": messages_to_summarize[0]["timestamp"],
 "key_points": self._extract_key_points(messages_to_summarize)
 }

 self.summaries.append(summary)

 # Remove summarized messages

 for _ in range(len(messages_to_summarize)):
 self.conversation_history.popleft()

 def _extract_key_points(self, messages: List[Dict]) -> List[str]:
 """Extract key points from messages (simplified version)"""
 # In production, use NLP or LLM for better extraction

 return [msg.get("content", "")[:50] + "..." for msg in messages[-3:]]

 def get_context(self, last_n: int = 10) -> List[Dict]:
 """Get recent context including summaries"""
 context = []

 # Add relevant summaries

 if self.summaries:
 context.extend(self.summaries[-2:]) # Last 2 summaries

 # Add recent messages

 recent_messages = list(self.conversation_history)[-last_n:]
 context.extend(recent_messages)

 return context

 def cleanup(self):
 """Explicit cleanup method"""
 self.conversation_history.clear()
 self.summaries.clear()
```

### 2. Agent Memory Management

Implement memory limits and cleanup for agents:

```python
import gc
import weakref
from datetime import datetime, timedelta

class MemoryManagedAgent:
 def __init__(self, name: str, memory_limit_mb: int = 100):
 self.name = name
 self.memory_limit_mb = memory_limit_mb
 self.created_at = datetime.now()
 self.last_cleanup = datetime.now()
 self._memory_store = {}
 self._weak_refs = weakref.WeakValueDictionary()

 def store_memory(self, key: str, value: Any, weak: bool = False):
 """Store data with option for weak references"""
 if weak:
 self._weak_refs[key] = value
 else:
 self._memory_store[key] = value

 # Check memory usage

 if self._estimate_memory_usage() > self.memory_limit_mb:
 self._cleanup_old_memories()

 def _estimate_memory_usage(self) -> float:
 """Estimate memory usage in MB"""
 import sys
 total_size = 0

 for obj in self._memory_store.values():
 total_size += sys.getsizeof(obj)

 return total_size / (1024 * 1024)

 def _cleanup_old_memories(self):
 """Remove old or less important memories"""
 # Sort by age or importance (simplified)

 if hasattr(self, 'memory_importance'):
 sorted_keys = sorted(
 self._memory_store.keys(),
 key=lambda k: self.memory_importance.get(k, 0)
 )
 else:
 sorted_keys = list(self._memory_store.keys())

 # Remove least important/oldest 20%

 remove_count = len(sorted_keys) // 5
 for key in sorted_keys[:remove_count]:
 del self._memory_store[key]

 # Force garbage collection

 gc.collect()
 self.last_cleanup = datetime.now()

 def periodic_cleanup(self):
 """Run periodic cleanup tasks"""
 if datetime.now() - self.last_cleanup > timedelta(minutes=30):
 self._cleanup_old_memories()
 gc.collect()
```

### 3. Resource Pool Management

Implement resource pooling to prevent resource exhaustion:

```python
from contextlib import contextmanager
import threading
from queue import Queue, Empty

class ResourcePool:
 def __init__(self, factory, max_size: int = 10, cleanup_func=None):
 self.factory = factory
 self.max_size = max_size
 self.cleanup_func = cleanup_func
 self.pool = Queue(maxsize=max_size)
 self.size = 0
 self.lock = threading.Lock()

 @contextmanager
 def acquire(self, timeout: float = 30):
 """Acquire a resource from the pool"""
 resource = None
 try:
 # Try to get from pool

 try:
 resource = self.pool.get(timeout=timeout)
 except Empty:
 # Create new resource if under limit

 with self.lock:
 if self.size self.max_memory_mb * 1024 * 1024 or
 len(self.cache) >= self.max_items):
 if not self.cache:
 break

 # Remove least recently used

 oldest_key = next(iter(self.cache))
 oldest_value = self.cache.pop(oldest_key)
 self.memory_usage -= sys.getsizeof(oldest_value)

 # Add new item

 self.cache[key] = value
 self.memory_usage += value_size

 def clear(self):
 """Clear the cache"""
 self.cache.clear()
 self.memory_usage = 0
```

## Memory Monitoring

### 1. Memory Usage Tracking

```python
import psutil
import os
from datetime import datetime

class MemoryMonitor:
 def __init__(self, alert_threshold_percent: float = 80):
 self.alert_threshold_percent = alert_threshold_percent
 self.process = psutil.Process(os.getpid())
 self.memory_history = []

 def get_memory_info(self) -> Dict[str, Any]:
 """Get current memory usage information"""
 memory_info = self.process.memory_info()
 memory_percent = self.process.memory_percent()

 info = {
 "timestamp": datetime.now(),
 "rss_mb": memory_info.rss / 1024 / 1024,
 "vms_mb": memory_info.vms / 1024 / 1024,
 "percent": memory_percent,
 "available_mb": psutil.virtual_memory().available / 1024 / 1024
 }

 self.memory_history.append(info)

 # Keep only last hour of history

 cutoff = datetime.now() - timedelta(hours=1)
 self.memory_history = [
 h for h in self.memory_history
 if h["timestamp"] > cutoff
 ]

 return info

 def check_memory_health(self) -> Tuple[bool, str]:
 """Check if memory usage is healthy"""
 info = self.get_memory_info()

 if info["percent"] > self.alert_threshold_percent:
 return False, f"Memory usage critical: {info['percent']:.1f}%"

 # Check for memory growth trend

 if len(self.memory_history) > 10:
 recent = self.memory_history[-10:]
 growth_rate = (recent[-1]["rss_mb"] - recent[0]["rss_mb"]) / len(recent)

 if growth_rate > 10: # Growing > 10MB per check

 return False, f"Memory growing rapidly: {growth_rate:.1f}MB/check"

 return True, "Memory usage normal"
```

### 2. Automatic Garbage Collection

```python
import gc
import schedule
from typing import Callable

class AutomaticMemoryManager:
 def __init__(self):
 self.cleanup_callbacks: List[Callable] = []
 self.last_gc_stats = None

 def register_cleanup(self, callback: Callable):
 """Register a cleanup callback"""
 self.cleanup_callbacks.append(callback)

 def aggressive_cleanup(self):
 """Perform aggressive memory cleanup"""
 # Run all registered cleanup callbacks

 for callback in self.cleanup_callbacks:
 try:
 callback()
 except Exception as e:
 print(f"Cleanup callback failed: {e}")

 # Force garbage collection

 gc.collect(2) # Full collection

 # Get statistics

 self.last_gc_stats = {
 "collected": sum(gc.get_count()),
 "uncollectable": len(gc.garbage),
 "timestamp": datetime.now()
 }

 return self.last_gc_stats

 def setup_automatic_cleanup(self, interval_minutes: int = 30):
 """Setup periodic automatic cleanup"""
 schedule.every(interval_minutes).minutes.do(self.aggressive_cleanup)

 # Also cleanup on high memory usage

 def conditional_cleanup():
 memory_monitor = MemoryMonitor()
 healthy, _ = memory_monitor.check_memory_health()
 if not healthy:
 self.aggressive_cleanup()

 schedule.every(5).minutes.do(conditional_cleanup)
```

## Best Practices

### 1. Use Context Managers

Always use context managers for resource management:

```python
class ManagedAgentSession:
 def __init__(self, agent_factory):
 self.agent_factory = agent_factory
 self.agents = []

 def __enter__(self):
 return self

 def __exit__(self, exc_type, exc_val, exc_tb):
 # Cleanup all agents

 for agent in self.agents:
 agent.cleanup()
 self.agents.clear()
 gc.collect()

 def create_agent(self, *args, **kwargs):
 agent = self.agent_factory(*args, **kwargs)
 self.agents.append(agent)
 return agent
```

### 2. Implement Memory Budgets

Set memory budgets for different components:

```python
class MemoryBudgetManager:
 def __init__(self, total_budget_mb: int = 1000):
 self.total_budget_mb = total_budget_mb
 self.allocations = {}

 def allocate(self, component: str, budget_mb: int) -> bool:
 """Allocate memory budget to a component"""
 current_allocated = sum(self.allocations.values())

 if current_allocated + budget_mb > self.total_budget_mb:
 return False

 self.allocations[component] = budget_mb
 return True

 def check_usage(self, component: str, current_usage_mb: float) -> bool:
 """Check if component is within budget"""
 if component not in self.allocations:
 return False

 return current_usage_mb List[Tuple[str, float]]:
 """Get top memory allocations"""
 if len(self.snapshots) 0
```

## Conclusion

Effective memory management is crucial for long-running multi-agent applications. By implementing proper cleanup strategies, monitoring, and resource management, you can ensure your applications remain stable and performant over extended periods.