Please analyze and fix the Github issue: $ARGUMENTS.

# PLAN

    1. use gh issue view' to get the details
    2. understand the problem described in the issue
    3. ask clarifying questions if neccessary
    4. understand the prior art for this issue
    - Search the scratchpads for previous thoughts related to the issue
    - Search PR's to see if you can find history on this issue
    - Search the codebase for relevant files
    5. Think harder about how to break the issue down into a serious of small, manageable tasks
    6. Document your plan in a new scratchpad
    - include the issue name in the file
    - include a link to the issue in the scratchpad

# CREATE

    - Create a new branch for the issue
    - Solve the issue in small, manageable steps, according to your plan
    - Commit your changes after each step

# TEST

    - use puppeteer via MCP to test the changes if you have made changes to the UI
    - Write tests to describe the expected behaviour of your code
    - Run the full test suite to ensure you haven't broken anything
    - If the tests are failing, fix them
    - Ensure that all tests are passing before moving on the top of the next step

# DEPLOY

    - Open a PR and request a review

Remember to use the Github CLI (`gh`) for all Github-related tasks.
