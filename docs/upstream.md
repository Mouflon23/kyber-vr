# Upstream scope (locked for kyber-vr)

This file records **which GitLab namespace**, **which repository**, and **which commit** we treat as the upstream baseline. Update it when you intentionally move pins.

## Primary GitLab group

**Canonical dependency path:** [`https://gitlab.com/groups/kyber`](https://gitlab.com/groups/kyber) (`gitlab.com/kyber`, subgroups `core`, `apps`, `deps`, `ops`).

**Rationale:** The public GitLab API returns many **public** repositories under `kyber` (desktop app, web client, core crates, forks). Anonymous `git ls-remote` succeeds for the repos we care about.

## `gitlab.com/kyber.stream` (secondary for now)

The group exists (`full_path`: `kyber.stream`), but **anonymous** `GET /api/v4/groups/<id>/projects?include_subgroups=true` returns an **empty** list. The top-level [`kyber/kyber`](https://gitlab.com/kyber/kyber) README still links to wiki/installer paths under `kyber.stream`; treat those as **documentation or packaging** references until you have a token that can see projects there.

**Implication for this repo:** implement and pin code against **`gitlab.com/kyber/*`**, not `kyber.stream`, until you confirm access or a mirror.

## Smallest runnable sample (video + control)

**Repository:** [`kyber/apps/kyber-desktop`](https://gitlab.com/kyber/apps/kyber-desktop)

**Clone URL:** `https://gitlab.com/kyber/apps/kyber-desktop.git`

**Default branch:** `main`

**Pinned commit (resolved 2026-05-14):** `4578ad30cc081c3f760946dafea8c35a1f37fb6c` (current `HEAD` on `main`; matches annotated tag **`0.26.0`**)

**Why this repo:** Upstream README describes it as the remote **host + client** using **FFmpeg** and **libVLC**, with scripts to run **`run_controller.sh`** (host) and **`run_client.sh <host_ip>`** (client)—i.e. desktop **video path + input path**, not an isolated codec demo.

**Build entrypoints (upstream):**

- Linux: `./build-linux.sh` → output under `rootfs-x86_64-linux-gnu/`, then `run_controller.sh` / `run_client.sh`
- Windows: `./build-win32.sh` (optional `-p` for flattened package)
- macOS: `./build-macos.sh`

**Local workspace (this repo):** `upstream/kyber-desktop` — clone at the pin + `git submodule update --init --recursive` completed **2026-05-14** (anonymous HTTPS). A **full compile** was **not** run here: the Windows dev machine had **no `rustc`/`cargo` in PATH**, and upstream’s **Windows artifact** path is **`./build-win32.sh` run from a Unix shell** with the **Debian/Ubuntu + `mingw-w64`** stack listed in the kyber-desktop `README` (cross-compile layout), not “double-click in PowerShell.” For a native Linux binary, use **Debian Bookworm / Ubuntu 24.04** (per README), install system deps + **Rust 1.89.0** + `cargo-c@0.10.15`, then `./build-linux.sh`. Re-pin `Pinned commit` only if your successful build differs.

## Meta / “where to start” documentation repo

**Repository:** [`kyber/kyber`](https://gitlab.com/kyber/kyber)

**Clone URL:** `https://gitlab.com/kyber/kyber.git`

**Default branch:** `main`

**Pinned commit (resolved 2026-05-14):** `c1c05062c13015e8082430158c82852633423bf1` (current `HEAD` on `main`)

**Role:** Architecture overview and index of **`core`** vs **`apps`** components—not necessarily the binary you run for desktop streaming.

## Core libraries (for later SDK-style integration)

Public **`core`** projects under `gitlab.com/kyber` (all default branch `main` per API):

| Project        | Path |
|----------------|------|
| `kyctl`        | `kyber/core/kyctl` |
| `kymux`        | `kyber/core/kymux` |
| `kynput`       | `kyber/core/kynput` |
| `kymedia`      | `kyber/core/kymedia` |
| `kyutil`       | `kyber/core/kyutil` |
| `kysdk`        | `kyber/core/kysdk` |

Use these when you outgrow “run upstream scripts” and need explicit crate/ABI boundaries.

## Related apps (not the minimal desktop path)

- [`kyber/apps/kyber-web`](https://gitlab.com/kyber/apps/kyber-web) — browser client
- [`kyber/apps/kyber-installer`](https://gitlab.com/kyber/apps/kyber-installer) — installer packaging

## License reminder (from upstream README)

Kyber materials describe **AGPLv3-or-later** with a **commercial** license option. Validate distribution plans against upstream `LICENSE` before shipping binaries.
