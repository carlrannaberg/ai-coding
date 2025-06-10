## EXECUTOR MODE — ONE TASK AT A TIME

### Instructions

1. **Read the “Rules & Tips” section in `plan.md` (if it exists) before starting.**
   - Ensure you understand all prior discoveries, insights, and constraints that may impact your execution of the current or following tasks.
2. Open `plan.md` and find the first unchecked (`[ ]`) task.
3. Apply exactly one atomic code change to fully implement this specific task.
   - **Limit your changes strictly to what is explicitly described in the current checklist item.**
   - Do not combine, merge, or anticipate future steps.
   - **If this step adds a new function, class, or constant, do not reference, call, or use it anywhere else in the code until a future checklist item explicitly tells you to.**
   - Only update files required for this specific step.
   - **Never edit, remove, or update any other code, file, or checklist item except what this step describes—even if related changes seem logical.**
4. Fix all lint errors flagged during editing.
5. When there are **no lint errors** and `yarn test` passes:
   a. Commit all code changes related to this task with the message: "`<task>` (auto via agent)".
   b. Mark the task as complete by changing `[ ]` to `[x]` in `plan.md`. *(Do not commit plan.md; it is in .gitignore.)*
   c. Summarize what changed, mentioning affected files and key logic.
6. **Reflect on learnings from this step:**
   - Write down only *general*, *project-wide* insights, patterns, or new constraints that could be **beneficial for executing future tasks**.
   - Do **not** document implementation details, code changes, or anything that only describes what was done in the current step (e.g. “Migrated to TypeScript”, “Added Winston logging”, “Created .gitignore”, etc.). Only capture rules, pitfalls, or lessons that *will apply to future steps* or are needed to avoid repeated mistakes.
   - Use this litmus test: *If the learning is only true for this specific step, or merely states what you did, do not include it.*
   - Before adding a new learning, check if a similar point already exists in the “Rules & Tips” section. If so, merge or clarify the existing point rather than adding a duplicate. Do not remove unique prior rules & tips.
   - Focus on discoveries, best practices, or risks that might impact how future tasks should be approached.
   - **Always** insert the “Rules & Tips” section *immediately after the “Notes” section* in plan.md (never at the end of the file). If “Rules & Tips” does not exist, create it directly after “Notes”.
7. STOP — do not proceed to the next task.
8. If `yarn test` fails:
   a. Fix only the test failures directly caused by your change.
   b. Re-run `yarn test`. Repeat up to 3 attempts.
   c. If tests still fail after 3 attempts, STOP and report all errors and your progress.
9. Never make changes outside the scope of the current task. Do not alter or mark other checklist items except the one just completed.
10. **Always commit code changes before marking the task as done in `plan.md`. Never check off a task until its related changes are safely committed and verified.**
11. If you are unsure or something is ambiguous, STOP and ask for clarification before making any changes.

---

**General Rules**
- Each run must be atomic and focused on a single checklist item.
- Never anticipate or perform actions from future steps, even if you believe it is more efficient.
- Never use new code (functions, helpers, types, constants, etc.) in the codebase until *explicitly* instructed by a checklist item.
- **When committing, always wrap the `git commit -m` message in single quotes.**

---

_Follow these steps for every agent run._