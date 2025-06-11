## EXECUTOR MODE — ONE TASK AT A TIME

---

### Instructions

1. **Worktree and Branch Verification**

   - Run the following shell commands:
     ```bash
     export ORIGINAL_WORKTREE="$(pwd)"
     export PLAN_FILE="$ORIGINAL_WORKTREE/plan.md"   # <-- always use THIS path
     echo "ORIGINAL_WORKTREE=$ORIGINAL_WORKTREE"
     echo "PWD: $PWD"
     echo "BRANCH: $(git branch --show-current)"
     ```
     - The **PWD** must end with `-agent-*` (e.g. `/your/repo/feature-agent-1717001123`)
     - The **BRANCH** must start with `agent-` (e.g. `agent-1717001123`)
   - If EITHER condition fails, create a new worktree and agent branch:
     1. **Generate a unique suffix ONCE and reuse for all related commands:**
        ```bash
        export AGENT_SUFFIX=$(date +%s)
        export AGENT_BRANCH="agent-$AGENT_SUFFIX"
        export AGENT_WORKTREE="../$(basename "$PWD")-$AGENT_SUFFIX"
        ```
     2. Record your current branch name as the parent branch:
        ```bash
        export AGENT_PARENT_BRANCH=$(git branch --show-current)
        echo "$AGENT_PARENT_BRANCH" > .agent_parent_branch
        ```
     3. Create and enter the agent worktree/branch:
        ```bash
        git worktree add -b $AGENT_BRANCH $AGENT_WORKTREE
        cd $AGENT_WORKTREE
        export AGENT_WORKTREE=$PWD
        ```
     4. **Immediately install all dependencies in the new agent worktree:**
        ```bash
        yarn install
        ```
     5. **Abort and report an error** if any step fails.
   - **Do not proceed until all environment variables are set, dependencies installed, and worktree checks are satisfied.**

2. **Read the “Learnings” section in `${PLAN_FILE}` (if it exists) before starting.**
   - Ensure you understand all prior discoveries, insights, and new constraints that may impact your execution of the current or following tasks.

3. **Open `${PLAN_FILE}` and find the first unchecked (`[ ]`) task.**

4. **Apply exactly one atomic code change to fully implement this specific task.**
   - **Limit your changes strictly to what is explicitly described in the current checklist item.**
   - Do not combine, merge, or anticipate future steps.
   - **If this step adds a new function, class, or constant, do not reference, call, or use it anywhere else in the code until a future checklist item explicitly tells you to.**
   - Only update files required for this specific step.
   - **Never edit, remove, or update any other code, file, or checklist item except what this step describes—even if related changes seem logical.**

5. **Fix all lint errors flagged during editing.**

6. **Run `yarn test` and ensure all tests pass.**

7. **If `yarn test` fails:**
   - Fix only the test failures directly caused by your change.
   - Re-run `yarn test`. Repeat up to 3 attempts.
   - If tests still fail after 3 attempts, STOP and report all errors and your progress.

8. **Mark the task as complete by changing `[ ]` to `[x]` in `plan.md`.**
   *(Do not commit plan.md; it is in .gitignore.)*

9. **Summarize what changed, mentioning affected files and key logic.**

10. **Reflect on general learnings from this step:**
    - Write down only *general*, *project-wide* insights, patterns, or new constraints that could be **beneficial for executing future tasks**.
    - Do **not** document implementation details, code changes, or anything that only describes what was done in the current step (e.g. “Migrated to TypeScript”, “Added Winston logging”, “Created .gitignore”, etc.). Only capture rules, pitfalls, or lessons that *will apply to future steps* or are needed to avoid repeated mistakes.
    - Use this litmus test: *If the learning is only true for this specific step, or merely states what you did, do not include it.*
    - Before adding a new learning, check if a similar point already exists in the “Learnings” section. If so, merge or clarify the existing point rather than adding a duplicate. Do not remove unique prior learnings.
    - Focus on discoveries, best practices, or risks that might impact how future tasks should be approached.
    - **Always** insert the “Learnings” section *immediately after the “Notes” section* in plan.md (never at the end of the file). If “Learnings” does not exist, create it directly after “Notes”.

11. **Commit and Merge:**
    - Commit all code changes related to this task with the message: "`<task>` (auto via agent)".
    - Push the agent branch.
    - Read the parent branch name from the `.agent_parent_branch` file or from the `$AGENT_PARENT_BRANCH` environment variable.
    - In the main worktree or original repository directory:
      1. Switch to the parent branch.
      2. Pull the latest changes if needed.
      3. Merge the agent branch into the parent branch:
         ```bash
         git merge <agent-branch-name>
         ```
      4. Optionally delete the agent worktree and branch:
         ```bash
         git worktree remove ../<agent-worktree-dir>
         git branch -d <agent-branch-name>
         ```
    - **Report success or any merge conflicts immediately.**
    - Do not proceed to the next task until the merge is complete and conflicts are resolved.

12. **STOP — do not proceed to the next task until the above steps are completed and merged.**

13. **Never make changes outside the scope of the current task. Do not alter or mark other checklist items except the one just completed.**

14. **If you are unsure or something is ambiguous, STOP and ask for clarification before making any changes.**

---

**General Rules**
- Each run must be atomic and focused on a single checklist item.
- Never anticipate or perform actions from future steps, even if you believe it is more efficient.
- Never use new code (functions, helpers, types, constants, etc.) in the codebase until *explicitly* instructed by a checklist item.
- **Absolute-path rule:** For every file-editing tool call, always prefix `target_file` with `$AGENT_WORKTREE/…` so the change is applied in the agent worktree

---

_Follow these steps for every agent run._
