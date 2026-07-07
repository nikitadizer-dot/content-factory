# Production stack — Mockup Clothing Design Tool

_Last updated: 2026-07-07_

## Tools available

| Tool | Role | Cost | Output |
|---|---|---|---|
| Higgsfield Soul / Soul 2 | AI persona faces + outfits (persona generated once, reused) | ~$0.60/persona | reference stills |
| Nano Banana / GPT Image | first frames (copy composition of a proven video), carousel covers | cents | 9:16 stills |
| Kling 3 (via `tools/gen_video.py`, fal.ai) | simple UGC-style motion from a first frame | $ | 5–10s clips |
| Seedance 2 | video with details/speech | $$ | clips |
| **The app itself** | screen recordings of real UI = the demo B-roll | free | screencasts |
| Remotion (`tools/render_video.py`) | assemble hook (~3s) + demo cut, reusable pieces | free | final 9:16 MP4 |
| `tools/caption_composite.py` + `safe_zones.py` | captions on stills + safe-zone QA | free | slides |
| Postiz (`tools/schedule_post.py`) | queue posts (AI-disclosure on) | free | scheduled posts |

## Preset → format mapping

| Mined format | Producible path | Cost |
|---|---|---|
| Screen-record tool demo ("watch this app do X") | real screencast → Remotion hook+demo | free |
| Listicle "3 apps every brand founder needs" | screencasts + caption slides, or AI persona voiceover-B-roll | free–cents |
| Before/after sketch → on-model reveal | app export stills → Nano Banana first frame → Kling transition | ~$0.5 |
| POV emotional brand-founder | AI persona (Soul) + app-output B-roll + text overlay | ~$1 |
| "Is this model real?" guessing game | app on-model exports only, text overlays | free |
| AI-persona UGC talking demo | Soul persona → Seedance 2 (speech) + screencast insert | $2–3 |

## Rules
- Personas: default **varied-within-aesthetic** (new face each video, same aesthetic cluster) per SKILL.md playbook; lock identity only if we later run a serial character.
- Screencasts to record once and reuse (see `screencasts/README.md`): upload design → pick garment → generate → scene picker → export. Never re-record per video.
- Every video: brand name spoken/shown 2× ("Mockup — upload your design, get the photoshoot"), CTA = App Store only, no "link in bio".
