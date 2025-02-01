# Memory

Aesoperator uses a sophisticated memory system powered by pgvector and Neon database to maintain context. Unlike other systems, Aesoperator uses a smaller judge model to continuously evaluate stored memories.

## Architecture

- **Database**: Neon PostgreSQL with pgvector extension for vector embeddings
- **Storage**: Key-value pairs + vector embeddings of content
- **Retrieval**: Semantic search using cosine similarity
- **Management**: Dynamic pruning and retention based on relevance

## How It Works

### Storage
```python
# Store key-value pairs
aesop.memory.set("user_preferences", {
    "theme": "dark",
    "language": "python"
})

# Store with vector embedding
aesop.memory.store(
    "Important context about user workflow",
    embedding_model="text-embedding-3-small"
)
```

### Retrieval
```python
# Get exact key matches
prefs = aesop.memory.get("user_preferences")

# Semantic search
relevant_context = aesop.memory.search(
    query="What was the user working on?",
    limit=5
)
```

### Dynamic Management

Aesoperator uses "judge" models like o3-mini to continuously evaluate stored memories:

1. **Relevance Scoring**: Judges rate memory importance based on:
   - Recency of access
   - Semantic relevance to current task
   - Historical utility

2. **Pruning Decisions**: Low-scoring memories are archived or deleted
   - Configurable retention thresholds
   - Task-specific memory lifetimes
   - Automatic cleanup of stale data

3. **Context Window Management**: 
   - Dynamically selects most relevant memories
   - Maintains optimal context size
   - Prevents context overflow

## Memory Types

1. **Short-term**: Task-specific, cleared after completion
2. **Working**: Persists across related tasks
3. **Long-term**: Permanent storage for critical information

## Example Usage

```python
# Store task context
aesop.memory.store("User is debugging a Python web app")

# Later retrieve relevant context
context = aesop.memory.search("What is the user's current task?")

# Judge model evaluates and prunes
aesop.memory.optimize(
    judge_model="context-evaluator-v1",
    retention_threshold=0.7
)
```

## Configuration

```python
aesop.memory.configure({
    "max_tokens": 100000,
    "pruning_threshold": 0.6,
    "judge_model": "gpt-4",
    "embedding_model": "text-embedding-3-small",
    "retention_days": 30
})
```

## Technical Details

- Uses pgvector's HNSW indexing
- Embeddings cached in Redis for fast retrieval
- Async pruning jobs run on schedule
- Automatic backups to object storage
- Monitoring via Prometheus metrics

Note: Dynamic memory management with judge models is currently in progress. The feature will be generally available in Q2 2025.
