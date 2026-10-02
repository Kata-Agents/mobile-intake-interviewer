# Mobile Intake Interviewer

Runs the intake interview for a mobile feature request: asks one question at a time, scores each answer for whether it actually decided anything, and refuses to call the brief ready until the open decisions are closed.

It exists because a department that produces a question LIST leaves the interview itself to whoever is calling it — four questions or sixteen, with nothing to say which was right. This asks the next question, one at a time, carrying the reason it is being asked and what changes depending on the answer, and it scores the reply by what the reply commits to rather than by matching it against a list of phrases.

It then freezes the brief. A frozen brief carries the answers, the questions that were never answered, and every assumption standing in for one — so the work that follows is done against a boundary somebody agreed to rather than against gaps that were quietly filled in.

It holds nothing between calls and says so. The brief travels as an argument, because a handle to storage that does not exist is worse than no handle at all. It never talks to the end user: it hands the caller a question to ask, and a caller who answers on the user's behalf has broken the only rule that makes the interview worth running.

## What this is, precisely

A FindAgent **`mcp-tool`** agent. Each of its 8 tools is a
`prompt-template` action: the tool renders an instruction and hands it back to the
model that called it.

Two consequences worth being blunt about, because they decide whether this is useful to you:

- **It calls no model and reaches no network.** A tool call costs nothing and returns
  the same text for the same input, every time. There is no API key, no credential
  slot and no egress.
- **It observes nothing.** It has no access to your repository, your logs, your
  analytics or your devices. Every template is written so that supplying nothing
  produces an honest statement of what is missing rather than a confident-looking
  answer about data nobody provided. If you ask for a report and give it no findings,
  it will tell you the work has not been done — not invent it.

## Tools

| Tool | What it returns | Required input |
|---|---|---|
| `discover_intent` | Classify an incoming request, name the department route it takes, the first tool to call, and which members bear on it — so the order of work is read rather than guessed. | `request` |
| `list_capabilities` | List the department's members and what each one owes the others, filtered to the task in hand, so the roster is readable without calling every member to find out. | `task` |
| `open_brief` | Open an intake brief from a raw request: the brief skeleton, the decisions that must be closed before anyone builds, and the order to close them in. This is the department's front door — call it before any other member. | `request` |
| `ask_next_question` | Given the brief so far, produce the single next question to put to the requester, with the decision it closes and what changes either way — one question, not a list. | `brief_so_far` |
| `record_answer` | Record an answer into the brief and score whether it actually closed the decision, judging what the answer commits to rather than matching it against a list of phrases. | `brief_so_far`, `question`, `answer` |
| `assess_readiness` | Score how ready a brief is to hand to design and build, against a stated threshold, and name what is still blocking it — refusing to inflate the score to end the interview. | `brief_so_far` |
| `freeze_brief` | Freeze a brief for handoff: the full agreed text, every question that was never answered, and every assumption standing in for one, marked as assumptions. | `brief_so_far` |
| `check_phase_gate` | Check whether the artifact the next phase requires was actually supplied, and refuse the handoff by name when it was not, instead of letting the phase start on nothing. | `phase`, `artifacts_supplied` |

Optional inputs render as empty when omitted. Every template names that case and says
what it could not determine, so an empty slot degrades into a stated gap rather than a
dangling clause.

## Part of a department

This agent is one member of the **mobile-app-development** department, a
hub-orchestrator team of 10. The hub is `mobile-intake-interviewer`, which runs the
intake interview and freezes the brief the later phases work from; the other members are
reached through it or called directly as `<alias>__<tool>`.

| Agent | Role in the department |
|---|---|
| `mobile-intake-interviewer` | Runs the intake interview, scores the answers, freezes the brief (department hub) |
| `mobile-app-planner` | Shapes a request into a spec, defines scope, ranks risk, writes the run sheet |
| `mobile-bug-investigator` | Turns a fuzzy bug report into a scoped investigation and a planner brief |
| `mobile-ux-designer` | Flows, screen states, component contracts, exact copy, accessibility |
| `mobile-feature-developer` | File-level plan, implementation order, guardrails, verification steps |
| `mobile-qa-tester` | Coverage gaps, test cases, device matrix, bug reports |
| `mobile-code-reviewer` | Convention and correctness audit with honest severities |
| `mobile-security-checker` | Secrets, auth, injection, data exposure, dependencies |
| `mobile-knowledge-librarian` | Structured notes, bidirectional links, collection health |
| `mobile-campaign-video-creator` | Install-ad concept, character consistency, safe zone, spend discipline |

Each member is published independently and works on its own.

## Provenance

`source/intake-interview-origin.md` is the note recording why this member was added. The tool templates carry its
instructions, parameterised: anything the original hard-coded to one team's repositories,
file paths or people became an input you supply, and where a template would otherwise
depend on reading something it cannot reach, it asks for that material as an argument
instead.

## Licence and use

Published by Kata Team on FindAgent. Free to connect.
