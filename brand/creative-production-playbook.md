# Creative Production Playbook — Accesify

The winning recipe for a premium hypermotion product ad, distilled from producing the
FlickLock (AirPods) and UltraFit (Screen Protector) creatives. Follow this to get it
right on the first or second try. Every rule here was learned from a real mistake.

## 0. Golden path (TL;DR)

1. **Study the product** — read specs + look at every supplier photo AND the functionality video frame-by-frame (extract frames with ffmpeg). Understand the exact mechanism BEFORE writing a prompt.
2. **Pick the start image** — a REAL, clean product frame, re-processed (see §2).
3. **Model:** `marketing_studio_video` + `--mode product_showcase` for premium hypermotion. NO `ad_reference` (see §3).
4. **Write the prompt** with the mandatory blocks (see §4).
5. **Preflight cost** (`higgsfield generate cost ...`) → tell the user → get OK if cost-sensitive.
6. **Generate**, then **download + extract frames + review honestly** (see §6) before showing the user.
7. **Outro + audio + deliver** (see §7–8).

## 1. Model & mode selection

- **Premium hypermotion product film (Apple/Dyson feel):** `marketing_studio_video` + `--mode product_showcase`. This is Higgsfield's "hypermotion" — multi-shot reel, speed-ramps, whip-pans, ARRI ALEXA look. This is what the user loves.
- **Do NOT use `ad_reference_id`** pointing at the supplier's own ad. It faithfully replicates the source ad's clichés (steel balls, hexagon shockwaves, split-screens) even when the prompt forbids them. Use a clean `start_image` + prompt instead.
- **`seedance_2_0`** follows the prompt literally (good for exact mechanism) but is less "hypermotion" and hallucinates UI on screens. Use only when product_showcase over-interprets.
- Default output: **9:16 vertical, 15s** for TikTok/Reels. Preflight cost first.

## 2. Start image preparation (critical)

- Use a **real product frame/photo**, not a text-to-image invention.
- **Re-process before upload:** minimum ~1080px on long edge, **JPG (no alpha channel)**, padded to clean 9:16. Small PNGs (e.g. 401×288) with alpha **break Seedance/marketing_studio** ("IP check" / silent fail).
- If pulling a frame from a supplier video, extract with ffmpeg, crop out watermarks/overlay text, pad to 1080×1920.

## 3. Screen / display rule (phones & devices)

- If the product involves a phone/tablet/watch screen, **always state the screen is COMPLETELY OFF — pure black glossy mirror glass, powered down. NO UI, NO icons, NO clock, NO text, NO Chinese characters, NO symbols.**
- Reason: the model hallucinates fake/garbled UI (Chinese characters) on lit screens. An OFF screen has nothing to hallucinate.

## 4. Prompt structure (mandatory blocks)

Every product-film prompt should contain, in order:

1. **Style line:** "Ultra-premium hypermotion product commercial, Apple keynote + Dyson aesthetic, 9:16, ARRI ALEXA, anamorphic, film grain, pitch-black studio void, [accent color] as only accent."
2. **Product block:** exact description from the start image (materials, colors, branding, distinctive parts).
3. **Screen-OFF rule** (if a screen is involved) — see §3.
4. **THE MECHANISM block** (the hero moment): describe the motion EXACTLY and step-by-step:
   - Direction of every motion (e.g. "slider slides LEFT to RIGHT", "tab pulled DOWNWARD out the BOTTOM").
   - What is physically connected to what and what happens as a consequence (causality: "as the tab is pulled, it drags the liner, and behind it the glass laminates top-to-bottom").
   - **Forbid degenerate shapes:** "the [part] stays small and flat — it must NEVER become a road, runway, carpet, path, ramp, or expand into a large shape."
   - Study the real functionality video frame-by-frame first and reproduce it.
5. **Beat list with timestamps** (Hook → mechanism → reveal → durability → hero).
6. **Durability beats** (if relevant): steel ball + soft WARM GOLD light ring (NOT blue cartoon hexagons); scratch = smooth tool gliding, no scratch left.
7. **Fidelity + restrictions block:** preserve branding; screen off; no person/face/hands/voice; no watermark; no captions; no on-screen UI; **no invented/garbled text** ("the only text is X; prefer no text over gibberish").
8. **Color grade line.**

