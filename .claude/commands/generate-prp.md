# Create PRP

Please analyze and fix the Github issue: $ARGUMENTS.

Generate a complete PRP for general feature implementation with thorough research. Ensure context is passed to the AI agent to enable self-validation and iterative refinement. Read the feature file first to understand what needs to be created, how the examples provided help, and any other considerations.

The AI agent only gets the context you are appending to the PRP and training data. Assume the AI agent has access to the codebase and the same knowledge cutoff as you, so its important that your research findings are included or referenced in the PRP. The Agent has Websearch capabilities, so pass urls to documentation and examples.

## PLAN

1. Use `gh issue view` to get the details
2. Understand the problem described in the issue
3. Ask clarifying questions if necessary
4. Understand the prior art for this issue
   - Search existing PRPs for previous thoughts related to the issue
   - Search PR's to see if you can find history on this issue
   - Search the codebase for relevant files
5. Think harder about how to break the issue down into a series of small, manageable tasks
6. Document everything directly in the PRP with comprehensive research and planning sections. Include a link to the issue.

### Research Process

1. **Codebase Analysis**

   - Search for similar features/patterns in the codebase
   - Identify files to reference in PRP
   - Note existing conventions to follow
   - Check test patterns for validation approach

2. **External Research**

   - Search for similar features/patterns online
   - Library documentation (include specific URLs)
   - Implementation examples (GitHub/StackOverflow/blogs)
   - Best practices and common pitfalls

3. **User Clarification** (if needed)
   - Specific patterns to mirror and where to find them?
   - Integration requirements and where to find them?

### PRP Generation

Using assets/docs/developer/prp/templates/prp_base.md as template:

#### PRP Structure with Integrated Research and Planning

The PRP should include comprehensive sections that combine traditional PRP content with research notes and planning:

1. **Issue Analysis Section**

   - Link to the GitHub issue
   - Problem breakdown and understanding
   - Requirements extraction

2. **Research Findings Section**

   - Prior art discoveries
   - Relevant codebase patterns
   - External documentation and examples
   - Historical context from PRs/issues

3. **Technical Planning Section**
   - Architecture decisions
   - Implementation approach
   - Risk analysis and mitigation strategies

#### Critical Context to Include and pass to the AI agent as part of the PRP

- **Documentation**: URLs with specific sections
- **Code Examples**: Real snippets from codebase
- **Gotchas**: Library quirks, version issues
- **Patterns**: Existing approaches to follow
- **Research Notes**: All findings from codebase and external research

#### Implementation Blueprint

- Start with pseudocode showing approach
- Reference real files for patterns
- Include error handling strategy
- List tasks to be completed to fulfill the PRP in the order they should be completed

#### Validation Gates (Must be Executable) eg for python

```bash
# Syntax/Style
ruff check --fix && mypy .

# Unit Tests
uv run pytest tests/ -v
```

**_CRITICAL AFTER YOU ARE DONE RESEARCHING AND EXPLORING THE CODEBASE BEFORE YOU START WRITING THE PRP_**

**_ULTRATHINK ABOUT THE PRP AND PLAN YOUR APPROACH THEN START WRITING THE PRP_**

### Output

Save as: `assets/docs/developer/prp/{feature-name}.md`

The PRP should be a comprehensive document that includes:

- All research findings and analysis
- Complete implementation planning
- Technical specifications
- Validation approach
- Everything an AI agent needs for one-pass implementation

### Quality Checklist

- [ ] All necessary context included
- [ ] Research findings documented within PRP
- [ ] Planning and architecture decisions included
- [ ] Validation gates are executable by AI
- [ ] References existing patterns
- [ ] Clear implementation path
- [ ] Error handling documented
- [ ] Issue link and context preserved

Score the PRP on a scale of 1-10 (confidence level to succeed in one-pass implementation using claude codes)

Remember: The goal is one-pass implementation success through comprehensive context all contained within the PRP.

Remember to use the Github CLI (`gh`) for all Github-related tasks.
