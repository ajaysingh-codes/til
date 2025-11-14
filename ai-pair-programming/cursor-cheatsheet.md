# Cursor Quick Reference

> **Essential shortcuts and commands for AI-powered development**

## Core Shortcuts

| Shortcut | Feature | Use Case |
|----------|---------|----------|
| `Cmd/Ctrl + K` | **Inline Edit** | Modify code in place with AI |
| `Cmd/Ctrl + L` | **AI Chat** | Ask questions about codebase |
| `Cmd/Ctrl + I` | **Agent** | Multi-file complex tasks |

## The 3 Core Features

### 1. Inline Edit (`Cmd + K`)
- Modify code directly where cursor is
- Quick refactoring
- Single-file changes

**Example prompts:**
```
"Add error handling"
"Convert to async/await"
"Add TypeScript types"
```

### 2. AI Chat (`Cmd + L`)
- Has full codebase context
- Ask questions
- Brainstorm solutions
- Code explanations

**Example prompts:**
```
"How does authentication work?"
"Where is user data validated?"
"Explain this function"
```

### 3. Agent (`Cmd + I`)
- Complex multi-file tasks
- Creates and executes plans
- Autonomous implementation

**Example prompts:**
```
"Add dark mode to the entire app"
"Refactor API layer to use repositories"
"Set up testing infrastructure"
```

## Advanced Features

### Mirroring
Replicate function structure/logic inline
- Select code
- Use Inline Edit
- "Mirror this pattern"

### Background Agents
Asynchronous work in remote environment
- Works while you do other things
- Perfect for:
  - Codebase-wide refactoring
  - Documentation generation
  - Predictable bug fixes
  - Repository cloning + feature work

## Model Selection

| Model | Best For | Context Window |
|-------|----------|----------------|
| **Sonnet 4.5** | Day-to-day coding | Standard |
| **Gemini 2.5 Pro** | Planning, reviews, large codebases | Very Large |
| **GPT-4/o1** | Architecture, design patterns | Standard/Large |

### When to Use Each

```
Sonnet 4.5       → Most tasks, execution, implementation
Gemini 2.5 Pro   → Code reviews, understanding large systems
GPT Models       → Deep refactoring, architectural decisions
```

## Cursor Rules

Custom instructions applied to every AI interaction.

### Rule Types

| Type | Scope | Location | Version Control |
|------|-------|----------|-----------------|
| **Project-level** | Specific project | `.cursor/rules` | ✅ Commit to git |
| **User-level** | All projects | Global settings | ❌ Local only |

### Application Modes

```
Always          → Every interaction
Auto Attached   → Based on file glob patterns
Agent Requested → AI decides when to apply
Manually        → You choose when to use
```

### Best Practices

**Use Positive + Negative Prompts:**
```markdown
# DO
- Use TypeScript strict mode
- Write unit tests for all functions
- Follow Airbnb style guide

# DON'T
- Use 'any' type
- Skip error handling
- Commit console.logs
```

**Example .cursor/rules:**
```markdown
## Project Rules
- Framework: Next.js 14 with App Router
- Styling: Tailwind CSS
- State: Zustand for global state
- Testing: Vitest + Testing Library
- Always use server components unless client interactivity needed
```

## MCP Server Integration

Extend LLM capabilities with custom tools.

### Use Cases
- Update documentation automatically
- Enforce linting rules
- Custom API integrations
- Project-specific tooling

**Learn more:** [MCP Guide](../MCP/README.md)

## Context Engineering

The key to effective AI interaction.

### Three Principles

1. **Provide Specifics**
   - Exact files
   - Error logs
   - Test outputs

2. **Share the Plan**
   - Requirements docs
   - Architecture diagrams
   - Design specs

3. **Iterate and Verify**
   - Brainstorm with AI
   - Execute suggestions
   - Verify results
   - Use multiple LLMs to find weaknesses

## Quick Workflows

### 1. Bug Fix
```
Cmd + L → "Where is [feature] implemented?"
Cmd + K → "Fix the [specific issue]"
```

### 2. Feature Addition
```
Cmd + I → "Add [feature] with [requirements]"
Review plan → Approve → Execute
```

### 3. Code Review
```
Use Gemini 2.5 Pro (large context)
Cmd + L → "Review this PR for issues"
```

### 4. Refactoring
```
Cmd + I → "Refactor [component] to use [pattern]"
Agent creates multi-file plan
```

### 5. Understanding Code
```
Cmd + L → "How does [system] work?"
Ask follow-ups
Request diagrams
```

## Pro Tips

### Effective Prompting
```
❌ "Make this better"
✅ "Add input validation and error handling to this form"

❌ "Fix the bug"
✅ "The login fails with 401 error when using special characters.
    Fix the encoding issue in the auth middleware"

❌ "Add tests"
✅ "Add unit tests for the payment processing logic,
    covering success cases and error scenarios"
```

### Model Switching Strategy
1. Start with Sonnet for implementation
2. Switch to Gemini for review
3. Use GPT for complex architectural changes

### Context Management
- Keep relevant files open
- Close unrelated tabs
- Use `.cursorignore` to exclude files
- Reference specific files in prompts

### Agent Usage
- Be specific about requirements
- Review the plan before execution
- Break very large tasks into phases
- Use background agents for independent work

## Troubleshooting

| Issue | Solution |
|-------|----------|
| AI missing context | Open relevant files, reference specifically |
| Wrong suggestions | Check Cursor rules, provide better context |
| Slow responses | Switch to faster model (Sonnet) |
| Large refactor issues | Break into smaller tasks |
| Agent stuck | Review plan, provide clarification |

## Keyboard Shortcuts Summary

```
Cmd/Ctrl + K    → Inline Edit
Cmd/Ctrl + L    → AI Chat
Cmd/Ctrl + I    → Agent
Cmd/Ctrl + /    → Toggle comment (standard)
Cmd/Ctrl + P    → Quick file open (standard)
```

## Configuration Hierarchy

```
User-level rules (global)
    ↓
Project-level rules (.cursor/rules)
    ↓
Inline prompts (specific request)
```

Lower levels override higher levels.

## Common Patterns

### Setup New Project
1. Create `.cursor/rules` with standards
2. Set application mode (always/auto)
3. Configure MCP servers if needed
4. Test with simple prompt

### Daily Development
1. Use Chat to understand
2. Use Inline Edit for quick changes
3. Use Agent for complex features
4. Switch models based on task

### Code Review
1. Switch to Gemini 2.5 Pro
2. Open PR files
3. Ask for comprehensive review
4. Address findings with Inline Edit

## Learn More

- [Full Cursor Guide](cursor.md)
- [Cursor Documentation](https://cursor.sh/docs)
- [Context Engineering Best Practices](cursor.md#1-core-principles-context-engineering)

## Quick Reference Card

```
┌─────────────────────────────────────────┐
│  CURSOR QUICK COMMANDS                  │
├─────────────────────────────────────────┤
│  Cmd+K  → Quick edits                   │
│  Cmd+L  → Ask questions                 │
│  Cmd+I  → Complex tasks                 │
│                                         │
│  MODELS                                 │
│  Sonnet    → Implementation             │
│  Gemini    → Planning/Review            │
│  GPT       → Architecture               │
│                                         │
│  RULES                                  │
│  .cursor/rules → Project rules          │
│  Settings      → User rules             │
└─────────────────────────────────────────┘
```
