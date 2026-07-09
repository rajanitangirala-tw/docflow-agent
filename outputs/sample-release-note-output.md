Sample Agent Output — Release Notes Pipeline
This file shows a real example of the release notes pipeline agent
running end to end. It demonstrates what the agent produced, what
the writer reviewed, and what was changed before publishing.
---
The Input — Raw GUS Story
```
STORY TITLE: Allow workspace admins to set custom session timeout
TYPE: New Feature
DESCRIPTION: Added a new setting in Workspace Settings > Security
that allows admins to configure the session timeout duration.
Previously fixed at 8 hours. Now configurable between 1 hour and 30 days.
Default remains 8 hours.
ACCEPTANCE CRITERIA:
- Setting visible in Workspace Settings > Security > Session Management
- Accepts values in hours (1-720)
- Changes apply to all new sessions immediately
- Existing sessions not affected until they expire naturally
ENGINEERING NOTES: Value stored in minutes internally but displayed in
hours in the UI
```
---
What the Agent Did — Step by Step Log
```
[09:02:14] Agent started — processing GUS story: session-timeout-config
[09:02:15] GitHub MCP — read story details ✓
[09:02:15] Classified as: New Feature ✓
[09:02:16] Running skill: release-note-generator
[09:02:18] Skill complete — draft generated ✓
[09:02:18] Filesystem MCP — reading existing release notes: v2.2.0.md ✓
[09:02:19] Terminology check: all terms consistent ✓
[09:02:19] No conflicts found ✓
[09:02:20] GitHub MCP — inserting release note into v2.2.0.md ✓
[09:02:21] GitHub MCP — committing: "docs: add release note for session timeout config" ✓
[09:02:22] Slack MCP — posting to #doc-reviews ✓
[09:02:22] Agent complete — review notification sent
Total time: 8 seconds
```
---
Agent Draft Output (Before Writer Review)
Session Timeout — Admin Configuration
What changed: Workspace admins can now set a custom session timeout
duration in Workspace Settings. Previously, sessions timed out after
8 hours for all users. Admins can now configure the timeout between
1 hour and 30 days to match their organization's security requirements.
Who this affects: Workspace admins on Enterprise plans who manage
session security settings.
What you need to do: No action required. The default session timeout
remains 8 hours. To change it, go to Workspace Settings >
Security > Session Management and enter your preferred timeout
duration in hours.
---
Writer's Review — What I Changed and Why
Change 1: Added a note about existing sessions
The engineering notes mentioned that existing sessions are not affected
until they expire naturally. This is important information for admins
who expect the change to take effect immediately for all users. The
agent did not include it because it was in the engineering notes, not
the acceptance criteria.
Before: "To change it, go to Workspace Settings..."
After: "To change it, go to Workspace Settings... Note: The new
timeout applies to new sessions only. Existing active sessions continue
until they expire at their original timeout."
Change 2: Verified the plan requirement
The agent wrote "Enterprise plans" because our CLAUDE.md says the
session management feature is enterprise-only. But I checked the
release spec and this feature is available to Professional plans and
above. I corrected this.
Before: "Workspace admins on Enterprise plans"
After: "Workspace admins on Professional and Enterprise plans"
Change 3: Added the value range clearly
The agent mentioned "1 hour and 30 days" but did not make the range
explicit enough for admins who might want to set a specific number.
I added the exact range.
Before: "between 1 hour and 30 days"
After: "between 1 hour (minimum) and 720 hours / 30 days (maximum)"
---
Final Published Release Note
Session Timeout — Admin Configuration
What changed: Workspace admins can now set a custom session timeout
duration in Workspace Settings. Previously, sessions timed out after
8 hours for all users. Admins can now configure the timeout between
1 hour (minimum) and 720 hours / 30 days (maximum) to match their
organization's security requirements. The default remains 8 hours.
Who this affects: Workspace admins on Professional and Enterprise plans.
What you need to do: No action required. The default session timeout
remains 8 hours. To change it, go to Workspace Settings >
Security > Session Management and enter your preferred timeout
duration in hours.
> **Note:** The new timeout applies to new sessions only. Existing active
> sessions continue until they expire at their original timeout duration.
---
