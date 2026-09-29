# pod-os-layer

The `os-layer` candy of the OpenCharly candy library, as a standalone repo (the
candy de-submodule cutover, kind-prefixed naming). It is a predictable
system-state fixture: it writes a known marker file and keeps a long-lived `bash`
process running so a harness can assert a stable pod state.

## What it provides

Writes `/etc/charly-os-marker` containing the marker
`charly-os-content-marker`, and registers a long-lived `bash` keepalive service
(`os-bash`, `restart: always`) so `pgrep -x bash` succeeds. Combined with a
Fedora base and `supervisord` it satisfies the `single-pod-system-state` recipe.

| Property | Value |
|---|---|
| Service | `os-bash` (`/usr/bin/bash -c 'while true; do sleep 60; done'`, `restart: always`, priority 50) |
| Marker file | `/etc/charly-os-marker` (mode `0644`, content `charly-os-content-marker`) |
| Packages | `bash`, `procps-ng`, `iproute`, `util-linux` (fedora) |
| Plan | `mkdir:` + `write:` steps; `file:` / `package:` / `service:` checks |

## How to use it

Compose the candy into a box:

```yaml
my-fixture:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/pod-os-layer:<tag>'
```

```bash
charly box build my-fixture
charly check box my-fixture
```

## Layout

- `charly.yml` — the `os-layer` candy entity (description, `distro`, `service`,
  `plan`). No `skill:` entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs, service declarations).
- `/charly-check:check` — the check/R10 framework (`charly check box`,
  `charly check run <bed>`).
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
