---
name: humanize-pro
description: >-
  Use when the user asks to humanize, unslop, de-slop, de-AI, clean up,
  tighten, or make text sound human or natural; when they say something reads
  like AI or like ChatGPT; when they ask whether something matches their voice
  or style guide; or when drafting or rewriting outbound human
  communication: LinkedIn, X, cold or warm email, internal email, exec memo,
  Slack, blog, landing page, help content, press or public statements. Applies
  to prose inside deliverables too: documents, decks, artifacts, and generated
  files, not only chat replies. Do not
  trigger for code, commit messages, PR descriptions, changelogs, legal or
  regulated text, or SEO structural elements (title tags, meta descriptions,
  keywords) unless the user explicitly asks to humanize those.
metadata:
  version: 1.4.1
---

# Humanize Pro

Two jobs, in this order: remove the statistical residue of a language model, then make the text fit the channel it ships in. Most humanizer skills only do the first, which produces clean prose that still lands wrong because a LinkedIn post is not a memo and a Slack reply is not an essay.

A third job sits underneath both: sound like *this* writer, not like a generically de-slopped one. That is what the voice profile is for.

## When to trigger

Use this skill when:

- The user asks to humanize, unslop, de-slop, de-AI, clean up, or tighten text, or to make it sound human or natural.
- The user says something reads like AI or like ChatGPT.
- The user asks whether text matches their voice, style guide, or voice profile, or wants a profile built or updated.
- You are drafting or rewriting outbound human communication, even if nobody said "humanize": LinkedIn, X, cold or warm email, internal email, exec memo, Slack, blog, landing page, help content, press or public statements.
- Prose is going inside a deliverable: a document, deck, artifact, or generated file. The rules apply to the payload, not only the chat reply.

Do not trigger for:

- Code, commit messages, PR descriptions, or changelogs.
- Legal or regulated text.
- SEO structural elements (title tags, meta descriptions, keywords), unless the user explicitly asks to humanize those.

## Modes

Pick one. If unclear, ask in a single line.

- **Rewrite** (default): user pastes text, you return the fixed version.
- **Draft**: user gives raw notes or a brief, you write it clean the first time.
- **Audit**: user wants the tells named, not fixed. Return a numbered list of what would flag, with the line quoted and a one-line fix each. No rewrite.
- **Profile**: user wants their voice profile built or updated from samples and past corrections. See "Voice profile" below.

## Precedence

Requirements outrank style. In order:

1. The no-fabrication rule.
2. Named external requirements: legal and regulated language, brand guidelines, SEO structure (title tags, meta descriptions, keyword placement, internal links).
3. The user's **standing constraints** (see below).
4. The user's voice profile or writing sample.
5. Every style rule in this skill.

When a rule collides with something above it, keep the higher item and flag the conflict in one line after the text. Never resolve it silently.

## Standing constraints

A standing constraint is a rule the user has stated once and expects to hold forever, in every output, without being restated. Typical examples: a banned punctuation mark, a banned word, a forbidden greeting, a required sign-off, a length ceiling.

Three things make constraints different from style preferences:

- **They are absolute, not weighted.** A single violation is a failure even if the rest is excellent.
- **They apply to every surface.** Chat prose, generated documents, artifacts, file contents, headings, image captions, table cells, code comments in user-facing snippets. A constraint violated inside a deliverable is the most common failure mode, because the model applies the rule to its reply and forgets the payload.
- **They survive tool boundaries.** Text written to a file, passed to another skill, or produced inside an artifact is still the user's text.

Record standing constraints at the top of the voice profile. Before delivering anything, run the constraint list as a literal scan of the output, including every file you wrote this turn. Do not rely on having intended to comply.

## Voice profile

The voice profile is a versioned file describing how one specific person writes. It is not part of this skill; it belongs to the user. Look for it in this order:

1. A path the user names.
2. A personal brand or voice skill already loaded in the session.
3. `references/voice-profile.md` inside a private fork of this skill.
4. Persistent memory, if the surface has it.

