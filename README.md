[ [中文](README.zh_CN.md) | English ]

# Write for ADHD

A set of skills that make your coding agent write for readers who won't sit still.

You bounce off walls of text, and halfway through a long post you've forgotten what it was about. If that's you, this repo was built for you. It distills a writing method that has already proven itself into an agent skill: **short chapters, homespun metaphors, a closing line for every chapter, hand-drawn Canvas animations**. Install it and your agent writes that way for you.

## What you get

- **Demystified**: you finish able to say in your own words what the thing is and where the hard part was, instead of "sounds impressive, still don't get it";
- **One homespun metaphor running through the whole piece**: exactly one thing to remember;
- **A bolded closing line for every chapter**: stop anywhere, pick it back up anywhere;
- **No paragraph longer than 4 lines**: walls of text are illegal here;
- **Hand-drawn Canvas diagrams + step animations**: they autoplay, pause when you scroll away, and hand you pause/step/replay buttons — animation serves you, it never holds you hostage;
- **The output is a finished artifact**: a single self-contained HTML page, a multi-page HTML book site, or Markdown — light and dark themes, works offline, zero external dependencies.

## Skills

Every skill comes in **two language variants**: identical rules and templates, only the output language differs. Pick the one that writes in your language — they share a name, so install just that one.

| Skill | What it does | 中文 | English |
|---|---|---|---|
| `write4adhd-writing` | Plain, ADHD-friendly documents that get read: research reports, experiment reports, summaries, memos, technical notes. Conclusion first, plain words, no undifferentiated information dumps | [`skills-cn/write4adhd-writing`](skills-cn/write4adhd-writing/) | [`skills-en/write4adhd-writing`](skills-en/write4adhd-writing/) |
| `write4adhd-book` | Explainers, docs and chaptered books in the demystified style, delivered as HTML (with animated diagrams) or Markdown. **Install it together with `write4adhd-writing`** — the plain-writing base layer lives there | [`skills-cn/write4adhd-book`](skills-cn/write4adhd-book/) | [`skills-en/write4adhd-book`](skills-en/write4adhd-book/) |
| `write4adhd-dev-onboarding` | Onboarding books for a software project: surveys an unfamiliar codebase first, then writes a primer that takes a junior developer from "knows nothing about this domain" to "can change the code". **Install it together with `write4adhd-book`** (and therefore writing) — the flourish and templates come from book, the plain base from writing | [`skills-cn/write4adhd-dev-onboarding`](skills-cn/write4adhd-dev-onboarding/) | [`skills-en/write4adhd-dev-onboarding`](skills-en/write4adhd-dev-onboarding/) |

All skills carry the **`write4adhd-`** prefix and live in a per-language directory: `skills-cn/` writes Chinese, `skills-en/` writes English. The three form one dependency chain: `write4adhd-writing` owns *plain and short* (conclusion first, plain words, the ADHD-friendly layout rules), `write4adhd-book` adds the explainer-book form and flourish on top of it, and `write4adhd-dev-onboarding` only says what to investigate and what to write. Pick by what you want to write: only reports/docs → install writing; explainer books → writing + book; onboarding books → all three, same language.

## Quick start

Copy the skill directory for your language into your project (or into your user-level directory to make it available everywhere):

```bash
# Reports / docs (plain style):
cp -r skills-en/write4adhd-writing <your-project>/.agents/skills/

# Explainer books (bring the base writing layer along):
cp -r skills-en/write4adhd-writing skills-en/write4adhd-book <your-project>/.agents/skills/

# Onboarding books (all three layers; use skills-cn for the Chinese skills):
cp -r skills-en/write4adhd-writing skills-en/write4adhd-book skills-en/write4adhd-dev-onboarding <your-project>/.agents/skills/

# Available in every project: replace <your-project>/.agents/skills/ with ~/.agents/skills/
```

Then, in any agent that supports [Agent Skills](https://agentskills.io/) (such as pi):

```text
/skill:write4adhd-writing turn this survey into a report
/skill:write4adhd-book write a plain-English primer on Docker
/skill:write4adhd-dev-onboarding help me get up to speed on this project
```

Or just ask naturally: "turn this research into a report" (writing), "explain XXX like I've never heard of it" (book) — and with `write4adhd-dev-onboarding` installed, a bare "help me get up to speed on this project" is enough; it infers everything else and tells you its defaults before starting.

## Repo layout

```text
write4adhd/
├── skills-cn/               # Chinese skills
│   ├── write4adhd-writing/  # plain writing layer: SKILL.md + references/
│   ├── write4adhd-book/
│   │   ├── SKILL.md         # entry point: workflow + non-negotiables
│   │   ├── references/      # style guide, HTML page craft
│   │   └── assets/          # copy-paste page / book-site templates
│   └── write4adhd-dev-onboarding/  # onboarding books: survey protocol + chapter blueprint + code-kit styles
├── skills-en/               # English skills (same layout)
│   ├── write4adhd-writing/
│   ├── write4adhd-book/
│   └── write4adhd-dev-onboarding/
├── README.md                # this file
├── README.zh_CN.md          # 中文版
├── AGENTS.md                # context for agents maintaining this repo
└── LICENSE
```

## License

[MIT](LICENSE)
