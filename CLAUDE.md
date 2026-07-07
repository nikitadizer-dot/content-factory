# content-factory

Organic-growth content pipeline: trendwatch → carousels → video → scheduling.

## Layout

- `.claude/skills/` — the three pipeline skills, run in this order:
  1. `trendwatch` — mine competitor accounts, decompose winners on 7 axes, ideate hooks. Self-bootstrapping: creates its state files (`brief.md`, `competitors/`, `trends/`, `ideas/`, `experiments.md`, `current-plan.md`) next to its SKILL.md on first run.
  2. `carousel-conveyor` — cheap carousel production for testing messages & character looks.
  3. `viral-content-factory` — video production, only for carousel-proven winners.
- `tools/` — standalone Python CLIs the skills call (uniform JSON envelope, chainable). See `tools/README.md` for the full map and canonical chains. Invoke as `python3 tools/<name>.py` from repo root.
- `prompts/` — standalone prompt docs (safe zones, roast review).

Note: skill files reference `../../tools/` (their path in the source repo `nestyme/awesome-prompts`); in this repo the tools live at repo root — `tools/`.

## Env keys (only for paid/generate steps)

`GEMINI_API_KEY` (gen_image) · `FAL_KEY` (gen_video) · `POSTIZ_API_KEY` + optional `POSTIZ_BASE_URL` (schedule_post). Keep them in `.env` (gitignored); template at `tools/.env.example`.
