# Project-local Codex software factory

**[Read the architecture article →](https://brennanowyong.github.io/software_factory_cc/)**

This repository is a project-local software factory. It turns product intent into a tested GitHub
pull request through explicit requirements, a computed dependency graph, isolated builders,
independent validation, and evidence-backed delivery. The [project page](https://brennanowyong.github.io/software_factory_cc/)
explains the architecture, the lessons applied, and the request data flow with diagrams.

## See it

- **[Project showcase](https://brennanowyong.github.io/software_factory_cc/)** — why the
  factory exists, how each stage works, and the sources behind the design.
- **[Source repository](https://github.com/BrennanOwYong/software_factory_cc)** — scripts,
  control UI, project memory, and test contracts.

Clone this repository as a product project and start Codex in its root. The project `SessionStart`
hook initializes its control plane, asks once for the GitHub repository URL, and starts a browser UI
for that project.

This is not another vibe-coding wrapper. It automates the software development life cycle (SDLC)
itself — product interview, architecture, roadmap, isolated builders, independent audit, tested
merge — instead of asking one agent in one context window to hold the whole thing in its head. See
the [showcase](https://brennanowyong.github.io/software_factory_cc/) for the reasoning and
the evidence behind that distinction.

## Sources of truth

- `docs/product/PRD.md`: current product overview and outcomes.
- `docs/product/principles.md`: optional, user-confirmed patterns spanning several features.
- `docs/product/features/<id>.md`: the only requirement and acceptance source for a feature.
- `docs/architecture/overview.md`: whole-project implementation architecture.
- `docs/architecture/features/<id>.md`: how a feature uses that architecture.
- `docs/architecture/roadmap.json`: the only chronology and dependency source.
- `bin/project-test`: the planner-defined whole-project check used by GitHub pull requests.

Kanban records contain mutable runtime state and links; they never copy feature requirements. Run
`bin/roadmap-sync --sync-kanban` to validate the roadmap and deterministically compute its DAG.

## Lifecycle

```text
Product interview
  → whole architecture
  → feature implementation views
  → parseable roadmap
  → ready builder in isolated worktree
  → self-test + correlated logs + optional optimization AAR
  → programmatic rebase and post-rebase test
  → independent criterion/event audit
  → optional subjective user test in the browser UI
  → push the exact tested candidate
  → GitHub pull request and remote checks
  → GitHub merges into remote main
  → local main fast-forwards from remote main
  → DONE and next roadmap-ready dispatch
```

High-reasoning models own product and technical planning. Low-cost execution models own bounded
implementation and browser operation. Programs own DAG computation, worktrees, lifecycle
transitions, ports, test launch, pull-request creation, remote merge observation, and handoffs.

## Project UI

`bin/factory-ui start` starts the project-local browser control surface on a free loopback port and
records its URL in `.factory/runtime/ui.json`. It shows:

- roadmap and ticket progress;
- external setup requiring human action;
- independently verified tickets awaiting subjective UX review;
- a Test Me preparation page linked to product outcomes;
- health-checked, dynamically ported test-environment launch;
- criterion, browser-journey, event, and optional AAR evidence; and
- GitHub pull-request and remote-merge status.

Herdr can supervise persistent coordinator, builder, validator, logs, and UI panes. It is the agent
runtime and visibility layer; the browser UI and project files remain the product control plane.

## Core commands

- `bin/factory-bootstrap <github-url>` — bind one project instance to its repository.
- `bin/roadmap-sync --sync-kanban` — validate planning sources and materialize the DAG/runtime records.
- `bin/kanban-dispatch` — dispatch every ready feature within the concurrency limit.
- `bin/prepare-for-test <id>` — protect product docs, rebase, test, and start independent validation.
- `bin/ticket-integrate <id>` — push the exact tested candidate and open its GitHub pull request.
- `bin/github-sync` — fast-forward local main after GitHub merges and dispatch newly ready work.

Runtime state lives under `.factory/` and `kanban/`; current product and architecture memory under
`docs/` is Git-tracked.
