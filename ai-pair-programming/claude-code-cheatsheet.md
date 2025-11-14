# Claude Code Quick Reference

> **Essential commands, shortcuts, and patterns for Claude Code**

## Getting Started

```bash
claude                                    # Start interactive CLI
claude --model=sonnet                     # Specify model
claude -p "your prompt here"              # One-shot command (no CLI)
claude --continue --dangerously-skip-permissions  # Continuous mode
```

## Thinking Modes

| Command | Tokens | When to Use |
|---------|--------|-------------|
| `think` or `think this through` | ~4,000 | Basic analysis |
| `think hard` or `mega think` | ~10,000 | Complex problems |
| `think harder` or `ultrathink` | ~32,000 | Maximum reasoning needed |

**Tip:** Use thinking mode for complex debugging, architecture decisions, or when Claude needs deep context understanding.

## Essential Commands

### Context Management
```bash
/compact              # Auto-compact context (may lose content)
/clear                # Completely clear context
/init                 # Create claude.md (read before every action)
#                     # Add to memory (writes to claude.md)
```

### Agent Management
```bash
/agents               # View and manage subagents
```

## Memory System

### claude.md File
- Created with `/init`
- Read by Claude before every action
- Use for project-specific instructions
- Can have multiple files up/down the tree (monorepo support)

**Example claude.md:**
```markdown
# Project Rules
- Use TypeScript strict mode
- Follow Airbnb style guide
- Write tests for all new features
- Use Playwright MCP for testing
```

## Subagents

### What Are They?
- Specialized contexts with own system prompts
- Separate context windows (doesn't contaminate main)
- Focused tool access per agent

### Best Practices

| Practice | Description |
|----------|-------------|
| **Start Small** | Begin with simple, focused agents |
| **Clear Names** | Be explicit (not generic names) |
| **Focused Roles** | Limit tools per agent |
| **Document** | Include agent descriptions in claude.md |
| **Concise** | Keep descriptions brief |

### Example Agents
```
test-engineer     → Playwright MCP access only
copy-editor       → Writing/documentation focus
security-auditor  → Code security analysis
```

## Hooks

Bash commands that run at lifecycle moments.

### Available Hooks
- `SessionStart` - When session begins
- `PreCompact` - Before context compaction
- `OnStop` - When stopping
- `PreToolUse` - Before tool execution
- `PostToolUse` - After tool execution

### Resources
- [Official Hooks Docs](https://code.claude.com/docs/en/hooks)
- [Claude Hooks Examples](https://github.com/johnlindquist/claude-hooks)
- [Awesome Claude Code - Hooks](https://github.com/hesreallyhim/awesome-claude-code?tab=readme-ov-file#hooks-)

## Commands (Slash Commands)

Reusable prompts across sessions (similar to Cursor notepads).

**Location:** `.claude/commands/`

**Learn More:** [Commands Documentation](https://code.claude.com/docs/en/commands)

## Git Worktrees

Spin off multiple timelines from main branch for parallel work on the same filesystem.

```bash
git worktree add ../feature-branch feature-branch
git worktree list
git worktree remove ../feature-branch
```

## Quick Tips

### Context Management Strategy
1. Use `/init` to set project context
2. Add to-do checklists in claude.md
3. Check off tasks as you complete them
4. Manually compact with specific instructions
5. Use subagents for isolated work

### One-Shot Mode (`-p`)
Perfect for:
- CI/CD pipelines
- Automated lint fixing
- GitHub issue processing
- Quick status checks

### Model Selection
```bash
--model=sonnet       # Best for most tasks
--model=opus         # Complex reasoning
--model=haiku        # Fast, simple tasks
```

## Common Workflows

### 1. Starting a New Project
```bash
claude --model=sonnet
/init
# Add project rules and guidelines
```

### 2. Code Review with Subagent
```bash
/agents
# Create review-agent with specific focus
# Let it analyze code in isolation
```

### 3. Long Running Task
```bash
claude --continue
# Work on complex feature
# Claude maintains context across sessions
```

### 4. Quick Fix in CI
```bash
claude -p "Fix all ESLint errors"
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Context too large | Use `/compact` or subagents |
| Lost important info | Manually compact with specific instructions |
| Need project rules | Create/update claude.md with `/init` |
| Repeated questions | Add answers to claude.md |
| Need specialized help | Create focused subagent |

## Advanced Patterns

### Markdown Checklist in claude.md
```markdown
## Current Sprint
- [x] Set up authentication
- [x] Create user dashboard
- [ ] Add payment integration
- [ ] Write integration tests
```

### Monorepo Setup
```
/root/claude.md           # Global rules
/root/frontend/claude.md  # Frontend-specific
/root/backend/claude.md   # Backend-specific
```

### Subagent for Testing
```
Name: test-engineer
Tools: Playwright MCP
Focus: Write and run integration tests
Prompt: "You are a QA engineer. Focus on edge cases and user flows."
```

## Learn More

- [Full Claude Code Guide](claude-code.md)
- [Claude Code Documentation](https://code.claude.com/docs)
- [Awesome Claude Code](https://github.com/hesreallyhim/awesome-claude-code)

## Quick Command Reference

```bash
# Most Used
/init                 # Setup project memory
/compact              # Reduce context
/clear                # Start fresh
/agents               # Manage subagents
#                     # Add to memory

# Model Selection
--model=sonnet
--model=opus
--model=haiku

# Modes
-p                    # One-shot
--continue            # Continuous mode

# Thinking
think                 # 4k tokens
think hard            # 10k tokens
think harder          # 32k tokens
```
