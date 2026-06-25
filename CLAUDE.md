# Accessify Creative Studio

Operating instructions for every Claude session in this repo.

## What this repo is

The creative production system for **Accessify**, a Colombian reseller of premium tech accessories (primarily ESR-manufactured iPhone cases, AirPods cases, screen protectors, iPad cases, keyboards, wireless chargers). This repo turns product specs + reference photos into premium ad-grade creatives (images, videos, UGC) using Higgsfield AI, ready to ship to TikTok Ads, Meta/Instagram, and Mercado Libre listings.

## Who we serve

- **Market:** Colombia
- **Channels:** TikTok Ads Manager, Meta/Instagram Ads, Mercado Libre listings, organic IG/TikTok
- **Goal:** maximize conversion per dollar of ad spend via data-informed, premium-positioned creatives

## Brand positioning — Accessify

Accessify is a **curator**, not a manufacturer. Positioning angle: *"Premium accessories, intelligently selected for the Colombian market."* Voice is confident, warm, and design-forward — closer to Apple/Dyson than to typical e-commerce.

- **Do** highlight engineering, materials, exclusivity, fit
- **Do** show the product as the hero with cinematic light and motion
- **Don't** alter ESR's product, branding, or specs in any creative
- **Don't** make functional claims beyond what the product actually does
- **Don't** use cheap-looking UGC unless data proves it converts in our market

## Critical creative rule — product fidelity

We are a reseller. The supplier's product (ESR, etc.) is shown **exactly as it is**, including the supplier's logo/branding. Accessify branding goes ONLY as:
1. An **outro frame** (1-2s) at the end of every video with the Accessify logo
2. A **subtle watermark** on static images (corner, low-opacity)

Never edit the supplier product itself. Never imply Accessify manufactured it. Use the customer's actual photos as `--start-image` for `seedance_2_0` (image-to-video) so the product stays 100% faithful — animate the camera, light, environment, not the product.

## Folder map

```
/CLAUDE.md                         <- this file (the constitution)
/brand/
  accessify.md                     <- brand positioning, voice, do/don'ts
  visual-identity.md               <- palette, logo, outro template
  tone-of-voice.md                 <- Colombian Spanish guidelines
  hook-bank.md                     <- proven hooks library (grows over time)
  insights.md                      <- (future) data-driven patterns from past campaigns
/products/
  <slug>/                          <- one folder per product
    specs.md                       <- normalized specs
    creative-brief.md              <- approved concepts for this product
    details/                       <- supplier detail images (reference)
    hero-shots/                    <- clean shots used as image-to-video inputs
    references/                    <- supplier docs (catalog, manuals)
    generated/                     <- creatives produced + their Higgsfield job IDs
/campaigns/
  <YYYY-MM-name>/
    brief.md
    creatives/
    decisions.md
    performance.md                 <- after publishing
/data/
  esr-catalog-full.pdf             <- raw catalog
  catalog.csv                      <- normalized when needed
/.agents/skills/                   <- Higgsfield agent skills (don't edit; installed via npx skills)
/.claude/
  commands/                        <- /brief, /hooks, /creative, /score, /optimize
  agents/                          <- specialized sub-agents
```

## Tool selection rules (Higgsfield)

| Task | Model / tool | Why |
|---|---|---|
| Premium static hero photo (banner, IG feed, MeLi listing) | `marketing_studio_image` or product-photoshoot skill | Brand-safe ad image with Accessify aesthetic |
| Animate a product photo (hypermotion, cinematic) | `seedance_2_0` with `--start-image <hero.jpg>` | SOTA image-to-video, preserves product fidelity |
| Full ad video with avatar/UGC presenter | `marketing_studio_video` | Has hooks/settings library + brand kit integration |
| Logo, vector, brand mark | `recraft_v4_1` | Vector-clean output |
| Score a finished video before launching | `brain_activity` (Virality Predictor) | Objective hook + retention score |
| Sound effects / foley for product videos | `mirelo_text_to_audio` | Premium audio without licensing |
| Music bed | `sonilo_music` | Custom backing track |

Default cadence: produce → score with `brain_activity` → only publish if score ≥ threshold (calibrated per platform).

## Workflow — new product from supplier

1. **Save** supplier specs to `/products/<slug>/specs.md` + reference images to `/products/<slug>/details/`
2. **Get hero shots** from supplier (clean white/black background) → `/products/<slug>/hero-shots/`
3. **Draft creative brief** in `/products/<slug>/creative-brief.md` — 3 concepts minimum (static hero, hypermotion video, UGC)
4. **Upload** hero shots to Higgsfield via `media_upload_widget` → capture `upload_id`
5. **Generate** each concept with the appropriate model, saving outputs to `/products/<slug>/generated/`
6. **Score** each video output with Virality Predictor
7. **Commit** brief + outputs + scores so the trail is reviewable

## Logos & branding

- Accessify logo lives at `/brand/assets/accessify-logo.png` (transparent PNG)
- Every generated video must end with a 1-2s outro frame showing the Accessify logo (see `/brand/visual-identity.md` for template prompt)
- Never edit supplier branding (ESR, etc.) out of the product itself

## Tone of voice — Colombian Spanish

- Default to neutral Spanish, with Colombian warmth (not slang-heavy)
- Premium-accessible: confident without being pretentious
- Avoid the agressive "¡COMPRA YA!" energy — favor understated authority
- See `/brand/tone-of-voice.md` for examples

## Git workflow

- Work on branch `claude/vigilant-cray-844egq` (or current feature branch)
- Commit after every product setup, every creative produced, every brief approved
- Push to remote after meaningful units of work
- Never commit `.env`, API tokens, or PII from any future customer database

## Privacy

- Customer/campaign performance data (when added later) lives in `/data/raw/` which is **gitignored**
- Only anonymized aggregates/patterns go to `/brand/insights.md` (committed)
