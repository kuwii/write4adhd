# The demystified style guide (book layer)

The book-form fingerprint of the demystified style: the form and the flourish a chaptered explainer adds on top of the plain rules. Everything quoted below is a **worked example** spanning different topics, there to show the rhythm and the sentence shapes — copy the moves, swap in your own subject matter.

The plain layer (plain words, conclusion first, the no-walls-of-text rule, bold density, instant reward, the general never-dos, the general self-check items) lives in `write4adhd-writing`'s `references/plain-style.md` — not repeated here. This document's rules stack on top of it.

## 1. Structure: a fast tour with dense checkpoints

- Numbered chapters (1, 2, 3…; the wrap-up chapter may use ∞), and **exactly one concept per chapter**.
- Each chapter is a few hundred words and skimmable; keep slicing inside it with subheadings (h3).
- Fix the main thread before you write: start from the most fundamental concept, and let each chapter end naturally raise the next chapter's question ("Ordering food on your phone is nice, but somebody still has to cook it and get it to your door… so what now? Hand it to the backend.").
- Close with a "Wrap-up" chapter: one formula that compresses the whole piece + one line the reader can carry away (cheeky is welcome, accurate comes first).
- **Shorter is better**: summarize hard, stop while it's still fun. Delete every urge to be exhaustive.

When the piece grows big enough to need chapters and pages (book form), the rhythm stays the same — the checkpoint just gets promoted to "page":

- **1–3 sections per page.** The page opens with one sentence of soft recap plus one sentence naming the problem this page solves, and closes with a wrap-up line. Every page is a complete little loop, so the reader can stop at any page boundary.
- **The TOC page is itself the hook**: give every chapter one line of suspense or reversal ("It's not actually on your phone?"), never "This chapter introduces storage principles".
- **The whole book still gets exactly one core metaphor**, tied back to at least once per page. Terminology stays consistent book-wide — you can't call it a "packet" in one chapter and a "chunk" in the next.
- Paging is not padding: chain chapters so that "the cliffhanger at the end of the previous page = the question at the start of the next one", which is what gives the reader the momentum to read one more page.

## 2. Titles: the headline names the subject, the subtitle brings in the metaphor

A reader should know what this page is about from the big headline alone, so the headline/subtitle division of labor is fixed:

- **The h1 (article title / book title / chapter title) is the subject itself**, named in plain words — **never the core metaphor as the headline**. "A plain-English primer on cloud storage" ✅; "The locker you rent inside someone else's gym" ❌ — the reader would have to guess what the piece is even about. A conversational hook is fine ("A primer on cloud storage: the 'cloud' is not as spooky as it sounds"), but the subject must be visible at a glance.
- **The .subtitle is where the core metaphor makes its entrance.** Main piece / cover page: one sentence brings the metaphor out and says what the reader will know by the end — "Turns out it's just a locker you rent inside someone else's gym: cloud storage in one sentence, including where your files actually sit". Chapter pages keep the intro as their subtitle (soft recap + the problem this page solves); the metaphor simply surfaces in the recap.
- The `<title>` tag matches the h1 — a plain subject in the browser tab, no metaphor.

## 3. The core metaphor: build exactly one anchor

Invent **one homespun metaphor** for the most central concept and keep returning to it, so the reader only has to remember one thing:

