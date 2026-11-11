# The plain style guide

The complete style fingerprint for plain documents. Everything quoted below is a **worked example** spanning different topics, there to show the rhythm and the sentence shapes — copy the moves, swap in your own subject matter.

**Ground rule: every stylistic move exists to make things clearer.** Bold, headings, examples and analogies are tools, not checkboxes to tick. Keep whatever makes the prose more natural and easier to follow; the moment you catch yourself performing a move for its own sake (manufacturing a quotable line, manufacturing a quip, announcing once again that "it's really not complicated"), cut it and say the thing plainly. **Plain and accurate always beats flashy and hollow.**

## 1. Voice: a person who knows the subject, stating facts

Write like a friend who knows the field explaining it over dinner — not like a memo, not like a textbook. Dial formality to the document type: a report can be half a notch more formal than a pop-science piece, but don't lose the human voice. A little teasing is allowed — aimed at **the jargon and the mystique**, never at the subject itself and never at the reader's intelligence:

> dead simple / comically over-engineered / deadpan nonsense delivered with a straight face

**"Simple" is the effect the reader comes away with, not a slogan you keep repeating.** Explain the complicated thing clearly and leave it at that — don't announce "it's really not complicated" on the reader's behalf. Let the content prove it.

> "When people say 'big data', here's what it's actually doing: writing down what you bought, then guessing what you'll buy next."

Humor is seasoning, not the meal: if one light line already landed the point, don't tack on a quip. A joke must never stretch a paragraph or bury the information. **Some of your readers may make a living from this stuff — never belittle how hard it is just to sound approachable.**

On questions that are genuinely unsettled, be straight about it — never fake authority:

> "I'm not confident in this conclusion: the two experiments disagree. The evidence so far supports 'probably X' — not 'it is X'."

## 2. Structure: conclusion first, effort weighted by importance

- **Open with a TL;DR**: one paragraph, or no more than three bullets, carrying everything the reader must take away. A reader who reads only the TL;DR should come away whole.
- **Headings are the importance hierarchy**: h2 = conclusion level, h3 = expansion level. Name them in plain words ("The bug: two modules grab the same lock" ✅, "Notes on the concurrency situation" ❌).
- **One idea per paragraph**; skeleton first, prose second — a paragraph you can't give a heading usually gets deleted or merged.
- **Key numbers and facts go where they're used**, not dumped at the end as a disclaimer. Data-heavy documents: table first — the table carries the numbers, the prose carries the interpretation.
- **Subtract**: side topics get one clause or move to an appendix; "I'm not sure this earns its place" = delete by default.
- **Shorter is better**: summarize hard, stop while it's still fun. Delete every urge to be exhaustive.

## 3. ADHD-friendly rules (hard requirements)

This skill's readers include impatient people and people with ADHD. These rules are not up for debate:

1. **No walls of text**: never more than 4 consecutive lines of plain text. Longer, break it up with a heading, turn it into a list, or bold the keywords. One idea per paragraph.
2. **Bold density**: at least 2–3 bolded spots per screenful (conclusions, key numbers, pivots), so a reader who only skims the bold text can still follow the main thread.
3. **Instant reward**: never more than 2–3 paragraphs without something light in them (an example, an analogy, a small table, a quote), so reading on always feels worth it — but vary the rhythm, and never let the reader smell that it's "quotable line o'clock" again.
4. **A sense of progress**: in a long document, provide a table of contents or obvious section breaks, so the reader always knows where they are and how much is left.

## 4. Never-do list (any of these means rework)

- ❌ Essay-ese: "Firstly / Secondly / Furthermore / In conclusion", "as summarized above"
- ❌ Long stretches of text with no bold and no subheadings
- ❌ Explaining jargon with more jargon (explaining "bandwidth" with "throughput", then "throughput" with "QPS")
- ❌ Even-handed effort: background gets the same page count as the conclusion
- ❌ The conclusion buried at the very end, with 80% of the piece as runway
- ❌ Exhaustive parameter dumps, version histories, edge cases
- ❌ Faking authority: a confident fake answer to an unsettled question
- ❌ Using "simple" / "it's really not complicated" as a catchphrase — simplicity is proved by the content, not announced

## Self-check list (run it item by item when you're done)

- [ ] Read only the bold text and the headings: do you still get every point of this document?
- [ ] Does the TL;DR carry everything the reader must take away — would a TL;DR-only reader come away whole?
- [ ] Any wall of text longer than 4 lines?
- [ ] Does every jargon term / internal concept get a one-line plain-words gloss the first time it appears?
- [ ] For each paragraph, "what would the reader not know if I cut this?": did you delete every paragraph that can't answer?
- [ ] Every uncertain conclusion marked with its confidence (or "not confirmed yet") and a way to check?
- [ ] Search the whole piece for catchphrases ("bottom line", "actually simple", "it's not that hard"…): did you delete every one that's decoration, not information?
- [ ] Any sentence written for style rather than for clarity? Rewrite it plainly.
- [ ] Read it through once: could an impatient reader get the points in a few minutes, without being worn down by even-handed padding?
