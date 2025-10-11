# Project Architecture

## Project Overview

**Claude Code Master** is a comprehensive repository demonstrating advanced Claude Code integration patterns through hooks, plugins, custom agents, slash commands, and context engineering techniques. This project serves as both a working implementation and an educational resource for mastering Claude Code customization.

### Project Goal

Enable developers to:
- Master Claude Code hooks for deterministic control over AI behavior
- Build custom plugins with specialized agents and slash commands
- Implement context engineering patterns for consistent AI outputs
- Create production-ready MCP (Model Context Protocol) servers
- Understand and apply advanced AI coding workflows

## Repository Structure

```
claude-code-master/
├── .agent/                      # Documentation (this folder)
│   ├── system/                  # System architecture docs
│   └── README.md               # Documentation index
│
├── .claude/                     # Claude Code configuration
│   ├── hooks/                   # Lifecycle hook implementations
│   ├── agents/                  # Custom agent definitions
│   ├── commands/                # Slash command definitions
│   └── settings.json            # Hook & permission configuration
│
├── plugins/                     # Reusable plugin packages
│   ├── feature-dev/             # Feature development workflow
│   ├── pr-review-toolkit/       # PR review automation
│   ├── commit-commands/         # Git workflow helpers
│   ├── agent-sdk-dev/           # Agent SDK development tools
│   └── security-guidance/       # Security best practices hooks
│
├── use-cases/                   # Working examples
│   ├── mcp-server/              # MCP server with GitHub OAuth
│   ├── template-generator/      # PRP template generation
│   └── pydantic-ai/             # Pydantic AI integration
│
├── context-engineering-intro/   # Context engineering education
│   ├── PRPs/                    # Product Requirement Prompts
│   └── use-cases/               # Nested use-case examples
│
├── ai_docs/                     # AI assistant documentation
├── scripts/                     # Automation scripts
└── logs/                        # Hook execution logs

```

## Technology Stack

### Core Technologies

| Technology | Purpose | Version |
|------------|---------|---------|
| **Claude Code** | AI coding assistant CLI | Latest |
| **Python 3.8+** | Hook scripts and automation | 3.8+ |
| **UV** | Fast Python package installer | Latest |
| **Node.js/TypeScript** | MCP server implementation | 18+ |
| **Cloudflare Workers** | Serverless MCP hosting | Latest |

### Key Dependencies

**Python Hooks:**
- No external dependencies (uses stdlib only for portability)
- UV single-file scripts with inline dependency declarations

**MCP Server (use-cases/mcp-server):**
- `@modelcontextprotocol/sdk` - MCP TypeScript SDK
- `agents/mcp` - Cloudflare Workers MCP framework
- `@cloudflare/workers-oauth-provider` - OAuth 2.1 implementation
- `postgres` - PostgreSQL client
- `zod` - Runtime type validation
- `hono` - Web framework for Workers

**Optional Integrations:**
- ElevenLabs - Text-to-speech (TTS) provider
- OpenAI - TTS provider (fallback)
- Sentry - Error tracking and monitoring

## Architecture Layers

### 1. Hook System Layer

**Purpose:** Intercept and control Claude Code lifecycle events

**Components:**
- `.claude/hooks/` - Python hook implementations
- `.claude/settings.json` - Hook configuration and permissions
- `logs/` - JSON logs of hook execution

**Hook Types:**
1. **PreToolUse** - Executes before tool calls (can block)
2. **PostToolUse** - Executes after tool completion
3. **Notification** - Handles Claude Code notifications
4. **Stop** - Executes when Claude finishes responding
5. **SubagentStop** - Executes when subagents complete

**Key Files:**
- `pre_tool_use.py` - Security validation (blocks dangerous commands)
- `post_tool_use.py` - Logging and transcript conversion
- `notification.py` - TTS notifications
- `stop.py` - AI-generated completion messages with TTS
- `subagent_stop.py` - Subagent completion TTS

### 2. Plugin System Layer

**Purpose:** Modular, reusable workflows and specialized agents

**Plugin Structure:**
```
plugins/{plugin-name}/
├── agents/          # Specialized agent definitions
├── commands/        # Slash command implementations
├── hooks/           # Plugin-specific hooks (optional)
└── README.md        # Plugin documentation
```

