Follow these steps to create a git worktree

Please analyze and fix the Archon issue: $ARGUMENTS. **Check Current Task** → `archon:manage_task(action="get", task_id="...")`



1. Get the details from Archon

2. Get the current project's folder name

3. Create a folder adjacent to the current project's folder and name it {current project folder}-worktrees. For example, if the current project folder is named myapp, create a folder called myapp-worktrees. Both myapp and myapp-worktrees should have the same parent folder.

4. Create a git worktree and branch named with $ARGUMENTS and a good issue description from the main project folder and save it inside the {current project folder name}-worktrees folder that was created.

5. Run the `scripts/copy-hidden-files-to-worktree.sh` script to copy the hidden and env files to new worktree repository

6. **Update Task Status** → `archon:manage_task(action="update", task_id="...", update_fields={"status": "doing"})`

Remember to use the Github CLI (`gh`) for all Github-related tasks.
