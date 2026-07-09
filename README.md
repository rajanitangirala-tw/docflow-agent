DocFlow Agent — AI-Powered Documentation Automation System
Author: Rajani Tangirala — Content Experience Lead, Salesforce
Built: July 2026
GitHub: github.com/rajanitangirala-tw/docflow-agent
---
What This Project Is
DocFlow Agent is a production-grade AI documentation automation system
I designed and built to demonstrate how Skills, MCP Servers, Plugins,
and Agents work together in a real technical writing workflow.
This is not a tutorial or a demo. It is the actual system I use to
manage documentation at Salesforce scale — adapted and documented here
as a portfolio project.
---
The Four Layers — How They Work Together
Layer 1: Skills
Skills are reusable Claude instruction playbooks.
Instead of writing a long detailed prompt every time I need Claude to
do a common task, I wrote the instructions once as a Skill file. I
trigger it by name and feed it the input. The output is always consistent,
always on-brand, always following our standards.
I built four Skills for this project:
Skill	What It Does
`release-note-generator`	Converts GUS engineering stories into customer-facing release notes
`api-endpoint-documenter`	Generates complete API reference pages from engineering specs
`sme-notes-processor`	Transforms raw SME call notes into structured doc outlines and follow-up questions
`doc-consistency-auditor`	Audits batches of articles against a style guide and produces a priority fix list
Each Skill encodes years of editorial judgment — what to include, what
to exclude, how to structure it, what to flag for verification. When a
new writer joins the team, they get the same quality output from day one.
---
Layer 2: MCP Servers
MCP Servers give Claude live access to my tools.
Instead of copying and pasting content into Claude manually, I configured
four MCP Server connections that give Claude direct read/write access to
my real tools in real time.
MCP Server	Tool	What Claude Can Do
GitHub MCP	GitHub repository	Read files, commit changes, open PRs, read issues
Filesystem MCP	Local docs folder	Read and write local Markdown files
Slack MCP	Slack workspace	Post notifications to channels
Google Drive MCP	Google Drive	Read SME notes, design docs, and PRDs
This means when an agent runs, it is working with real data — not
data I pasted in. It reads the actual documentation file, makes the
actual change, and commits to the actual repository.
---
Layer 3: Plugins
Plugins give Claude specialized capabilities it does not have by default.
For this project I use two plugins within the agent workflows:
Plugin	Purpose
Web Search	Claude searches for the current version of third-party APIs when documenting integrations
Code Execution	Claude runs code examples to verify they work before including them in documentation
The code execution plugin was particularly important for the API
endpoint documenter — every code example in the documentation was
actually run and verified by the agent before publishing.
---
Layer 4: Agents
Agents are autonomous orchestrators that combine Skills, MCPs, and Plugins.
I built two agents for this project:
Agent 1: Release Notes Pipeline Agent
Triggered when a GUS story is closed with a "doc-impact" tag. The agent
reads the story, classifies it, runs the release-note-generator Skill,
checks consistency against existing docs, commits to GitHub, and notifies
Slack. Human review at the end — no human involvement in the middle.
Agent 2: Weekly Feedback Triage Agent
Runs every Monday morning. Reads a week of collected feedback from Google
Drive, processes it through the doc-consistency-auditor Skill, creates the
top five issues as GitHub stories, fixes the most critical issue immediately,
and posts a weekly health summary to Slack.
---
The Complete Pipeline — How It All Flows
```
MONDAY MORNING — FEEDBACK TRIAGE

Google Drive (MCP)
    → Reads last week's feedback doc
    → doc-consistency-auditor SKILL
    → Deduplicates and prioritizes
    → GitHub MCP: creates 5 sprint stories
    → Filesystem MCP + GitHub MCP: fixes top issue
    → Slack MCP: posts health summary
    → Writer reviews in 20 minutes

CONTINUOUS — RELEASE NOTES

GitHub (MCP)
    → Detects closed GUS story with doc-impact tag
    → release-note-generator SKILL
    → Consistency check via Filesystem MCP
    → GitHub MCP: commits to release notes file
    → Slack MCP: posts review notification
    → Writer approves PR in 10 minutes
```
---
What I Learned Building This
1. Skills are the highest-leverage investment.
Writing a good Skill takes 2-3 hours. But it pays back immediately —
every task that uses that Skill is faster, more consistent, and requires
less review. The release-note-generator Skill alone has probably saved
20+ hours of writing time in the first month.
2. MCP connections remove the biggest friction in AI workflows.
Before MCP, I was spending significant time copying content into Claude,
copying results back out, pasting into files. MCP eliminated that entirely.
Claude reads the real file. Claude writes back to the real file. The
friction is gone.
3. Agents are only as good as the quality gates you build.
The most important design decision in each agent was not what it could do —
it was what it could not do without human approval. Every agent in this
project ends with a human review step. The agent handles the mechanical
work. The writer handles the judgment.
4. This is documentation systems architecture, not documentation writing.
Building this project shifted how I think about my role. I am not a writer
who uses AI tools. I am a documentation systems architect who designs
pipelines that produce documentation. The writing is still there — in the
Skills, in the review, in the final judgment. But the primary value I add
is in the system design.
---
Repository Structure
```
docflow-agent/
├── CLAUDE.md                          # Project briefing for Claude Code
├── README.md                          # This file
├── skills/
│   ├── release-note-generator.md      # Skill: GUS stories → release notes
│   ├── api-endpoint-documenter.md     # Skill: API specs → reference pages
│   ├── sme-notes-processor.md         # Skill: Call notes → doc outlines
│   └── doc-consistency-auditor.md     # Skill: Batch doc quality audit
├── mcp-config/
│   └── mcp-servers.md                 # MCP server configurations and setup
├── agents/
│   ├── release-notes-pipeline-agent.md  # Agent: End-to-end release notes
│   └── weekly-feedback-triage-agent.md  # Agent: Monday feedback processing
├── examples/
│   ├── sample-gus-story.md            # Sample input for release notes agent
│   ├── sample-api-spec.md             # Sample input for API doc Skill
│   └── sample-sme-notes.md           # Sample input for SME notes Skill
└── outputs/
    ├── sample-release-note-output.md  # Example agent output
    └── sample-api-doc-output.md       # Example Skill output
```
---
