# nx-voice

A lightweight CLI text-to-speech tool for macOS using Microsoft Edge's Neural Voices. Works standalone or as a Claude Code hook.

## Features

- **High Quality**: Free Microsoft Edge Neural TTS voices (no API key required)
- **Smart Language Detection**: Auto-switches between Thai and English based on text content
- **Voice Identities**: Pre-configured personas via `agent-voices.toml`
- **Claude Code Integration**: Built-in hook to speak Claude's responses aloud
- **Fast**: Generates and plays audio instantly from terminal

## Prerequisites

```bash
# Install pipx (Python tool manager)
brew install pipx
pipx ensurepath

# Install edge-tts
pipx install edge-tts

# Required for Claude Code integration
brew install jq
```

## Installation

```bash
# Clone the repository
git clone https://github.com/pangpond/nx-voice.git
cd nx-voice

# Make scripts executable
chmod +x speak claude-hook claude-session-start

# (Optional) Add to PATH for global access
echo 'export PATH="$PATH:'$(pwd)'"' >> ~/.zshrc
source ~/.zshrc
```

## Usage

### CLI Usage

```bash
# Auto-detect language
speak "Hello, how are you?"           # English (AriaNeural)
speak "สวัสดีครับ วันนี้เป็นอย่างไรบ้าง"     # Thai (PremwadeeNeural)

# Use specific voice identity
speak main "System ready."
speak agent_1 "This is a confidential briefing."
speak subagent "Task completed successfully."
```

### Available Voice Identities

| Identity      | Voice                 | Description              |
| ------------- | --------------------- | ------------------------ |
| `main`        | en-US-AriaNeural      | Default clear voice      |
| `agent_1`     | en-GB-RyanNeural      | British male             |
| `agent_2`     | en-US-AvaNeural       | US female                |
| `subagent`    | en-GB-RyanNeural      | British male             |
| `antigravity` | en-US-AriaNeural      | Antigravity system voice |
| `thai`        | th-TH-PremwadeeNeural | Thai female              |
| `thai_male`   | th-TH-NiwatNeural     | Thai male                |

### Custom Voices

Edit `agent-voices.toml` to add or modify voice mappings:

```toml
[voices]
my_voice = "en-AU-NatashaNeural"

[rate]
my_voice = 200  # Words per minute
```

List all available voices:

```bash
edge-tts --list-voices
```

## Claude Code Integration

nx-voice includes hooks for Claude Code:

- `claude-session-start` - Announces "NX voice protocol active" when session begins
- `claude-hook` - Speaks Claude's responses when tasks complete

### Setup

Add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/nx-voice/claude-session-start"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/nx-voice/claude-hook"
          }
        ]
      }
    ],
    "SubagentStop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/nx-voice/claude-hook"
          }
        ]
      }
    ]
  }
}
```

Replace `/path/to/nx-voice` with your actual installation path.

### How It Works

1. Claude finishes responding (Stop/SubagentStop event)
2. Hook reads the transcript path from Claude's JSON input
3. Extracts the last assistant message
4. Cleans markdown formatting (code blocks, links, etc.)
5. Speaks the first 1-2 sentences using the `main` voice

### Change Claude's Voice

Edit the voice identity in `agent-voices.toml`:

```toml
[voices]
main = "en-GB-SoniaNeural"  # British female for Claude
```

## Antigravity Integration

For Antigravity workflows, use the `antigravity` identity:

```bash
speak antigravity "NX voice protocol active."
```

Or call directly from scripts:

```bash
/path/to/nx-voice/speak antigravity "$MESSAGE"
```

### Add Agent Workflow

Copy .agent/ to your project root.

## Troubleshooting

**edge-tts not found**

```bash
pipx install edge-tts
pipx ensurepath
source ~/.zshrc
```

**jq not found (Claude hook)**

```bash
brew install jq
```

**No audio playback**

- macOS uses `afplay` (built-in)
- Linux needs `play` from sox: `sudo apt install sox`

**Antigravity Voice Protocol not initialized**

- prompt: read .cursorrules file
