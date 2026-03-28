# Experiment: Custom Hook (Edit Logger)

**Topic:** [Practical Claude Code](../../topics/05-practical-claude-code.md#hooks-system)
**Difficulty:** Beginner (30-60 min)
**Concepts tested:** Hook configuration, matcher patterns, PostToolUse event, stdin JSON parsing, exit codes

## Goal

Build a PostToolUse hook that logs every file edit Claude makes during a session. After a work session, you'll have a changelog showing what files were modified, when, and what tool was used.

## Prerequisites

- Read the [Hooks System](../../topics/05-practical-claude-code.md#hooks-system) section
- `jq` installed (`brew install jq` if not)
- Familiarity with bash scripting basics

## What You'll Build

A bash script (`log-edits.sh`) that:
1. Receives PostToolUse event JSON on stdin
2. Extracts the filename, tool name, and timestamp
3. Appends a line to a log file (`/tmp/claude-edit-log.txt`)

Plus the hook configuration in `.claude/settings.local.json` to wire it up.

## Steps

### Step 1: Understand the input

When a PostToolUse event fires for an Edit or Write tool, Claude Code sends JSON on stdin. The relevant fields are:

```json
{
  "hook_event_name": "PostToolUse",
  "tool_name": "Edit",
  "tool_input": {
    "file_path": "/path/to/file.md",
    "old_string": "...",
    "new_string": "..."
  },
  "tool_output": "..."
}
```

Your script needs to read this JSON and extract what you need.

### Step 2: Write the script

Create `log-edits.sh` in this folder. It should:

1. Read JSON from stdin
2. Use `jq` to extract `tool_name` and `tool_input.file_path`
3. Get the current timestamp
4. Append a line to `/tmp/claude-edit-log.txt`
5. Exit with code 0 (success, non-blocking)

Hints:
- `jq -r '.tool_name'` extracts a field
- `date '+%Y-%m-%d %H:%M:%S'` gives a timestamp
- The script must be executable: `chmod +x log-edits.sh`

### Step 3: Configure the hook

Add to your project's `.claude/settings.local.json` (create if it doesn't exist):

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "/full/path/to/log-edits.sh",
            "timeout": 5
          }
        ]
      }
    ]
  }
}
```

Replace `/full/path/to/` with the actual path to your script.

### Step 4: Test it

1. Start a new Claude Code session in the project
2. Ask Claude to make a small edit to any file
3. Check `/tmp/claude-edit-log.txt` -- you should see a log entry
4. Make a few more edits, check the log grows

### Step 5: Verify

- [ ] Script receives JSON on stdin and doesn't crash
- [ ] Log file shows filename, tool name, and timestamp for each edit
- [ ] Hook doesn't block or slow down Claude's edits (exit code 0, fast timeout)
- [ ] Hook only fires on Edit and Write (not on Bash, Read, etc.)

## Expected Output

After a session with a few edits, `/tmp/claude-edit-log.txt` should look something like:

```
2026-03-28 14:23:01 | Edit | /path/to/topics/05-practical-claude-code.md
2026-03-28 14:23:45 | Write | /path/to/experiments/05-custom-hook/log-edits.sh
2026-03-28 14:25:12 | Edit | /path/to/resources.md
```

## Stretch Goals (if you finish early)

- Add the number of lines changed (compare old_string vs new_string length)
- Add a SessionStart hook that clears the log at the beginning of each session
- Try a PreToolUse hook that blocks edits to files matching a pattern (e.g., `*.json` config files)
