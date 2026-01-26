# NotebookLM Assistant

A Claude Code skill that enables you to generate podcasts, videos, presentations, mind maps, infographics, and more from any content using NotebookLM.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before using this skill, ensure you have:

1. **Claude Code CLI** installed ([Installation Guide](https://docs.anthropic.com/claude-code))
2. **NotebookLM CLI** installed and configured
3. A Google account with access to NotebookLM

## Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/alfredang/notebooklm-assistant.git
cd notebooklm-assistant
```

### Step 2: Authenticate with NotebookLM

Run the login command and follow the prompts:

```bash
notebooklm login
```

Verify your authentication:

```bash
notebooklm list
```

You should see your existing notebooks (or an empty list if new).

### Step 3: Add the Skill to Claude Code

Copy the `SKILL.md` file to your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills
cp SKILL.md ~/.claude/skills/notebooklm-assistant.md
```

Alternatively, you can reference the skill directly in your Claude Code configuration.

### Step 4: Configure MCP Server (Optional)

For enhanced functionality, add the NotebookLM MCP server to `~/.claude/config.json`:

```json
{
  "mcpServers": {
    "notebooklm": {
      "command": "notebooklm",
      "args": ["mcp"]
    }
  }
}
```

Restart Claude Code after making configuration changes.

## Usage

### Basic Workflow

1. **Start Claude Code** in your terminal:
   ```bash
   claude
   ```

2. **Provide your content source** (URL, file path, or paste text directly)

3. **Request your desired output format** using natural language

### Supported Content Sources

| Source | How to Provide |
|--------|----------------|
| Web Page | Paste the URL |
| YouTube Video | Paste the YouTube URL |
| Local File | Provide the file path |
| Text | Paste directly or reference a .txt file |
| Multiple Sources | List multiple URLs or files |

### Available Output Formats

- Podcast (audio discussion)
- Video content
- PowerPoint/Slides
- Mind Map
- Infographic
- Quiz/Test
- Flashcards
- Summary Report
- Data Table

## Examples

### Example 1: Generate a Podcast from a Web Article

```
User: Create a podcast from https://example.com/ai-trends-2024

Claude: I'll process this article and generate a podcast discussion...
[Extracts content, uploads to NotebookLM, generates podcast]
Done! Your podcast has been saved to ./ai-trends-podcast.mp3
```

### Example 2: Create Presentation Slides from a PDF

```
User: Make a presentation from ./quarterly-report.pdf

Claude: I'll convert your PDF into presentation slides...
[Parses PDF, uploads to NotebookLM, generates slides]
Done! Your presentation has been saved to ./quarterly-report-slides.pptx
```

### Example 3: Generate a Mind Map from YouTube Video

```
User: Create a mind map from https://www.youtube.com/watch?v=dQw4w9WgXcQ

Claude: I'll extract the video transcript and create a mind map...
[Fetches transcript, uploads to NotebookLM, generates mind map]
Done! Your mind map has been saved to ./video-mindmap.png
```

### Example 4: Create Flashcards from Multiple Sources

```
User: Generate flashcards from these sources:
- https://en.wikipedia.org/wiki/Machine_Learning
- ./ml-textbook-chapter1.pdf
- ./lecture-notes.docx

Claude: I'll combine these sources and create flashcards...
[Processes all sources, uploads to NotebookLM, generates flashcards]
Done! Your flashcards have been saved to ./ml-flashcards.json
```

### Example 5: Create an Infographic from CSV Data

```
User: Make an infographic from sales-data.csv showing quarterly trends

Claude: I'll analyze your data and create a visual infographic...
[Parses CSV, uploads to NotebookLM, generates infographic]
Done! Your infographic has been saved to ./sales-infographic.png
```

### Example 6: Generate a Quiz from Course Material

```
User: Create a 20-question quiz from ./biology-chapter5.pdf

Claude: I'll generate quiz questions from your course material...
[Extracts content, uploads to NotebookLM, generates quiz]
Done! Your quiz has been saved to ./biology-quiz.json
```

### Example 7: Batch Processing Multiple Articles

```
User: Create podcasts for each of these articles:
- https://techblog.com/article1
- https://techblog.com/article2
- https://techblog.com/article3

Claude: I'll process each article and generate separate podcasts...
[Processes each URL, generates individual podcasts]
Done! Created 3 podcasts:
- ./article1-podcast.mp3
- ./article2-podcast.mp3
- ./article3-podcast.mp3
```

## Command Reference

| Intent | Example Commands |
|--------|------------------|
| Podcast | "generate podcast", "create audio", "make a discussion" |
| Video | "make video", "create video content" |
| Slides | "create slides", "make PPT", "generate presentation" |
| Mind Map | "create mind map", "make concept map" |
| Infographic | "make infographic", "create visual summary" |
| Quiz | "generate quiz", "create test", "make questions" |
| Flashcards | "create flashcards", "make study cards" |
| Report | "write report", "create summary" |
| Table | "make table", "organize as spreadsheet" |

## Troubleshooting

### Authentication Issues

**Problem**: "Not authenticated" error

**Solution**:
```bash
notebooklm logout
notebooklm login
```

### File Not Found

**Problem**: Claude can't find your file

**Solution**: Use absolute paths or ensure you're in the correct directory:
```bash
# Use absolute path
Create slides from /Users/yourname/Documents/report.pdf

# Or navigate to the directory first
cd ~/Documents
claude
# Then use relative path
Create slides from ./report.pdf
```

### Unsupported File Type

**Problem**: File type not supported

**Solution**: Convert to a supported format:
- Documents: Convert to .docx, .pdf, or .txt
- Spreadsheets: Export as .csv or .xlsx
- Images: Use .png, .jpg, or .jpeg

### Generation Timeout

**Problem**: Output generation takes too long

**Solution**:
- For large files, try splitting into smaller chunks
- For videos, complex outputs may take 2-5 minutes
- Check NotebookLM status at [notebooklm.google.com](https://notebooklm.google.com)

### MCP Server Not Found

**Problem**: MCP features not working

**Solution**:
1. Verify config in `~/.claude/config.json`
2. Restart Claude Code
3. Check that notebooklm CLI is in your PATH

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License - see LICENSE file for details.
