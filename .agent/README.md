# .agent Documentation Index

Welcome to the Claude Code Master documentation. This directory contains comprehensive documentation about the project's architecture, systems, and best practices.

## Quick Navigation

### For New Engineers

Start here to understand the project:

1. **[Project Architecture](system/project_architecture.md)** - Complete system overview
   - Project goals and structure
   - Technology stack and dependencies
   - All architecture layers explained
   - Integration points and data flows

### Documentation Structure

```
.agent/
├── README.md (this file)      # Documentation index and navigation
├── system/                     # System architecture and design
│   └── project_architecture.md # Complete project architecture
└── [Future additions]
    ├── sop/                    # Standard Operating Procedures
    └── tasks/                  # Feature PRDs and implementation plans
```

## What This Project Is

**Claude Code Master** is a comprehensive repository demonstrating advanced Claude Code integration through:

- **Hooks System** - Lifecycle event interception (PreToolUse, PostToolUse, Notification, Stop, SubagentStop)
- **Plugin Architecture** - Modular, reusable workflows with custom agents
- **Custom Agents** - Specialized AI assistants (12+ built-in agents)
- **Slash Commands** - Custom workflows triggered by `/command` syntax
- **Context Engineering** - Systematic context provisioning for consistent AI outputs
- **MCP Server** - Production-ready Model Context Protocol server with GitHub OAuth

## Key Documentation Files

### System Documentation

| File | Purpose | When to Read |
|------|---------|--------------|
| **[project_architecture.md](system/project_architecture.md)** | Complete system architecture, all layers, integration points, data flows | First read for all engineers |

### External Documentation

| File | Purpose | Location |
|------|---------|----------|
| **README.md** | Project overview, setup instructions | `/README.md` |
| **CLAUDE.md** | Global AI assistant rules (Archon integration) | `/CLAUDE.md` |
| **PROMPTS.md** | Prompt collection and examples | `/PROMPTS.md` |
| **Hooks Docs** | Complete Claude Code hooks documentation | `/ai_docs/cc_hooks_docs.md` |
| **Context Engineering** | Context engineering guide and patterns | `/context-engineering-intro/README.md` |
| **MCP Implementation** | MCP server implementation guide | `/use-cases/mcp-server/CLAUDE.md` |

## Quick Reference by Topic

### Hooks

**What:** Intercept Claude Code lifecycle events for deterministic control

