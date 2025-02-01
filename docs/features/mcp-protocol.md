# Model Context Protocol

The Model Context Protocol (MCP) is an open standard that enables secure, two-way connections between AI agents and data sources. Aesoperator implements MCP to give its operators direct access to your systems and data while maintaining security and control.

## How MCP Works in Aesoperator

```python
from aesoperator import MCPServer, MCPClient

# Create an MCP server to expose your data
server = MCPServer(
    name="github-connector",
    data_sources=["repos", "issues", "pull_requests"],
    auth_config={"type": "oauth2"}
)

# Connect Aesoperator as an MCP client
client = MCPClient(
    server_url="http://localhost:8000",
    credentials={"access_token": "..."}
)
```

### Key Components

1. **MCP Servers**: Expose your data sources through a standardized API
- Code repositories (Git, GitHub)
- Documentation (Google Drive, Notion)
- Databases (Postgres, MongoDB) 
- Web apps (via Puppeteer)

2. **MCP Clients**: AI agents that connect to MCP servers
- Aesoperator operators act as MCP clients
- Can access multiple data sources
- Maintain context across interactions

3. **Context Management**: 
- Persistent memory across sessions
- Knowledge graph of relationships
- Semantic search capabilities

## Benefits of MCP in Aesoperator

1. **Universal Data Access**
- Single protocol for all data sources
- No custom integrations needed
- Standardized authentication

2. **Context Awareness** 
- Operators maintain state across calls
- Can reference previous interactions
- Build knowledge over time

3. **Security & Control**
- Fine-grained access control
- Audit logging
- Rate limiting

## Example: GitHub Integration

```python
# Setup GitHub MCP server
github_server = MCPServer(
    name="github",
    repo="username/repo",
    access_token="..."
)

# Create Aesoperator task with MCP context
task = aesop.Task(
    name="code_review",
    mcp_context=[github_server],
    inputs={
        "pr_number": 123,
        "review_type": "security"
    }
)

# Operator can now access GitHub data
result = aesop.execute_task(task)
```

## Example: Database Integration

```python
# Setup Postgres MCP server
db_server = MCPServer(
    name="analytics_db",
    connection_string="postgresql://...",
    allowed_tables=["users", "events"]
)

# Analyze data with MCP context
task = aesop.Task(
    name="user_analysis",
    mcp_context=[db_server],
    inputs={
        "metric": "retention",
        "date_range": "last_30_days"
    }
)

result = aesop.execute_task(task)
```

## Available MCP Actions

```python
# Query data source
mcp_query(
    server: str,
    query: str,
    params: Dict = None
) -> Dict

# Update data
mcp_update(
    server: str,
    operation: str,
    data: Dict
) -> None

# Stream changes
mcp_subscribe(
    server: str,
    event_type: str,
    handler: Callable
) -> None
```

## Best Practices

1. **Security**
- Use minimal access permissions
- Rotate credentials regularly
- Monitor usage patterns

2. **Performance** 
- Cache frequently accessed data
- Use efficient queries
- Implement rate limiting

3. **Reliability**
- Handle connection errors
- Implement retries
- Monitor server health

## Learn More

- [MCP Specification](https://mcp.dev/spec)
- [Building MCP Servers](servers.md)
- [MCP Security Guide](security.md)
- [Example Implementations](examples.md)
