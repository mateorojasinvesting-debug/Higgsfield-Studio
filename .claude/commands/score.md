---
description: Score a finished video with Virality Predictor and interpret the result for Accesify decision-making.
argument-hint: <path-to-video-or-job-id>
---

# /score

Run Virality Predictor on a finished video and decide if it's launch-ready.

## Steps

1. Verify the input is a video (mp4/mov/webm). If a job_id is given, retrieve the underlying video URL first.

2. Run:
   ```bash
   higgsfield generate create brain_activity --video "$1" --wait
   ```

3. Parse the output for: Overall score, Peak hook, Sustain, Strongest region, Risk indicators, Report URL.

4. Interpret against Accesify thresholds:

| Score | Decision | Action |
|---|---|---|
| ≥ 75 | **Launch-ready** | Ship to first ad set |
| 60–74 | **Promising** | A/B test; refine hook in v2 |
| 45–59 | **Iterate** | Identify weakest region, regenerate with adjusted prompt |
| < 45 | **Reject** | Don't launch; redesign concept |

Additional flags:
- Peak hook second > 2s → hook lands too late, tighten first 1.5s
- Sustain < 70% → mid-video drops; check if motion/reveal pacing is too slow
- High Default Mode → mind-wandering risk; add motion or reveal earlier

5. Save the result to `/products/<slug>/generated/<creative>.score.json` with raw values + interpretation + decision.

6. Report to user as the standard Higgsfield shape:
```
Overall score: <N>/100
Peak hook: <X>% at <T>s
Sustain: <Y>%
Strongest region: <region>
Risk: <interpretation>

Decision: <Launch-ready / Promising / Iterate / Reject>
Recommended action: <specific next step>

Open report: <url>
```
