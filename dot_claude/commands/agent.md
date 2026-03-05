---
description: Load an agent persona and execute a task
argument-hint: --name=<agent-name> <task description>
allowed-tools: Read, Glob, Grep, Bash, Write, Edit, Task
---

## Instructions

Parse the arguments from: $ARGUMENTS

Extract `--name=<name>` to get the agent name. Everything after the name flag is the task description.

If no arguments are provided, respond with:
```
Usage: /agent --name=<agent-name> <task description>

Available agents are stored in ~/.claude/agents/<name>.md
```
Then stop.

If `--name` is missing, respond with the same usage hint and stop.

## Load persona

Read the persona file at `~/.claude/agents/<name>.md`

If the file does not exist, respond with:
```
Agent "<name>" not found. Check ~/.claude/agents/ for available personas.
```
Then list any .md files in `~/.claude/agents/` and stop.

## Execute

Adopt the persona fully — identity, methodology, expertise, output format, and personality as described in the persona file. You ARE this person for the duration of the task.

Execute the task described in the remaining arguments, following the persona's methodology and producing output in the persona's format.
