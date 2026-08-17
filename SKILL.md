---
name: humanize-pro
description: >-
  Use when the user asks to humanize, unslop, de-slop, de-AI, clean up,
  tighten, or make text sound human or natural; when they say something reads
  like AI or like ChatGPT; or when drafting or rewriting outbound human
  communication: LinkedIn, X, cold or warm email, internal email, exec memo,
  Slack, blog, landing page, help content, press or public statements. Do not
  trigger for code, commit messages, PR descriptions, changelogs, legal or
  regulated text, or SEO structural elements (title tags, meta descriptions,
  keywords) unless the user explicitly asks to humanize those.
metadata:
  version: 1.1.0
---

# Humanize Pro

Two jobs, in this order: remove the statistical residue of a language model, then make the text fit the channel it ships in. Most humanizer skills only do the first, which produces clean prose that still lands wrong because a LinkedIn post is not a memo and a Slack reply is not an essay.

## Modes

Pick one. If unclear, ask in a single line.

- **Rewrite** (default): user pastes text, you return the fixed version.
- **Draft**: user gives raw notes or a brief, you write it clean the first time.
- **Audit**: user wants the tells named, not fixed. Return a numbered list of what would flag, with the line quoted and a one-line fix each. No rewrite.

## Precedence

Requirements outrank style. In order: the no-fabrication rule, then named external requirements (legal and regulated language, brand guidelines, SEO structure: title tags, meta descriptions, keyword placement, internal links), then the user's voice sample, then every style rule in this skill. When a style rule collides with something above it, keep the requirement and flag the conflict in one line after the text. Never resolve it silently.

## Workflow

### 1. Establish channel and audience

Before writing anything, fix these three:

- **Channel**: LinkedIn, X, cold email, internal email, exec memo, Slack, blog, landing page, product docs, press or analyst, board.
- **Direction**: internal or external. This governs candor, hedging, jargon tolerance, and how much context you restate.
- **Relationship**: cold, warm, peer, report, manager, exec, customer, public.

Infer from the text when it is obvious. Ask one short question when it is not. Do not ask three. If no answer is possible (non-interactive run, pipeline, subagent), assume rewrite mode and external direction, and flag the assumption in one line after the text.

Then load the matching section of `references/channels.md`. Load only what applies.

### 2. Calibrate voice

If the user has supplied a writing sample, a brand or voice skill, or prior approved work, read it and match sentence rhythm, vocabulary, and quirks. **A voice sample outranks every style rule in this skill except the no-fabrication rule.** If their real writing uses em dashes, keep them. If it uses one-line paragraphs, keep them.

With no sample, aim for competent-professional-with-a-pulse: plain words, varied sentence length, an opinion where an opinion belongs.

### 3. Universal pass

The checklist below covers most short texts. For long-form work (blog, memo, landing page), or when the audit pass flags something you cannot name, read `references/ai-tells.md` in full. In priority order:

1. **Cut the opener.** Delete throat-clearing, context-setting first sentences, and any signposting ("Let's dive in", "Here's the thing", "In today's landscape"). Start with the claim.
2. **Cut the closer.** Delete summary paragraphs that restate what was just said, "The future looks bright" style endings, "I hope this helps", and offers to continue.
3. **Break the shape.** AI text has uniform paragraph length, uniform bullet length, and triads everywhere. Vary paragraph length deliberately. If you wrote three items, check whether there are actually two or four.
4. **Kill the vocabulary.** No delve, leverage, landscape, tapestry, testament, pivotal, seamless, robust, journey, unlock, elevate, navigate, realm, crucial, vital, comprehensive, holistic. See the full list in references. The list is a frequency heuristic, not a blocklist: a listed word stays when it is the plain, precise choice and no simpler word does the same job.
5. **Kill the constructions.** No "It's not just X, it's Y." No "This isn't about A. It's about B." No "not because X, but because Y." No copula avoidance (serves as, stands as, boasts, features). No superficial -ing analysis (highlighting, underscoring, reflecting, showcasing).
6. **Kill the punctuation tells.** No em dashes as clause separators: use periods, commas, colons, parentheses. En dashes in numeric ranges (pages 3–5, 2019–2024) are correct typography and stay. No curly quotes outside typeset surfaces. No ellipsis for drama.
7. **Kill formatting slop.** No bold on random nouns. No "**Label:** sentence" bullets when prose works. No title case headings. No emoji unless the channel norm says otherwise. No markdown in a channel that does not render it.
8. **Cut hedge stacks.** "may potentially help" becomes "may help". Keep a hedge only when the claim actually is uncertain.
9. **Name the source or cut the claim.** No "experts say", "studies show", "many companies find".

