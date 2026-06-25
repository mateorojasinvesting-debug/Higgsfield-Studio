# Creative Brief — Pulse FlickLock™ for AirPods Pro 3

**Status:** Draft v1 — awaiting hero shots + Accesify logo + MeLi URL/price

## Goal of this first production run

Validate the Accesify creative pipeline end-to-end with one product, before scaling to the rest of the catalog. Output: 3 premium creatives (1 static hero, 2 hypermotion videos) across 3 platforms (MeLi listing, IG feed, TikTok Ads).

Success criteria:
- ✅ Visual quality at Apple/Dyson level (subjective: founder approves on first or second iteration)
- ✅ Virality Predictor score ≥ 60/100 on the video concepts
- ✅ Product fidelity 100% (no altered features or branding)
- ✅ Accesify outro stamp present on all videos
- ✅ Full creative + prompt + score + job ID committed to repo

## Concept 1 — "Engineered for Pro 3" (Premium static hero)

**Use:** Mercado Libre listing main image, IG feed post, web banner

**Format:** Multiple aspect ratios from same composition: 1:1 (IG feed, MeLi square), 4:5 (IG portrait), 16:9 (web banner)

**Higgsfield model:** `marketing_studio_image` (preferred, has brand kit) or `gpt_image_2` (fallback for highest fidelity)

**Visual direction:**

> Apple keynote product photography. Pulse FlickLock case (black) photographed from a low 3/4 angle, hovering 8cm above pitch-black surface with soft circular reflection beneath. Single large soft key light from top-right producing a rim light along the slider edge. The mint-green slider button catches a single hot specular highlight. Background: pure black gradient with subtle radial vignette toward edges. The green status LED is illuminated, casting a tiny glow on the polycarbonate next to it. Product is sharp tip-to-tip. No props, no hands, no text. Ultra-clean. Resolution 2K.

**Variants to generate:**
1. Black case, 1:1
2. White case, 1:1 (same composition, swapped product)
3. Black case, 4:5 (slightly different framing to fill vertical)
4. Both cases together, 16:9 (banner — side by side with lanyard draped between)

**Copy overlay (added in post or via prompt):**
- Title (top-left, weight 200, white): *"Diseñado solo para AirPods Pro 3."*
- Subtitle (bottom, weight 300, white 60% opacity): *"FlickLock™ · 20.000 aperturas · doble defensa"*
- Accesify watermark, bottom-right corner, 8% opacity white

---

## Concept 2 — "The FlickLock moment" (Hypermotion 9:16)

**Use:** TikTok Ads, IG Reels, IG Stories

**Format:** 9:16 vertical video, 6 seconds (Higgsfield supports 4-15s on Seedance 2.0; 6s hits the TikTok sweet spot)

**Higgsfield model:** `seedance_2_0` with `--start-image <hero-front-3-4-angle.jpg>` and `--duration 6` and `--aspect_ratio 9:16`

**Prompt:**

> Macro cinematic product film, Dyson-quality. Open on extreme close-up of the FlickLock slider mechanism, mint-green button centered. Slow finger enters from frame right and pushes the slider laterally. The green status LED beside it ignites with a soft halo bloom. Camera dollies in 15% as the lid begins to rise — magnetic auto-open, lid floats upward separating from the body in slow motion (60% speed). Camera pulls back smoothly to reveal the full case, now open, with AirPods Pro 3 inside softly catching light. Camera continues into a final 180° orbit revealing the case profile against pitch-black background. Final 1.5 seconds: fade to black, Accesify wordmark appears in white center, holds, fades out. No music yet (added in post). Color grade: warm-cool contrast, deep blacks, mint green accent preserved.

**Critical instructions for the model:**
- The product in `--start-image` MUST remain dimensionally and visually identical — only camera, light, and the slider/lid motion are animated
- The AirPods Pro 3 visible inside must match real AirPods Pro 3 silhouette
- The mint-green slider color is preserved exactly
- ESR wordmark on the case (visible on the side) is preserved, not erased

**Variants to test (after first one approved):**
- Same shot with white case version
- Alternate ending: case closes (instead of opens) for "secure" angle
- Add subtle copy text overlay: "20.000 aperturas. Una sola mano." (mid-video, white sans-serif fade-in/out)

**Audio plan (separate generation):**
- `mirelo_text_to_audio`: "Soft mechanical slider click, single magnet snap, premium product sound design"
- `sonilo_music`: "Cinematic minimal electronic track, 6 seconds, low ambient drone with one synth pulse on the slider click"
- Mix in post (ffmpeg in cloud env)

---

