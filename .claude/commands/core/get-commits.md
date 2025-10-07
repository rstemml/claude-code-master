---
description: Get the latest changes from ai-chatbot upstream project
argument-hint: [start-commit-sha]
---

# Use gh cli to get all commit-id's since the given id including the given one from:

https://github.com/vercel/ai-chatbot

- use the main branch

1. Read the start commit id sha specified in: $ARGUMENTS
2. Create a list with commit id's sha from this commit to the latest. Include the start commit id sha.

Ask the user if this list is okay. If okay proceed with:

# Create All Tasks in Archon

For EACH commit id identified in the list:
1. Create a corresponding task in Archon using `mcp__archon__manage_task("create", ...)`
2. Set initial status as "todo"
3. Use the same title as it is in the github commit
4. Include detailed descriptions from the list
5. Use the same priorization as it is in the commit history


**IMPORTANT**: Create ALL tasks in Archon upfront before starting implementation. This ensures complete visibility of the work scope.

