---
name: opencut
description: Launch the vendored OpenCut editor (external/OpenCut) as the manual polish station of the pipeline. Trigger when the user wants to hand-edit a render before posting — trim by feel, nudge text/cut timing, quick captions/filters — or asks for "a video editor", "поправить руками", "подрезать в редакторе". This is a human-in-the-loop GUI stage: the agent launches and hands off, it cannot drive the editing itself. Downstream of carousel-conveyor / viral-content-factory / openmontage renders; upstream of safe_zones QA and schedule_post.
tools: Read, Write, Edit, Bash, Glob, Grep
---

# OpenCut — manual polish station

[OpenCut](https://github.com/OpenCut-app/OpenCut) (MIT, privacy-first CapCut
alternative — videos never leave the machine) lives as a git submodule at
[`external/OpenCut/`](../../../external/OpenCut). We pin **OpenCut-classic**,
the working web editor (Next.js + Bun + Rust/WASM GPU compositor); the upstream
main repo is a ground-up rewrite that has not shipped an editor yet.

## Place in our pipeline

```
carousel-conveyor / viral-content-factory / openmontage
        └─ render (mp4/png) ──> opencut (HUMAN hand-polish) ──> safe_zones QA ──> schedule_post
```

Everything before and after this stage is agent-driven; this stage is the
deliberate exception. Route here when automated output is 95% right and the
last 5% is taste: trim feel, a caption nudge, cut timing against a beat,
quick color/filter passes. Do NOT route here for batch work or anything the
pipeline tools already do deterministically (`caption_composite.py`,
`render_video.py`, OpenMontage edit stage).

## Operating rules

1. **Bootstrap if empty:** `git submodule update --init external/OpenCut`.
2. **The agent launches, the human edits.** OpenCut has no headless/CLI/API in
   the classic version — never try to script the GUI or "edit on behalf of"
   the user here. Prepare inputs, start the app, tell the user the URL and
   which file to import, then wait for their export.
3. **Handoff conventions.** Input: give the user the absolute path of the
   render to import (e.g. `external/OpenMontage/projects/<id>/renders/final.mp4`).
   Output: ask them to export and drop the file back into the same project's
   directory (e.g. `renders/final_polished.mp4`). Then continue the normal
   chain: `python3 tools/safe_zones.py` QA → `schedule_post.py` (dry-run first).
4. **Don't edit the submodule** — it's an archived upstream (read-only);
   we pin it as-is.

## Launching

Prerequisites: [Bun](https://bun.sh), Docker + Compose (for db/redis), from
`external/OpenCut/`:

```bash
# Full self-host (recommended, one command) → http://localhost:3100
docker compose up -d

# Dev mode (frontend-only work) → http://localhost:3000
cp apps/web/.env.example apps/web/.env.local
docker compose up -d db redis serverless-redis-http
bun install
bun dev:web
```

In a remote/headless session there is no browser for the user — say so and
offer the fallback: describe the wanted tweak and apply it with FFmpeg/our
tools instead, or defer the polish to their local machine.

## Upstream watch

The OpenCut rewrite ([OpenCut-app/OpenCut](https://github.com/OpenCut-app/OpenCut))
promises an Editor API, **headless mode for batch rendering, and an MCP server
for AI agents**. When that ships, this skill should be revisited: re-pin the
submodule to the rewrite and promote OpenCut from human-only polish station to
an agent-drivable editing backend (a real alternative to the FFmpeg path).
Until then, classic + human hands is the contract.
