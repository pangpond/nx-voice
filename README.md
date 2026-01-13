# nx-voice

A lightweight, high-quality CLI text-to-speech tool for macOS using Microsoft Edge's Neural Voices.

**Features:**

- 🗣️ **High Quality**: Uses Microsoft Edge's Neural TTS voices (free).
- 🇹🇭 **Smart Language Detection**: Automatically switches between Thai (Premwadee) and English (Aria) based on text content.
- 🚀 **Fast**: Generates and plays audio instantly from your terminal.
- ⚙️ **Configurable**: Easily add or change voice aliases in `agent-voices.toml`.

## Prerequisites

This tool requires `python3` and `pipx` to run.

### 1. Install Dependencies

```bash
# Install pipx if you haven't already (for managing python tools)
brew install pipx
pipx ensurepath

# Install edge-tts globally
pipx install edge-tts
```

## Installation

1.  Clone or download this folder.
2.  Make the script executable (if it isn't already):
    ```bash
    chmod +x speak
    ```
3.  (Optional) Add it to your PATH or alias it in your `.zshrc` or `.bashrc`:
    ```bash
    alias speak="/path/to/nx-voice/speak"
    ```

## Usage

### Basic Usage

The script automatically detects if you are speaking Thai or English.

```bash
# Speaks in English (AriaNeural)
./speak "Hello, how are you today?"

# Speaks in Thai (PremwadeeNeural)
./speak "สวัสดีครับ วันนี้ทานข้าวหรือยัง"
```

### Using Specific Identities

You can specify a persona defined in `agent-voices.toml`.

```bash
# Use a specific identity (e.g. 'agent_1' mapped to a British male voice)
./speak agent_1 "This is a confidential briefing."
```

### Configuration

Edit `agent-voices.toml` to change default voices or add new personas.

```toml
[voices]
main = "en-US-AriaNeural"
agent_1 = "en-GB-RyanNeural"
thai = "th-TH-PremwadeeNeural"
```

You can find a list of all available voices by running:

```bash
edge-tts --list-voices
```
