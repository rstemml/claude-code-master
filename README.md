# Claude Code Hooks Mastery

[Claude Code Hooks](https://docs.anthropic.com/en/docs/claude-code/hooks) - Quickly master how to use Claude Code hooks to add deterministic (or non-deterministic) control over Claude Code's behavior.

<img src="images/hooked.png" alt="Claude Code Hooks" style="max-width: 800px; width: 100%;" />

## Prerequisites

This requires:
- **[Astral UV](https://docs.astral.sh/uv/getting-started/installation/)** - Fast Python package installer and resolver
- **[Claude Code](https://docs.anthropic.com/en/docs/claude-code)** - Anthropic's CLI for Claude AI

Optional:
- **[ElevenLabs](https://elevenlabs.io/)** - Text-to-speech provider
- **[OpenAI](https://openai.com/)** - Language model provider + Text-to-speech provider
- **[Anthropic](https://www.anthropic.com/)** - Language model provider

## Hook Lifecycle & Payloads

This demo captures all 5 Claude Code hook lifecycle events with their JSON payloads:

### 1. PreToolUse Hook
**Fires:** Before any tool execution  
**Payload:** `tool_name`, `tool_input` parameters  
**Enhanced:** Blocks dangerous commands (`rm -rf`, `.env` access)

### 2. PostToolUse Hook  
**Fires:** After successful tool completion  
**Payload:** `tool_name`, `tool_input`, `tool_response` with results

### 3. Notification Hook
**Fires:** When Claude Code sends notifications (waiting for input, etc.)  
**Payload:** `message` content  
**Enhanced:** TTS alerts - "Your agent needs your input" (30% chance includes name)

### 4. Stop Hook
**Fires:** When Claude Code finishes responding  
**Payload:** `stop_hook_active` boolean flag  
**Enhanced:** AI-generated completion messages with TTS playback

### 5. SubagentStop Hook
**Fires:** When Claude Code subagents (Task tools) finish responding  
**Payload:** `stop_hook_active` boolean flag  
**Enhanced:** TTS playback - "Subagent Complete"

## What This Shows

- **Complete hook lifecycle coverage** - All 5 hook events implemented and logging
- **Intelligent TTS system** - AI-generated audio feedback with voice priority (ElevenLabs > OpenAI > pyttsx3)
- **Security enhancements** - Blocks dangerous commands and sensitive file access
- **Personalized experience** - Uses engineer name from environment variables
- **Automatic logging** - All hook events are logged as JSON to `logs/` directory  
- **Chat transcript extraction** - PostToolUse hook converts JSONL transcripts to readable JSON format

> **Warning:** The `chat.json` file contains only the most recent Claude Code conversation. It does not preserve conversations from previous sessions - each new conversation is fully copied and overwrites the previous one. This is unlike the other logs which are appended to from every claude code session.

## UV Single-File Scripts Architecture

This project leverages **[UV single-file scripts](https://docs.astral.sh/uv/guides/scripts/)** to keep hook logic cleanly separated from your main codebase. All hooks live in `.claude/hooks/` as standalone Python scripts with embedded dependency declarations.

**Benefits:**
- **Isolation** - Hook logic stays separate from your project dependencies
- **Portability** - Each hook script declares its own dependencies inline
- **No Virtual Environment Management** - UV handles dependencies automatically
- **Fast Execution** - UV's dependency resolution is lightning-fast
- **Self-Contained** - Each hook can be understood and modified independently

This approach ensures your hooks remain functional across different environments without polluting your main project's dependency tree.

## Key Files

- `.claude/settings.json` - Hook configuration with permissions
- `.claude/hooks/` - Python scripts using uv for each hook type
  - `pre_tool_use.py` - Security blocking and logging
  - `post_tool_use.py` - Logging and transcript conversion
  - `notification.py` - Logging with optional TTS (--notify flag)
  - `stop.py` - AI-generated completion messages with TTS
  - `subagent_stop.py` - Simple "Subagent Complete" TTS
  - `utils/` - Intelligent TTS and LLM utility scripts
    - `tts/` - Text-to-speech providers (ElevenLabs, OpenAI, pyttsx3)
    - `llm/` - Language model integrations (OpenAI, Anthropic)
- `logs/` - JSON logs of all hook executions
  - `pre_tool_use.json` - Tool use events with security blocking
  - `post_tool_use.json` - Tool completion events
  - `notification.json` - Notification events
  - `stop.json` - Stop events with completion messages
  - `subagent_stop.json` - Subagent completion events
  - `chat.json` - Readable conversation transcript (generated by --chat flag)
- `ai_docs/cc_hooks_docs.md` - Complete hooks documentation from Anthropic

Hooks provide deterministic control over Claude Code behavior without relying on LLM decisions.

## Features Demonstrated

- Command logging and auditing
- Automatic transcript conversion  
- Permission-based tool access control
- Error handling in hook execution

Run any Claude Code command to see hooks in action via the `logs/` files.

## Hook Error Codes & Flow Control

Claude Code hooks provide powerful mechanisms to control execution flow and provide feedback through exit codes and structured JSON output.

### Exit Code Behavior

Hooks communicate status and control flow through exit codes:

| Exit Code | Behavior           | Description                                                                                  |
| --------- | ------------------ | -------------------------------------------------------------------------------------------- |
| **0**     | Success            | Hook executed successfully. `stdout` shown to user in transcript mode (Ctrl-R)               |
| **2**     | Blocking Error     | **Critical**: `stderr` is fed back to Claude automatically. See hook-specific behavior below |
| **Other** | Non-blocking Error | `stderr` shown to user, execution continues normally                                         |

### Hook-Specific Flow Control

Each hook type has different capabilities for blocking and controlling Claude Code's behavior:

#### PreToolUse Hook - **CAN BLOCK TOOL EXECUTION**
- **Primary Control Point**: Intercepts tool calls before they execute
- **Exit Code 2 Behavior**: Blocks the tool call entirely, shows error message to Claude
- **Use Cases**: Security validation, parameter checking, dangerous command prevention
- **Example**: Our `pre_tool_use.py` blocks `rm -rf` commands with exit code 2

```python
# Block dangerous commands
if is_dangerous_rm_command(command):
    print("BLOCKED: Dangerous rm command detected", file=sys.stderr)
    sys.exit(2)  # Blocks tool call, shows error to Claude
```

#### PostToolUse Hook - **CANNOT BLOCK (Tool Already Executed)**
- **Primary Control Point**: Provides feedback after tool completion
- **Exit Code 2 Behavior**: Shows error to Claude (tool already ran, cannot be undone)
- **Use Cases**: Validation of results, formatting, cleanup, logging
- **Limitation**: Cannot prevent tool execution since it fires after completion

#### Notification Hook - **CANNOT BLOCK**
- **Primary Control Point**: Handles Claude Code notifications
- **Exit Code 2 Behavior**: N/A - shows stderr to user only, no blocking capability
- **Use Cases**: Custom notifications, logging, user alerts
- **Limitation**: Cannot control Claude Code behavior, purely informational

#### Stop Hook - **CAN BLOCK STOPPING**
- **Primary Control Point**: Intercepts when Claude Code tries to finish responding
- **Exit Code 2 Behavior**: Blocks stoppage, shows error to Claude (forces continuation)
- **Use Cases**: Ensuring tasks complete, validation of final state use this to FORCE CONTINUATION
- **Caution**: Can cause infinite loops if not properly controlled

### Advanced JSON Output Control

Beyond simple exit codes, hooks can return structured JSON for sophisticated control:

#### Common JSON Fields (All Hook Types)
```json
{
  "continue": true,           // Whether Claude should continue (default: true)
  "stopReason": "string",     // Message when continue=false (shown to user)
  "suppressOutput": true      // Hide stdout from transcript (default: false)
}
```

#### PreToolUse Decision Control
```json
{
  "decision": "approve" | "block" | undefined,
  "reason": "Explanation for decision"
}
```

- **"approve"**: Bypasses permission system, `reason` shown to user
- **"block"**: Prevents tool execution, `reason` shown to Claude
- **undefined**: Normal permission flow, `reason` ignored

#### PostToolUse Decision Control
```json
{
  "decision": "block" | undefined,
  "reason": "Explanation for decision"
}
```

- **"block"**: Automatically prompts Claude with `reason`
- **undefined**: No action, `reason` ignored

#### Stop Decision Control
```json
{
  "decision": "block" | undefined,
  "reason": "Must be provided when blocking Claude from stopping"
}
```

- **"block"**: Prevents Claude from stopping, `reason` tells Claude how to proceed
- **undefined**: Allows normal stopping, `reason` ignored

### Flow Control Priority

When multiple control mechanisms are used, they follow this priority:

1. **`"continue": false`** - Takes precedence over all other controls
2. **`"decision": "block"`** - Hook-specific blocking behavior
3. **Exit Code 2** - Simple blocking via stderr
4. **Other Exit Codes** - Non-blocking errors

### Security Implementation Examples

#### 1. Command Validation (PreToolUse)
```python
# Block dangerous patterns
dangerous_patterns = [
    r'rm\s+.*-[rf]',           # rm -rf variants
    r'sudo\s+rm',              # sudo rm commands
    r'chmod\s+777',            # Dangerous permissions
    r'>\s*/etc/',              # Writing to system directories
]

for pattern in dangerous_patterns:
    if re.search(pattern, command, re.IGNORECASE):
        print(f"BLOCKED: {pattern} detected", file=sys.stderr)
        sys.exit(2)
```

#### 2. Result Validation (PostToolUse)
```python
# Validate file operations
if tool_name == "Write" and not tool_response.get("success"):
    output = {
        "decision": "block",
        "reason": "File write operation failed, please check permissions and retry"
    }
    print(json.dumps(output))
    sys.exit(0)
```

#### 3. Completion Validation (Stop Hook)
```python
# Ensure critical tasks are complete
if not all_tests_passed():
    output = {
        "decision": "block",
        "reason": "Tests are failing. Please fix failing tests before completing."
    }
    print(json.dumps(output))
    sys.exit(0)
```

### Hook Execution Environment

- **Timeout**: 60-second execution limit per hook
- **Parallelization**: All matching hooks run in parallel
- **Environment**: Inherits Claude Code's environment variables
- **Working Directory**: Runs in current project directory
- **Input**: JSON via stdin with session and tool data
- **Output**: Processed via stdout/stderr with exit codes

### Best Practices for Flow Control

1. **Use PreToolUse for Prevention**: Block dangerous operations before they execute
2. **Use PostToolUse for Validation**: Check results and provide feedback
3. **Use Stop for Completion**: Ensure tasks are properly finished
4. **Handle Errors Gracefully**: Always provide clear error messages
5. **Avoid Infinite Loops**: Check `stop_hook_active` flag in Stop hooks
6. **Test Thoroughly**: Verify hooks work correctly in safe environments

## Master AI Coding
> And prepare for Agentic Engineering

Learn to code with AI with foundational [Principles of AI Coding](https://agenticengineer.com/principled-ai-coding?y=cchookmast)

Follow the [IndyDevDan youtube channel](https://www.youtube.com/@indydevdan) for more AI coding tips and tricks.