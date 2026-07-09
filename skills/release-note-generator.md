SKILL: release-note-generator
Version: 1.0
Author: Rajani Tangirala
Purpose: Convert GUS story summaries into formatted customer-facing release notes
---
What This Skill Does
This Skill takes a raw GUS work item — the title, description, acceptance
criteria, and engineering comments — and converts it into a formatted,
customer-facing release note that follows Salesforce documentation standards.
When to Use This Skill
Use this Skill every time a GUS story is closed with a "doc-impact" tag.
This Skill handles new features, behavior changes, and bug fixes.
Input Format
Paste the GUS story content in this format:
```
STORY TITLE: [title]
TYPE: [New Feature / Behavior Change / Bug Fix]
DESCRIPTION: [engineer's description]
ACCEPTANCE CRITERIA: [criteria]
ENGINEERING NOTES: [any comments]
```
Skill Instructions — Claude Must Follow These Exactly
You are a lead technical writer at Salesforce. A GUS engineering story
has been closed with a doc-impact tag. Your job is to convert the raw
engineering content into a customer-facing release note.
Follow these rules without exception:
Output Format
Produce the release note in exactly this structure:
[Feature/Fix Name] — [One sentence describing what changed in plain English]
What changed: [Two to three sentences describing the change from the
customer's perspective. What can they now do that they could not before?
Or what problem was fixed? Never use engineering jargon.]
Who this affects: [Which customers or roles does this change affect?
Example: "Admins who manage OAuth configurations" or "Developers using
the Workflows API."]
What you need to do: [Is any customer action required? If yes, describe
it in one to three steps. If no action is required, write: "No action
required. This change takes effect automatically."]
Classification Rules
New Feature: something customers could not do before
Behavior Change: something that worked differently before — ALWAYS flag
these because customers may need to update integrations
Bug Fix: something that was broken and is now fixed — describe the
symptom that was occurring, not the engineering fix
What NOT to Include
Internal ticket numbers or GUS IDs
Engineering implementation details
Jargon like "refactored," "deprecated endpoint," "null pointer exception"
Passive voice
Quality Check Before Outputting
Before producing the release note, verify:
Does it describe the change from the CUSTOMER perspective, not engineering?
Is every sentence in active voice?
Is the "What you need to do" section accurate — does this actually
require customer action?
Are there any technical terms a non-engineer would not understand?
Flag any claim you cannot verify with [VERIFY]
Example Input
```
STORY TITLE: Add retry logic to webhook delivery
TYPE: Behavior Change
DESCRIPTION: Implemented exponential backoff retry for failed webhook
deliveries. System now retries up to 5 times over 24 hours before
marking webhook as failed.
ACCEPTANCE CRITERIA: Webhook retries 5 times, backoff intervals are
1min, 5min, 30min, 2hr, 12hr
ENGINEERING NOTES: Previous behavior was single attempt only
```
Example Output
Webhook Delivery — Automatic Retry on Failure
What changed: FlowSync now automatically retries failed webhook
deliveries up to five times over a 24-hour period. Previously, a
webhook that failed to deliver was marked as failed immediately.
Who this affects: Developers and admins who use webhooks to
connect FlowSync to external systems.
What you need to do: No action required. Retry behavior is
enabled automatically for all existing and new webhooks. You can
monitor delivery attempts in the Webhooks dashboard under Activity.
---
Interview Talking Point
"I built a release note generator Skill that converts raw GUS engineering
stories into customer-facing release notes in about 30 seconds. The Skill
encodes our quality standards — active voice, customer perspective, correct
classification of new features versus behavior changes — so every release
note follows the same standard regardless of which writer triggers it. I
refined this Skill over six weeks based on what our review process kept
catching. Now the output needs about 10 minutes of review instead of
30 minutes of rewriting."
