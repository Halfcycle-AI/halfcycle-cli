# Halfcycle

**Your coding agents are the engineers. Halfcycle is the rest of the team.**

```bash
npx halfcycle
```

That is the whole install. One command, in the repo you want to work in.

---

## What it is

Good software gets built by several people doing different jobs. Somebody decides what is being built. Somebody decides how it fits together. Somebody cuts it into pieces small enough to finish. Somebody else checks the work, because whoever did it is the worst judge of it. And somebody writes the code.

AI gave everybody that last one. Halfcycle is the other four, running inside your own Claude Code.

| | |
|---|---|
| **A product owner** | Drafts what you are building, then argues with it. One question at a time, ordered so nothing gets asked before the thing it depends on, each with a recommended answer and the reasoning. Facts it can resolve, it resolves and shows you. Decisions only you can make, it actually asks, and it records the options you rejected so nobody reopens them in month three. |
| **An architect** | Works out how the pieces fit, draws the diagram, and pulls out the rules your system must never break so later work cites them instead of quietly reinventing them. The diagram and the prose have to agree. |
| **A planner** | Cuts the work into pieces that can actually ship, rather than a plan for the whole product that stops being true in week two. Anything it assumed rather than checked gets labelled as assumed, so it cannot travel downstream wearing the same voice as verified fact. |
| **A reviewer** | Reads it all back before any detailed design starts, decides whether it is ready, and stops you when it is not. Runs in a session that has never seen the work, and after every round of fixes the method sends it to another new one rather than back to the reviewer that asked for them. |
| **A delivery team** | Runs the build across parallel agents in separate worktrees, with a coordinator holding the graph, gates at every boundary, and checks firing on the real changes. |

You do not need to know any of the process. You say what you want to build and it takes it from there, telling you what happens next at each step.

## The problem it solves

AI coding agents are extremely good at giving you what you asked for. They are not good at noticing you asked for the wrong thing.

So the failure is not bad code. It is three weeks of confident, well-tested, well-structured work built on a decision nobody examined: a data model that cannot hold the second customer, an auth design that quietly assumed one tenant, a "quick" integration that becomes the architecture.

A method document cannot fix that, because a document cannot interrupt you. This can.

---

## Why this is not another framework

Credit where it is due, and it is due. GStack, Matt Pocock's skills, GitHub Spec Kit, BMAD, OpenSpec and claude-flow are genuinely good, and between them they taught this whole category that the bottleneck is discipline rather than model intelligence. We took two things from them deliberately and say so: the one-question-at-a-time interrogation shape, and an install that is one command and then stays out of your way.

If you are one person on a two week project, install one of those instead. They are excellent at exactly that, and you may not need any of this yet.

Writing the process down was never the hard part. What a folder you download structurally cannot have is these four things.

**1. Something other than you checks the work.** A document cannot audit itself, and the session that wrote the draft is the worst available reviewer of it. So Halfcycle's consistency check runs in a session that has never seen your draft, and the method makes that non-negotiable rather than advisory: after every round of fixes it runs again in another new session, never the one that asked for those fixes, because the reviewer that asked for a fix is the one who cannot see what the fix stranded. The guard checks are the harder edge — they run off your machine, on ours, on every real change, and one that fires either warns you or stops the work outright, depending on how serious the rule it broke is.

**2. Evidence that a piece of work actually finished.** Each phase closes with a Build Record: what was decided, what fired, what was accepted, assembled from your repo's own artefacts. That is a document an auditor, a board or a client can read. "The agent said it was done" is not.

**3. The same gates on every contributor.** The requirement lives in the repository, so everybody inherits identical checks with nothing to set up, and a new hire is compliant in one command. No framework is single-player by accident; they are single-player by construction, and several people changing the same codebase faster than they can synchronise is the one problem a single-session rulebook cannot see.

**4. A record of how builds break that keeps growing.** This is the part that compounds, and it is why a fork is a snapshot that starts depreciating the day you take it. See below.

---

## It fits the work, not the other way round

A process that treats every piece of work the same is a process tuned for nobody. A one-line copy change and a payments integration do not need the same ceremony. A team of one and a team of nine do not need the same gates. Handed one checklist for all of it, most people quietly stop using the checklist, and they are right to.

Halfcycle decides the shape per piece of work rather than asking you to.

