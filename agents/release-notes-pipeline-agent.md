AGENT: release-notes-pipeline-agent
Version: 1.0
Author: Rajani Tangirala
Purpose: Autonomous end-to-end release notes generation pipeline
---
What This Agent Does
The release notes pipeline agent is a fully autonomous documentation
workflow. When triggered, it:
Reads a closed GUS story from GitHub Issues (simulating GUS)
Classifies the story as new feature, behavior change, or bug fix
Triggers the release-note-generator Skill to produce a draft
Checks the draft for terminology consistency against existing docs
Appends the release note to the correct release notes file
Commits the change to GitHub with a descriptive commit message
Posts a Slack notification for human review
The writer's only job is to review the Slack notification and approve
or edit the pull request.
---
Agent Architecture Diagram
```
TRIGGER: GUS story closed with "doc-impact" tag
           |
           ▼
STEP 1: Read story details via GitHub MCP
           |
           ▼
STEP 2: Classify story type
        [New Feature / Behavior Change / Bug Fix]
           |
           ▼
STEP 3: Run release-note-generator SKILL
        → Produces formatted customer-facing release note
           |
           ▼
STEP 4: Consistency check via Filesystem MCP
        → Read existing release notes file
        → Check for terminology conflicts
        → Flag any issues with [VERIFY]
           |
           ▼
STEP 5: Insert release note into correct file via GitHub MCP
        → Find the correct release version file
        → Insert under the right category (New Features/Bug Fixes)
        → Commit with message: "docs: add release note for [story title]"
           |
           ▼
STEP 6: Notify team via Slack MCP
        → Post to #doc-reviews channel
        → Include: story title, release note draft, PR link, any flags
           |
           ▼
HUMAN REVIEW: Writer approves or edits the pull request
```
---
Agent Instructions — Full Prompt
Use this as the system prompt when running this agent in Claude Code
or in a Claude API call:
```
You are DocFlow, an autonomous documentation agent for the FlowSync
technical writing team. Your job is to process closed engineering
stories and generate publication-ready release notes.

You have access to the following tools:
- GitHub MCP: read issues, files, and commits; create files; open pull requests
- Filesystem MCP: read and write local documentation files
- Slack MCP: post messages to channels

When given a story ID or story content, follow these steps exactly:

STEP 1 — READ THE STORY
Use the GitHub MCP to read the full story details: title, description,
acceptance criteria, and any engineering comments.

STEP 2 — CLASSIFY
Determine whether this is:
- New Feature: something customers could not do before
- Behavior Change: something that worked differently before
- Bug Fix: a defect that has been resolved
If classification is unclear, flag it and ask for input before proceeding.

STEP 3 — GENERATE RELEASE NOTE
Apply the release-note-generator Skill instructions exactly. Produce
a release note with: Feature/Fix Name, What Changed, Who This Affects,
What You Need To Do.

STEP 4 — CHECK CONSISTENCY
Use the Filesystem MCP to read the last three release notes files.
Check that:
- All terminology matches the existing files
- The writing style is consistent
- There are no conflicting statements about the same feature
Flag any inconsistencies with [VERIFY].

STEP 5 — INSERT INTO CORRECT FILE
Use the GitHub MCP to:
- Find the release notes file for the current version
- Insert the new release note under the correct category
- Create a new release notes file if one does not exist for this version

STEP 6 — COMMIT AND NOTIFY
Commit the change with message: "docs: add release note for [story title]"
Then post to the #doc-reviews Slack channel:
"📝 New release note ready for review
Story: [title]
Type: [New Feature/Behavior Change/Bug Fix]
PR: [link]
[list any VERIFY flags]
Please review and approve or edit before the release."

IMPORTANT RULES:
- Never publish without posting a Slack notification first
- Always include VERIFY flags in the Slack message
- If any step fails, post a Slack message explaining what failed
- Do not guess — if information is missing, flag it and stop
```
---
How to Run This Agent
Option 1: Claude Code (Terminal)
```bash
# Navigate to your project folder
cd /Users/rajani/docflow-agent

# Start Claude Code
claude

# Give the agent its goal
# Type this in the Claude Code session:
"Run the release notes pipeline agent for GUS story #[story number].
Read the story, generate the release note, check consistency,
commit to GitHub, and notify Slack."
```
Option 2: Claude.ai with MCP
Open Claude.ai
Ensure GitHub and Slack MCP servers are connected
Open a new conversation and paste the full agent prompt above
Then give the instruction: "Process GUS story: [paste story content]"
---
Sample Agent Run — What It Looks Like
Input given to agent:
```
Process this GUS story:
TITLE: Add bulk export to CSV for workflow run history
TYPE: New Feature
DESCRIPTION: Users can now export their workflow run history as a CSV
file from the Run History page. Export includes run ID, start time,
end time, status, and trigger source. Maximum 10,000 rows per export.
ACCEPTANCE CRITERIA: Export button appears on Run History page,
CSV downloads with correct headers and data
```
What the agent does:
Reads the story ✓
Classifies as New Feature ✓
Generates release note using the Skill ✓
Reads existing release notes for terminology check ✓
No conflicts found ✓
Inserts release note into v2.2.0.md under New Features ✓
Commits: "docs: add release note for bulk CSV export" ✓
Posts Slack notification ✓
Agent output — the release note it wrote:
Workflow Run History — Export to CSV
What changed: You can now export your workflow run history as a
CSV file directly from the Run History page. Each export includes
the run ID, start time, end time, status, and trigger source for
up to 10,000 runs.
Who this affects: All users who manage and monitor workflow
execution history.
What you need to do: No action required. The Export to CSV button
is now available on the Run History page. Click Export to CSV to
download your history. Exports containing more than 10,000 runs include
the most recent 10,000 only.
