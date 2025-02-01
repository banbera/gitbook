# Tasks

Tasks are the primary way to define work for Aesoperator to perform. A Task represents a high-level goal or objective that will be broken down into a series of actions and function calls.

## Task Structure

A Task consists of:

```python
from aesoperator import Task, TaskConfig

task = Task(
    name="research_topic",
    description="Research and summarize information about a given topic",
    config=TaskConfig(
        max_pages=10,
        max_duration_seconds=300,
        allowed_domains=["wikipedia.org", "arxiv.org"]
    ),
    inputs={
        "topic": "artificial intelligence",
        "depth": "technical"
    },
    memory_scope="session"
)
```

Key components:
- `name`: Unique identifier for the task
- `description`: Human-readable description of what the task does
- `config`: Runtime constraints and settings
- `inputs`: Input parameters that control task behavior
- `memory_scope`: How long to persist memory ("session", "permanent", etc.)

## Task Configuration

The `TaskConfig` object lets you control how the task executes:

```python
config = TaskConfig(
    # Resource limits
    max_pages=10,              # Maximum pages to visit
    max_duration_seconds=300,  # Time limit in seconds
    max_memory_mb=1024,       # Memory limit in MB
    
    # Access controls
    allowed_domains=["wikipedia.org"],  # Allowed domains to visit
    blocked_domains=["ads.com"],        # Blocked domains
    require_https=True,                 # Require HTTPS connections
    
    # Browser settings
    viewport_width=1920,       # Browser viewport width
    viewport_height=1080,      # Browser viewport height
    user_agent="...",         # Custom user agent
    
    # Retry behavior
    max_retries=3,            # Max retry attempts
    retry_delay_seconds=1,    # Delay between retries
    
    # Rate limiting
    requests_per_second=2,    # Max requests per second
    concurrent_requests=1      # Max concurrent requests
)
```

## Executing Tasks

Tasks can be executed synchronously or asynchronously:

```python
# Synchronous execution
result = aesop.execute_task(task)
print(f"Task completed with result: {result}")

# Asynchronous execution
execution = aesop.execute_task_async(task)
execution_id = execution.id

# Poll for completion
while True:
    status = aesop.get_task_status(execution_id)
    if status.state == "completed":
        result = status.result
        break
    time.sleep(1)
```

## Task Lifecycle

A task goes through several states during execution:

1. `pending`: Task is queued for execution
2. `running`: Task is actively executing
3. `completed`: Task finished successfully
4. `failed`: Task encountered an error
5. `cancelled`: Task was manually cancelled

You can monitor task progress and get detailed status:

```python
status = aesop.get_task_status(execution_id)
print(f"State: {status.state}")
print(f"Progress: {status.progress}%")
print(f"Current action: {status.current_action}")
print(f"Error (if any): {status.error}")
```

## Task Memory

Tasks can read and write to memory that persists across function calls:

```python
# Write to task memory
aesop.memory.set("visited_urls", ["https://example.com"])

# Read from task memory
visited = aesop.memory.get("visited_urls")

# Memory persists based on scope
task = Task(
    name="persistent_task",
    memory_scope="permanent",  # Memory persists forever
    # ...
)

task = Task(
    name="session_task", 
    memory_scope="session",   # Memory cleared after task completes
    # ...
)
```

## Task Composition

Tasks can be composed into larger workflows:

```python
def research_and_summarize(topic):
    # Research task
    research_task = Task(
        name="research",
        inputs={"topic": topic},
        config=TaskConfig(max_pages=5)
    )
    research_result = aesop.execute_task(research_task)
    
    # Summarization task
    summary_task = Task(
        name="summarize",
        inputs={
            "text": research_result.content,
            "max_length": 1000
        }
    )
    summary = aesop.execute_task(summary_task)
    
    return summary

# Execute composed workflow
summary = research_and_summarize("quantum computing")
```

## Example Tasks

### Web Research Task

```python
research_task = Task(
    name="research_quantum_computing",
    description="Research recent advances in quantum computing",
    config=TaskConfig(
        max_pages=10,
        allowed_domains=["arxiv.org", "nature.com", "science.org"]
    ),
    inputs={
        "topic": "quantum computing",
        "start_date": "2023-01-01",
        "focus_areas": ["algorithms", "hardware"]
    }
)

result = aesop.execute_task(research_task)
print(result.summary)
```

### Data Processing Task

```python
process_task = Task(
    name="process_sales_data",
    description="Extract and analyze sales data from CSV files",
    config=TaskConfig(
        max_memory_mb=2048,
        max_duration_seconds=600
    ),
    inputs={
        "data_files": ["sales_q1.csv", "sales_q2.csv"],
        "metrics": ["revenue", "growth", "customers"],
        "output_format": "excel"
    }
)

result = aesop.execute_task(process_task)
result.save_report("sales_analysis.xlsx")
```

### Monitoring Task

```python
monitor_task = Task(
    name="monitor_website",
    description="Monitor website availability and performance",
    config=TaskConfig(
        max_duration_seconds=86400,  # Run for 24 hours
        requests_per_second=0.1      # One request every 10 seconds
    ),
    inputs={
        "url": "https://example.com",
        "checks": ["uptime", "latency", "ssl"],
        "alert_threshold_ms": 1000
    }
)

# Run monitoring task in background
execution = aesop.execute_task_async(monitor_task)

# Setup alert handler
def handle_alert(alert):
    print(f"Alert: {alert.message}")
    print(f"Metrics: {alert.metrics}")
    
execution.on_alert(handle_alert)
```

## Best Practices

1. Set appropriate resource limits in TaskConfig to prevent runaway tasks

2. Use memory scoping to control data persistence

3. Break complex workflows into smaller composed tasks

4. Include good descriptions and documentation

5. Handle errors and implement retries for reliability

6. Monitor task progress and set up alerting

7. Clean up resources when tasks complete

## Learn More

- [Task API Reference](task-api.md) for complete API details
- [Task Patterns](task-patterns.md) for common task patterns and recipes
- [Task Monitoring](task-monitoring.md) for monitoring and debugging tasks
- [Task Security](task-security.md) for security best practices

