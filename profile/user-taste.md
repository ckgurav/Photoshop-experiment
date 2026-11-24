# User taste profile

Living document. Each `taste-transfer` run refines it. Claims are tagged with
sample count `[n=N]` and confidence. Treat `low` claims as hypotheses to confirm
with more examples, not rules.

> Status: **learned from the Veer catalog** — statistical pass over **all 1,877
> edited photos** (ProcessVersion 15.4, read from the catalog; values validated
> against the live bridge), plus **~11 eye-confirmed examples** rendered across
> the shoots whose RAWs are on this machine (Yug, Half Sari, Veer janmashtami,
> Chandan). Supersedes the 1-example seed. Scope note: 5 shoots (Praveen,
> sonali, Jyoti, Veena Ji birthday, Headshot — ~1,228 edits) are **offline**, so
> they count in the statistics but could not be rendered/eye-confirmed. Lens-blur
> frames render slowly and a few timed out, but the effect was confirmed on two.
> Genre: **family / event portraiture** (studio-ish and outdoor), Nikon + some Sony.

## How to read the numbers
Tone values are the **`*2012`** fields; the bare legacy fields are ignored
(1,580 of 1,877 photos carry a dead `Contrast = 25` while `Contrast2012 = 0` —
reading legacy gets the style backwards). Percentages are of the 1,877 edited.

## Hard constraints (very high confidence)

- **Restraint is the look; the profile + WB + exposure *is* the edit.** Color is
  never hand-pushed: **HSL nonzero on 1/1877, Color Grading 0/1877, Split Toning
  1/1877, user tone curve 0/1877.** Also never used: post-crop vignette (0),
  grain (0), point color (0), parametric curve (0). `[n=1877]` **high** — this is
  the strongest signal in the whole catalog.
- **Don't stack presence sliders.** Clarity/Texture/Dehaze are left at 0 on ~90%
  of edits; when touched they are tiny (Clarity +3, Texture +5, Dehaze +2) and
  only as part of the Vivid batch recipe below, not reached for individually.
  `[n=1877]` high
- **Never synthesize faces, jewellery, or clothing.** No generative recreation of
  people or their garments/ornaments. `[stated]` high
- **Edits stay in the Lightroom catalog for review** — not auto-exported.
  `[stated]` high

## Profile / color

- **Adobe Color is the default profile — 88% (1,657).** `[n=1877]` high
- **Adobe Vivid is the deliberate alternate — 11% (211)**, used for punchy
  colour on decor/detail and some outfit-forward portraits (the "Half Sari"
  shoot used it throughout). A few one-offs: Adaptive B&W (3), Camera Landscape
  (2). `[n=1877]` high
- Colour otherwise comes only from WB + profile. No HSL, no grading. `[n=1877]` high

## White balance  *(corrects the seed's ~4900K guess)*

- **Custom WB on 79% (1,485); As-Shot on 21% (392).** They dial WB deliberately
  most of the time, but trust the camera on a fifth of frames (notably much of
  the Yug shoot). `[n=1877]` high
- **Temperature: median 5500K, middle 80% ≈ 5000–5800K**, full range 3800–5950K.
  Warm-neutral daylight, not clinical. Lower (~4100–5000K) in grey-walled indoor
  rooms to keep walls neutral; ~5500–5800K outdoors/golden hour. `[n=1483]` high
- **Tint: median +5, typical 0 to +15** (slight magenta), occasionally up to +45.
  `[n=1483]` high

## Exposure

- **Touched on 58% (1,082). Usual move is a gentle lift: median +0.35, middle 80%
  −0.25 to +0.70.** `[n=1877]` high
- **Corrects hard when the frame demands it:** dim indoor frames pushed to
  +1.1…+1.6 (airy, faces clearly lit); bright/over frames pulled to −0.6…−1.3.
  The lever is exposure, not fill/curves. `[n=1877, eye-confirmed]` high
- Target: bright and natural, nothing blown; backlit subjects lifted while
  highlights are held by Whites (below). `[eye-confirmed]` high

