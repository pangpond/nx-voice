# Antigravity Voice Protocol for Claude Code

## Voice Reporting

When completing significant tasks or milestones, use the `speak` command to verbally report the result:

```bash
/Users/pangpond/Sites/Nextensions/nx-voice/speak "Your concise report here"
```

## Guidelines

- **Be Concise**: Keep voice reports to 1-2 sentences
- **Significant Events Only**: Report task completions, errors, or important status changes
- **Identity**: Use `main` for general reports, `antigravity` for system-level updates

## Examples

```bash
# Task completion
speak main "Build completed successfully."

# Error reporting
speak main "Tests failed. 3 errors found."

# Status update
speak antigravity "Deployment to staging complete."
```

## Available Identities

| Identity | Use Case |
|----------|----------|
| `main` | Default Claude responses |
| `antigravity` | System/workflow updates |
| `subagent` | Subagent task reports |
| `thai` | Thai language messages |
