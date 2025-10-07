---
description: Excute the Sync from the Upstream Repo
argument-hint: [archon-id]
---


# Start Task
- Move the current task to "doing" status in Archon: `mcp__archon__manage_task("update", task_id=..., status="doing")`
- Use TodoWrite to track local subtasks if needed

# Implement the Task
**IMPORTANT: Use the `upstream-sync-integrator` agent for integration**

- Execute the implementation based on commit id you can find in archon task


# Complete Task
- Once implementation is complete, move task to "review" status: `mcp__archon__manage_task("update", task_id=..., status="review")`
- DO NOT mark as "done" yet - this comes after validation



# Persist CommitID

- Update the commit ID in assets/docs/developer/state/ai-chat-sync-state.md