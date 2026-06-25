# Visual Identity — accesify

## Logo

**Wordmark:** `accesify.` — all lowercase, ultra-bold geometric sans-serif (looks like Inter Black, Söhne Breit Kräftig, or Neue Haas Grotesk Black), with a **copper/orange square accent dot** at the end as the brand mark.

**File location:** uploaded to Higgsfield via `media_upload_widget` (see `/brand/assets/accesify-logo-upload-id.txt` once captured).

**Composition rule:** the wordmark always reads `accesify.` with the period and the copper dot. The dot is a critical brand element — never remove it. The dot color is the only spot of warmth in an otherwise black-and-white system.

**Variants needed (for future):**
- Wordmark on transparent (have it) ✅
- Wordmark in white on black (for dark video outros) — to generate from the black version
- Square icon version (just the copper dot, or the "a." monogram) — for favicons, app icons

**Capitalization rules:**
- **Logo:** always lowercase `accesify.`
- **Running text at sentence start:** Capital "Accesify" (e.g., "Accesify is a Colombian curator...")
- **Inside sentences when stylized:** can use lowercase `accesify` as a vibe choice (like *spotify*, *figma*)
- **In titles/headlines:** Capital "Accesify"
- **In product copy where minimal aesthetic matters:** lowercase preferred

## Outro template (every video must end with this)

A 1.5-second logo stamp at the end of every generated video.

**Prompt template for Higgsfield to generate the outro frame:**

```
Final brand stamp. Pitch black background. Centered: "Accesify" logo
[white version], 18% of frame height. Below logo, in small white type,
tagline placeholder. Subtle radial vignette. The frame holds for 1.5
seconds, then fades to black.
```

For now (before logo arrives), the outro can be generated as a typographic-only frame:

```
Final frame, 1.5s. Pitch black background. Center: "Accesify" in
ultra-light geometric sans-serif (Inter Display Light or similar),
white, 14% of frame height. 600ms fade-in. Hold for 700ms. 200ms
fade-out to black.
```

## Color palette (final — derived from logo)

| Role | Color | Use |
|---|---|---|
| **Primary text / wordmark** | Almost-black `#1A1A1A` | Logo wordmark on light backgrounds |
| **Brand accent (THE dot)** | Copper / burnt orange `#C5602E` | The wordmark's terminal dot. Used sparingly as the single warm accent — CTAs, badge backgrounds, hover states |
| **Primary background** | Pitch black `#000000` | Video outros, hero backgrounds, premium static shots |
| **Premium contrast** | Pure white `#FFFFFF` | Inverted wordmark, text on dark, MeLi-style clean shots |
| **Studio warm dark** | Deep charcoal `#1A1A1A` | Less-cold alternative to pure black for studio compositions |
| **Lifestyle warm neutral** | Linen / camel `#D4C5A8` | Lifestyle environments (fabric, materials, soft props) |
| **Product accent (ESR's color)** | Mint green `#00D4A8` | Only present on the actual product (slider, LED). Preserve exactly when generating; do NOT use in copy or brand chrome — that's ESR's color, not accesify's |

**Key insight:** The accesify palette is intentionally **monochrome + one warm dot of color** (copper). This contrasts with the cold mint of the ESR product — the two coexist visually because the product is the hero (with its mint) and the brand is the frame (with its copper). Don't mix them; let each own its role.

## Typography (when overlaying text on creatives)

- **Display:** Inter Display, SF Pro Display, or Helvetica Now Display — geometric sans, weights 200-300 only
- **Body:** Inter, SF Pro Text — weight 400
- **Numbers/specs:** Same as display but tabular figures

Never use:
- Stroke effects, drop shadows, or text glow (cheap-looking)
- ALL CAPS for long phrases (single words OK for emphasis)
- More than 2 typefaces in a single creative
- Brand-default fonts like Arial, Times, Comic Sans

## Lighting and composition reference

**Mood references (the look we want):**
- Apple keynote product reveals (controlled studio, single key light, gradient backdrop)
- Dyson product films (extreme close-ups, slow rotation, material-revealing light)
- Aesop product photography (warm minimal environments, leather, marble, linen)

**Cinematography defaults for video:**
- Camera moves: slow dolly-in, parallax orbit, locked-off macro
- Lenses: 50mm and 85mm equivalents (no fish-eye, no wide)
- Depth of field: shallow on hero shots, full DoF on lifestyle
- Frame rate: 24fps for cinematic, 60fps for slow-motion details
- Resolution: 1080p minimum for delivery, 720p only for fast Marketing Studio iterations

**Backgrounds:**
- ✅ Pitch black or gradient charcoal (hero shots)
- ✅ Marble, linen, leather, raw concrete (lifestyle)
- ✅ MacBook, leather notebook, ceramic coffee cup (context)
- ❌ Bright white sterile e-commerce backgrounds (too generic)
- ❌ Confetti, gradients with rainbow, sparkles (childish)
- ❌ Vague office stock environments

## Outro implementation in Higgsfield

The outro can be implemented two ways depending on the model used:

**Option A — Single shot (preferred):** Compose the brand stamp directly into the final 1.5 seconds of the generated video by extending the prompt. Works with `seedance_2_0` and `marketing_studio_video`.

**Option B — Post-concat:** Generate the main video and a separate 1.5s stamp video, then concatenate with `ffmpeg` (CLI in cloud env). More controllable, slightly more steps.

For first tests, use Option A. If precision becomes a problem, switch to Option B.
