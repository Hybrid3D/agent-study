# MAINTAINERS.md

This file is the source of truth for generating and maintaining `AGENTS.md`. If `AGENTS.md` needs to be rewritten or updated, start here.

English · [한국어](MAINTAINERS.ko.md)

Not loaded by any AI agent — for human maintainers only.

---

## Project Direction

**agent-study** is a project template that gives an AI agent a consistent workflow for structured, multi-session learning. The goal is continuity: the agent picks up exactly where the user left off, with full context of what they understood, what they were shaky on, and what questions they'd deferred.

The template is tool-agnostic and public. It should work with any AI agent that can read files.

---

## What AGENTS.md Must Do

AGENTS.md is the single instruction file for the AI agent. All agent behavior derives from it. It must cover:

1. **Session start** — detect language from first message; read saved state if it exists; run onboarding if it doesn't
2. **Onboarding** — iterative loop: ask questions → draft plan → revise until approved → save files → begin
3. **Learning** — one phase at a time, adaptive depth, grounding checks, never skip past confusion
4. **knowledge state tracking** — track per concept: understood / to revisit / misconceptions corrected / open questions
5. **Note-taking** — proactively create one `notes/*.md` file per new concept
6. **Progress tracking** — checklist in `notes/learning_plan.md`, phase confirmation before moving on
7. **Reference management** — `references/references.md` as master index, `references/raws/` for PDFs

---

## Core Design Decisions

**Language detection**: The language of the user's first message sets the language for the entire session — questions, explanations, and notes all follow it.

**Iterative onboarding**: Steps 1 (questions) and 2 (draft plan) form a loop, not a sequence. The agent must not begin teaching until the user approves the plan. First drafts are rarely right.

**One question at a time**: Never present a questionnaire. Ask, wait for answer, ask next.

**knowledge state tracking is the core differentiator**: "Understood", "to revisit", "misconceptions corrected", and "open questions" must be tracked separately. Collapsing them into a single progress field loses the tailoring that makes each session different from a cold start.

**Persisted State table at the top of AGENTS.md**: The table listing all files the agent reads and writes must come first — it's the at-a-glance overview of the whole system.

---

## File Structure

| File | Role |
|------|------|
| `MAINTAINERS.md` | This file. Generates AGENTS.md. Not auto-loaded. |
| `AGENTS.md` | All agent instructions. Generated from this file. |
| `CLAUDE.md` | Claude Code shim — `@AGENTS.md` import only |
| `.github/copilot-instructions.md` | Copilot shim — prose reference to AGENTS.md |
| `README.md` | English user docs |
| `README.ko.md` | Korean user docs |
| `notes/project_setup.md` | Goal, background, learning style, assumptions, current phase |
| `notes/learning_plan.md` | Phased checklist |
| `notes/state.md` | Live knowledge state tracking — read every session, updated immediately |
| `notes/phase_XX.md` | Permanent snapshot of state at each phase completion |

CLAUDE.md and copilot-instructions.md are thin entry points only. All actual instructions live in AGENTS.md.

---

## State Tracking Design

knowledge state tracking is the core differentiator. The four states must be kept separate — collapsing them loses the tailoring that makes each session resume as if the agent was there the whole time.

| State | Trigger | Effect |
|-------|---------|--------|
| **Concepts Understood** | User confirms grasp confidently | Skip in future explanations |
| **Concepts to Revisit** | Hesitation, vague answer, repeated question | Surface at next session/phase start |
| **Misconceptions Corrected** | User held a wrong model, now corrected | Prevents same wrong framing from reappearing |
| **Open Questions** | Tangential question that doesn't fit current flow | Brought back when relevant topic comes up |

**State is NOT stored in `notes/project_setup.md`.** It lives in `notes/state.md` (current) and `notes/phase_XX.md` (phase snapshots).

Each entry stores both a short summary and a detail version:

```markdown
### [Concept Name]
**Summary**: one-line
**Detail**: full context — how it was confirmed, what was unclear, etc.
```

`notes/state.md` is updated immediately during a session. At phase completion, `notes/phase_01.md` etc. is written as a structured learning record — not a copy of `state.md`. It captures four things: what the user already knew coming in, what was newly covered, the knowledge state at that moment, and how this phase connects to the next. Then `notes/state.md` continues for the next phase.

---

## What Not to Add

- Specific tool or brand names in AGENTS.md — it's tool-agnostic
- Duplicate instructions across entry point files
- LICENSE file — intentionally omitted
- CHANGELOG — git log is the changelog