### 4. Add texture

Removing tells produces text that is clean and dead. A human wrote this, so something in it should only be true for that human.

- Concrete specifics where the source has them: a number, a name, a date, a place, a version, a cost. A full paragraph with none is a flag to check the source again, not a quota to fill.
- An opinion where an opinion is appropriate, including the unflattering one.
- Admitted uncertainty when uncertainty is real ("I don't know why this worked" beats a manufactured explanation).
- Varied sentence length. Vary it where the argument varies, not as a formula: the long-sentence-then-short-one pattern applied everywhere is itself becoming a recognizable humanizer tell.

**No-fabrication rule, absolute:** specificity comes from the source text or the user, never from you. If a rewrite needs a number, a name, a date, or an anecdote that is not in the input, stop and ask for it or leave the sentence general. Never invent a statistic, a quote, a customer, or a personal experience. This rule outranks every instruction above.

### 5. Channel fit pass

Now apply the channel section from `references/channels.md`. This is where length, structure, opening convention, formatting, and sign-off get set. Text that survives step 3 and fails step 5 still reads wrong.

### 6. Audit pass

Read your own output cold and answer: **would a reader assume a model wrote this?**

Check specifically: uniform rhythm, a triad you did not notice, a resurfaced banned word, a tidy summary you re-added, bullets where prose belongs, a symmetrical structure (three paragraphs of equal length).

If yes on any, rewrite once. First drafts of a humanized rewrite reliably keep tells the second pass catches. The audit happens silently: catch a tell, fix it, show only the fixed text. No trace of the correction appears in the deliverable.

### 7. Deliver

Return the text and nothing else, unless the user asked for reasoning. No preamble about what you changed, no narration of the audit pass or mid-draft corrections. If you had to leave a gap because of the no-fabrication rule, flag it in one line after the text.

## Do not humanize

Leave alone: direct quotes, legal and compliance language, regulated claims, security disclosures, contract terms, safety instructions, and anything where a stripped qualifier changes accuracy. In regulated or external formal contexts, a hedge is often the substance and not a tell. When a caveat carries legal weight, keep it and say so.

Do not apply personality guidance to text where voice is not wanted: status reports, runbooks, API docs, incident timelines.

## False positives

Some patterns are legitimate. Do not strip them mechanically:

- A rule of three that is the actual number of items.
- Parallel structure in a board deck or a spec, where convention demands it.
- The word "leverage" when it means financial leverage.
- Formal register in a legal letter or an analyst briefing.
- Repetition of a term for clarity. Synonym cycling is the tell, not repetition.

## References

- `references/ai-tells.md`: full banned vocabulary, phrases, openers, structural patterns, punctuation and formatting tells, with before and after examples. Read for long-form work or when the audit pass flags a tell you cannot name; the step 3 checklist covers most short texts.
- `references/channels.md`: per-channel specs for length, structure, opening, formatting, sign-off, and the tells unique to that channel. Read only the sections that apply.

Pattern taxonomy draws on Wikipedia's "Signs of AI writing" (WikiProject AI Cleanup) and the open-source humanizer skill lineage that built on it.
