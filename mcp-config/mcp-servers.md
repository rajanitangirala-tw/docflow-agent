MCP Server Configuration — DocFlow Agent
Author: Rajani Tangirala
Version: 1.0 | July 2026
What MCP Servers Are
MCP (Model Context Protocol) servers are live connections between Claude
and external tools. Instead of copying and pasting information into Claude
manually, MCP servers give Claude direct read and write access to your
tools in real time.
Think of each MCP server as a direct line between Claude and a tool.
Claude can read from it, act on it, and write back to it — all from
within a single conversation or agent workflow.
---
MCP Servers Used in DocFlow Agent
1. Perfoce (P4) MCP Server
Purpose: Read and write access to the documentation repository
What Claude can do through this connection:
Read any file in the documentation repo
Check when a file was last modified
Search for specific terms across all files
Create new files
Commit changes with a message
Open pull requests
Read open issues and link fixes to them
How it is used in DocFlow Agent:
The P4 MCP server is the backbone of the pipeline. When the release
notes agent detects a new GUS story, it uses the P4 connection to:
Read the existing release notes file for that release
Add the new release note in the correct format
Check for terminology conflicts across other files
Commit the updated file with a descriptive commit message

2. Filesystem MCP Server
Purpose: Read and write access to local documentation files
What Claude can do through this connection:
Read any file in your local documentation folder
Create new files
Edit existing files
List folder contents
Check file metadata (size, modified date)
How it is used in DocFlow Agent:
The filesystem MCP server lets Claude work directly on your local files
without you having to copy and paste content into the conversation.
This is used for:
Running the doc consistency auditor across all local files
Generating first drafts and saving them to the correct folder
Reading multiple files simultaneously to check cross-references
Configuration:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/rajani/Documents/documentation-repo"
      ]
    }
  }
}
```
---
3. Slack MCP Server
Purpose: Post notifications and updates to Slack channels
What Claude can do through this connection:
Post messages to specific Slack channels
Read recent messages from a channel
Send direct messages
How it is used in DocFlow Agent:
The Slack MCP server is used as the notification layer in the pipeline.
When an agent completes a documentation task, it posts a Slack message
to the #doc-reviews channel with:
What was changed
Which files were affected
A link to the pull request for review
Any flags that need human attention
Configuration:
```json
{
  "mcpServers": {
    "slack": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-slack"],
      "env": {
        "SLACK_BOT_TOKEN": "YOUR_SLACK_BOT_TOKEN",
        "SLACK_TEAM_ID": "YOUR_TEAM_ID"
      }
    }
  }
}
```
---
4. Google Drive MCP Server
Purpose: Read access to Google Drive files (SME notes, design docs, PRDs)
What Claude can do through this connection:
Read Google Docs files
Read Google Sheets files
Search Drive for files by name or content
How it is used in DocFlow Agent:
After SME calls, meeting notes and design documents are stored in Google
Drive. The SME notes processor agent uses the Drive MCP server to:
Read the latest SME notes document automatically
Process them through the sme-notes-processor Skill
Save the structured outline as a new file in the documentation folder
---
How MCP Servers Fit Into the Agent Architecture
```
AGENT (the orchestrator)
    ↓
    ├── Reads GUS story via MCP → GitHub
    ├── Runs release-note-generator SKILL
    ├── Checks existing files via MCP → Filesystem
    ├── Commits changes via MCP → GitHub
    └── Notifies team via MCP → Slack
```
The agent does not do any of the heavy lifting itself. It orchestrates
the Skills and MCP connections — choosing which tool to use at each step
and passing information between them.