**Key Files:**
- `.claude/hooks/*.py` - Hook implementations
- `.claude/settings.json` - Hook configuration
- `logs/*.json` - Hook execution logs
- [Project Architecture - Hook System Layer](system/project_architecture.md#1-hook-system-layer)

**Hook Types:**
1. PreToolUse - Before tool execution (can block)
2. PostToolUse - After tool completion
3. Notification - Claude Code notifications
4. Stop - When Claude finishes
5. SubagentStop - When subagents complete

### Plugins

**What:** Modular, reusable workflows with specialized agents

**Key Files:**
- `plugins/feature-dev/` - Multi-phase feature development
- `plugins/pr-review-toolkit/` - PR review automation
- `plugins/commit-commands/` - Git workflow helpers
- [Project Architecture - Plugin System Layer](system/project_architecture.md#2-plugin-system-layer)

**Available Plugins:**
- Feature Development (7-phase workflow)
- PR Review Toolkit (6 specialized agents)
- Commit Commands (git automation)
- Agent SDK Development
- Security Guidance

### Custom Agents

**What:** Specialized AI assistants with focused capabilities

**Key Files:**
- `.claude/agents/*.md` - Agent definitions (12+ agents)
- [Project Architecture - Custom Agents Layer](system/project_architecture.md#3-custom-agents-layer)

**Built-in Agents:**
- azure-devops-engineer
- code-reviewer
- data-scientist
- debugger
- design-systems-expert
- documentation-engineer
- nextjs-vercel-ai-engineer
- product-owner-story-writer
- security-audit-engineer
- test-coverage-engineer
- test-runner
- upstream-sync-integrator

### Slash Commands

**What:** Custom workflows triggered by `/command`

**Key Files:**
- `.claude/commands/*.md` - Command definitions (20+ commands)
- [Project Architecture - Slash Commands Layer](system/project_architecture.md#4-slash-commands-layer)

**Command Categories:**
- Project Management (/specify, /plan, /tasks, etc.)
- Development Workflow (/constitution, /dedupe, etc.)
- Context Engineering (/generate-prp, /execute-prp)
- Customer/Eval Workflows
- Core Operations

### Context Engineering

**What:** Systematic context provisioning for AI assistants

**Key Files:**
- `context-engineering-intro/` - Complete guide
- `context-engineering-intro/PRPs/` - Product Requirement Prompts
- [Project Architecture - Context Engineering Layer](system/project_architecture.md#5-context-engineering-layer)

**Workflow:**
1. Create INITIAL.md (feature requirements)
2. Run /generate-prp (research and create PRP)
3. Run /execute-prp (implement feature)
4. Validation loops ensure quality

### MCP Server

**What:** Production-ready Model Context Protocol server with GitHub OAuth

**Key Files:**
- `use-cases/mcp-server/` - Complete implementation
- `use-cases/mcp-server/CLAUDE.md` - Implementation guide
- [Project Architecture - Use Cases Layer](system/project_architecture.md#6-use-cases-layer)

**Features:**
- GitHub OAuth 2.0 authentication
- PostgreSQL database access
- Role-based access control
- Cloudflare Workers deployment
- Optional Sentry monitoring

## Common Tasks

### Understanding the Codebase

1. Read [Project Architecture](system/project_architecture.md)
2. Explore `.claude/` directory structure
3. Review sample plugins in `plugins/`
4. Check use-case examples in `use-cases/`

### Adding New Hooks

1. Create Python script in `.claude/hooks/`
2. Use UV single-file script format
3. Update `.claude/settings.json` configuration
4. Test with sample JSON input
5. Document in [Project Architecture](system/project_architecture.md)

### Creating New Plugins

1. Create directory in `plugins/{plugin-name}/`
2. Add agents in `agents/` subdirectory
3. Add commands in `commands/` subdirectory
4. Create README.md with usage instructions
5. Document in [Project Architecture](system/project_architecture.md)

### Building Custom Agents

1. Create Markdown file in `.claude/agents/`
2. Define frontmatter (name, description, tools, model, color)
3. Write agent instructions and behavior
4. Test with Task tool
5. Document in [Project Architecture](system/project_architecture.md)

### Implementing MCP Tools

1. Navigate to `use-cases/mcp-server/`
2. Create tool module (e.g., `src/tools/your-feature-tools.ts`)
3. Define Zod schemas for validation
4. Implement tool handlers with error handling
5. Register in `src/tools/register-tools.ts`
6. Update documentation

### Using Context Engineering

1. Read `context-engineering-intro/README.md`
2. Create INITIAL.md with feature requirements
3. Run `/generate-prp INITIAL.md`
4. Review generated PRP in `PRPs/`
5. Run `/execute-prp PRPs/your-feature.md`
6. Validate implementation

## Development Workflow

### Daily Development

1. **Start:** Check hook logs for any issues
2. **Develop:** Use agents and commands as needed
3. **Test:** Verify hooks execute correctly
4. **Review:** Check logs for patterns and issues
5. **Commit:** Follow git workflow with `/commit`

### Testing Changes

**Hooks:**
```bash
# Test hook manually
echo '{"tool_name":"Test","tool_input":{}}' | \
  uv run .claude/hooks/pre_tool_use.py

# Check logs
cat logs/pre_tool_use.json | jq
```

**MCP Server:**
```bash
cd use-cases/mcp-server
npm run test        # Unit tests
wrangler dev        # Local testing
```

**Agents:**
- Launch with Task tool
- Review output quality
- Validate recommendations

### Debugging

**Hook Issues:**
- Check `.claude/settings.json` syntax
- Verify UV installation
- Review hook logs in `logs/`
- Test with manual JSON input

**MCP Issues:**
- Check Wrangler is running
- Verify OAuth configuration
- Test with MCP Inspector
- Review Sentry (if enabled)

**Agent Issues:**
- Check available tools
- Verify file permissions
- Review agent prompts
- Test with simpler inputs

## Architecture Patterns

### Hook Patterns

**Security Hooks:**
- Block dangerous commands (rm -rf)
- Prevent sensitive file access (.env)
- Validate inputs before execution

**Logging Hooks:**
- Capture all tool usage
- Convert transcripts to readable format
- Audit trail for analysis

**Notification Hooks:**
- TTS alerts for user input needed
- AI-generated completion messages
- Subagent completion notifications

### Agent Patterns

**Multi-Agent Workflows:**
- Launch agents in parallel
- Each agent focuses on specific aspect
- Consolidate findings for comprehensive analysis

**Specialized Agents:**
- Single responsibility (code review, architecture, debugging)
- Specific tool access patterns
- Targeted output formats

### MCP Patterns

**Authentication:**
- OAuth 2.0 flow with signed cookies
- User context in Durable Objects
- Per-user session management

**Authorization:**
- Role-based tool access
- Permission validation before execution
- Username whitelists for privileged operations

**Data Access:**
- Connection pooling (max 5 connections)
- SQL injection protection
- Prepared statements for caching

## Performance Guidelines

### Hook Performance

- Keep hooks fast (<1 second typical)
- Avoid blocking operations
- Use async file I/O for logging
- Maximum 60 second timeout

### Agent Performance

- Launch multiple agents concurrently when possible
- Agents have no rate limits but use judiciously
- Large file reading is efficient
- Return focused, actionable results

### MCP Performance

- PostgreSQL connection pooling (max 5)
- Prepared statements enabled
- 10-second connect timeout
- 20-second idle timeout

## Security Considerations

### Hook Security

- PreToolUse blocks dangerous commands
- Pattern-based validation for safety
- No sensitive data in logs
- Exit code 2 blocks execution

### MCP Security

- GitHub OAuth 2.0 authentication
- HMAC-signed approval cookies
- SQL injection protection
- Error message sanitization
- Role-based access control

### Permission System

- Granular tool access control
- Wildcard pattern matching
- Allow/deny lists in settings.json

## Troubleshooting

### Common Issues

| Issue | Solution | Reference |
|-------|----------|-----------|
| Hooks not executing | Check settings.json, UV installation, permissions | [Troubleshooting Guide](system/project_architecture.md#troubleshooting-guide) |
| MCP connection failed | Verify Wrangler, OAuth secrets, Claude Desktop config | [Troubleshooting Guide](system/project_architecture.md#troubleshooting-guide) |
| Agent failures | Check tools, permissions, prompt clarity | [Troubleshooting Guide](system/project_architecture.md#troubleshooting-guide) |
| Permission errors | Review settings.json permissions and patterns | [Troubleshooting Guide](system/project_architecture.md#troubleshooting-guide) |

## Related Resources

### Official Documentation

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Claude Code Hooks](https://docs.anthropic.com/en/docs/claude-code/hooks)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Cloudflare Workers](https://developers.cloudflare.com/workers/)

### External Resources

- [UV Documentation](https://docs.astral.sh/uv/)
- [Context Engineering Best Practices](https://www.philschmid.de/context-engineering)
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/)
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)

### Community

- [GitHub Repository](https://github.com/anthropics/claude-code) - Claude Code issues and feedback
- [IndyDevDan YouTube](https://www.youtube.com/@indydevdan) - AI coding tips and tricks
- [Agentic Engineer](https://agenticengineer.com/principled-ai-coding) - Principles of AI Coding

## Future Documentation

As the project evolves, we'll add:

### Standard Operating Procedures (sop/)

- How to add a schema migration
- How to add a new page route
- How to configure a new agent
- How to create custom hooks
- How to deploy MCP servers
- How to write PRPs effectively

### Feature Tasks (tasks/)

- Individual feature PRDs
- Implementation plans
- Progress tracking
- Post-implementation reviews

### Best Practices

- Code style guidelines
- Testing strategies
- Documentation standards
- Security patterns

## Maintenance

### Keeping Documentation Updated

After implementing features:
1. Update relevant system documentation
2. Add new files to this index
3. Update quick reference sections
4. Add troubleshooting entries if needed

### Documentation Standards

- Use clear, descriptive headings
- Include code examples where helpful
- Reference specific files with paths
- Keep navigation links updated
- Use tables for structured information

## Getting Help

If you need assistance:

1. Check this README for topic location
2. Read the relevant documentation file
3. Review troubleshooting guide
4. Check external resources
5. Search GitHub issues
6. Ask in community channels

## Contributing

When adding documentation:

1. Follow existing structure and format
2. Update this index with new files
3. Add quick reference entries
4. Include examples and code snippets
5. Test all links and references
6. Keep language clear and concise

---

**Last Updated:** 2025-10-11

**Documentation Version:** 1.0.0

**Related Files:**
- [Project Architecture](system/project_architecture.md)
- [Main README](/README.md)
- [CLAUDE.md](/CLAUDE.md)
