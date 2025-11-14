# MCP Quick Reference

> **Quick access to key MCP concepts, APIs, and patterns**

## Core Concepts

| Concept | Purpose | Model |
|---------|---------|-------|
| **Resources** | Provide context/information to LLM | Push (provided to LLM) |
| **Prompts** | Give commands/tasks for LLM to execute | Command execution |
| **Tools** | Functions LLM can choose to invoke | Pull (LLM decides when) |

### When to Use What?

```
Resources → Static context (schemas, docs, configs)
Prompts   → Specific commands/tasks (code review, style check)
Tools     → Dynamic functionality (API calls, database queries)
```

## Quick Start

### Basic Server Setup

```javascript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new McpServer({
    name: "my-server",
    version: "1.0.0",
});

const transport = new StdioServerTransport();
await server.connect(transport);
```

## Resources API

### Register a Resource

```javascript
server.registerResource(
    "resource-id",              // Unique identifier
    "schema://resource-uri",    // URI
    {
        title: "Resource Title",
        description: "What this resource provides",
        mimeType: "text/plain"
    },
    async (uri) => ({
        contents: [{
            uri: uri.href,
            mimeType: "text/plain",
            text: "Your resource content here"
        }]
    })
);
```

### Common Use Cases
- ✅ Database schemas
- ✅ API documentation
- ✅ Configuration files
- ✅ Static reference data

## Prompts API

### Register a Prompt

```javascript
import { z } from "zod";

server.registerPrompt(
    "prompt-name",
    {
        title: "Prompt Title",
        description: "What this prompt does",
        argsSchema: {
            argName: z.string()
        }
    },
    ({ argName }) => ({
        messages: [{
            role: "user",
            content: {
                type: "text",
                text: `Your prompt template here: ${argName}`
            }
        }]
    })
);
```

### Common Use Cases
- ✅ Code review with style guides
- ✅ Automated issue fixing
- ✅ Documentation generation
- ✅ Linting and formatting

## Code Snippets

### ES Modules: Get __dirname

```javascript
import { fileURLToPath } from "url";
import path from "path";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);
```

### Read File Synchronously

```javascript
import { readFileSync } from "fs";

const content = readFileSync("/path/to/file.md", "utf-8");
```

### SQLite Schema Query

```javascript
import sqlite3 from "sqlite3";

const schema = await new Promise((resolve, reject) => {
    const db = new sqlite3.Database(dbPath, sqlite3.OPEN_READONLY);

    db.all(
        "SELECT sql FROM sqlite_master WHERE type='table' AND sql IS NOT NULL ORDER BY name",
        (err, rows) => {
            db.close();
            if (err) reject(err);
            else resolve(rows.map(row => row.sql + ';').join('\n'));
        }
    );
});
```

## Best Practices

### Resources
- Keep resources focused and specific
- Use appropriate MIME types
- Provide clear descriptions
- Consider lazy loading for large resources

### Prompts
- Make prompts reusable
- Use Zod for argument validation
- Include clear examples in descriptions
- Template variables for flexibility

### General
- Follow semantic versioning
- Handle errors gracefully
- Document your server's capabilities
- Test with different LLM clients

## Common Patterns

### Resource for Database Schema
```javascript
// Use when: LLM needs to understand your data structure
// Pattern: Query schema → Return as text resource
```

### Prompt for Code Review
```javascript
// Use when: Enforcing coding standards
// Pattern: Load style guide → Inject into prompt template
```

### Dynamic Resource Templates
```javascript
// Use when: Resource content varies by user
// Pattern: Accept URI parameters → Generate custom content
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `__dirname is not defined` | Use `fileURLToPath(import.meta.url)` pattern |
| Resource not loading | Check URI format and MIME type |
| Prompt args undefined | Validate with Zod schema |
| Server won't start | Verify transport connection |

## Learn More

- [Full MCP Guide](README.md)
- [MCP Official Docs](https://modelcontextprotocol.io)
- [MCP SDK on GitHub](https://github.com/modelcontextprotocol/sdk)
