# Current plan — Mockup Clothing Design Tool

_Last updated: 2026-07-07 · Status: **cold start, account(s) not created yet**_

## Next action (in order)

1. **Create TikTok + IG accounts** (handle ideas: @mockup.app / @mockupclothing / @madewithmockup — check availability). Bio: "your designs, photoshoot-ready 📱" + App Store link.
2. **Record the screencast pack once** (see `screencasts/README.md`): upload design → pick garment → generate → scene picker → export. These are the reusable B-roll for ~every idea.
3. **Produce week-1 posts** from `ideas/2026-07-07.md`:
   - Post 1: **Idea 1** — sketch→photoshoot reveal (8–12s)
   - Post 2: **Idea 3** — "3 apps every clothing brand founder needs" (our app #1)
   - Post 3: **Idea 2** — "$500 photoshoot vs this app"
   - Reserve: Idea 4 ("is this model real?"), Idea 9 (scene poll)
4. Schedule via Postiz (`tools/schedule_post.py`, dry-run first), 3/week, cross-post Reels. Keep 1–2 manual posts/week from a real phone (shadow-ban hygiene).

## Pre-flight checklist (every post)

- [ ] Sound: pick **rising/peak** from TikTok Creative Center **day-of-posting** (env can't fetch it — do this from phone/browser)
- [ ] First frame passes the 1.5s test: is something already happening?
- [ ] Verbal hook in first caption sentence; hashtags at the END (#clothingbrand #clothingbrandtips #howtostartaclothingbrand + format-specific)
- [ ] Brand name 2×: voiceover/text + end-card "Mockup · upload your design · App Store"
- [ ] No "link in bio" CTA
- [ ] Safe zones QA: `python3 tools/safe_zones.py --image <frame>`
- [ ] AI-disclosure toggle on when scheduling

## Decision tree (per post, ~48h after publish)

- **≥1000 views (cold account)** → WINNER: log experiment, run `trendwatch: iterate on Idea-N` → 5–8 variants (lock message; vary persona look / setting / aesthetic)
- **500–1000** → re-test same idea with new thumb-stop + fresh sound (don't change message yet)
- **≤500 twice in a row for the same idea** → diagnose in playbook order (thumb-stop → sound → niche-match → caption → cold-start patience); kill after 2 failed re-tests
- **3 posts ≥1000** → algo knows the niche: shift mix toward longer winners, start variant ladders

## This week's references (watch before producing)

- Idea 1: https://www.instagram.com/gordonly/reel/DGVz_5hPqyj/ · https://www.tiktok.com/@virtualthreads/video/7267904058793413894
- Idea 3: https://www.tiktok.com/@sbhelpers/video/7295106849639124257
- Idea 2: https://www.tiktok.com/@itsbetterwithai/video/7507778124310924574 (caption structure)
- Live trend shelf to check day-of: https://www.tiktok.com/discover/kling-ai-clothing-mock-up

## Blocked / needs local run

- Per-video stats, comments, follower counts (TikTok blocked in cloud sandbox) → run locally: `python3 tools/tiktok_account.py --handle sbhelpers --count 30 --comments | python3 tools/account_stats.py --in -` then `engagement.py`
- Creative Center trending sounds (`tools/trending_sounds.py`) — same constraint

## Update log

- 2026-07-07 — plan created (first trendwatch run; discovery + WebSearch-mode collection + 10 ideas)
