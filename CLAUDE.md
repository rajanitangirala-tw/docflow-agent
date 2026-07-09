# DocFlow Agent — AI-Powered Documentation Automation System
# Built by: Rajani Tangirala — Content Experience Lead
# Version: 1.0 | July 2026

## What This Project Is
DocFlow Agent is a production-grade AI documentation automation system
built using Claude Skills, MCP Server connections, and autonomous agent
workflows. It demonstrates how a senior technical writer architects and
deploys AI agents to automate documentation pipelines at Salesforce scale.

## Project Architecture
This project uses four layers working together:

1. SKILLS — Reusable Claude instruction playbooks for common doc tasks
2. MCP SERVERS — Live connections to GitHub, GUS, Confluence, and Slack
3. PLUGINS — Extended capabilities (web search, code execution)
4. AGENTS — Autonomous orchestrators that combine all three layers

## My Role in This Project
I am the documentation systems architect. I designed every Skill,
configured every MCP connection, defined every agent workflow, and
built the quality review gates. I do not write documentation from
scratch — I design the system that produces documentation.

## Writing Standards — Apply to All Agent Outputs
- Second person: you / your — never "the user"
- Active voice always
- Present tense for product behavior
- Sentence case for all headings
- Sentences under 20 words
- Every procedure ends with a result statement
- Never use: simple, easy, just, please, in order to, utilize

## Terminology
- FlowSync (capital F, capital S)
- workflow (not pipeline or flow)
- trigger (not event or hook)
- API key (not token unless OAuth-specific)
- workspace (not org or tenant)

## Agent Behavior Rules
- Flag any technical claim it cannot verify with [VERIFY]
- Flag any content gap with [NEEDS INPUT]
- Never publish without human review approval
- Always log what changed and why
- Match style of surrounding content in any file edited

## Folder Structure
- /skills — Claude Skill definition files (.md)
- /mcp-config — MCP Server configuration files
- /agents — Agent workflow definition files
- /examples — Sample inputs and outputs for each Skill
- /outputs — Agent-generated documentation drafts