## Concept 3 — "The complete kit" (Studio trio reveal 9:16) — ADAPTED

> **Adaptation note:** Original Concept 3 was a lifestyle wrist shot, but no lifestyle hero photo is available. Reframed as a premium **studio trio reveal** that uses the available image-5 (3-piece composition: open case + closed case + lanyard) as `--start-image`. Stays true to Apple-style product showcase aesthetic.

**Use:** IG Reels organic + Reels Ads, TikTok organic + Ads, MeLi listing secondary image

**Format:** 9:16 vertical, 7 seconds

**Higgsfield model:** `seedance_2_0` with `--start-image <image-5-trio.jpg>` (the 3-piece studio composition) and `--duration 7` and `--aspect_ratio 9:16`

**Prompt:**

> Premium studio product reveal, Apple-style. Open on a clean white-to-soft-gray gradient background with three Pulse FlickLock pieces arranged in a row: open case (showing AirPods Pro 3 inside) on the left, closed case in the center, lanyard accessory on the right — exactly as in the start image. Hold static for 0.8 seconds. Camera begins a slow rightward dolly with subtle parallax depth — the closed case in the center grows in scale as it becomes the focus. At 2.5 seconds, the camera arrives at a hero close-up of the closed case, the mint-green slider catching a soft specular highlight, the green status LED visible. Hold for 1 second on the hero detail. Final 1.5 seconds: smooth fade to pitch black. The accesify wordmark fades in centered (lowercase, with the copper-orange terminal dot), holds for 700ms, fades out. Cinematic, deliberate pacing. Color: clean, neutral, true-to-product (preserve mint green slider exactly). No props beyond what's in the start frame.

**Critical instructions for the model:**
- The start-image is the literal frame 0 — do not invent new objects, do not change the three product pieces
- The closed case, open case, and lanyard must remain visually identical to the source — Seedance 2.0 should only animate camera, depth, and lighting
- The mint-green slider color is preserved
- ESR wordmark on the case (visible on the side) is preserved
- The motion is camera-driven, NOT object-driven — products don't levitate or rotate; only the camera moves

**Why this adaptation works:**
- Uses the asset we actually have
- Stays premium (Apple-style hero reveals are exactly this — locked-off studio with cinematic camera moves)
- Showcases the "complete kit" message visually without needing a person
- Cheaper/faster to iterate than a lifestyle shot with humans (no uncanny hand risk)
- Reusable copy hook: *"Todo lo que necesitas. En una sola caja."*

**Audio plan:**
- Mirelo: "soft mechanical accents — subtle slider click on the hero close-up, ambient designed silence, premium product film sound design"
- Sonilo: "minimal cinematic synth track, 7 seconds, slow build, calm, premium tech aesthetic"

**Future variant (when lifestyle shot becomes available):**
Once founder/team captures a real lifestyle photo of the case on a wrist with MacBook/coffee context, swap back to the original wrist-shot concept. Keep this studio version as the always-on baseline.

---

## Production order

1. **Hero shots arrive from user** → save to `hero-shots/` → upload to Higgsfield via `media_upload_widget` → capture upload IDs
2. **Logo arrives** → save to `/brand/assets/accesify-logo.png` → update `/brand/visual-identity.md` with real color palette
3. **MeLi URL or COP price** → update `specs.md` "Pricing" section + decide if positioning prompt needs adjustment
4. **Generate Concept 1** first (cheapest, fastest, validates look) → review → iterate if needed
5. **Generate Concept 2** (FlickLock moment) → review → score with Virality Predictor → iterate
6. **Generate Concept 3** (lifestyle) → review → score → iterate
7. **Approve final 3** → commit to `generated/` with full metadata (prompt, model, job ID, score, decision rationale)
8. **Deliver** to founder for first ad launch
9. **After ad runs**: capture performance → write `/campaigns/<name>/performance.md` → feed back into `/brand/insights.md`

## Risks and mitigations

| Risk | Mitigation |
|---|---|
| Seedance 2.0 alters product details despite `--start-image` | Run a low-cost test first (Seedance 1.5 or shorter duration); if drift, switch to more constrained motion |
| Generated lifestyle hands look AI-uncanny | Crop tight on case + wrist; avoid full hand visibility; or generate without person and add real UGC after |
| Mint-green slider color shifts in generated video | Explicitly reinforce in prompt; if drift, post-color-correct |
| Outro logo placement looks pasted-on | Use Option B (separate logo video + ffmpeg concat) instead of in-prompt outro |
| Virality Predictor score is low (<60) on all concepts | Iterate hooks; test alternative formats; consider that Colombian market may rate differently than predictor's training data |
