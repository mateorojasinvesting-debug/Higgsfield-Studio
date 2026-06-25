# Visual Identity — Accessify

> ⚠️ This file will be updated once the user provides the Accessify logo + brand colors.
> Until then, defaults below are placeholders aligned with the premium positioning.

## Logo

**File location:** `/brand/assets/accessify-logo.png` *(pending upload)*

Requirements:
- PNG with transparent background
- Minimum 2000 px on long edge
- Both light variant (white logo) and dark variant (black logo) if available

## Outro template (every video must end with this)

A 1.5-second logo stamp at the end of every generated video.

**Prompt template for Higgsfield to generate the outro frame:**

```
Final brand stamp. Pitch black background. Centered: "Accessify" logo
[white version], 18% of frame height. Below logo, in small white type,
tagline placeholder. Subtle radial vignette. The frame holds for 1.5
seconds, then fades to black.
```

For now (before logo arrives), the outro can be generated as a typographic-only frame:

```
Final frame, 1.5s. Pitch black background. Center: "Accessify" in
ultra-light geometric sans-serif (Inter Display Light or similar),
white, 14% of frame height. 600ms fade-in. Hold for 700ms. 200ms
fade-out to black.
```

## Color palette (initial — to be refined with logo)

| Role | Color | Use |
|---|---|---|
| **Primary background** | Pitch black `#000000` | Hero backgrounds, outro |
| **Premium contrast** | Pure white `#FFFFFF` | Logo, text on dark |
| **Accent (signal)** | Mint green `#00D4A8` | LED-style highlights, cues |
| **Lifestyle warm** | Camel/beige `#C9A87C` | Lifestyle environments, materials |
| **Premium dark** | Charcoal `#1A1A1A` | Studio backgrounds (warmer than pure black) |

The mint accent works particularly well because it matches the ESR Pulse FlickLock's slider color — gives brand continuity with the product photo without altering it.

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
