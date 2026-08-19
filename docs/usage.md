# Usage

## You are never lost

There is no process to memorise, no checklist to keep open in another tab, and nothing to learn before you can start.

- **It works out where you are** by reading your project, rather than asking you or trusting a box somebody ticked last month. Come back after two weeks away and it already knows.
- **It tells you the next step** in plain language, when it is relevant: what you are one step from finishing, what you just finished, what you are about to do that you are not ready for. The rest of the time it stays quiet.
- **You can just ask.** Say what you want in your own words. You do not need to know that the thing you are asking for is called a phase.

If you have never run a project this way before, you are not expected to. It is meant to feel like somebody senior sitting next to you who has shipped this kind of thing many times.

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