- **The size of the work sets the depth.** A sizing diagnostic runs on every phase, and when the answers point the wrong way it splits the work rather than pushing something oversized through the same pipeline.
- **Each design artefact has to earn its place.** A component design, a sequence diagram, a user flow: included where they carry weight, explicitly skipped where they do not, and the decision recorded either way so nobody wonders later whether it was skipped or forgotten.
- **The gates scale with the team.** What one developer needs from a review and what four people changing the same code need are different things, and the process reflects that instead of averaging them into something that suits neither.
- **A new project and a live codebase get different routes.** Starting from nothing and arriving at something with ten years of history are not the same problem and are not treated as one.

The result is a process that is heavy exactly where the risk is and light everywhere else, which is the only version anybody keeps using past the first month.

---

## The part that compounds

Halfcycle carries a library of the ways real builds actually break. It started at six patterns, written up after one project's post-mortem. It has doubled since — twelve now — and the six that were added came from four later builds. Not one entry was invented: every pattern in it names the bugs it was distilled from.

The loop is a written procedure rather than a good intention, which is the difference between a library that grows and a folder that ages.

1. **A piece of work finishes.** List every problem that reached a person. Not the ones a check caught, because those are already handled. Only what got through.
2. **Ask one question of each.** Which check would have caught this, and at which step? Not who wrote it and not why they missed it. Those questions produce apologies. This one produces a check.
3. **Install the check where it belongs.** Into the planning step, into what the reviewer looks for, and into automation wherever it can run on its own. From then on it runs on every build, including yours.

The aim is not zero problems. A person walking the finished product is supposed to find things. The aim is zero problems reaching that person which an earlier step could have caught.

A framework you download knows exactly what its author knew on the day they wrote it, and it will never know more. **Your build does not start where the last one started. It starts where the last one finished.** The work you never have to redo is the only kind of speed we are willing to claim.

---

## What it catches

Most process claims cannot be checked, because nobody writes down the bugs that never happened. Here is one that can.

A real feature on a live product: let somebody restore an older version of a document they had edited. Small by any measure, two screens of work and a button. It was reviewed five times before anybody was allowed to start building. Blockers found each round: **6, then 2, then 1, then none, then none.** Nine in all.

Here are eight of the findings, and the first four would have reached real users:

- **Restore could overwrite a file that is meant to be locked.** The rule was enforced in the normal save path and nobody copied it into the new restore path. Every test of the new code passes; somebody has to notice a rule that is not there.
- **The undo feature could destroy the backup it exists to protect.** Save a backup, update the search index, commit, all as one step. If the index step failed, the backup was discarded and the new content had already been written.
- **The button would never have appeared for anybody.** Keyed on a piece of data the system only attaches to the exact records the feature refuses to act on. The code is correct in isolation. It ships to nobody.
- **Restoring a document would have silently deleted its labels.** A shared helper wipes all labels and puts back whatever it is handed, and the design handed it an incomplete list. Nothing fails. The loss turns up weeks later, somewhere else.
- **The plan described a bug that did not exist,** from a search of the codebase that was accurate and drew the wrong conclusion.
- **The design could not be built where it was going.** The place the plan chose to hang the new screen is a sibling of the thing it needed to replace, and cannot replace it. Correct design, wrong address.
- **The final checklist pointed at addresses nobody had ever written down,** and survived four rounds of review doing it. It needed no knowledge of the code to catch, and it consumed an expensive reader anyway.
- **The summary everybody reads first still described a plan three rounds of review had thrown out.**

Not one of them is a coding mistake. Every one sits in a join: between two pieces of code, between two people's work, or between the plan and what was already there. An agent writing the file in front of it cannot see any of them, and neither can a reviewer reading the change on its own.

**Honest about what that took:** five rounds on a feature that small is heavy. The first round alone returned six blockers, and rounds two to four found five more defects — nearly all of those five introduced by the previous round's own fixes. The method was amended because of it, in four places: what a scoping document has to mark as unverified before it travels, what a design has to check rather than assume, a cheap mechanical pass that runs before any reviewer is spent on the things a reviewer is wasted on, and when the summary everybody reads first is allowed to be written. We do not defend the five.

---

## You are never lost

There is no process to memorise, no checklist to keep open in another tab, and nothing to learn before you can start.

- **It works out where you are** by reading your project, rather than asking you or trusting a box somebody ticked last month. Come back after two weeks away and it already knows.
- **It tells you the next step** in plain language, when it is relevant: what you are one step from finishing, what you just finished, what you are about to do that you are not ready for. The rest of the time it stays quiet.
- **You can just ask.** Say what you want in your own words. You do not need to know that the thing you are asking for is called a phase.

If you have never run a project this way before, you are not expected to. It is meant to feel like somebody senior sitting next to you who has shipped this kind of thing many times.

---

## How it works

Five layers, one direction, a loop at the bottom.