**Available Plugins:**

#### Feature Development (`plugins/feature-dev/`)
Multi-phase feature implementation workflow:
- **Agents:**
  - `code-architect.md` - Architecture design and blueprints
  - `code-explorer.md` - Deep codebase analysis
  - `code-reviewer.md` - Quality and correctness review
- **Command:** `/feature-dev` - Guided feature development

#### PR Review Toolkit (`plugins/pr-review-toolkit/`)
Comprehensive pull request analysis:
- **Agents:**
  - `code-reviewer.md` - General code review
  - `code-simplifier.md` - Complexity reduction
  - `comment-analyzer.md` - Review comment analysis
  - `pr-test-analyzer.md` - Test coverage analysis
  - `silent-failure-hunter.md` - Error handling review
  - `type-design-analyzer.md` - Type safety review

#### Commit Commands (`plugins/commit-commands/`)
Git workflow automation:
- `/commit` - Smart commit with context
- `/commit-push-pr` - Complete PR workflow
- `/clean_gone` - Clean up merged branches

#### Agent SDK Development (`plugins/agent-sdk-dev/`)
Tools for building custom agents:
- Verification agents for Python and TypeScript
- SDK app scaffolding commands

#### Security Guidance (`plugins/security-guidance/`)
Security best practices enforcement:
- Hooks for security validation
- Common vulnerability detection

### 3. Custom Agents Layer

**Purpose:** Specialized AI agents with focused capabilities

**Location:** `.claude/agents/`

**Built-in Agents:**
- `azure-devops-engineer.md` - Azure infrastructure and deployment
- `code-reviewer.md` - Code quality analysis
- `data-scientist.md` - SQL queries and data analysis
- `debugger.md` - Error diagnosis and fixing
- `design-systems-expert.md` - UI/UX design guidance
- `documentation-engineer.md` - Documentation creation
- `nextjs-vercel-ai-engineer.md` - Next.js and AI SDK expertise
- `product-owner-story-writer.md` - User story creation
- `security-audit-engineer.md` - Security vulnerability analysis
- `test-coverage-engineer.md` - Test creation and coverage
- `test-runner.md` - Test execution and debugging
- `upstream-sync-integrator.md` - Repository synchronization

**Agent Capabilities:**
- Read-only tools (Read, Grep, Glob)
- File modification tools (Edit, Write)
- Execution tools (Bash)
- Specialized tools (WebFetch, TodoWrite)

### 4. Slash Commands Layer

**Purpose:** Custom workflows triggered by `/command` syntax

**Location:** `.claude/commands/`

**Command Categories:**

**Project Management:**
- `/specify` - Create feature specifications
- `/plan` - Generate implementation plans
- `/clarify` - Ask targeted clarification questions
- `/tasks` - Generate actionable task lists
- `/analyze` - Cross-artifact consistency analysis
- `/implement` - Execute implementation plan

**Development Workflow:**
- `/constitution` - Create project constitution
- `/create-worktree` - Git worktree management
- `/dedupe` - Find duplicate GitHub issues
- `/final-step` - Final task validation
- `/primer` - Project orientation

**Context Engineering:**
- `/generate-prp` - Generate Product Requirement Prompts
- `/execute-prp` - Execute PRPs to implement features

**Customer/Eval Workflows:**
- `/customer:create-plan` - Customer plan creation
- `/customer:execute-plan` - Customer plan execution
- `/eval:create-plan` - Evaluation plan creation
- `/eval:execute-plan` - Evaluation plan execution

**Core Operations:**
- `/core:execute-sync-task` - Upstream sync execution
- `/core:get-commits` - Get upstream commits

### 5. Context Engineering Layer

**Purpose:** Systematic context provisioning for AI assistants

**Location:** `context-engineering-intro/`

**Key Concepts:**

**PRPs (Product Requirement Prompts):**
- Comprehensive implementation blueprints
- Include context, documentation, validation steps
- Similar to PRDs but for AI instruction
- Template-based generation

**Workflow:**
1. Create `INITIAL.md` with feature requirements
2. Run `/generate-prp` to research and create PRP
3. Run `/execute-prp` to implement feature
4. Validation loops ensure quality

