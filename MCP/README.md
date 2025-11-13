# MCP Resources & Prompts Guide

This document contains notes and examples for working with MCP (Model Context Protocol) Resources and Prompts.

## MCP Resources

Resources provide additional context to the LLM from users to the LLMs. Unlike tools (which the LLM can choose to invoke), resources are pushed to the LLM as context.

### Key Characteristics

- **Purpose**: Providing files, database schemas, or app-specific information as context
- **Nature**: Static, not dynamic (though resource templates can be dynamic with some clients)
- **Model**: Push vs pull - resources are pushed to the LLM rather than pulled via tool invocation

### Use Cases

- Providing database schemas as context to the LLM
- Building a CMS where users can add specific docs or files and ask questions about their content
- Sharing static configuration or reference information

> **Note from the course**: Brian created a resource on database schema and gave it to the LLM instead of creating a tool around it. This demonstrates when to use resources vs tools.

### Example: Database Schema Resource

This example shows how to register a database schema as an MCP resource:

```javascript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import sqlite3  from "sqlite3";
import path from "path";
import { fileURLToPath } from "url";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const server = new McpServer({
    name: "issue-server",
    version: "1.0.0",
});

server.registerResource(
    "database-schema",
    "schema://database",
    {
        title: "Database Schema",
        description: "SQLite schema for the issue database",
        mimeType: "text/plain"
    },
    async (uri) => {
        const dbPath = path.join(__dirname, '..', "backend", "database.sqlite");

        const schema = await new Promise((resolve, reject) => {
            const db = new sqlite3.Database(dbPath, sqlite3.OPEN_READONLY)

            db.all(
                // Dump all schema from the SQLite database
                "SELECT sql FROM sqlite_master WHERE type='table' \
                AND sql IS NOT NULL ORDER BY name",
                (err, rows) => {
                    db.close();
                    if (err) {
                        reject(err);
                    } else {
                        resolve(rows.map(row => row.sql + ';').join('\n'));
                    }
                }
            )
        })

        return {
            contents: [
                {
                    uri: uri.href,
                    mimeType: "text/plain", // Tells the LLM how to interpret resource's contents
                    text: schema
                },
            ]
        }
    }
)

const transport = new StdioServerTransport();
await server.connect(transport);
```

### Technical Note: `__dirname` in ES Modules

Why `dirname` is not directly available in ES modules?

In ES modules, developers must use a specific method involving `fileURLToPath` and `path.dirname()` to recreate the current directory path:

```javascript
import { fileURLToPath } from "url";
import path from "path";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);
```

## MCP Prompts

Prompts are different from resources - while resources provide context to the LLM, **prompts are commands for the LLM to execute**.

### Key Differences

- **Resources**: Context given to LLM (information)
- **Prompts**: Commands for the LLM to do (actions)

### Use Cases

- Style checking code
- Creating coding agents that fix GitHub issues
- Setting up automated code review processes
- Providing prescriptive guidelines for repository creation

### Example: Code Review Prompt

This example shows how to register a code review prompt that uses a style guide:

```javascript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import { readFileSync } from "fs";

const standardMarkdownPath = "/Users/ajaysingh/Desktop/my-mcp/style-guide.md"
const standardMarkdown = readFileSync(standardMarkdownPath, "utf-8");

const server = new McpServer({
    name: "code-review-server",
    version: "1.0.0",
});

server.registerPrompt(
    "review-code",
    {
        title: "Code Review",
        description: "Review code for best practices and potential issues",
        argsSchema: { code: z.string() }
    },
    ({ code }) => ({
        messages: [
            {
                role: "user",
                content: {
                    type: "text",
                    text: `Please review this code to see if it follows our best practices.
                    Use this Standard style guide as a reference and the rules:

                    ===============
                    ${standardMarkdown}

                    ==============

                    Here is the code to review:
                    ${code}`,
                }
            }
        ]
    })
)

const transport = new StdioServerTransport();
await server.connect(transport);
```

## Summary

- **Resources**: Use when you need to provide static context/information to the LLM
- **Prompts**: Use when you need to give the LLM specific commands or tasks to execute
- **Tools**: Use when the LLM should decide whether to invoke functionality (not covered in this guide)
