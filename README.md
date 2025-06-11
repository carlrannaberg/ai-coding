# AI Coding Prompts

Prompts for implementing a Plan & Execute workflow in Cursor or similar AI coding tool, which is inspired by Cline's built-in Plan & Act modes.

## Files

- [`planner.md`](./prompts/planner.md) - Planning mode prompt
- [`executor.md`](./prompts/executor.md) - Execution mode prompt

## Setup in Cursor

### Planner Mode
1. Create a new custom mode called "Planner"
2. Model: Gemini 2.5 Pro Max
3. Disable edit tools (uncheck "Edit" in tool settings)
4. Enable full folder context in settings
5. Add keybinding for quick access (e.g., `⌘⇧P`)
6. Paste the [planner.md](./prompts/planner.md) prompt

Example `plan.md` file:
```markdown
# Plan: Refactor User Validation

## Tasks
- [ ] 1.0 Update user schema to allow null values for optional fields
- [ ] 1.1 Add tests for null value handling
- [ ] 1.2 Update API endpoints to handle new validation rules
```

### Executor Mode
1. Create a new custom mode called "Executor"
2. Model: Claude Sonnet 4
3. Enable all tools
4. Add keybinding for quick access (e.g., `⌘⇧E`)
5. Enable auto-apply edits, auto-run, and auto-fix errors
6. Paste the [executor.md](./prompts/executor.md) prompt

## Usage

1. Switch to Planner mode
2. Include relevant files and folders in the context
3. Describe your idea and create a `plan.md` file through iterative Q&A
4. Switch to Executor mode
5. Add `plan.md` to the context
6. Type "go" to start execution
7. The executor will implement the task and commit the changes
8. Type "go" to continue to the next task until all tasks are complete

## Isolated Executor

The [isolated-executor.md](./prompts/isolated-executor.md) prompt is a variant of the executor prompt that is used to run the executor agent in parallel in its own git worktree.

### Setup

1. Create a new custom mode called "Isolated Executor"
2. Model: Claude Sonnet 4
3. Install [mcp-text-editor](https://github.com/tumf/mcp-text-editor) MCP server
4. Enable all tools, including the new text edit MCP server
5. Disable native edit tools (uncheck "Edit" in tool settings)
6. Add keybinding for quick access (e.g., `⌘⇧I`)
7. Paste the [isolated-executor.md](./prompts/isolated-executor.md) prompt

### Usage

1. Open 2-3 new chat tabs in Cursor
2. In each tab, switch to Isolated Executor mode
3. Add `plan.md` to the context in each tab
4. Type the task number you want each agent to work on (e.g., "work on task 8" in tab 1, "work on task 12" in tab 2)
5. Each agent will work on their assigned task in separate worktrees
6. Agents will merge their changes back to the parent branch when complete
7. Monitor progress and assign new tasks as agents finish

## Notes

- Make sure to adjust the plan along the way, when spotting missing details or constraints
- Executor prompt currently expects `plan.md` to be in the root of the project and included in `.gitignore`. You can change the prompt to also commit the `plan.md` file.
- Executor prompt expects `yarn test` for running tests. You can change it to your preferred test command.
- Feel free to adapt the prompts for your specific needs