If none exists and the user wants one, offer to build it. Do not build it silently.

### Format

```
# Voice profile: <name>
Version <n>, updated <date>

## Standing constraints        (absolute, never violated)
## Register                    (who they sound like, to whom)
## Sentence and paragraph shape
## Vocabulary: use / avoid
## Structural habits           (openers, closers, lists vs prose)
## Channel deltas              (how the voice shifts by surface)
## Open questions              (unresolved, needs more evidence)
## Changelog                   (what changed, what triggered it)
```

Every line carries a confidence marker:

- **stated**: the user said it directly. Treat as binding.
- **observed**: inferred from three or more independent samples of their own writing. Strong default, overridable.
- **provisional**: inferred from one or two samples, or from a single edit. Apply, but flag when it drives a visible choice.

Never promote a provisional line to stated. Only the user promotes.

### Building one

Sources, in descending value: text the user wrote themselves; text the user edited and shipped; text the user approved unchanged; text the user rejected, with the reason. Chat messages count, and are often the most honest sample, because nobody performs in a chat message.

Aim for one page. A voice profile that runs long stops being read.

## Capture corrections

The profile is worth having only if it stays current, and the raw material arrives constantly: every edit the user makes to your draft is a labeled training example.

When the user returns an edited version, rejects a draft, or says a line is wrong:

1. **Diff intent, not tokens.** Ask what class of change it is: a constraint violation, a register mismatch, a structural habit, a vocabulary preference, or a one-off factual fix. One-off fixes do not belong in the profile.
2. **Write the rule in their words where possible.** "Cut the setup sentence, start on the claim" beats "reduce preamble."
3. **Check for contradiction.** If the new correction contradicts an existing line, do not overwrite. Surface both and ask which holds. Contradictions are usually context-dependent rules that need a channel delta, not a replacement.
4. **Log it.** Add a changelog entry with the date and the triggering example. The changelog is what lets the user audit whether the profile is drifting.
5. **Update in the same turn.** A correction noted but not written is lost.

Two failure modes to watch. **Over-fitting**: one heavy edit becomes a permanent rule and the voice narrows. Require three instances before a provisional line becomes observed. **Sycophantic drift**: the profile fills with rules that make outputs more agreeable rather than more accurate. If a proposed line would suppress disagreement, a caveat, or an unwelcome finding, it is not a voice rule. Leave it out.

## Workflow

### 1. Establish channel and audience

Before writing anything, fix these three:

- **Channel**: LinkedIn, X, cold email, internal email, exec memo, Slack, blog, landing page, product docs, press or analyst, board.
- **Direction**: internal or external. This governs candor, hedging, jargon tolerance, and how much context you restate.
- **Relationship**: cold, warm, peer, report, manager, exec, customer, public.

Infer from the text when it is obvious. Ask one short question when it is not. Do not ask three. If no answer is possible (non-interactive run, pipeline, subagent), assume rewrite mode and external direction, and flag the assumption in one line after the text.

Then load the matching section of `references/channels.md`. Load only what applies.

### 2. Calibrate voice

Load the voice profile if one exists. Otherwise, if the user has supplied a writing sample, a brand or voice skill, or prior approved work, read it and match sentence rhythm, vocabulary, and quirks. **A voice profile or sample outranks every style rule in this skill except the no-fabrication rule and the user's standing constraints.** If their real writing uses em dashes, keep them. If it uses one-line paragraphs, keep them.

With no sample, aim for competent-professional-with-a-pulse: plain words, varied sentence length, an opinion where an opinion belongs.

### 3. Universal pass

The checklist below covers most short texts. For long-form work (blog, memo, landing page), or when the audit pass flags something you cannot name, read `references/ai-tells.md` in full. In priority order:

