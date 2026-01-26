# NotebookLM Assistant for Claude (Desktop & CLI)

This project provides a powerful MCP (Model Context Protocol) server that brings the full power of Google NotebookLM into Claude.

## Features

- **Research**: List and create notebooks
- **Content**: Add URLs, text, and files as sources
- **Generation**: Create Podcasts, Videos, Slides, Mind Maps, Infographics, Quizzes, Flashcards, and Reports
- **Natural Interaction**: Chat directly with your sources using Claude's reasoning

---

## Prerequisites

Before you begin, ensure you have the following installed:

### 1. Install uv (Python Package Manager)

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Or with Homebrew
brew install uv
```

Verify installation:
```bash
uv --version
```

### 2. Install Dependencies

```bash
cd /Users/alfredang/projects/notebooklm-assistant
uv sync
```

This will create a `.venv` folder and install all required packages.

---

## Step 1: Authenticate with NotebookLM

NotebookLM uses browser-based authentication. You must login once to save your session cookies.

```bash
cd /Users/alfredang/projects/notebooklm-assistant
uv run notebooklm login
```

**What happens:**
1. A browser window will open automatically
2. Log in to your Google account
3. Navigate to NotebookLM if not redirected automatically
4. Wait until the terminal displays **"Success"**
5. Close the browser

**Verify authentication:**
```bash
uv run python -c "
from notebooklm import NotebookLMClient
import asyncio
async def test():
    client = await NotebookLMClient.from_storage()
    async with client:
        notebooks = await client.notebooks.list()
        print(f'Authenticated! Found {len(notebooks)} notebooks.')
asyncio.run(test())
"
```

You should see: `Authenticated! Found X notebooks.`

---

## Step 2: Test the MCP Server

Before configuring Claude, verify the server starts correctly:

```bash
cd /Users/alfredang/projects/notebooklm-assistant
uv run python server.py
```

**Expected output:**
```
Starting NotebookLM MCP server...
NotebookLM client initialized successfully
Starting MCP server 'NotebookLM' with transport 'stdio'
```

Press `Ctrl+C` to stop the server after confirming it works.

---

## Step 3: Setup for Claude Desktop (GUI)

### 3.1 Locate the Config File

Open Terminal and run:
```bash
# Create the config file if it doesn't exist
mkdir -p ~/Library/Application\ Support/Claude
touch ~/Library/Application\ Support/Claude/claude_desktop_config.json

# Open in your default editor
open ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

### 3.2 Find Your uv Path

Claude Desktop doesn't inherit your shell's PATH, so you need the **full path** to `uv`:

```bash
which uv
```

This will output something like `/Users/alfredang/.local/bin/uv` - use this path in the next step.

### 3.3 Add the MCP Server Configuration

**If the file is empty**, paste this entire block (replace the uv path if different):

```json
{
  "mcpServers": {
    "notebooklm": {
      "command": "/Users/alfredang/.local/bin/uv",
      "args": [
        "--directory",
        "/Users/alfredang/projects/notebooklm-assistant",
        "run",
        "python",
        "server.py"
      ]
    }
  }
}
```

**If you already have other MCP servers**, add the `notebooklm` entry inside the existing `mcpServers` object:

```json
{
  "mcpServers": {
    "existing-server": { ... },
    "notebooklm": {
      "command": "/Users/alfredang/.local/bin/uv",
      "args": [
        "--directory",
        "/Users/alfredang/projects/notebooklm-assistant",
        "run",
        "python",
        "server.py"
      ]
    }
  }
}
```

### 3.4 Restart Claude Desktop

1. **Fully quit** Claude Desktop (Cmd+Q, not just close the window)
2. Reopen Claude Desktop
3. Look for the **hammer icon** (🔨) in the chat input area - this indicates MCP tools are available

### 3.5 Verify Connection

In Claude Desktop, type:
```
List my NotebookLM notebooks
```

Claude should use the `list_notebooks` tool and show your notebooks.

---

## Step 4: Setup for Claude Code (CLI)

### 4.1 Add the MCP Server

```bash
claude mcp add notebooklm -- uv --directory /Users/alfredang/projects/notebooklm-assistant run python server.py
```