## Whites & highlight recovery  *(new, strong signal)*

- **Whites pulled DOWN on 33% (611): median −33, routinely −40 to −96** on bright,
  backlit, or white-heavy frames (white backdrops, sunlit saris, golden hour).
  This is their main highlight-protection move — paired with an exposure lift on
  the subject. `[n=611]` high
- Highlights2012 / Shadows2012 / Blacks2012 are each touched only ~10–12% of the
  time; when used they cluster at Highlights −25, Shadows +15, Blacks −12 (mostly
  the Vivid recipe). Big shadow lifts (+100) appear only on harsh-sun rescues.
  `[n=1877]` med-high

## Contrast & the "Vivid batch recipe"

- **Contrast2012 is touched on only 16% (294); when added it clusters at +18.**
  `[n=294]` high
- That +18 is part of a **repeatable batch look applied per-shoot** (the entire
  Half Sari set): **Adobe Vivid + Exposure +0.7, Contrast +18, Highlights −25,
  Shadows +15, Whites +12, Blacks −12, Vibrance +18, Saturation +4, Clarity +3,
  Texture +5, Dehaze +2, WB 5000K/+6.** It reads as a rich, warm, slightly punchy
  "finished gallery" look for a themed shoot — applied uniformly, not tuned per
  frame. `[n~185]` high — a genuine signature recipe, distinct from the restrained
  Adobe-Color default.

## Crop & geometry

- **Cropped on 46% (866); when cropped, substantial — median ~63% of area kept**
  (often reframing to a tighter vertical portrait). `[n=866]` high
- **Straightened on 34% (643): small angles, typically ±3°** (median ~0). A few
  large rotations for deliberate reframing. `[n=643]` high
- Upright/perspective analyzed but left **Off** (nonzero transform counts with
  `PerspectiveUpright = 0`). They straighten by angle, not by Upright. `[n=1877]` high

## Signatures

- **AI Lens Blur for subject separation — 48% (909), BlurAmount almost always
  exactly 50** (a handful at 62). Optical background separation instead of
  clarity/vignette tricks — the single most distinctive habit, now confirmed at
  scale (the seed's hunch was right). `[n=909]` **high**
- **Selective local masking is rare — 5% (88), all AI masks** (subject/face lift,
  or subject-select + background cleanup for utility shots like the passport
  photo). Not a routine move; reserved for specific needs. `[n=1877]` high
- **Negative Clarity (e.g. −6) to soften skin** on some close portraits (babies).
  `[eye-confirmed, small-n]` med
- **Modest default detail**: Sharpness ~40 and Color NR ~25 are the Lightroom
  defaults left in place (not a deliberate style move). `[n=1877]` high

## What this means for producing new edits
1. Set profile **Adobe Color** (or **Vivid** for a punchy decor/themed set).
2. **Custom WB ~5200–5700K outdoors, ~4100–5000K in grey rooms; tint +5.** (Leave
   As-Shot if the camera nailed it.)
3. **Expose for the subject** (gentle lift ~+0.35, more if dim); **pull Whites
   down (−30 to −60+) to protect highlights.** Leave Highlights/Shadows/Blacks
   near 0 unless rescuing harsh light.
4. **No** HSL, color grading, split toning, tone curve, vignette, grain, or
   stacked clarity/texture.
5. **Crop/straighten** to a clean vertical portrait when it helps (small angle).
6. **AI Lens Blur @ 50** to separate the subject from a busy/outdoor background.
7. Local masks only when a face needs a whisper of light or a background needs
   cleanup. Leave the result in the catalog for review.

## Open questions for the next pass
- Eye-confirm the offline shoots (Praveen/sonali/Jyoti/Veena/Headshot) once their
  RAWs are present — especially the 3 **Adaptive B&W** frames (Praveen) and the
  Sony "sonali" set, to see if B&W and the Sony files follow the same rules.
- Is the Vivid batch recipe reused across other themed shoots, or Half-Sari-only?
- When do they choose As-Shot vs Custom WB? (Yug leaned As-Shot — camera/body or
  lighting dependent?)
