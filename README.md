# humanize-pro

Make AI-drafted text read like a person wrote it, for the channel it ships in.

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Claude skill](https://img.shields.io/badge/Claude-skill-d97757.svg)](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)
[![GitHub stars](https://img.shields.io/github/stars/msdanyg/humanize-pro?style=flat)](https://github.com/msdanyg/humanize-pro/stargazers)

humanize-pro is a skill for Claude, Claude Code, and the Claude Agent SDK. It removes the tells that make writing read as AI-generated (the em dashes, the rule of three, "delve" and "seamless" and "testament to", the tidy summary ending) and then reshapes the result for where it will actually be read: LinkedIn, X, cold email, internal email, exec memos, Slack, blogs, landing pages, help docs, press.

That second pass is the point. Most humanizer prompts stop after stripping vocabulary, which produces clean prose that still lands wrong, because a LinkedIn post is not a memo and a Slack reply is not an essay.

## Before and after

**Slack reply to "is the deploy done?"**

> **Before:** "Hi team! I wanted to provide a quick update regarding the deployment status. **Summary:** The deployment to production has been completed successfully. **Next steps:** 1. We will continue to monitor error rates. 2. The changelog will be updated accordingly. Please don't hesitate to reach out with any questions!"
>
> **After:** "yep, done. watching errors for an hour, then I'll update the changelog."

**Cold email opener**

> **Before:** "I hope this email finds you well. I noticed you're the VP of Marketing at Acme and wanted to reach out because I believe our platform could help you unlock significant efficiencies."
>
> **After:** "Saw Acme is hiring two PMMs this quarter. We built the positioning tooling one of them would otherwise spend six weeks on."

**Marketing copy**

> **Before:** "It's not just about efficiency, it's about clarity, alignment, and momentum."
>
> **After:** "It cut the review cycle from nine days to three."

## Install

Claude Code:

```
git clone https://github.com/msdanyg/humanize-pro ~/.claude/skills/humanize-pro
```

Claude.ai: zip this folder, rename the archive to `humanize-pro.skill`, and upload it under Settings → Capabilities → Skills.

Claude Agent SDK: point `skills` at a directory containing this folder.

## Use

Say "humanize this", "unslop this", or "this sounds like ChatGPT", or just draft in a channel the skill covers. Four modes:

- **Rewrite**: paste text, get the fixed version back, nothing else.
- **Draft**: give raw notes, get a clean first version shaped for the channel.
- **Audit**: "audit this for AI tells" returns a numbered list of what would flag, with a fix for each, and no rewrite.
- **Profile**: "build my voice profile" reads your samples and past corrections and writes a one-page file the skill loads on every future run.

## Voice profiles

Stripping AI tells gets you clean text. It does not get you *your* text. A voice profile is a one-page versioned file describing how one specific person writes: their standing constraints, register, sentence shape, vocabulary, and how the voice shifts by channel. When present, it outranks every style rule in this skill.

The profile is meant to be maintained, not written once. Every edit you make to a draft is a labeled example, so the skill classifies the change, writes the rule in your words, and logs it with the date and trigger. Two guards keep the file honest: a provisional rule needs three independent instances before it hardens, and no rule may enter that suppresses disagreement or an unwelcome finding. See [references/voice-profile-template.md](references/voice-profile-template.md).

Profiles are personal, so this repo ships the template rather than any real one. Keep yours at `references/voice-profile.md` in a private fork, in persistent memory, or anywhere you can point the skill at.

## What it will not do

- **Invent facts.** Specifics come from your input. A missing number, name, or anecdote is flagged as a gap, never filled. This rule outranks every style rule in the skill.
- **Break your requirements.** Legal language, brand guidelines, and SEO structure (title tags, keywords, internal links) are kept, and any conflict with a style rule gets flagged in one line instead of resolved silently.
- **Flatten your voice.** A writing sample from you outranks the skill's own style rules. If you use em dashes, you keep them.

## FAQ

**Why does my writing sound like ChatGPT?**
Language models over-produce specific patterns: uniform paragraph rhythm, three of everything, em-dash clause breaks, words like "delve" and "robust" and "landscape", a summary paragraph that restates the piece. Readers pattern-match on these fast. [references/ai-tells.md](references/ai-tells.md) catalogs them with fixes.

**Does this bypass AI detectors?**
Not the goal. Detectors are unreliable in both directions, and gaming them is a treadmill. This skill optimizes for the only detector that matters: a human reader deciding whether you wrote it. Text that passes that test tends to score better everywhere, but write for readers, not scanners.

**Will it work with models other than Claude?**
The skill format is Claude's, but the two reference files are plain markdown and model-agnostic. Paste them into any system prompt.

**What counts as an AI tell in 2026?**
A moving target, which is why the vocabulary list is documented as a frequency heuristic rather than a blocklist. "Delve" is already fading; formatting and structural tells (label-bulleted Slack messages, symmetrical sections) age slower than word lists.

## Contributing

The highest-value PRs are new channel specs (decks, SMS, forum replies, YouTube descriptions) in the format of [references/channels.md](references/channels.md), and new tells with before/after pairs for [references/ai-tells.md](references/ai-tells.md). One rule: examples must be real patterns you have seen, not invented ones. The skill has a no-fabrication rule and so does the repo.

## Lineage

The pattern taxonomy draws on Wikipedia's [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) (WikiProject AI Cleanup) and the open-source humanizer skill lineage that built on it.

[MIT license](LICENSE).