### 4.2 Verify the Server is Added

```bash
claude mcp list
```

You should see `notebooklm` in the list.

### 4.3 Test in Claude Code

Start a new Claude Code session:
```bash
claude
```

Then ask:
```
List my NotebookLM notebooks
```

---

## Usage Examples

Once configured, you can use natural language commands in Claude Desktop or Claude Code:

| Task | Example Command |
|------|-----------------|
| List notebooks | "Show me all my NotebookLM notebooks" |
| Create notebook | "Create a new notebook called 'Research Project'" |
| Add URL source | "Add this URL to my notebook: https://example.com/article" |
| Generate podcast | "Generate a podcast for notebook ID xyz123" |
| Create slides | "Make a slide deck from my 'Research Project' notebook" |
| Generate mind map | "Create a mind map for notebook abc456" |
| Create quiz | "Generate a quiz based on my notebook sources" |
| Make flashcards | "Create study flashcards from this notebook" |

---

## Available Tools

| Tool | Description |
|------|-------------|
| `list_notebooks` | List all notebooks in your account |
| `create_notebook` | Create a new notebook |
| `add_source_url` | Add a website URL as a source |
| `add_source_text` | Add raw text as a source |
| `ask_notebook` | Ask a question based on notebook sources |
| `get_notebook_summary` | Get summary and key insights |
| `generate_audio_overview` | Generate a podcast-style audio |
| `generate_video_overview` | Generate a video overview |
| `generate_slide_deck` | Generate PowerPoint-style slides |
| `generate_mind_map` | Generate an interactive mind map |
| `generate_infographic` | Generate a visual infographic |
| `generate_quiz` | Generate quiz questions |
| `generate_flashcards` | Generate study flashcards |
| `generate_summary_report` | Generate a briefing document |
| `generate_data_table` | Extract data into a table |

---

## Troubleshooting

### "Server disconnected" or "Failed to spawn process: No such file or directory"

**Cause**: Claude Desktop can't find `uv` because it doesn't inherit your shell's PATH.

**Solution**: Use the **full path** to `uv` in the config:

1. Find your uv path:
   ```bash
   which uv
   ```

2. Update `claude_desktop_config.json` to use the full path:
   ```json
   "command": "/Users/alfredang/.local/bin/uv"
   ```
   (Replace with your actual path from step 1)

3. Fully restart Claude Desktop (Cmd+Q and reopen)

**If authentication expired**, also run:
```bash
cd /Users/alfredang/projects/notebooklm-assistant
uv run notebooklm login
```

### "Command not found: uv"

**Cause**: uv is not in your PATH.

**Solution**:
```bash
# Add to your shell profile (~/.zshrc or ~/.bashrc)
export PATH="$HOME/.local/bin:$PATH"

# Reload shell
source ~/.zshrc
```

### MCP Server Not Appearing in Claude Desktop

**Cause**: Invalid JSON in config file or Claude not restarted properly.

**Solution**:
1. Validate your JSON at https://jsonlint.com/
2. Ensure no trailing commas in the JSON
3. Fully quit Claude Desktop (Cmd+Q) and reopen

### "NotebookLM client not initialized"

**Cause**: Server started before authentication was complete.

**Solution**:
1. Run `uv run notebooklm login` first
2. Restart Claude Desktop or re-add the MCP server in Claude Code

### Check Claude Desktop Logs

For detailed error information:
```bash
# View recent logs
tail -100 ~/Library/Logs/Claude/mcp*.log

# Or open in Console app
open ~/Library/Logs/Claude/
```

### Remove and Re-add MCP Server (Claude Code)

If issues persist:
```bash
claude mcp remove notebooklm
claude mcp add notebooklm -- uv --directory /Users/alfredang/projects/notebooklm-assistant run python server.py
```

---

## Updating

To update the NotebookLM library:
```bash
cd /Users/alfredang/projects/notebooklm-assistant
uv sync --upgrade
```

---

## Project Structure

```
notebooklm-assistant/
├── server.py          # MCP server implementation
├── pyproject.toml     # Project dependencies
├── README.md          # This file
├── SKILL.md           # Claude Code skill definition
└── .venv/             # Virtual environment (auto-created)
```