- cloud storage = "a locker you rent inside someone else's gym"
- an operating system = "the building's property management" (hands out rooms, fixes the plumbing, keeps order)
- a cache = "the sticky note on the fridge" (keep the everyday stuff within arm's reach instead of walking to the warehouse every time)
- one fast core vs. many cores = "one master chef vs. 50 people on a dumpling-folding line"

Give each secondary concept **one throwaway mini-analogy**, then move on: bandwidth = how wide the pipe is; an API = the order window at a diner; encryption = a suitcase with a combination lock; QR-code payment = a secret handshake.

How to pick: **the more everyday and the cheesier, the better** — kitchens, construction sites, classrooms, the grocery store, parcel lockers. Never explain a tech concept with another tech concept.

## 4. Conversational momentum: ask the reader's questions for them

Ask and answer your own questions where it helps — voice the objection that just popped into the reader's head and catch it immediately:

> "**'My phone only has 128 GB — how can the cloud hold basically everything?'** — Great question. The cloud isn't magic; it's a whole rack of hard drives in somebody else's data center, storing it for you…"
>
> "**But isn't a QR code just a picture?** No, it isn't. … A house key opens your own front door, and a hotel key card works on exactly the same idea — the lock changed, but 'only the matching one gets you in' didn't."

Right when the reader's attention starts to drift, the article has already asked their question for them. Put the questions where **the reader genuinely gets stuck** — roughly one every 2–3 paragraphs. A chapter that is nothing but self-posed questions reads like a performance; where a plain declarative sentence does the job, use one.

## 5. The closing line: land every chapter

Close every chapter (or every section) with one bolded line that compresses it into something the reader can carry away:

> "**Bottom line: you don't memorize every recipe — you just order whatever you feel like eating.**"
> "**What matters isn't how fast each person is; it's whether the work can be split up and handed out.**"
> "**The network didn't suddenly get faster — the road got wider.**"
> "**The app is the front counter, the API is the pass-through window, and the server is the chef actually working the wok.**"

Optional shapes (**options, not a quota**): `Bottom line: …` / `In one sentence: …` / `It's really just …` / `It's not X, it's Y` / balanced parallel phrasing — most of the time a plain declarative sentence is the best choice. A closing line must **stand on its own**: screenshot it, post it with no context, and it still reads.

Two anti-repetition requirements:

- **Never open with the same lead-in more than twice in a piece.** Three chapters in a row starting "Bottom line:" and the reader can recite your next ending for you.
- **If it won't compress, don't force it.** When a chapter carries more than one idea, write two accurate closing sentences rather than sacrifice information for one pretty line.

## 6. The three-step term intro

When a piece of jargon is unavoidable, follow the fixed pattern:

1. **Describe the phenomenon in plain words first** ("you send a message, your phone chops it into little pieces, ships them off separately, and the other side glues them back together");
2. **then name the jargon**, bolded, with the original term in parentheses ("those little pieces have a name in networking — a **packet**");
3. **then stop**: one intuitive sentence and move on. No implementation details, no edge cases, no history lesson (unless the history *is* the main thread).

If you can avoid the term, avoid it. Prefer the plain phrasing; put the original term in parentheses.

## 7. Diagrams: captions in human language

Every diagram gets one plain-words caption: one sentence saying what the diagram wants the reader to see — never a dry "Figure 1: XX architecture":

> "You order, the kitchen cooks, a rider shows up at your door — you never once see the chef's face"
> "Left: data you rarely touch sits in the warehouse. Right: data you use constantly stays within arm's reach (illustrative, not to scale)"
> "It checks who you are before it moves your money — that's a doorman, not a decoration"

An honest disclaimer inside the caption is welcome: "illustrative, not to scale".

## 8. ADHD-friendly extras (book form, on top of write4adhd-writing's hard rules)

1. **Soft recaps**: one sentence at the start of every chapter linking back to what came before ("Remember from last chapter: cloud storage is a locker you rent inside someone else's gym") — skimmers never get lost.
2. **A single anchor**: one core metaphor for the whole piece (see section 3). Zero working-memory load.
3. **Animation never hijacks attention**: animations autoplay but **pause automatically when scrolled out of the viewport**, and pause/step/replay controls are mandatory. Hand the controls to the reader.

## 9. Bonus pages

Side questions the reader may well ask, but whose expansion would wreck the main thread — **do not expand them in the body**. Split them into standalone bonus pages:

- Link to them from the body with a fixed format: "👉 Curious how the data center avoids losing your file? See the bonus page: *How Your Data Gets Backed Up*"
- Open the bonus page with the fixed hand-off: "After reading the main piece, you might be wondering: …"
- A bonus page uses exactly the same style as the main piece and ties back to the same core metaphor.
- In book form, a bonus page is a `.side` card in the table of contents (orange "B" badge); it takes no chapter number. The body still links to it with "👉 … see the bonus page".
- For a single short article with no bonus page, apply the same judgment as **subtraction**: dispose of the side question in one clause, or cut it entirely.

## 10. Never-do list (book form only; any of these means rework — the general items live in write4adhd-writing)

- ❌ The core metaphor used as the headline, so the reader can't tell from it what the piece is about
- ❌ Math formulas (unless the formula itself is the joke, e.g. `front counter + pass-through window + kitchen = takeout`)
- ❌ Dry captions ("Figure 1: system architecture diagram")
- ❌ A chapter with no closing line
- ❌ Using "demystify" or "that's it?" as a catchphrase — simplicity is proved by the content, not announced
- ❌ Ending every chapter with the same template, so that three chapters in, the reader can recite the next ending
- ❌ Belittling how hard the technology is, or mocking the industry and its practitioners, just to sound approachable
- ❌ A forced joke: a quip that stretches the paragraph without explaining anything more

## Self-check list (run it item by item when you're done; run write4adhd-writing's list first, then this one)

- [ ] Does every h1 name the subject at a glance (no metaphor headline), with the core metaphor brought in by the main-piece/cover subtitle?
- [ ] Does one core metaphor run through the whole piece, tied back to at least 3 times?
- [ ] Does every chapter end with a bolded closing line, with no lead-in repeated back to back ("Bottom line:" no more than twice in the whole piece)?
- [ ] Everywhere the reader is likely to get stuck, does the piece ask their question for them (rather than inserting one mechanically every two paragraphs)?
- [ ] Did every term go through the three-step intro?
- [ ] Does every chapter open with a soft recap (except chapter 1)?
- [ ] Does every diagram have a plain-words caption?
- [ ] Does the wrap-up chapter have the one-line formula + a line the reader can carry away?
- [ ] Read it through once: could a reader with zero background skim it in 15 minutes (book form: skim one page) and then say in their own words what the thing roughly is, what's hard about it, and why it isn't that hard?
- [ ] (Book form) Does every TOC blurb hook? Does every page have its intro recap + closing line? Do all three navigation links (Contents / Previous / Next) work?
- [ ] (Book form) Are the core metaphor, the terminology and the voice of the closing lines consistent across the whole book?