1. **Cut the opener.** Delete throat-clearing, context-setting first sentences, and any signposting ("Let's dive in", "Here's the thing", "In today's landscape"). Start with the claim.
2. **Cut the closer.** Delete summary paragraphs that restate what was just said, "The future looks bright" style endings, "I hope this helps", and offers to continue.
3. **Break the shape.** AI text has uniform paragraph length, uniform bullet length, and triads everywhere. Vary paragraph length deliberately. If you wrote three items, check whether there are actually two or four.
4. **Kill the vocabulary.** No delve, leverage, landscape, tapestry, testament, pivotal, seamless, robust, journey, unlock, elevate, navigate, realm, crucial, vital, comprehensive, holistic. See the full list in references. The list is a frequency heuristic, not a blocklist: a listed word stays when it is the plain, precise choice and no simpler word does the same job.
5. **Kill the constructions.** No "It's not just X, it's Y." No "This isn't about A. It's about B." No copula avoidance (serves as, stands as, boasts, features). No superficial -ing analysis (highlighting, underscoring, reflecting, showcasing).
6. **Kill redundant negation.** A clause asserted, then restated as its own negative: "to fit in and not feel excluded", "fast setup, no configuration needed". Delete the negative half. One exception: when the alternative being ruled out is genuinely what the reader would otherwise assume ("due to layoffs, not performance" on a resume gap), the clause carries information and stays. Apply the deletion test in `references/ai-tells.md`. Do not strip these mechanically.
7. **Kill the punctuation tells.** No em dashes as clause separators: use periods, commas, colons, parentheses. En dashes in numeric ranges (pages 3–5, 2019–2024) are correct typography and stay unless a standing constraint says otherwise. No curly quotes outside typeset surfaces. No ellipsis for drama.
8. **Kill formatting slop.** No bold on random nouns. No "**Label:** sentence" bullets when prose works. No title case headings. No emoji unless the channel norm says otherwise. No markdown in a channel that does not render it.
9. **Fix bullets that are sentences.** A bullet past roughly fifteen words, or carrying a subordinate clause, is prose in disguise. If the bullets only make sense read in order, they are an argument: convert to paragraphs. Otherwise compress to parallel fragments.
10. **Cut announced honesty.** No "the honest answer", "real talk", "I'll be blunt", "not the polite version". Honest writing does not flag itself, and the frame implies everything around it was less honest. Deliver the content instead.
11. **Make titles name their payload.** No "The X, and the Y sitting inside it". No concealment metaphors (lurking beneath, hiding in plain sight, what nobody tells you). No "Topic: a deeper look". State the claim. Sentence case.
12. **Cut hedge stacks.** "may potentially help" becomes "may help". Keep a hedge only when the claim actually is uncertain.
13. **Name the source or cut the claim.** No "experts say", "studies show", "many companies find".

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

Two gates, in order. The first is mechanical and has no judgment in it.

**Gate A, literal scan.** Scan the actual characters of every output produced this turn, files included:

- Each standing constraint from the voice profile.
- The em dash used as a clause separator.
- Banned vocabulary from step 3.
- The constructions from step 5, and redundant negation from step 6.
- Bullets over roughly fifteen words. Announced honesty. Titles that promise instead of name.
- Triads: three bullets, three clauses, three examples in a row.

Failing Gate A is not a matter of degree. Fix and rescan.

**Gate B, read it cold.** Answer: would a reader assume a model wrote this? Check for uniform rhythm, a tidy summary you re-added, bullets where prose belongs, paragraphs of equal length, a resurfaced banned word.

If yes on any, rewrite once. First drafts of a humanized rewrite reliably keep tells the second pass catches. Both gates happen silently: catch a tell, fix it, show only the fixed text. No trace of the correction appears in the deliverable.

### 7. Deliver

Return the text and nothing else, unless the user asked for reasoning. No preamble about what you changed, no narration of the audit pass or mid-draft corrections. If you had to leave a gap because of the no-fabrication rule, flag it in one line after the text.

If this turn produced a correction worth keeping, update the voice profile now. Say what you added in one line. Do not narrate anything else.

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
- `references/voice-profile-template.md`: blank template for building a personal voice profile, plus the maintenance protocol.

Pattern taxonomy draws on Wikipedia's "Signs of AI writing" (WikiProject AI Cleanup) and the open-source humanizer skill lineage that built on it.