| | | |
|---|---|---|
| **L0** | Set up | The context your agents read on every session. Refuses to record a decision with no reason attached. |
| **L1** | Decide | The product spec, drafted then interrogated, with nothing left hand-waved. |
| **L2** | Write it down | Architecture, a diagram, and the rules the system must never break, named so later work cites them. Diagram and prose must agree. |
| **L3** | Slice it | Phases that actually close. Oversized work gets split. Assumptions get labelled as assumptions. |
| **gate** | | **The readiness audit.** Eight checks over everything decided so far, each one a command that runs against your own repository rather than an opinion. Too many red is a hard stop. |
| **L4** | Design it | Feature specs. Every claim about existing code anchored to a real file and line or not asserted. A fresh reviewer that has never seen the spec, never the same one twice. |
| **L5** | Build it | Ordered work, then coordinator and worker agents in parallel. Checks on the real changes, an anti-tautology pass over the tests, reachability, and a cold walk of the deployed product by somebody who did not build it. |

Underneath all five: one authoritative copy of every document, rules referenced by name rather than restated, a deliberate limit on what loads into every session, and gates in both directions at every boundary.

**The method arrives one step at a time.** Nothing dumps fifty pages into your context. The step you are on is fetched when you need it, sized for the moment you are in.

---

## Your code stays yours

This matters enough to be specific about.

**Halfcycle never holds or proxies your Claude credential.** Inference runs on your own Claude Code, on your own subscription or seats. We are not a hosted runtime reselling you inference.

**What the checks receive is the shape of a change, not its contents.** File paths, structural shapes, environment variable names and how they are used, the tables a write touches, route signatures, outbound host names. There is no file body and no diff in the request.

**Source excerpts are off, at both ends, for every engagement today.** The wire format does have a field for them, so the honest version of this is a conditional statement rather than a flat one, and it rests on three things rather than on our good intentions: your runner does not attach an excerpt unless you turn a flag on, and that flag is off by default; anything that does arrive for an engagement that has not opted in is stripped on our side before the checks or any model see it; and there is no way to opt in today, because nothing in the product can set that flag. If that ever changes, our privacy policy changes with it, and it is the policy — not this paragraph — that states the guarantee.

**A promise, for the day a check does need to read something you wrote.** No route in the product does this today — there is nothing to submit anything to, and we would rather say so than let a paragraph imply otherwise. When one exists, what you send it will be read and not kept: the finding and its evidence persist, the source does not, and training or benchmarking on it is foreclosed. It is written here now, labelled as a commitment rather than a feature, so the promise is public before the mechanism is.

**Nothing method-shaped is left behind.** The runbooks, templates and question sets reach the session that asks for them, scoped to the step you are on, and are never at rest in your repository.

## What you keep

Everything the method produces is yours, under your own `docs/`, in your git history, in plain markdown: the product spec, the architecture, the phase plans, the feature specs, the ordered work and the Build Records. It reads as documentation your team wrote, because it is.

Nothing you already had is changed. Existing settings are preserved, and a name collision is reported rather than resolved on your behalf.

## Requirements

- Claude Code **2.1.118+**
- Node 20+
- A git repository

## Getting started

```bash
mkdir my-project && cd my-project
git init
npx halfcycle
```

Then open the folder in Claude Code and run `/halfcycle-setup`, and answer the questions it asks about your project.

**Accept the workspace trust prompt when Claude Code shows it.** The install writes a credential helper and a set of hooks that are shell commands, so Claude Code asks before it runs them. It is expected, and it appears once.

You will get a board link on install. That is where the work becomes visible to anyone who is not you.

---

[halfcycle.ai](https://halfcycle.ai) · MIT licensed

Halfcycle is built with the Halfcycle method. Every check in it fired on us first.

---

## Releases

Every entry on the [Releases page](../../releases) matches a version published to npm — releases are
created automatically when a publish completes, never by hand and never for a dry run. Versions
published before this repository existed, or before that automation was in place, may not appear here.

---

## Support and issues

Found a bug, or something not working the way the docs describe? [Open an issue](../../issues) — it is
watched, and it is what "help and support" means for this project.

---

## Source availability

The published package is MIT licensed. Its implementation is not developed in the open, and this
repository is not a source mirror — it holds releases, install and usage documentation, and issue
tracking for the published npm package. No pull requests against the implementation are accepted. If
that changes, this page changes with it.

---

Install docs: [docs/install.md](docs/install.md) · Usage docs: [docs/usage.md](docs/usage.md) · License: MIT ([LICENSE](LICENSE))
