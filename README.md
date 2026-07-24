# Qoory Skills

Give AI coding agents a reusable crypto intelligence workflow backed by Qoory's remote MCP server and Developer API.

## Install

```bash
npx skills add qoory-ai/qoory-skills --skill qoory
```

The installer supports Codex, Claude Code, Cursor, GitHub Copilot, Windsurf, Gemini CLI, OpenCode, Cline, and other agents that implement the Skills format.

## What It Does

The Qoory Skill helps an agent:

- discover and resolve crypto projects, funds, people, tokens, stocks, and X accounts;
- research funding, relationships, narratives, events, news, TVL, tokenomics, unlocks, and market signals;
- choose the smallest useful set of Qoory tools for a question;
- keep facts, Qoory-derived signals, and agent inference distinct;
- avoid bulk enumeration, uncontrolled retries, and credential exposure.

## Runtime

The Skill uses Qoory Remote MCP at `https://api.qoory.ai/mcp` and can fall back to the REST API when needed. Authentication, credits, rate limits, and access controls are enforced by Qoory's hosted service; this repository contains no credentials or application source.

Developer access is currently a limited beta. See [Qoory documentation](https://docs.qoory.ai/developers/skills) for setup details.

## Package

The installable Skill is under [`skills/qoory`](skills/qoory). Its references describe the supported tools, request bounds, credit costs, and failure handling.
