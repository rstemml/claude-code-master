# Execute BASE PRP

Implement a feature using using the PRP file.

## PRP File: $ARGUMENTS

## Preparation

    - Create a new branch for the issue
    - Solve the issue in small, manageable steps, according to your plan
    - Commit your changes after each step

## Execution Process

1. **Load PRP**

   - Read the specified PRP file
   - Understand all context and requirements
   - Follow all instructions in the PRP and extend the research if needed
   - Ensure you have all needed context to implement the PRP fully
   - Do more web searches and codebase exploration as needed

2. **ULTRATHINK**

   - Think hard before you execute the plan. Create a comprehensive plan addressing all requirements.
   - Break down complex tasks into smaller, manageable steps using your todos tools.
   - Use the TodoWrite tool to create and track your implementation plan.
   - Identify implementation patterns from existing code to follow.

3. **Execute the plan**

   - Execute the PRP
   - Implement all the code

4. **Validate**

   - Run each validation command
   - Fix any failures
   - Re-run until all pass
   - use puppeteer via MCP to test the changes if you have made changes to the UI
   - Write tests to describe the expected behaviour of your code
   - Run the full test suite to ensure you haven't broken anything
   - If the tests are failing, fix them
   - Ensure that all tests are passing before moving on the top of the next step

5. **Complete**

   - Ensure all checklist items done
   - Run final validation suite
   - Report completion status
   - Read the PRP again to ensure you have implemented everything

6. **Reference the PRP**
   - You can always reference the PRP again if needed

Note: If validation fails, use error patterns in PRP to fix and retry.

## DEPLOY

    - Request if the agent should open a PR for reviewing

Remember to use the Github CLI (`gh`) for all Github-related tasks.