## 5. Known failure modes & fixes

| Symptom | Cause | Fix |
|---|---|---|
| Fake Chinese/garbled UI on screen | Lit screen hallucination | Screen OFF rule (§3) |
| Garbled small text on product ("Tempered Fit Ulass") | AI can't render tiny text | Instruct "only X text, no gibberish". Minor garble can't be cleanly removed in post (moving text → blur/delogo leaves artifacts). Best avoided at generation, not fixed after. |
| Pull-tab/slider becomes a giant "road/runway" | Model exaggerates a directional element | Forbid explicitly (§4.4) |
| Cliché steel balls / hexagons appear unasked | `ad_reference` replicating source ad | Drop ad_reference; use start_image |
| Ends showing back of phone / weird screen | Underspecified final shot | "Final shot is the FRONT, never the back; screen off; no reflective image" |
| Job fails "nsfw" | False positive (words like "blade", "strike", "thumb near object") | Soften language: blade→smooth stylus, strike→rests-then-bounces, remove "thumb". Retry. |
| HTTP 502 | Higgsfield API intermittent | Wait ~60s, retry |
| Silent fail / "IP check" | Bad start image (small/alpha PNG) | Re-process image (§2) |

## 6. Review process (never skip)

After every generation, **download the mp4 and extract a frame montage** to review with your own eyes before showing the user:

```bash
curl -sS -o v.mp4 "<result_url>"
mkdir -p rev && for t in 0 1 2 3 4 5 6 8 10 12 14; do \
  ffmpeg -nostdin -loglevel error -ss $t -i v.mp4 -frames:v 1 -q:v 2 rev/t$t.jpg; done
ffmpeg -nostdin -loglevel error -pattern_type glob -i 'rev/t*.jpg' \
  -filter_complex "tile=4x3" rev/montage.jpg
```

Give an HONEST review (what worked, what's wrong) — don't guess, look.

## 7. Outro (accesify logo)

- Generate a clean outro frame: `nano_banana_2` with the logo (media_id `6e2229f7-8e59-4e08-8627-2bde1cde8679`) as `--image` reference → white "accesify." wordmark + copper dot on pitch black, 9:16.
- **Make the logo appear spontaneously**, not after dead black: trim the video's trailing pure-black tail, then ffmpeg `xfade` from the hero into the logo ~1.5–2s before the end:

```bash
ffmpeg -i video.mp4 -loop 1 -t 2.5 -i outro.png -filter_complex \
"[0:v]trim=0:13.5,setpts=PTS-STARTPTS,scale=720:1280,setsar=1,fps=30[v0]; \
 [1:v]scale=720:1280,setsar=1,fps=30[v1]; \
 [v0][v1]xfade=transition=fade:duration=0.6:offset=12.9[outv]" \
-map "[outv]" -c:v libx264 -crf 18 master.mp4
```

## 8. Audio

- **KEEP the native Higgsfield-generated audio** that comes with `marketing_studio_video` (it generates a good music bed). Mux it under the final cut with a gentle fade-out over the logo.
- Do NOT replace it with custom Sonilo/Mirelo unless the user explicitly asks — a custom mix was tried once and rejected ("muy malo").
- To keep native audio while re-cutting video: `-map 0:v` (edited video) `-map 1:a` (original mp4 audio), `afade=t=out` near the end.

## 9. Cost & approval discipline

- **Preflight every render:** `higgsfield generate cost <model> --prompt "..." <flags>`.
- Rough costs: `marketing_studio_video` 15s ≈ 75 cr; `seedance_2_0` 10–12s ≈ 45–54 cr; `nano_banana_2` image ≈ 2 cr; `sonilo_music`/`mirelo` ≈ small.
- When the user is cost-sensitive, present the full prompt + plan + cost and get an OK BEFORE generating.
- Failed jobs (nsfw/502) generally are not charged, but verify balance with `higgsfield account status`.

## 10. Brand rules (always)

- Product shown 100% faithful (ESR etc. unchanged). Accesify appears ONLY as the outro logo.
- Preserve supplier branding on the product; never edit it out.
- Colombian market; premium Apple/Dyson tone.
