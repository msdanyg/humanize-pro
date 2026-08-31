# humanize-pro

Make AI-drafted text read like a person wrote it, for the channel it ships in.

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Claude skill](https://img.shields.io/badge/Claude-skill-d97757.svg)](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)
[![GitHub stars](https://img.shields.io/github/stars/msdanyg/humanize-pro?style=flat)](https://github.com/msdanyg/humanize-pro/stargazers)

humanize-pro is a skill for Claude, Claude Code, and the Claude Agent SDK. It removes the tells that make writing read as AI-generated (the em dashes, the rule of three, "delve" and "seamless" and "testament to", the tidy summary ending) and then reshapes the result for where it will actually be read: LinkedIn, X, cold email, internal email, exec memos, Slack, blogs, landing pages, help docs, press.

## Why this over other humanizer skills

Most humanizers on the marketplace are a banned-word list: cut "delve", swap out the em dashes, return the text. That gets you clean prose that still lands wrong and sounds like nobody. This skill treats the word list as step one of four:

1. **De-AI pass.** The full tell catalog: vocabulary, constructions, punctuation, formatting, structure. Documented as a frequency heuristic rather than a blocklist, so a listed word survives when it is the plain, precise choice.
2. **Channel fit.** Per-channel specs for length, structure, opening convention, formatting, and sign-off across LinkedIn, X, cold and internal email, exec memos, Slack, blogs, landing pages, docs, and press. A stripped-clean Slack reply that reads like an essay still fails.
3. **Your voice.** A versioned one-page profile built from your own writing and updated from every edit you make to a draft, with confidence markers on each rule and guards against overfitting and sycophantic drift. When present, it outranks the skill's own style rules.
4. **A two-gate audit before delivery.** First a literal scan of every output, generated files included, against your standing constraints and the tell list. Then a cold read: would a reader assume a model wrote this? Fix and rescan until the answer is no.

Under all four sits an absolute no-fabrication rule: the skill never invents a statistic, a customer, a quote, or an anecdote to make text feel human. A humanizer that manufactures specifics is a liability, not a feature.

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

**Claude Code** (recommended; this path gets updates):

```
/plugin marketplace add msdanyg/humanize-pro
/plugin install humanize-pro@humanize-pro
```

Updates ship as version bumps to this repo. To receive them automatically, open `/plugin`, go to the Marketplaces tab, and enable auto-update for `humanize-pro`. Without auto-update, run `/plugin marketplace update humanize-pro` whenever you want the latest.

**Upgrading from a manual copy** (pre-1.5.0 installs): delete the old copy first so two versions of the skill don't both load, then install via the marketplace above.

```
rm -rf ~/.claude/skills/humanize-pro
```

**Claude.ai**: zip the `skills/humanize-pro` folder, rename the archive to `humanize-pro.skill`, and upload it under Settings → Capabilities → Skills. Claude.ai has no update channel, so re-upload when a new version ships.

**Claude Agent SDK**: point `skills` at a directory containing `skills/humanize-pro`.

## Use

Say "humanize this", "unslop this", or "this sounds like ChatGPT", or just draft in a channel the skill covers. Four modes:

- **Rewrite**: paste text, get the fixed version back, nothing else.
- **Draft**: give raw notes, get a clean first version shaped for the channel.
- **Audit**: "audit this for AI tells" returns a numbered list of what would flag, with a fix for each, and no rewrite.
- **Profile**: "build my voice profile" reads your samples and past corrections and writes a one-page file the skill loads on every future run.

## Voice profiles

Stripping AI tells gets you clean text. It does not get you *your* text. A voice profile is a one-page versioned file describing how one specific person writes: their standing constraints, register, sentence shape, vocabulary, and how the voice shifts by channel. When present, it outranks every style rule in this skill.

The profile is meant to be maintained, not written once. Every edit you make to a draft is a labeled example, so the skill classifies the change, writes the rule in your words, and logs it with the date and trigger. Two guards keep the file honest: a provisional rule needs three independent instances before it hardens, and no rule may enter that suppresses disagreement or an unwelcome finding. See [the voice profile template](skills/humanize-pro/references/voice-profile-template.md).

Profiles are personal, so this repo ships the template rather than any real one. Keep yours at `skills/humanize-pro/references/voice-profile.md` in a private fork, in persistent memory, or anywhere you can point the skill at.

## What it will not do

- **Invent facts.** Specifics come from your input. A missing number, name, or anecdote is flagged as a gap, never filled. This rule outranks every style rule in the skill.
- **Break your requirements.** Legal language, brand guidelines, and SEO structure (title tags, keywords, internal links) are kept, and any conflict with a style rule gets flagged in one line instead of resolved silently.
- **Flatten your voice.** A writing sample from you outranks the skill's own style rules. If you write one-line paragraphs, you keep them. The one exception is the em dash: it stays banned unless you explicitly ask for it back, because it is the single loudest AI tell regardless of who actually wrote it.

## FAQ

**Why does my writing sound like ChatGPT?**
Language models over-produce specific patterns: uniform paragraph rhythm, three of everything, em-dash clause breaks, words like "delve" and "robust" and "landscape", a summary paragraph that restates the piece. Readers pattern-match on these fast. [ai-tells.md](skills/humanize-pro/references/ai-tells.md) catalogs them with fixes.

**Does this bypass AI detectors?**
Not the goal. Detectors are unreliable in both directions, and gaming them is a treadmill. This skill optimizes for the only detector that matters: a human reader deciding whether you wrote it. Text that passes that test tends to score better everywhere, but write for readers, not scanners.

**Will it work with models other than Claude?**
The skill format is Claude's, but the two reference files are plain markdown and model-agnostic. Paste them into any system prompt.

**What counts as an AI tell in 2026?**
A moving target, which is why the vocabulary list is documented as a frequency heuristic rather than a blocklist. "Delve" is already fading; formatting and structural tells (label-bulleted Slack messages, symmetrical sections) age slower than word lists.

## Contributing

The highest-value PRs are new channel specs (decks, SMS, forum replies, YouTube descriptions) in the format of [channels.md](skills/humanize-pro/references/channels.md), and new tells with before/after pairs for [ai-tells.md](skills/humanize-pro/references/ai-tells.md). One rule: examples must be real patterns you have seen, not invented ones. The skill has a no-fabrication rule and so does the repo.

## Lineage

The pattern taxonomy draws on Wikipedia's [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) (WikiProject AI Cleanup) and the open-source humanizer skill lineage that built on it.

[MIT license](LICENSE).