**Template Structure:**
```
context-engineering-intro/
├── PRPs/
│   ├── templates/
│   │   └── prp_base.md          # Base PRP template
│   └── EXAMPLE_multi_agent_prp.md  # Complete example
├── examples/                      # Code patterns to follow
├── CLAUDE.md                     # Global AI rules
├── INITIAL.md                    # Feature request template
└── INITIAL_EXAMPLE.md            # Example feature request
```

### 6. Use Cases Layer

**Purpose:** Working implementations of specific patterns

**MCP Server (`use-cases/mcp-server/`):**

Complete production MCP server with:
- GitHub OAuth 2.0 authentication
- PostgreSQL database access tools
- Role-based access control
- HMAC-signed cookie approval system
- Optional Sentry monitoring
- Cloudflare Workers deployment

**Technology:**
- TypeScript + Cloudflare Workers
- MCP SDK + agents/mcp framework
- Postgres client with connection pooling
- Zod validation for all inputs
- Hono web framework

**Available Tools:**
1. `listTables` - Schema discovery (all users)
2. `queryDatabase` - Read-only SQL (all users)
3. `executeDatabase` - Write operations (privileged users)

## Integration Points

### 1. Claude Code Settings

**File:** `.claude/settings.json`

**Structure:**
```json
{
  "permissions": {
    "allow": ["Bash(mkdir:*)", "Write", "Edit", ...],
    "deny": []
  },
  "hooks": {
    "PreToolUse": [...],
    "PostToolUse": [...],
    "Notification": [...],
    "Stop": [...],
    "SubagentStop": [...]
  }
}
```

### 2. Hook Execution Flow

```
User Input → Claude Decision → PreToolUse Hook
                                     ↓
                              [Block or Allow]
                                     ↓
                              Tool Execution
                                     ↓
                           PostToolUse Hook
                                     ↓
                           Notification Hook (if applicable)
                                     ↓
                           Stop Hook (when complete)
```

### 3. Plugin Discovery

Plugins are loaded from:
1. Local `.claude/` directory
2. `plugins/` directory (sharable)
3. External plugin repositories

### 4. MCP Server Deployment

**Local Development:**
```bash
cd use-cases/mcp-server
wrangler dev  # Available at http://localhost:8792/mcp
```

**Production:**
```bash
wrangler deploy
# Configure Claude Desktop with production URL
```

## Security Architecture

### Hook-Based Security

**PreToolUse Hook (`pre_tool_use.py`):**
- Blocks dangerous `rm -rf` commands
- Prevents `.env` file access (sensitive data)
- Logs all tool usage for audit

**Patterns Blocked:**
- `rm -rf /*` and variants
- Access to `.env` files (allows `.env.sample`)
- Destructive operations on system directories

### MCP Server Security

**Authentication:**
- GitHub OAuth 2.0 flow
- HMAC-signed approval cookies
- Per-user session management via Durable Objects

**Authorization:**
- Role-based tool access (standard vs privileged users)
- Username whitelist for write operations
- SQL injection protection via pattern validation

**Data Protection:**
- Error message sanitization (hide credentials)
- Connection string security
- Prepared statements for database queries

### Permission System

**Claude Code Permissions:**
- Granular tool access control
- Wildcard pattern matching
- Allow/deny lists

**Example:**
```json
{
  "allow": [
    "Bash(mkdir:*)",  // Allow all mkdir commands
    "Write",          // Allow file writing
    "Edit"            // Allow file editing
  ],
  "deny": []
}
```

## Data Flow Patterns

### 1. Hook Data Flow

```
Claude Code Event
       ↓
Hook receives JSON via stdin:
{
  "tool_name": "Bash",
  "tool_input": { "command": "ls -la" },
  "session_id": "...",
  "timestamp": "..."
}
       ↓
Hook processes and logs
       ↓
Hook returns via stdout/stderr + exit code:
- Exit 0: Success (continue)
- Exit 2: Block execution (show error to Claude)
- Other: Non-blocking error
```

### 2. Agent Task Flow

