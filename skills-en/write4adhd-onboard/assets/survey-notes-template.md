# Survey notes: <project> @ <short commit>

> Copy into a scratch area (a gitignored directory in your project) and fill in there. **Never mix it into the deliverable directory.** This is writing material, not a deliverable.
> Tag every finding with a confidence mark as you write the notes, not later from memory:
> **✅ read the source/config** ｜ **▶ actually ran it** ｜ **⚠️ inferred (implementation not read)** ｜ **❓ didn't get it**
> Writing discipline: a ⚠️ either gets verified and upgraded, or appears in the prose hedged; a ❓ gets confessed plainly with an entry point for checking.

## 0. One sentence

<what this project is, whose problem it solves> (✅/⚠️)

- Base commit: `<hash>` (<date>)
- Order of magnitude: <thousands / hundreds of thousands / millions of lines; primary language>
- Core metaphor candidate: <the one everyday metaphor that can carry the whole architecture>

## 1. Meta layer

| Item | Finding | Confidence |
|---|---|---|
| Official docs entry | | |
| Contributing guide location & key points | | |
| CI config / which checks run | | |
| Issue templates, newcomer-friendly labels | | |
| Licence | | |
| Release cadence (last few tags) | | |
| Places where docs and code disagree | | |

## 2. Shape layer

- Language composition (top file types):
- Top-level dirs with file counts (where the weight sits):

| Dir | Files | One-line responsibility | Core / ignorable |
|---|---|---|---|

- Build system & dependency management:
- Module split and dependency direction (who depends on whom):
- Dirs excluded from the survey (generated code / deps / build output / test data):

## 3. History layer

- Most active dirs in the last six months (top 5):
- Core team / main contributors:
- What's being worked on lately (summarised from commit messages):
- Commit message conventions:
- Branch and release model:

## 4. Entry layer

| Entry type | Location (file:line) | Notes | Confidence |
|---|---|---|---|
| User entry (command / API / job) | | | |
| Process entry (main / startup script) | | | |
| Extension entry (plugin / SPI / registry / config-driven) | | | |

## 5. Lifeline layer ★ spine trace

**Chosen main line**: <the smallest, user-visible, meaningful operation, e.g. `xxx --input a.txt` or `spark.range(3).show()`>
**Why this one**: <small enough / runs core paths / the reader can run it themselves>

| # | Location (file:line or file:symbol) | What this step does (plain words) | Quote in book? | Confidence |
|---|---|---|---|---|
| 1 | | | | |
| 2 | | | | |

**Break points** (async / callbacks / message queues / RPC / reflection / dynamic registration / codegen):

| Where it breaks | What reconnects it | How you found out | Confidence |
|---|---|---|---|

**Core abstractions (≤7)**:

| Term | One plain sentence | Defined at | Who creates it | Who consumes it | Lifetime | Confidence |
|---|---|---|---|---|---|---|

**Code snippets worth quoting in the book (pick 4–8, each trimmed to ≤15 lines)**:

| Source | What quoting it explains |
|---|---|

## 6. Dev-loop layer (what the newcomer needs on day two)

| Action | Command | Expected duration | Ran it? | Notes / pit |
|---|---|---|---|---|
| Environment prep (version requirements) | | | ▶/⚠️ | |
| Full build | | | | |
| Build only the module you care about | | | | |
| Run all tests | | | | |
| **Run a single test file / case** | | | | |
| Run a minimal example | | | | |
| Style / static checks | | | | |
| Read logs (where, how to change level) | | | | |
| Attach a debugger / remote debug | | | | |

- Submission flow: branch naming → commit message → PR template → CLA/DCO → where CI failures show up:
- Checks that must pass locally before submitting (can they run in one command?):

## 7. Prerequisite gaps

Reader profile: junior programmer, zero background in this domain. List only what **genuinely blocks understanding the main line**.

| Concept | Why it's needed | One plain sentence | Where it goes (ch. 1 / backfill at point of use / appendix) |
|---|---|---|---|

## 8. Pits and warnings (this book's differentiating value)

-

## 9. Unresolved questions (handle each while writing: verify, or confess + give an entry point)

- ❓

## 10. Chapter cut decisions

| Blueprint chapter | In this book? | Why / which spine segment it covers |
|---|---|---|
| 1 Prerequisites crash course | | |
| 2 What it is | | |
| 3 Make it run | | |
| 4 Codebase map | | |
| 5 Core abstractions | | |
| 6 Walk it (the spine) | | |
| 7 Hard bones | | which ones: |
| 8 Changing code: build to PR | | |
| 9 Your first change | | practice-task candidates: |
| ∞ Wrap-up + where next | | |
| app Jargon glossary | | |
| app Errors & pitfalls lookup | | |

## 11. Unfamiliar noun collection point (feeds the jargon glossary)

| Term | Plain words | Original term | What it's called in code | Defined at |
|---|---|---|---|---|
