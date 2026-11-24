# User taste profile

Living document. Each `taste-transfer` run refines it. Claims are tagged with
sample count `[n=N]` and confidence. Treat `low` claims as hypotheses to confirm
with more examples, not rules.

> Status: **seed** — built from 1 validated example (DSC01502) plus constraints
> the user stated directly. Everything numeric here is low-confidence until the
> reference batch on the other laptop runs.

## Hard constraints (high confidence — stated by the user)

- **Restraint is the look.** Correct white balance + exposure *is* the edit. The
  "pop" comes from exposing right, not from stacking contrast / clarity /
  saturation. Under-process when in doubt. `[stated]` high
- **Do not stack tonal/color effects.** On a corrected frame, contrast,
  highlights, shadows, whites, blacks, clarity, texture, dehaze, vibrance and
  saturation are typically left at **0**. `[n=1]` med
- **Never synthesize faces, jewellery, or clothing.** No generative recreation of
  people or their garments/ornaments. `[stated]` high
- **Edits stay in the Lightroom catalog for the user to review.** Don't run
  end-to-end to exports/prints unless asked. `[stated]` high

## White balance

- Warm-neutral for indoor events: **~4900K, tint ~+4** (slight magenta).
  Inviting, not clinical. `[n=1]` low — confirm the range across examples.

## Exposure

- **Corrects hard when needed.** A dim indoor frame was pushed **+1.3** to reach a
  natural, airy level — the single biggest lever used. "Lean darker" is relative
  to an over-bright auto, NOT a refusal to brighten: they will push exposure a
  lot to *correct*, then stop. `[n=1]` med
- Target: bright enough that faces are clearly lit, nothing blown. `[n=1]` low

## Crop & geometry

- Light tidy crops (a few % per side); straighten only when needed (DSC01502 was
  0°). Upright analyzed but left Off in the one example. `[n=1]` low

## Signatures

- **AI Lens Blur for subject separation.** Lifts subjects off a busy event
  background optically (`BlurAmount ~50`) rather than via clarity/vignette tricks.
  Looks like a genuine signature — confirm frequency. `[n=1]` low
- **Modest AI Denoise** (~25), clean but not scrubbed. `[n=1]` low
- **Whisper-level local light on the subject's face** (portrait mask,
  `LocalExposure2012 ~+0.06`) — barely perceptible. `[n=1]` low

## Color

- Profile left at **Adobe Color**; no HSL, no color grading, no split toning, tone
  curve linear in the one example. Color is left natural. `[n=1]` med

## Open questions for the reference batch

- Is the warm ~4900K WB consistent, or scene-dependent?
- How often does Lens Blur actually appear? Signature or one-off?
- Do they ever add a small vibrance/contrast, and in what scenes?
- Crop aspect-ratio habits; straighten frequency.
- Any difference in approach between portraits, groups, and decor/detail shots?
