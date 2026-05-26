# skills-and-workflows

Curated skills and workflows — prompts, frameworks, and playbooks I
keep coming back to. Each entry is a single self-contained markdown
file: source attribution, the workflow itself, and notes on when to
use vs skip it.

## Layout

```
marketing/                — brand messaging, copy, growth
  storybrand-messaging-engine.md     7-prompt StoryBrand workflow
                                     (Donald Miller framework via
                                     @godofprompt, May 2026)
```

Future folders (when there's actual content for them):

- `engineering/` — debugging, refactoring, code-review playbooks
- `research/` — research synthesis, literature review, citation
- `product/` — PRD writing, user research, prioritisation
- `ops/` — incident response, on-call runbooks

## Conventions

- One workflow per file. If a workflow grows beyond ~500 lines, it
  probably needs to be split.
- Every file starts with **source attribution** — where the
  workflow came from, when it was captured, and a link to the
  original if there is one.
- Workflows that depend on other workflows link to them via
  relative path so the dependency graph is grep-able.
- Code blocks contain the actual prompt / shell / config the
  workflow uses, verbatim — paste-ready, no commentary inside the
  block.

## Why a repo and not a Notion / Obsidian / etc.

Two reasons:

1. **Diffable.** When I tune a prompt, I want the diff visible.
   Notion doesn't show what changed.
2. **Quotable.** I can link to a specific line of a specific
   workflow from anywhere (PR comment, chat, email) by URL —
   git-host-resolved.
