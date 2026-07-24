# Baseline — Adoption Plan

**Getting 4 designers and 6 developers who like their own habits to actually use this.**

A design system doesn't fail on the tokens. It fails on adoption. Ten people who each have a working way of shipping will treat a mandate as friction and route around it — copying an old project's buttons is faster than learning a library, so that's what they'll do. The plan below is built on one premise: **make the shared path the fastest path, prove it on real work before mandating anything, and let the team shape it so it's theirs, not something imposed on them.**

---

## The core principle

You cannot win this with a policy. You win it three ways, in order of power:

1. **Make it easier than the old habit.** Adoption is a cost/benefit calculation each person makes silently. If pulling a Baseline button is slower than copy-pasting, they copy-paste, and no memo changes that. Every decision below reduces the cost of doing it right.
2. **Prove it on real client work, not a demo.** A system justified by principles loses to a system that visibly shipped a real project faster. Land one win first.
3. **Give them authorship.** People defend what they helped build and resist what's done to them. The team co-writes the rules; you don't hand them down.

---

## Phase 0 — Co-design the foundations (2 weeks, before any mandate)

Don't launch a finished system. Launch a *draft* and run two working sessions where the team argues about it.

- Pull the actual buttons, inputs and colours from the last 3–4 shipped projects and put them side by side. The inconsistency sells itself — nobody has to be told reinvention is a problem when they see six different buttons that all do the same thing.
- Let the team fight over the type scale and the primary colour in the room. Change at least one real thing based on their input, and say so out loud. That single visible change converts the loudest skeptic from "this was imposed" to "we decided this."
- Name two respected people — one designer, one developer — as **maintainers**, not a committee. Peers adopt what peers own; nobody adopts what a manager owns.

Output: a v1 the team recognises as partly theirs.

---

## Phase 1 — Prove it on one real project (one project cycle)

Pick **one live client project** — small, low-drama, with a willing team — as the pilot. Not a sandbox; real work with a real deadline.

- The maintainers pair with the pilot team so friction gets fixed in hours, not sprints. Every "the button doesn't do X" becomes a same-day library fix, not a reason to abandon it.
- Measure two things and nothing else: **time from design to coded component**, and **number of one-off components created**. These are the numbers that will convince the other nine people.
- At the end, the pilot team — not the maintainers — presents what happened to everyone else. "We shipped the settings screen in a day because the components already existed" from a peer beats any slide from leadership.

The goal of Phase 1 is a story, told by a colleague, that the shared path was faster.

---

## Phase 2 — Make it the default, remove the alternatives (ongoing)

Now reduce the cost of the right path until it's lower than any other:

- **Starter files.** New Figma projects open from a Baseline template with the library already linked. New repos scaffold with tokens wired in. The right path is what you get by doing nothing.
- **Kill the copy-paste source.** Archive or clearly mark the old project files people were cloning buttons from. If the old habit has nowhere to copy from, the library becomes the shortest route by default.
- **Lint it, gently.** Add a check that flags raw hex values, off-scale spacing and primitive references in code — as a warning first, not a blocking error. Automation catches drift without a human having to police it, and a warning teaches instead of punishing.
- **One question, one place.** A single channel where the maintainers answer within the day. Slow answers are the fastest way to send someone back to their old habit.

---

## Phase 3 — Governance so it survives contact with deadlines (ongoing)

A system that can't change under deadline pressure gets abandoned under deadline pressure.

- **A contribution path.** When a project needs a component Baseline doesn't have, the answer is never "don't" — it's "build it in your project, and if a second project needs it, the maintainers promote it into the library." This turns the people bending the rules into the people extending the system.
- **A fast lane for exceptions.** Client brand demands a shape the system doesn't have? That's a token override at the project level (swap `action-primary`, override the display face), not a fork. Document that this is allowed and how — an escape hatch you designed is better than one they improvise.
- **A standing 30-minute review** every two weeks: what got proposed, what got promoted, what got deprecated. Small and regular beats a big annual overhaul nobody attends.

---

## Handling the specific objections you will hear

- *"This slows me down."* True in week one, false by week three — and the pilot numbers prove it. Frame Baseline as removing the boring decisions (which grey, which radius) so their time goes to the client-specific work that actually needs a designer.
- *"My client needs something different."* That's the override path, not a reason to opt out. The system flexes at the token layer precisely so brand differences don't require abandoning it.
- *"I already have a way that works."* Their way works for them and breaks for the next person who inherits the file. Baseline is the version that survives hand-off between 10 people — that's the whole point of a shared system.

---

## How you'll know it worked

Adoption is a behaviour, so measure behaviour, not opinions:

- **New components created per project trends toward zero** — the library covers the common cases.
- **Design-to-code time drops** and stays down across projects, not just the pilot.
- **Contributions flow back in** — the surest sign of ownership is people extending the system instead of routing around it.
- **A new hire ships a Baseline screen in their first week** without a maintainer walking them through it. If onboarding is self-serve, the system has become the team's default rather than a project.

The finish line isn't "everyone was told to use it." It's the day someone reaches for Baseline without thinking about it, because it's simply how work gets done here.
