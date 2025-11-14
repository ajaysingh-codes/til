# Claude Code

> **🟡 Intermediate** | ⏱️ 20 minutes | [Quick Reference](claude-code-cheatsheet.md)

## Overview

A comprehensive guide to using Claude Code for AI pair programming.

### Prerequisites
- Basic command line knowledge
- Familiarity with git workflows
- Experience with at least one code editor
- Understanding of basic programming concepts

### What You'll Learn
- Claude Code's thinking modes for complex reasoning
- Context management strategies
- Memory system with claude.md files
- Hooks for lifecycle automation
- Subagents for specialized tasks
- Git worktrees for parallel development
- One-shot commands for CI/CD integration

### Related Topics
- [Cursor](cursor.md) - Alternative AI code editor
- [MCP](../MCP/README.md) - Extend Claude with custom tools

## Getting Started

- `claude --model=sonnet` - Put Claude in a specific model mode
- Use the `-p` flag to run Claude in a one-shot command without entering the CLI (useful for CI/CD processes like addressing lint errors or writing GitHub issue synopses)

## Thinking Mode for Extended Reasoning

Model takes an analytical pause to understand complex context:

- **Basic thinking**: Include the word `think` or `think this through` - triggers around 4,000 tokens of reasoning
- **Deep thinking**: `Think hard` or `mega think` - allocates around 10,000 tokens
- **Maximum thinking**: `Think harder` or `ultrathink` - 31,999 tokens to reason

## Context Management

### Compacting Context Windows
- `/compact` - Auto-compact (might lose content)
- Manually compact with specific instructions during a chat ("hey this is what I care about to get in the summary")

### Clearing Context
- `/clear` - Completely clear the context

### Memory Management
- `/init` - Creates a `claude.md` file (Claude Code reads this file every time before it does anything)
- `#` - Adds to the memory (claude.md file)

### Automate Context Summaries
- Write to-do items as a markdown checklist and check off next tasks as you go
- Multiple claude files - up and down the tree (in specific directories to have different rules for different parts of monorepo)

## Advanced Usage

### Continuous Mode
```bash
claude --continue --dangerously-skip-permissions
```

## Commands

Similar to notepads in Cursor - shortcuts for prompts that you can reuse across sessions.

Learn more: [Claude Code Commands Documentation](https://code.claude.com/docs/en/commands)

## Claude Hooks

Bash commands that run at different lifecycle moments:
- Session start, pre-compact, and on stop
- Tool use stages like PreToolUse and PostToolUse

**Resources:**
- [Official Hooks Documentation](https://code.claude.com/docs/en/hooks)
- [Claude Hooks Examples](https://github.com/johnlindquist/claude-hooks)
- [Awesome Claude Code - Hooks](https://github.com/hesreallyhim/awesome-claude-code?tab=readme-ov-file#hooks-)

## Subagents

Subagents help you work with specialized contexts and perspectives.

### Key Benefits
- Each subagent gets its own system prompt - convincing it to look at tasks from a specific perspective
- Shards the context window without contaminating the main context window
- Each subagent has their own context window
- Frees up main agent from having to compact the context window as often

### Managing Subagents
- `/agents` - View and manage agents at user level and project level
- Examples: Copy editor, Test engineer with Playwright MCP access

### Best Practices

**Start Small**
- Begin with simple, focused agents

**Clear Names and Focused Roles**
- Limit tool use per agent
- Example: Test-engineer has the ability to have 'Playwright' MCP access

**Include Them in Claude.md**
- Document your agents in your project's claude.md file

**Keep Descriptions Concise**
- Clear, brief descriptions work best

**Name Your Agents**
- Don't let Claude pick generic names - be explicit and descriptive

## Git Worktrees

Allows spinning off multiple timelines from the main branch, enabling parallel work on the same file system.

---

## Additional Resources

- [Claude Code Documentation](https://code.claude.com/docs)
- [Awesome Claude Code](https://github.com/hesreallyhim/awesome-claude-code)
