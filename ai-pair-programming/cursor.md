# Cursor: AI-First Code Editor

> **🟢 Beginner** | ⏱️ 15 minutes | [Quick Reference](cursor-cheatsheet.md)

## Overview

Cursor is a fork of VS Code designed specifically for AI-powered development. This guide covers essential features, best practices, and advanced customization.

### Prerequisites
- Basic coding experience in any language
- Familiarity with VS Code (helpful but not required)
- Understanding of what LLMs (AI models) are

### What You'll Learn
- Three core Cursor features: Inline Edit, Chat, and Agent
- Context engineering principles for better AI interactions
- Model selection strategies (Sonnet, Gemini, GPT)
- Cursor Rules for project-specific AI behavior
- Background agents for asynchronous work
- MCP server integration

### Related Topics
- [Claude Code](claude-code.md) - CLI-based AI pair programming
- [MCP](../MCP/README.md) - Extend LLMs with custom tools

## 1. Core Principles: Context Engineering

Before diving into the tools, it's essential to understand that AI models are not stateful and have a limited "context window." Effective AI interaction, or **context engineering**, is key.

- **Provide Specifics:** Feed the AI precise information, such as exact files, error logs, or failing test outputs.
- **Share the Plan:** Give the AI high-level documents like product requirements or architecture designs to guide its responses.
- **Iterate and Verify:** Use a cycle of brainstorming with the AI, executing on its suggestions, and verifying the results. It's often useful to leverage multiple different LLMs to find weaknesses in an AI-generated plan.

## 2. Cursor: The AI-First Code Editor

Cursor is a fork of VS Code designed specifically for AI-powered development.

### Key Features

- **Inline Edit (`Cmd + K`):** Modify code directly in place with an AI prompt.
- **AI Chat (`Cmd + L`):** Chat with an AI that has indexed your entire codebase. This is great for asking questions or brainstorming.
- **Agent (`Cmd + I`):** For complex, multi-file tasks. Describe a goal, and the agent will create and execute a plan to achieve it.
- **Mirroring:** A useful inline chat feature to replicate a function's structure or logic.

### Choosing the Right Model for the Job

- **Sonnet 4.5:** Best for most day-to-day coding and execution tasks.
- **Gemini 2.5 Pro:** Its large context window makes it ideal for planning, code reviews, and understanding large codebases.
- **OpenAI's GPT Models:** Excellent for deep architectural restructuring and applying design patterns.

### Advanced Customization

- **Cursor Rules:** Set persistent instructions for the AI to follow in every interaction. This is powerful for enforcing coding standards. Use both positive and negative prompts for the best results.
- **Types of rules**:
	- Project-level rules: rules specific to a given project, lives in the `.cursor/rules` directory and can commit them into git and share with your team
	- User-level rules: applies globally to all projects on your machine and are not checked into version control (individual coding preferences)
- **Applying the rules**:
	- Always
	- *Auto Attached*: Define some globs for types of files you want this rule applied to.
	- Agent Requested: Leave it up to AI to decide
	- *Manually*: Manually choose to pull particular rules in context

- **Background Agents:** Asynchronous agents that can work in a remote environment. You can delegate tasks like cloning a repo, working on a feature, and generating a PR with minimal supervision. This is great for codebase-wide chores, documentation, or predictable bug fixes.

- **MCP Server:** Extend an LLM's capabilities by giving it access to APIs. For example, you can create custom tools for updating documentation or enforcing linting rules.
