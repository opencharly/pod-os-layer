# AGENTS.md — pod-os-layer

Standalone candy repo for the `os-layer` candy — a predictable system-state
fixture that writes a known marker file and keeps a long-lived `bash` process
alive. The entire candy lives in `charly.yml` at the repo root. There is no
source tree.

Canonical files:

- `charly.yml` — the `os-layer:` candy entity (description, `distro`, `service`,
  `plan`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, service declarations). Load before editing,
  building, or troubleshooting this candy.
- `/charly-check:check` — the check/R10 framework and the
  `single-pod-system-state` fixture surface (`charly check box`,
  `charly check run <bed>`).
- `/charly-internals:git-workflow` — before any git/PR action.

The candy carries no `skill:` entity of its own; the authoring reference
`/charly-image:layer` covers the surface. The gap is routed to the named
skill-authoring batch
[opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291);
when that lands, add the owning skill here.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The candy's own `check:` steps assert the marker file, its content, the
  `/usr/bin/bash` binary, the `procps-ng` package, and the running `os-bash`
  service.

## Modify this repo

- Edit the `os-layer:` candy entity in `charly.yml`. The marker path/content and
  the `os-bash` keepalive service are the product.
- The marker content is asserted verbatim by the `check:` steps; a change to the
  content must move with them.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
