# AI tells

Detection and repair reference. The SKILL.md step 3 checklist covers most short texts; read this file in full for long-form work or when the audit pass flags something you cannot name.

## Contents

1. Vocabulary
2. Phrases and openers
3. Sentence constructions
4. Structural patterns
5. Punctuation
6. Formatting
7. Conversational artifacts
8. Accuracy failures
9. Before and after

---

## 1. Vocabulary

Words that spike in model output and rarely in ordinary writing. Replace with the plain word, or cut the sentence. This list is a frequency heuristic, not a blocklist: a listed word stays when it is the plain, precise choice ("essential" in docs stating a field is required, "vital" in a medical claim). It also ages; which words spike shifts with each model generation, so judge by whether a plainer word does the same job, not by list membership alone.

delve, dive into, tapestry, landscape, realm, journey, testament, pivotal, crucial, vital, essential, robust, seamless, streamlined, holistic, comprehensive, nuanced, multifaceted, intricate, myriad, plethora, leverage (as a verb), utilize, facilitate, foster, harness, unlock, elevate, empower, navigate (figurative), underscore, highlight (figurative), showcase, embark, cultivate, resonate, align (figurative), amplify, transformative, groundbreaking, revolutionary, cutting-edge, state-of-the-art, game-changing, next-generation, best-in-class, unparalleled, unwavering, ever-evolving, fast-paced, dynamic, vibrant, bustling, nestled, boasts, stands as, serves as, ensure (as filler), meticulous, invaluable, paramount, profound, remarkable, compelling, captivating.

Also flag: significance inflation ("marking a turning point in the evolution of"), promotional adjectives on neutral facts, and any adjective doing the job a number should do.

## 2. Phrases and openers

**Openers to delete outright**: Certainly. Absolutely. Of course. Great question. In today's [anything]. In the world of. When it comes to. It's important to note. It's worth noting. Let's dive in. Let's break it down. Here's the thing. Here's what you need to know. The truth is. At its core. Fundamentally. In essence. Simply put. Look. Honestly. I'll be honest.

**Connectives on repeat**: Moreover, Furthermore, Additionally, Consequently, Notably, Importantly, That said, Ultimately, In conclusion.

**Closers to delete**: I hope this helps. Let me know if you have any questions. Feel free to reach out. The future looks bright. Only time will tell. One thing is clear. At the end of the day. In summary (when the piece is short enough not to need one).

**Vague attributions**: experts say, studies show, research suggests, many believe, it is widely regarded, industry leaders agree. Name the source, or cut the claim.

## 3. Sentence constructions

| Pattern | Example | Fix |
|---|---|---|
| Negative parallelism | "It's not just a tool, it's a platform." | State what it is. |
| Contrastive reframe | "This isn't about speed. It's about trust." | Pick one and say it. |
| Tailing negation | "Fast setup, no configuration needed." | "Setup takes two minutes." |
| Copula avoidance | "serves as / stands as / boasts / features" | is, has |
| Superficial -ing analysis | "reflecting a broader shift, highlighting the need for" | Cut, or make it a claim with a source. |
| Synonym cycling | protagonist, then main character, then central figure | Repeat the clearest term. |
| False range | "from onboarding to churn, everything changed" | Name the actual things. |
| Aphorism formula | "Trust is the currency of teams." | Say the concrete claim instead. |
| Manufactured staccato | "No meetings. No decks. No excuses." | Vary length, make one real claim. |
| Rhetorical fake-candor | "Honestly? It depends." | Answer. |
| Hedge stack | "may potentially be able to help" | "may help" |
| Filler | "in order to", "due to the fact that", "the fact that" | to, because, that |

## 4. Structural patterns

- **Rule of three everywhere.** Three benefits, three examples, three adjectives, three sections. Count your items and check whether the real number is two, four, or five.
- **Uniform rhythm.** Every sentence 15 to 20 words. Every paragraph 3 sentences. Every bullet one line. Break it on purpose.
- **Symmetry.** Sections of matched length, lists of matched length, a mirrored intro and conclusion.
- **Formulaic challenge arc.** "Despite challenges, X continues to thrive." Cut the arc, keep the facts.
- **Tidy takeaway.** A closing paragraph that resolves everything neatly. Real writing often ends on the unresolved part.
- **Signposted sections.** A heading announcing what the next paragraph will do, followed by the paragraph doing it.
- **Fragmented headers.** A heading followed by a single short sentence.
- **Diff-anchored writing.** Describing what changed rather than what the thing is or does.

## 5. Punctuation

- **Em dash as clause separator**: cut. Use a period, comma, colon, or parentheses. This is the single most recognized tell. Exceptions: the user's own voice sample uses them; en dashes in numeric ranges (pages 3–5, 2019–2024, a 2–1 vote) are correct typography and stay.
- **Curly quotes and apostrophes**: convert to straight, unless the destination is a typeset page.
- **Exclamation points**: at most one, only if the enthusiasm is real.
- **Ellipsis for suspense**: cut.
- **Semicolons**: fine in long-form, wrong in Slack and social.

## 6. Formatting

- Bold on scattered nouns and phrases inside body copy. Cut.
- "**Label:** sentence" bullet blocks where prose works. Convert.
- Title Case Headings. Use sentence case.
- Emoji as bullets or section markers. Cut, unless the channel norm allows.
- Markdown in a surface that does not render it: LinkedIn, X, plain-text email, Slack headers.
- Nested bullets three levels deep.
- Horizontal rules between every section.

## 7. Conversational artifacts

- Sycophancy: "Great question", "You're absolutely right", "What a fascinating problem".
- Offers to continue: "Would you like me to expand on any of these?"
- Meta-commentary about the writing: "I've structured this into three sections."
- Knowledge or source disclaimers inside the deliverable: "While details are limited in available sources".
- Restating the prompt before answering it.

## 8. Accuracy failures

These are worse than style tells because they survive editing.

- Invented statistics, percentages, and dollar figures.
- Fabricated quotes and attributed opinions.
- Invented anecdotes and customer examples.
- Fake precision ("a 34% lift") where the source said "a meaningful lift".
- Confident specifics filling a gap the source left open.

**Rule**: specificity comes from the source or the user. When a sentence needs a fact you do not have, ask for it or leave it general. Never fill it.

## 9. Before and after

**Significance inflation**
- Before: "The launch marked a pivotal moment in the company's evolution, showcasing its unwavering commitment to innovation."
- After: "The launch added self-serve signup, which had been on the roadmap for two years."

**Negative parallelism plus triad**
- Before: "It's not just about efficiency, it's about clarity, alignment, and momentum."
- After: "It cut the review cycle from nine days to three."

**Corporate hedge stack**
- Before: "We believe this may potentially represent a significant opportunity going forward."
- After: "This looks like a real opportunity. We'll know by the end of Q3."

**Slack overreach**
- Before: "**Summary:** The deploy is complete. **Next steps:** 1. Monitor errors 2. Update the changelog. Please let me know if you have any questions!"
- After: "deploy's done. watching errors for an hour, then i'll update the changelog."
- Also fine: "Deploy's done. I'm watching errors for an hour, then updating the changelog." Register follows the user and the channel; lowercase is an option, not a rule.

**Cold email**
- Before: "I hope this email finds you well. I noticed you're the VP of Marketing at Acme and wanted to reach out because I believe our platform could help you unlock significant efficiencies. Would you be opposed to a quick 15 minutes?"
- After: "Saw Acme is hiring two PMMs this quarter. We built the positioning tooling one of them would otherwise spend six weeks on. Worth a look, or is this already handled?"