```
User invokes agent (Task tool)
       ↓
Agent receives prompt with:
- Context from CLAUDE.md
- Specific task instructions
- Available tools
       ↓
Agent executes autonomously:
- Searches codebase (Grep, Glob)
- Reads files (Read)
- Analyzes patterns
- Generates recommendations
       ↓
Agent returns final report
       ↓
Main Claude receives report and continues
```

### 3. MCP Tool Flow

```
Claude Desktop → MCP Request
       ↓
Cloudflare Worker receives request
       ↓
GitHub OAuth validation
       ↓
Durable Object state (user context)
       ↓
Tool permission check
       ↓
Database operation with validation
       ↓
Sentry tracing (if enabled)
       ↓
Response to Claude Desktop
```

## Performance Considerations

### Hook Performance

- **Timeout:** 60 seconds per hook
- **Parallelization:** All matching hooks run in parallel
- **Optimization:** Keep hooks fast (<1s typical)
- **Logging:** Asynchronous file writing

### MCP Server Performance

- **Connection Pooling:** Max 5 PostgreSQL connections (Workers limit)
- **Prepared Statements:** Enabled for query caching
- **Idle Timeout:** 20 seconds
- **Connect Timeout:** 10 seconds

### Agent Performance

- **Parallel Agents:** Launch multiple agents concurrently
- **Tool Limits:** Agents have no rate limits but should be used judiciously
- **Context Size:** Agents can read large files efficiently

## Error Handling Patterns

### Hook Errors

```python
try:
    # Hook logic
    log_data.append(input_data)
    sys.exit(0)
except json.JSONDecodeError:
    sys.exit(0)  # Graceful failure
except Exception:
    sys.exit(0)  # Don't break Claude Code
```

### MCP Tool Errors

```typescript
try {
  const result = await db.unsafe(sql);
  return { content: [{ type: "text", text: JSON.stringify(result) }] };
} catch (error) {
  return {
    content: [{
      type: "text",
      text: `**Error**\n\n${formatDatabaseError(error)}`,
      isError: true
    }]
  };
}
```

### Agent Errors

Agents log errors but don't fail catastrophically:
- Return partial results with error notes
- Include debug information in reports
- Allow continuation of workflow

## Development Workflow

### 1. Local Development

```bash
# Install dependencies
# (No installation needed - UV handles hook dependencies)

# Test hooks
echo '{"tool_name":"Bash","tool_input":{"command":"ls"}}' | \
  uv run .claude/hooks/pre_tool_use.py

# View logs
cat logs/pre_tool_use.json | jq

# Test MCP server
cd use-cases/mcp-server
wrangler dev
```

### 2. Creating New Hooks

```python
#!/usr/bin/env -S uv run --script
# /// script
# requires-python = ">=3.8"
# dependencies = [
#   "package-name==1.0.0"
# ]
# ///

import json
import sys

def main():
    input_data = json.load(sys.stdin)
    # Hook logic here
    sys.exit(0)

if __name__ == '__main__':
    main()
```

### 3. Creating New Agents

```markdown
---
name: my-agent
description: Agent purpose and capabilities
tools: Read, Grep, Glob, Bash
model: sonnet
color: blue
---

You are an expert [role] who [capabilities].

[Detailed instructions...]
```

### 4. Creating New Slash Commands

```markdown
---
description: Command description
argument-hint: Optional argument hint
---

# Command Title

[Command instructions that Claude will receive...]

Arguments: $ARGUMENTS
```

## Testing Strategy

### Hook Testing

**Manual Testing:**
```bash
# Test with sample input
echo '{"tool_name":"Bash","tool_input":{"command":"rm -rf /"}}' | \
  uv run .claude/hooks/pre_tool_use.py
# Should exit with code 2 and error message
```

**Integration Testing:**
- Run Claude Code commands
- Verify logs in `logs/` directory
- Check hook behavior in transcripts

### MCP Server Testing

**Unit Tests:**
```bash
cd use-cases/mcp-server
npm run test
```

**Integration Tests:**
```bash
# MCP Inspector
npx @modelcontextprotocol/inspector@latest

# Manual testing
wrangler dev
# Configure Claude Desktop with local URL
```

### Agent Testing

- Launch agents with test prompts
- Verify output quality and completeness
- Check tool usage patterns
- Validate recommendations

## Deployment

### Hook Deployment

