# fyp-env-setup-development-team

The **setup repo** for every FYP-Desk development team member. When the team
takes on a new FYP idea (a deal), a member clones this repo and turns the
`kit/` directory into that project's own controlled-environment repo —
context-engine pattern + FYP-Desk-specific modules, wired 0–100 out of the box.

## Content table

| Section | What it tells you |
|---------|-------------------|
| [How to start a new project repo](#how-to-start-a-new-project-repo) | the 6 steps from clone to working engine |
| [What is in the kit](#what-is-in-the-kit) | every module, one line each |
| [The flow inside a project repo](#the-flow-inside-a-project-repo) | who builds what, 0→100 |
| [The contribution rule](#the-contribution-rule) | proof-of-work → pay split |
| [Using the agent skill library](#using-the-agent-skill-library) | 279 specialists, one curl |
| [FAQ](#faq) | the questions everyone asks |

## How to start a new project repo

```bash
git clone https://github.com/FYP-DESK/fyp-env-setup-development-team.git
cd fyp-env-setup-development-team

# 1. break the connection to the setup repo
rm -rf .git

# 2. rename the kit into the new project repo
mv kit <fyp-idea-NN>-<slug>        # e.g. mv kit fyp-idea-01-zameenchain
cd <fyp-idea-NN>-<slug>

# 3. delete the setup-repo instructions (you already followed them)
rm README.md                       # (this file — only exists at the setup repo root)

# 4. fill the placeholders: repo name + IDEA-NNN in SYSTEM.md and INDEX.md
#    (pick the IDEA-NNN from fyp-ideas IDEAS_INDEX.json — status must be "selected")

# 5. make it your repo and push it
git init && git add -A && git commit -m "kit: initialize controlled environment for <fyp-idea-NN>"
git remote add origin https://github.com/FYP-DESK/<fyp-idea-NN>-<slug>.git
git push -u origin main

# 6. build the knowledge graph — must exist before the first task
graphify update .
```

You now have a complete controlled environment. Every session after this starts
from `SYSTEM.md`.

## What is in the kit

| Module | Purpose |
|--------|---------|
| `SYSTEM.md` | engine bootstrap — every session starts here |
| `INDEX.md` | wiring map of all modules |
| `ARCHITECTURE.md` | how the layers fit together |
| `chat-history/` | Q/A history, word-for-word (1st instance) |
| `tasks-history/` | task definitions, trackers, logs, executions (2nd+3rd instance) |
| `instructions/` | one rule file per module + the instance decision flow |
| `notes/` | distilled knowledge, topic-organized |
| `guides/` | compiled how-to guides |
| `contribution-history/` | append-only proof-of-work feed (powers valuation) |
| `deliverables/` | proposal, PPT, the 4 codebase-guidance docs, final docs/PPT |
| `app/` | ONE codebase, ONE idea — the only place code is allowed |
| `graphify-out/` | knowledge graph over context AND app code |
| `utility/` | on-demand runnable procedures |

## The flow inside a project repo

```
idea selected in fyp-ideas (IDEAS_INDEX.json status → "selected")
        │
        ▼
[any member]  deliverables/proposal/         ① proposal .md (+ .docx export)
[any member]   deliverables/proposal/ppt/     ② teacher PPT
[team lead*]   deliverables/codebase-guidance/ ③ the 4 docs: SRS · SDD · tests · API/data
[any member]   app/                           ④ the codebase (guided by the 4 docs)
[any member]   deliverables/final-docs/       ⑤ final documentation + PPT
```

\* the team lead builds the 4 docs by default; any member who has become
familiar with the process may take it for a later project.

## The contribution rule

Every completed step ①–⑤ = **one appended file** in
`contribution-history/` (e.g. `c001.md`) with its row in `contribution-history/index.md`.

- No contribution file = the work is not recorded = **no pay split**.
- Small questions and answers do NOT go here — they stay in the instance
  modules (chat-history / tasks-history).
- The org repo [contribution-tracker-valueation](https://github.com/FYP-DESK/contribution-tracker-valueation)
  imports these records, values the work, and computes each member's share:
  10% awareness + 10% management are fixed; the remaining 80% is split across
  the work slices (weights in the tracker's `data/splits.json`).

## Using the agent skill library

While working inside a project repo (or any repo), any LLM agent can adopt one
of 279 specialist roles with a single call to the org's skill library:

```bash
curl -s "https://<deployment>/api/agent?q=<task keywords>"
# → adopt the returned "instructions" as your role for this task
```

Repo + local fallback: https://github.com/FYP-DESK/specilized-agents-skills-from-agency-agents

## FAQ

**Why is the app module different from the FYPCE engine pattern?**
FYPCE hosts many apps in a registry; a project repo hosts exactly one codebase
for one idea. Different purpose, different pattern.

**Can two members work in the same project repo?**
Yes — git branches plus the append-only modules keep authorship unambiguous.
Only append your own contribution records and your own idea/chat files.

**Where do exported .docx/.pptx files go?**
Beside their `.md` source in `deliverables/`. The `.md` is the source of truth;
re-export after edits, never edit the export.

**What if I want to redo a finished task?**
New files, new numbers — history is append-only. Reference the old task/record
in the new one.
