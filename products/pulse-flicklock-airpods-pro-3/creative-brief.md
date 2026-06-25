# Creative Brief — Pulse FlickLock™ for AirPods Pro 3

**Status:** Draft v1 — awaiting hero shots + Accessify logo + MeLi URL/price

## Goal of this first production run

Validate the Accessify creative pipeline end-to-end with one product, before scaling to the rest of the catalog. Output: 3 premium creatives (1 static hero, 2 hypermotion videos) across 3 platforms (MeLi listing, IG feed, TikTok Ads).

Success criteria:
- ✅ Visual quality at Apple/Dyson level (subjective: founder approves on first or second iteration)
- ✅ Virality Predictor score ≥ 60/100 on the video concepts
- ✅ Product fidelity 100% (no altered features or branding)
- ✅ Accessify outro stamp present on all videos
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
- Accessify watermark, bottom-right corner, 8% opacity white

---

## Concept 2 — "The FlickLock moment" (Hypermotion 9:16)

**Use:** TikTok Ads, IG Reels, IG Stories

**Format:** 9:16 vertical video, 6 seconds (Higgsfield supports 4-15s on Seedance 2.0; 6s hits the TikTok sweet spot)

**Higgsfield model:** `seedance_2_0` with `--start-image <hero-front-3-4-angle.jpg>` and `--duration 6` and `--aspect_ratio 9:16`

**Prompt:**

> Macro cinematic product film, Dyson-quality. Open on extreme close-up of the FlickLock slider mechanism, mint-green button centered. Slow finger enters from frame right and pushes the slider laterally. The green status LED beside it ignites with a soft halo bloom. Camera dollies in 15% as the lid begins to rise — magnetic auto-open, lid floats upward separating from the body in slow motion (60% speed). Camera pulls back smoothly to reveal the full case, now open, with AirPods Pro 3 inside softly catching light. Camera continues into a final 180° orbit revealing the case profile against pitch-black background. Final 1.5 seconds: fade to black, Accessify wordmark appears in white center, holds, fades out. No music yet (added in post). Color grade: warm-cool contrast, deep blacks, mint green accent preserved.

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

## Concept 3 — "Carry it like a charm" (Lifestyle hypermotion 9:16)

**Use:** IG Reels organic + Reels Ads, TikTok organic + Ads

**Format:** 9:16 vertical, 7 seconds

**Higgsfield model:** `seedance_2_0` with `--start-image <hero-with-lanyard-lifestyle.jpg>` and `--duration 7` and `--aspect_ratio 9:16`

**Prompt:**

> Lifestyle product cinematography, soft natural light. Close shot of a person's wrist (no face), walking through a sunlit coffee shop or co-working space, the Pulse FlickLock case hanging from a black braided lanyard wrapped around their wrist. The case sways gently with the walking rhythm. Light bokeh in the background — warm tones, defocused MacBook, coffee cup, plants. Camera tracks alongside at wrist-height, smooth gimbal-like motion. Mid-shot, the wearer's other hand enters frame and lifts the case toward camera, lid opens magnetically revealing AirPods Pro 3 inside. Take one AirPod out. Final beat: close on the case in hand, slider with green LED visible. Cut to black, Accessify outro 1.5s. Color: warm filmic, soft falloff, real-world contrast.

**Critical instructions:**
- The case remains 100% accurate to `--start-image`
- The lanyard, slider, ESR wordmark all preserved
- Hand is real-looking (Seedance 2.0 handles hands well); avoid AI-uncanny issues
- The setting is recognizably Colombian-friendly (modern, urban, premium-but-not-elite)

**Audio plan:**
- Ambient room tone (Mirelo): "soft café ambience, distant chatter, espresso machine"
- Music (Sonilo): "warm uplifting indie-electronic, slow tempo, optimistic, 7 seconds"

---

## Production order

1. **Hero shots arrive from user** → save to `hero-shots/` → upload to Higgsfield via `media_upload_widget` → capture upload IDs
2. **Logo arrives** → save to `/brand/assets/accessify-logo.png` → update `/brand/visual-identity.md` with real color palette
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
