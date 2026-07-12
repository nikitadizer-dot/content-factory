---
name: openmontage
description: Full agentic video production via the vendored OpenMontage framework (external/OpenMontage). Trigger when a carousel-proven winner needs to graduate beyond the simple hook+demo Remotion render — animated explainers, real-footage documentary montages, talking-head edits, clip factories from long-form content, avatar spokespersons, localization/dubbing, podcast repurposing — or when the user pastes a reference video ("make me something like this"). Complements viral-content-factory: that skill does persona carousels and mannequin videos; this one does everything else video.
tools: Read, Write, Edit, Bash, Glob, Grep, Agent
---

# OpenMontage — heavy video production stage

[OpenMontage](https://github.com/calesthio/OpenMontage) (AGPLv3) lives as a git
submodule at [`external/OpenMontage/`](../../../external/OpenMontage). It is an
instruction-driven video studio: the agent reads its pipeline manifests
(`pipeline_defs/*.yaml`) and stage-director skills (`skills/`) and drives ~52
Python tools (video/image/TTS/music providers, FFmpeg, Remotion, HyperFrames)
with approval gates, budget caps, and a decision audit log.

## Place in our pipeline

```
trendwatch ──> carousel-conveyor ──> viral-content-factory ──> openmontage
 (what to     (cheap message         (persona carousels,       (full video prod:
  post)        testing)               mannequin videos)         explainers, docs,
                                                                clips, dubs, …)
```

Use it when the format a winner calls for is **not** covered by our own
`tools/render_video.py` (hook + app demo) or the mannequin-video flow:

| Need | OpenMontage pipeline |
|---|---|
| Animated/narrated explainer from a topic | `animated-explainer`, `animation` |
| Real-footage montage (Pexels/Archive.org/NASA/Wikimedia — no paid video API) | `documentary-montage` |
| Cut long-form (stream, webinar, interview) into shorts | `clip-factory` |
| Edit raw footage of a person speaking (transcribe, cut, subtitle, mix) | `talking-head` |
| Digital presenter / spokesperson video | `avatar-spokesperson` |
| Reusable cartoon character animation | `character-animation` |
| App/screen walkthrough video | `screen-demo` |
| Translate + dub + subtitle an existing video | `localization-dub` |
| Podcast audio → audiograms/quote clips | `podcast-repurpose` |
| Trailer / brand film / mood-led montage | `cinematic`, `hybrid` |
| "Make me a video like this <URL>" | reference-video analysis entry point |

## Operating rules (non-negotiable)

OpenMontage brings its own agent contract — **defer to it, not to this file**:

1. **Bootstrap if empty.** The submodule may not be checked out on a fresh
   clone: `git submodule update --init external/OpenMontage` (`.gitmodules`
   sets `shallow = true`).
2. **Read `external/OpenMontage/AGENT_GUIDE.md` in full before doing anything
   inside it.** It defines Rule Zero (every production request goes through a
   pipeline manifest — never ad-hoc scripts or direct API calls), the
   reference-video entry point, the decision-communication contract
   (announce provider/model/cost before paid calls, present both Remotion and
   HyperFrames runtimes, append-only decision log), and blocker escalation.
3. **Work from its root.** Run its tools with `external/OpenMontage` as cwd —
   its registry, checkpoints, and Backlot board assume that. Our repo-root
   `tools/` contract does not apply inside the submodule and vice versa.
4. **Keep boundaries clean.** Don't copy OpenMontage code into `tools/`
   (AGPLv3 vs this repo) and don't edit the submodule — pin a newer upstream
   commit instead. Final MP4s/exports it produces get pulled out into the
   relevant character/content directory, then scheduled with our
   `tools/schedule_post.py` (dry-run first, as always).

## Setup

One-time, inside `external/OpenMontage/` (needs Python 3.10+, FFmpeg, Node 18+):

```bash
make setup            # or: python3 -m venv .venv && . .venv/bin/activate \
                      #     && pip install -r requirements.txt \
                      #     && cd remotion-composer && npm install
```

**Keys:** it reads its own `.env` (template: `external/OpenMontage/.env.example`).
Our existing keys map directly — `FAL_KEY` is used as-is (FLUX/Veo/Kling/
MiniMax) and `GEMINI_API_KEY` is an accepted alias for `GOOGLE_API_KEY`
(Imagen, Google TTS). Copy them over from repo-root `.env`; add others
(ELEVENLABS_API_KEY, PEXELS_API_KEY, …) only when a chosen pipeline needs them.

**Free path:** documentary-montage, Piper TTS narration, stock archives, and
Remotion/HyperFrames/FFmpeg composition all work with **no keys at all** — good
default for testing a format before paying for generation.

**Monitoring:** `python -m backlot open [<project-id>]` — live production board.
