# AGENTS.md — example project file

Reference template for a real codebase. Copy the shape, not the contents. Every line here should be replaced with something true about your project.

The point of this file is not to describe the project. It is to tell an agent how to change the project. Descriptions only earn their place when they change a decision the agent would otherwise get wrong.

---

## What this is

`<product>` is a `<one sentence>`. A `<server>` wraps `<the thing>` and serves `<the clients>`. Think of it as `<the closest well-known comparison>`.

That comparison is here so you spend zero tool calls figuring out what we are.

## What we never compromise on

Changes that hurt any of these do not ship, even if they are otherwise correct.

1. **Open at the core.** People run forks. We work in the open. Assume anything you write is read by strangers.
2. **Performance.** We audit for regressions: oversized payloads, animations that peg the GPU, lists that re-render. Every change is weighed for cost.
3. **Remote ready.** The architecture assumes remote clients. New features must work over the network, not only on localhost.
4. **Every surface.** Web, desktop, and mobile are all first-class.

## A note from me

I like ambitious ideas, simple systems, and software that feels obvious.

Do not preserve complexity because it already exists. Do not introduce machinery because it looks impressive. Understand the real constraint, then fight for the smallest model that makes correct behavior unsurprising. Measure twice, cut once. YAGNI.

Treat the rest of this file as good defaults, not hard rules. A developer's prompt overrides anything here. If my instructions and their prompt disagree, say so and follow them.

## Glossary

Use these words back to me.

- **you** — the agent changing this codebase.
- **we** / **maintainers** — the people who own this repo.
- **user** — the person using the product.
- **agent** — a coding agent the user runs. Depending on context, that includes you.
- **client** — the web, desktop, and mobile UIs.
- **provider** — an external runtime we adapt to.
- **environment** — one running server plus the filesystem, credentials, and state it owns.
- **project** — an environment-local workspace record rooted at a directory.
- **thread** — the durable conversation and work history for a project.
- **turn** — one user-to-agent cycle, including follow-up work.

Define the words that mean something specific *here* and would otherwise be guessed at — especially the ones that collide with a general meaning, like `project`, `environment`, or `agent`.

A glossary is less about you understanding me and more about you describing things back to me the way I think about them.

## Three ways to hurt yourself here

Drawn from real failures in past sessions. Replace with yours.

1. **Killing the wrong process.** Most contributions here are made from an agent running on the contributor's own machine. A broad `pkill` can kill the session you are running inside. Save the PID when you start something and kill only that.
2. **Touching user data.** `~/.<product>/` holds real state. Read-only inspection is fine. Never write there to test something.
3. **Repo-wide checks for a one-file change.** Typecheck and test the package you touched. Full builds cost minutes and rarely find anything the focused run missed.

## Hit every surface

The most common defect here is a change that works on the path you tested and is missing everywhere else. Before calling frontend work done, walk this list and say which entries apply and which you handled.

- **Entry points** — settings page, command palette, keyboard shortcuts, context menu.
- **Clients** — web, desktop, mobile. Shared logic lives in the shared package.
- **Providers** — each adapter needs a decision, even if the decision is "not supported here".
- **Contracts** — anything crossing the wire is defined in the contracts package. Changes made in the app but not reflected there are a bug.
- **Reverse states** — if you added snooze, add unsnooze. If you added a mute, add the unmute and the state where both are visible.
- **Connection modes** — local, LAN, tunnel. Confirm each still works.
- **Docs** — user docs and maintainer docs are separate trees. Internal mechanics go in maintainer docs. Never in the user-facing ones.

## Dev servers

- Use `<the exact command>`. Not the obvious one. `<explain the trap>`.
- Point the server at a scratch home directory so it cannot damage the running instance the contributor is using.
- Save the PID. Kill only that PID when you are done.
- When sharing a dev server over the tunnel, include the pairing token in the URL you hand back. A URL without it is useless.

## Test data

Say where fixtures come from and what an agent is allowed to invent.

- Seed with `<the command>`. Do not hand-write records that the seeder already produces.
- Never point a test at real user state. `<the real data path>` is the developer's live data, in use while you work.
- When you need a new fixture, add it to the seeder rather than inlining it in one test, so the next person gets it too.
- Fake data should look real. `asdf` and `test1` hide bugs that a plausible name, a long name, and an empty string would each catch.

## Verifying

Typecheck, lint, then the focused test. Full builds last.

User-visible frontend changes get one integrated pass in a real client, on request. Do not spin up browser automation for a change that cannot be seen.

## How it works

A short runtime tour. Enough that an agent can predict where a change lands without reading everything first.

`<entry point>` starts `<the process>`, which owns `<the state>`. Requests arrive over `<transport>` and are handled by `<layer>`. `<The thing that surprises people>` — call it out explicitly, because an agent that guesses wrong here writes a plausible change in the wrong layer.

Keep this to a paragraph or two. It is a map, not the territory. If it grows past a screen, the architecture is the problem, not the doc.

## Where code lives

```
<dir>/   <one line each. Only the dirs an agent must know to avoid guessing.>
```

## Taste

- Complexity belongs at the adapter boundary. Orchestration stays pure. UI stays dumb.
- Inferred types over annotations. `any` is the enemy.
- Comments describe how a thing is used and move with the code they describe.
- Users drive this app all day and notice dropped frames, lying spinners, and stale labels.
- Security matters, but do not over-index on it for dev-mode and maintainer-only features that never leave the local network.
- If a real constraint fights the task in front of you, say so loudly and get a human sign-off before breaking it.

## Pull requests

Follow the `file-pr` skill. Titles say why. Descriptions open with the problem in the words of the person who reported it.

## Additional tips

The bin for real, specific things that do not deserve a section of their own. This is where most of the value accumulates over time — one line per lesson, added the third time you correct the same mistake.

- `<exact command>`, not `<the obvious one>`. `<why the obvious one fails>`.
- `<the file people always forget to update>` must change whenever `<X>` changes.
- `<the check that looks broken but is not>`.

Add to this list rather than rewriting the sections above. A rule earns promotion into its own section once it needs more than a line to state.
