Follow these steps to create a git worktree

Please analyze and fix the Github issue: $ARGUMENTS.

1. use gh issue view' to get the details

2. Get the current project's folder name

3. Create a folder adjacent to the current project's folder and name it {current project folder}-worktrees. For example, if the current project folder is named myapp, create a folder called myapp-worktrees. Both myapp and myapp-worktrees should have the same parent folder.

4. Create a git worktree and branch named with $ARGUMENTS and a good issue description from the main project folder and save it inside the {current project folder name}-worktrees folder that was created.

5. Run the `scripts/copy-hidden-files-to-worktree.sh` script to copy the hidden and env files to new worktree repository

Remember to use the Github CLI (`gh`) for all Github-related tasks.
