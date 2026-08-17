# humanize-pro

A Claude skill that rewrites or drafts text so it reads like a person wrote it for the channel it ships in.

Two passes, in order: strip the statistical residue of a language model (flagged vocabulary, rule-of-three, em-dash clause breaks, signposting, formulaic structure), then fit the result to the destination. LinkedIn, X, cold email, internal email, exec memo, Slack, blog, landing page, help content, press.

Most humanizers stop after the first pass. Clean prose still lands wrong when a LinkedIn post is shaped like a memo.

## Install

Claude Code:

```
git clone https://github.com/msdanyg/humanize-pro ~/.claude/skills/humanize-pro
```

Claude.ai: zip this folder, rename the archive to `humanize-pro.skill`, and upload it under Settings → Capabilities → Skills.

## Use

- "humanize this", "unslop this", "this sounds like ChatGPT"
- Draft mode: give raw notes, get a clean first version
- Audit mode: "audit this for AI tells" returns a numbered list of what would flag, with fixes, and no rewrite

Style yields to requirements. Legal language, brand guidelines, and SEO structure (title tags, keywords, internal links) are kept, and any conflict with a style rule gets flagged in one line instead of being resolved silently.

## What it will not do

Invent facts. Specifics come from your input; a missing number, name, or anecdote is flagged as a gap, never filled.

## Files

- `SKILL.md`: modes, workflow, precedence rules
- `references/ai-tells.md`: vocabulary, constructions, structural and formatting tells, before/after examples
- `references/channels.md`: per-channel specs for length, structure, opening, formatting, sign-off

## Lineage

The pattern taxonomy draws on Wikipedia's "Signs of AI writing" (WikiProject AI Cleanup) and the open-source humanizer skill lineage that built on it.

MIT license.