Hooks are deployed by:
1. Copying `.claude/` directory to project
2. Ensuring UV is installed
3. Configuring `.claude/settings.json`

### MCP Server Deployment

```bash
cd use-cases/mcp-server

# Set production secrets
wrangler secret put GITHUB_CLIENT_ID
wrangler secret put GITHUB_CLIENT_SECRET
wrangler secret put COOKIE_ENCRYPTION_KEY
wrangler secret put DATABASE_URL
wrangler secret put SENTRY_DSN  # Optional

# Deploy
wrangler deploy

# Get Worker URL
wrangler deployments list
```

### Plugin Distribution

Plugins can be distributed via:
1. Git submodules
2. npm packages
3. Direct directory copying
4. Plugin marketplace (future)

## Monitoring and Observability

### Hook Logging

**Log Files:**
- `logs/pre_tool_use.json` - Pre-execution events
- `logs/post_tool_use.json` - Post-execution events
- `logs/notification.json` - Notification events
- `logs/stop.json` - Completion events
- `logs/subagent_stop.json` - Subagent completions
- `logs/chat.json` - Readable conversation transcript

**Log Format:**
```json
[
  {
    "tool_name": "Bash",
    "tool_input": { "command": "ls -la" },
    "session_id": "abc123",
    "timestamp": "2025-10-11T10:42:00Z"
  }
]
```

### MCP Server Monitoring

**Sentry Integration:**
- Error tracking with context
- Performance monitoring (100% sampling)
- User identification (GitHub username)
- Distributed tracing across tool calls

**CloudWatch Logs:**
- Available via Cloudflare dashboard
- Real-time log streaming
- Query and filtering capabilities

### Agent Monitoring

- No built-in monitoring (agents are ephemeral)
- Rely on TodoWrite for progress tracking
- Review agent outputs in transcripts

## Troubleshooting Guide

### Common Issues

**Hooks Not Executing:**
- Check `.claude/settings.json` syntax
- Verify UV is installed: `uv --version`
- Check hook file permissions: `chmod +x .claude/hooks/*.py`
- Review hook logs for errors

**MCP Server Connection Issues:**
- Verify Wrangler is running: `wrangler dev`
- Check Claude Desktop MCP config
- Ensure OAuth secrets are set
- Test with MCP Inspector

**Agent Failures:**
- Check available tools match agent needs
- Verify codebase accessibility (file permissions)
- Review agent prompts for clarity
- Check for infinite loops in agent logic

**Permission Errors:**
- Review `.claude/settings.json` permissions
- Verify wildcard patterns match
- Check for deny rules blocking operations

### Debug Techniques

**Hook Debugging:**
```bash
# Test hook manually
echo '{"tool_name":"Test","tool_input":{}}' | \
  uv run .claude/hooks/pre_tool_use.py
echo $?  # Check exit code

# View logs
tail -f logs/pre_tool_use.json | jq
```

**MCP Debugging:**
```bash
# Enable Wrangler debug mode
wrangler dev --log-level debug

# Check database connectivity
# (Add test endpoint in src/index.ts)

# View Sentry events
# https://sentry.io/...
```

**Agent Debugging:**
- Add TodoWrite to track agent progress
- Review agent outputs carefully
- Test agents with simpler prompts first
- Check tool usage patterns

## Related Documentation

- `.agent/README.md` - Documentation index
- `README.md` - Project overview and setup
- `ai_docs/cc_hooks_docs.md` - Complete hooks documentation
- `use-cases/mcp-server/CLAUDE.md` - MCP implementation guide
- `context-engineering-intro/README.md` - Context engineering guide

## Maintenance and Updates

### Regular Maintenance Tasks

- Update UV dependencies in hooks
- Review and clean old logs
- Update MCP server dependencies
- Refresh AI documentation
- Test hooks with new Claude Code versions

### Version Control

- Commit `.claude/` directory structure
- Exclude `.claude/settings.local.json` (user-specific)
- Exclude `logs/` directory (runtime data)
- Include all plugin and use-case code

### Contributing

When adding new features:
1. Document in appropriate `.agent/` file
2. Add examples to `use-cases/` if applicable
3. Update `.agent/README.md` index
4. Test thoroughly before committing
5. Follow existing patterns and conventions
