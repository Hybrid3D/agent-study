# agent-study

A project template for structured study and research sessions powered by AI agents.

English · [한국어](README.ko.md)

## What this is

This template gives an AI agent a consistent workflow for studying any topic deeply — whether it's a research paper, a technical field, or a specific concept. The agent works with you to design a learning plan, takes notes automatically, progresses phase by phase, and resumes exactly where you left off each session.

## What you get

If you just chat with an AI agent to learn something, the session ends and most of it is gone. Next time, the agent doesn't remember which concepts you grasped, which ones you were shaky on, what misconceptions you corrected along the way, or what tangential questions you parked for later. You re-explain your background, repeat questions, and lose the thread.

This template makes that continuity explicit. As you work, the agent persists the following as plain markdown files in the project:

- **Goal & background** — your end goal, prior knowledge, learning style
- **Plan & current phase** — the phased plan and where you are in it
- **Concepts grasped** — confidently understood, skipped in future explanations
- **Concepts to revisit** — partially understood, surfaced for review
- **Misconceptions corrected** — wrong models you held → correct understanding
- **Open questions** — tangential or deferred questions to come back to
- **References** — papers, articles, links, and PDFs you've collected

Each new session starts by reading those files, so the agent picks up *where you actually were*, not where the conversation reset to. Studying becomes a long-running, incremental process tailored to you, instead of a series of disconnected chats.

## Key features

- **Iterative onboarding** — the agent asks about your goal and background, drafts a phased learning plan, and revises it with you until you approve
- **Personalized state tracking** — separately tracks what you've grasped, what's still shaky, misconceptions you've corrected, and questions you parked for later, so future sessions resume tailored to you
- **Phase-by-phase progression** — work moves through one phase at a time, and the agent waits for your confirmation before moving on
- **Adaptive depth** — the agent adjusts how deep it goes based on your responses, never rushing past things you haven't understood
- **Automatic note-taking** — when a new concept comes up, the agent creates a note in `notes/` without being asked
- **Document / paper-driven learning** — provide a paper and the agent identifies prerequisites, explains them in order, and builds toward the paper step by step
- **Reference library** — papers, articles, and links are kept in `references/`, with raw PDFs in `references/raws/`
- **Language detection** — the language of your first message sets the language for the entire session, including all questions, explanations, and notes

## Persisted state

The agent keeps four files in `notes/` that carry your learning state across sessions:

| File | Contents | Updated |
|------|----------|---------|
| `project_setup.md` | Goal, background, learning style, current phase | When phase changes |
| `learning_plan.md` | Phased checklist | When items complete |
| `state.md` | knowledge state tracking: understood, to revisit, misconceptions corrected, open questions | Immediately during session |
| `phase_01.md`, `phase_02.md`, … | Permanent snapshot of state at each phase completion | Once, when phase ends |

`state.md` is the live record — updated as soon as your understanding of a concept changes. At the end of each phase, its contents are copied into a numbered snapshot file and preserved as a permanent record.

## Supported AI agents

| Tool | Instruction file |
|------|-----------------|
| Claude Code | `CLAUDE.md` (imports `AGENTS.md`) |
| GitHub Copilot | `.github/copilot-instructions.md` (references `AGENTS.md`) |

## Customizing the agent instructions

All agent behavior is defined in `AGENTS.md`. If you want to change how the agent conducts sessions — the onboarding flow, state tracking format, note-taking rules, or anything else — edit that file directly.

[`MAINTAINERS.md`](MAINTAINERS.md) at the project root documents the design decisions behind `AGENTS.md`. You don't need to touch it as a user. It exists for anyone who wants to understand why `AGENTS.md` is structured the way it is before making changes to it.

## How to use

1. Copy this directory to a new project folder.
2. Open the project folder with your AI agent.
3. Send any message to start. The agent walks you through onboarding, drafts a learning plan, and waits for your approval before beginning Phase 1.

On every subsequent session, the agent reads `notes/project_setup.md` and `notes/learning_plan.md`, and resumes from your current phase.

## Usage examples

### Claude Code

```bash
# Copy the template
cp -r agent-study my-study-project
cd my-study-project

# Start Claude Code
claude
```

On first launch, send any message to start. **The language of your first message sets the language for the entire session.** The agent asks questions to understand your goal and background:

```
> let's start

Let me set up this project. I'll ask you a few questions.

What is the ultimate thing you want to understand or achieve in this project?
> I want to understand how diffusion models work.

What do you already know related to this topic?
> CS background. I know the basics of neural networks and probability.

(... follow-up questions ...)
```

After collecting enough information, the agent drafts a phased plan and asks you to review it:

```
Does this plan look right?

Phase 1 — Foundations of probabilistic generative models
  - VAE recap
  - Markov chains, forward/reverse process

Phase 2 — DDPM core
  - Forward diffusion process
  - Reverse process and training objective
  - Loss derivation

Phase 3 — Practical details
  - Noise schedules
  - Sampling strategies

Assumptions: I'm assuming you're comfortable with KL divergence and Jensen's inequality.

> Phase 1 feels too light — skip it and start from Phase 2.
```

The agent revises the plan and asks again. Once you approve, it creates `notes/project_setup.md` and `notes/learning_plan.md`, and begins Phase 1.

On subsequent sessions, just run `claude` and start talking — the agent reads the saved files and picks up from your current phase.

## File structure after setup

```
notes/
  project_setup.md       # your goal, background, learning style, assumptions, current phase
  learning_plan.md    # phased checklist agreed upon during onboarding
  *.md                   # concept notes created automatically during sessions

references/
  references.md          # master index of all papers, articles, and links
  raws/                  # raw PDF files
  *.md                   # papers converted to markdown for closer study
```
