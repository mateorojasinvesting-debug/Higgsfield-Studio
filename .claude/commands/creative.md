---
description: Produce a premium Accessify creative for a given product and concept. Uses Higgsfield, respects brand rules, adds Accessify outro.
argument-hint: <product-slug> <concept-id> [--format 1:1|9:16|16:9|4:5]
---

# /creative

Generate one Accessify-grade creative end-to-end.

## Steps to execute

1. **Read brand context**:
   - `/CLAUDE.md`
   - `/brand/accessify.md`
   - `/brand/visual-identity.md`
   - `/brand/tone-of-voice.md`

2. **Read product spec**:
   - `/products/$1/specs.md`
   - `/products/$1/creative-brief.md` (find the concept matching `$2`)
   - List `/products/$1/hero-shots/` — confirm hero shots exist; if not, halt and tell the user.

3. **Pick the model** per `.agents/skills/higgsfield-generate/SKILL.md` rules. Default routing:
   - Static premium hero → `marketing_studio_image`
   - Image-to-video hypermotion → `seedance_2_0` with `--start-image`
   - UGC ad with avatar → `marketing_studio_video`

4. **Upload the hero shot** to Higgsfield if not already uploaded:
   - Check `/products/$1/hero-shots/README.md` for existing `upload_id`
   - If missing, call `higgsfield upload create <path> --image` and record the id

5. **Generate** with the prompt from the creative brief. Always include `--wait`.

6. **For videos**: append the Accessify outro frame.
   - Option A (preferred): include outro in the prompt directly (final 1.5s fade-to-black + logo stamp)
   - Option B: generate outro separately, concat with ffmpeg

7. **Score** the result with `brain_activity` (Virality Predictor) — only for videos. Save the score next to the creative.

8. **Save outputs**:
   - Save the result URL + local copy to `/products/$1/generated/$2-<timestamp>.{jpg,mp4}`
   - Create `/products/$1/generated/$2-<timestamp>.meta.json` with: model, prompt used, all params, upload_id, job_id, virality score, generated_at, decision (pending/approved/rejected)

9. **Report** to user with:
   - One line: model, format, duration if video, file path
   - Result URL
   - Virality score + interpretation (videos only)
   - Next suggested action (iterate, approve, generate variants)

## Branding rules to enforce

- ⛔ Never alter product features, ESR branding, slider color, LED, magnets, or AirPods Pro 3 model
- ✅ Always end videos with Accessify outro stamp
- ✅ Always use hero shot as `--start-image` for image-to-video — don't text-to-video products
- ✅ Always preserve mint-green slider color (specific instruction in prompt if needed)
- ✅ Tone of voice: Spanish neutro with Colombian warmth (per `tone-of-voice.md`)

## On failure

- `Session expired` → ask user to run `higgsfield auth login`
- Model rejects param → check `higgsfield model get <model> --json`, retry with corrected param
- Generated video drifted from product → re-run with stronger fidelity language ("preserve product exactly as shown in start-image, animate only camera and lighting"), reduce duration

## Example invocations

```
/creative pulse-flicklock-airpods-pro-3 concept-1
/creative pulse-flicklock-airpods-pro-3 concept-2 --format 9:16
/creative pulse-flicklock-airpods-pro-3 concept-3
```
