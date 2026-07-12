# content-factory

Organic-growth content pipeline: trendwatch → carousels → video → scheduling.

## Layout

- `.claude/skills/` — the pipeline skills, run in this order:
  1. `trendwatch` — mine competitor accounts, decompose winners on 7 axes, ideate hooks. Self-bootstrapping: creates its state files (`brief.md`, `competitors/`, `trends/`, `ideas/`, `experiments.md`, `current-plan.md`) next to its SKILL.md on first run.
  2. `carousel-conveyor` — cheap carousel production for testing messages & character looks.
  3. `viral-content-factory` — video production, only for carousel-proven winners.
  4. `openmontage` — heavy video production (explainers, real-footage documentaries, clip factories, talking heads, dubbing, podcast repurposing, reference-video remixes) via the vendored [OpenMontage](https://github.com/calesthio/OpenMontage) framework, for formats the simpler skills don't cover. Thin wrapper: the real contract is `external/OpenMontage/AGENT_GUIDE.md`.
  5. `opencut` — **manual polish station** (human-in-the-loop, the only non-agent stage): launch the vendored [OpenCut](https://github.com/OpenCut-app/OpenCut) GUI editor for hand-tweaks on finished renders before QA/scheduling. The agent launches and hands off; it never edits there itself.
- `tools/` — standalone Python CLIs the skills call (uniform JSON envelope, chainable). See `tools/README.md` for the full map and canonical chains. Invoke as `python3 tools/<name>.py` from repo root.
- `external/OpenMontage/` — git **submodule** (AGPLv3, shallow; run `git submodule update --init external/OpenMontage` after a fresh clone). Self-contained agentic video studio: 13 YAML pipelines, ~52 tools, own skills/checkpoints/Backlot board. Operate it from its own root per its AGENT_GUIDE.md; don't copy its code into `tools/`.
- `external/OpenCut/` — git **submodule** (MIT, shallow): OpenCut-classic, the working privacy-first web video editor (Next.js + Bun; `docker compose up -d` → `:3100`). Archived upstream — pinned as-is, don't edit. Upstream rewrite promises headless/MCP; re-pin and make it agent-drivable when that ships.
- `prompts/` — standalone prompt docs (safe zones, roast review).

Note: skill files reference `../../tools/` (their path in the source repo `nestyme/awesome-prompts`); in this repo the tools live at repo root — `tools/`.

## Env keys (only for paid/generate steps)

`GEMINI_API_KEY` (gen_image) · `FAL_KEY` (gen_video) · `POSTIZ_API_KEY` + optional `POSTIZ_BASE_URL` (schedule_post). Keep them in `.env` (gitignored); template at `tools/.env.example`.

OpenMontage reads its **own** `.env` inside `external/OpenMontage/` (template there: `.env.example`). Our `FAL_KEY` works as-is and `GEMINI_API_KEY` is an accepted alias for its `GOOGLE_API_KEY` — copy them over; its free path (stock archives + Piper TTS + FFmpeg/Remotion) needs no keys.
