DocFlow Agent — AI-Powered Documentation Automation System
Author: Rajani Tangirala — Content Experience Lead, Salesforce  
Built: July 2026  
Repository: github.com/rajanitangirala-tw/docflow-agent
---
What This Project Is
DocFlow Agent is a production-grade AI documentation automation system I designed and built to demonstrate how Skills, MCP Servers, Plugins, and Agents work together in a real technical writing workflow.
This is not a tutorial or a demo. It is the actual system I use to manage documentation at Salesforce scale — adapted and documented here as a portfolio project.
---
The Four Layers — How They Work Together
Layer 1: Skills
Skills are reusable Claude instruction playbooks. Instead of writing a long detailed prompt every time I need Claude to do a common task, I wrote the instructions once as a Skill file. I trigger it by name and feed it the input. The output is always consistent, always on-brand, always following our standards.
I built four Skills for this project:
Skill	What It Does
release-note-generator	Converts GUS engineering stories into customer-facing release notes
api-endpoint-documenter	Generates complete API reference pages from engineering specs
sme-notes-processor	Transforms raw SME call notes into structured doc outlines and follow-up questions
doc-consistency-auditor	Audits batches of articles against a style guide and produces a priority fix list
---
Layer 2: MCP Servers
MCP (Model Context Protocol) Servers give Claude live access to my tools — instead of copy-pasting content into Claude manually, I configured direct connections so Claude reads and writes to real tools in real time.
MCP Server	Tool	What Claude Can Do
GitHub MCP	GitHub repository	Read files, commit changes, open PRs, read issues
Filesystem MCP	Local docs folder	Read and write local Markdown files
Slack MCP	Slack workspace	Post notifications to channels
Google Drive MCP	Google Drive	Read SME notes, design docs, and PRDs
See the full configuration: mcp-config/mcp-servers.md
---
Layer 3: Plugins
Plugins give Claude specialized capabilities it does not have by default.
Plugin	Purpose in This Project
Web Search	Looks up current API versions when documenting integrations
Code Execution	Runs code examples to verify they work before including in documentation
---
Layer 4: Agents
Agents are autonomous orchestrators. I give an agent a goal — it decides which Skills and MCP connections to use at each step and executes the full workflow without me guiding it manually.
I built two agents for this project:
Agent	What It Does
Release Notes Pipeline Agent	Reads a GUS story, generates a release note, checks consistency, commits to GitHub, notifies Slack — all autonomously
Weekly Feedback Triage Agent	Reads a week of feedback from Google Drive, prioritizes top 5 issues, creates GitHub sprint stories, fixes the top issue immediately
---
The Complete Pipeline
```
MONDAY MORNING — FEEDBACK TRIAGE

Google Drive (MCP) → Reads last week's feedback
    → doc-consistency-auditor SKILL
    → Deduplicates and prioritizes top 5 issues
    → GitHub MCP: creates 5 sprint stories
    → Filesystem MCP + GitHub MCP: fixes top issue immediately
    → Slack MCP: posts weekly health summary
    → Writer reviews in 20 minutes ✓

CONTINUOUS — RELEASE NOTES

GitHub (MCP) → Detects closed GUS story with doc-impact tag
    → release-note-generator SKILL
    → Consistency check via Filesystem MCP
    → GitHub MCP: commits to release notes file
    → Slack MCP: posts review notification
    → Writer approves PR in 10 minutes ✓
```
---
Repository Structure
```
docflow-agent/
├── CLAUDE.md                              # Claude Code project briefing file
├── README.md                              # This file
│
├── skills/
│   ├── release-note-generator.md          # Skill: GUS stories → release notes
│   ├── api-endpoint-documenter.md         # Skill: API specs → reference pages
│   ├── sme-notes-processor.md             # Skill: Call notes → doc outlines
│   └── doc-consistency-auditor.md         # Skill: Batch doc quality audit
│
├── mcp-config/
│   └── mcp-servers.md                     # MCP server configurations
│
├── agents/
│   ├── release-notes-pipeline-agent.md    # Agent: End-to-end release notes
│   └── weekly-feedback-triage-agent.md    # Agent: Monday feedback processing
│
├── examples/
│   └── sample-gus-story-input.md          # Sample input for release notes agent
│
└── outputs/
    └── sample-release-note-output.md      # Real example: agent output + editorial review
```
---
Sample Output — The Proof
The most important file in this project is outputs/sample-release-note-output.md.
It shows a complete, real example of the release notes agent running:
Raw GUS story input — what the engineer wrote
Agent step-by-step log — what the agent did autonomously (completed in 8 seconds)
Agent draft output — what Claude produced
Writer's editorial review — three specific changes I made and exactly why
Final published release note — the approved version
This before-and-after demonstrates the human-AI collaboration model: AI handles the mechanical work, the writer provides the judgment.
---
Tools Used
Tool	Purpose
Claude (claude.ai)	Primary AI brain — reasoning, drafting, auditing
Claude Code	Autonomous repo-level agent operations
Cursor	Code-aware editor with AI for cross-file consistency checks
GitHub	Version control and repository hosting
Slack	Notification layer for agent-to-writer handoff
Google Drive	SME notes and design document source
---
What I Learned Building This
Skills are the highest-leverage investment. Writing a good Skill takes 2–3 hours. But it pays back immediately — every task that uses that Skill is faster, more consistent, and requires less review.
MCP connections remove the biggest friction. Before MCP, I was spending significant time copying content into Claude and copying results back out. MCP eliminated that entirely.
Agents are only as good as the quality gates you build. The most important design decision in each agent was what it could not do without human approval. Every agent ends with a human review step.
This is documentation systems architecture, not documentation writing. Building this project shifted how I think about my role. I am not a writer who uses AI tools. I am a documentation systems architect who designs pipelines that produce documentation.
---
This project was designed and built by Rajani Tangirala as a portfolio demonstration of AI-powered documentation architecture. The Skills, Agents, and MCP configurations are based on real workflows used in enterprise documentation environments at Salesforce.
