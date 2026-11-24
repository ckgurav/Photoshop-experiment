---
name: taste-transfer
description: >
  Learn a photographer's personal editing style from their EXISTING Adobe
  Lightroom Classic edits and write it to an inspectable taste profile, then keep
  refining it as more examples arrive. Use when the user wants to "learn my
  edits", "figure out my style and why I do it", build a personal edit profile or
  "AI profile" from past work, or migrate their look between machines. It reads
  final Develop settings live from the catalog via the Claude Bridge (the
  `lightroom` MCP), diffs them against process-version-correct baselines, renders
  each shot to judge intent by eye, and synthesizes repeatable rules with a
  confidence level. Do NOT use it to make new edits (use lightroom-bridge /
  lightroom-bridge-pro for that) or for generic image analysis.
---

# Taste Transfer — learn an editor's style from their Lightroom edits

Goal: turn a pile of the user's **finished** Lightroom Classic edits into a
small, readable `profile/user-taste.md` that states **what they do and why**, so
their look can be reproduced on new photos — and so it can travel between
machines. Each run refines the profile; that is the "recursive self-improvement."

Work the golden rule the bridge teaches: **look → read → look.** Never infer a
style from numbers alone; render the edit and confirm the numbers match the
image.

## 0. Preconditions

1. `mcp__lightroom__ping` → confirm the bridge is connected. If it reports
   `stopped`/unreachable, tell the user: Lightroom → File → Plug-in Extras →
   Claude Bridge: Start (or reopen Lightroom). Do not proceed blind.
2. `mcp__lightroom__list_selected`. If fewer than ~5 photos are selected, ask the
   user to select **5–10 of their finished reference edits** in the Library grid
   (varied scenes: bright vs. dim, tight portrait vs. wide group, a detail/decor
   shot). More examples ⇒ higher-confidence profile.

Never reset, re-edit, or overwrite the reference photos. You are reading.

## 1. Per-example extraction — for EACH selected photo

Iterate the selection (read UUIDs from `list_selected`, step with
`select_photo`). For each:

### a. Pull the data
- `mcp__lightroom__get_develop_settings` — the full edit.
- `mcp__lightroom__get_metadata` — scene context (ISO, focal length, shutter,
  camera) to ground the "why".

### b. Resolve the ACTIVE recipe (critical — avoid the legacy-field trap)

Lightroom keeps dead pre-2012 fields in the settings. At `ProcessVersion` ≥ 2012
(e.g. "15.4") the live tone values are the **`*2012`-suffixed** ones; the bare
names are ignored legacy residue.

- Tone (use the 2012 keys): `Exposure2012`, `Contrast2012`, `Highlights2012`,
  `Shadows2012`, `Whites2012`, `Blacks2012`, `Clarity2012`, plus `Texture`,
  `Dehaze`, `Vibrance`, `Saturation`.
- **IGNORE** `Exposure`, `Contrast`, `Shadows`, `Highlights`, `Brightness`,
  `Clarity` with NO `2012` suffix. Reporting `Contrast: 25` when `Contrast2012`
  is `0` will get the style exactly backwards. (Real case: DSC01502 had
  `Contrast: 25` legacy but `Contrast2012: 0` — the true edit added **no**
  contrast.)
- White balance: `WhiteBalance`, `Temperature`, `Tint`.
- Crop: `CropTop/Left/Right/Bottom` → % trimmed per side; `CropAngle` → straighten
  degrees.
- Geometry: only report `PerspectiveUpright`/`Perspective*` if **non-zero**. A
  nonzero `UprightTransformCount` with `PerspectiveUpright: 0` means Upright was
  *analyzed but set to Off* — not applied.
- Tone curve: is `ToneCurvePV2012` the identity `[0,0,255,255]` (none) or shaped?
  A profile `Look` (e.g. "Adobe Color") carries its own curve — note the profile
  separately from any user curve.
- Color: HSL (`HueAdjustment*`/`SaturationAdjustment*`/`LuminanceAdjustment*`),
  color grading (`ColorGrade*`, split toning), `PointColors`.
- Local / AI (these live ONLY in the catalog, never in XMP — which is the whole
  reason we read live, not from sidecars):
  - Masks: `MaskGroupBasedCorrections` → how many, what type (subject, sky,
    face/skin, background — from `MaskName`/`MaskSubCategoryID`), and the local
    adjustments on each (`LocalExposure2012`, etc.).
  - AI Denoise: a `FilterList` entry titled Denoise/Enhance → note it and amount.
  - AI Lens Blur: `LensBlur.Active` + `BlurAmount`.
- Profile: `CameraProfile`, `Look.Name`.

### c. See it
- `mcp__lightroom__render_preview` (size ~1024). View the edited result.
- Check the numbers against the pixels. Does +1.3 exposure read as "bright and
  natural" or "blown"? Is the lens blur visible as background separation? If the
  image contradicts your reading of the numbers, trust the image and re-read.
- (Optional before/after) Do NOT reset the catalog photo to get a "before". If a
  visual baseline is wanted, render a neutral from the RAW path with an external
  engine — never by mutating their edit.

### d. Record it
Write `profile/examples/<FILENAME>.md` using the template in the existing
`DSC01502.md` record: scene one-liner, the resolved active recipe (table), and
**2–4 sentences on what they did and why**, grounded in the scene. The "why" is
the valuable part — the numbers are cheap, the intent is the style.

## 2. Synthesis — across ALL examples

Open every `profile/examples/*.md` plus the current `profile/user-taste.md`. Look
for what **repeats**, not what any single edit did:

- Hard constraints / never-dos (sliders always left at 0; things they never do).
- White-balance behaviour (typical Kelvin range + tint direction).
- Exposure behaviour: do they correct hard (big + on dim frames) or sit near 0?
  Do they aim bright, neutral, or a touch dark?
- Which tools they actually reach for vs. never touch.
- Crop/straighten habits (how often, how much).
- Signatures — distinctive moves that recur (e.g. "AI Lens Blur for subject
  separation instead of clarity/vignette").
- Color/curve tendencies.

Rewrite `profile/user-taste.md` as **rules with confidence**: each claim tagged
with how many examples support it (`[n=3]`) and `high/med/low` confidence.
Low-sample or contradictory claims must be flagged as "needs more examples," not
laundered into certainty. State ranges (e.g. "WB 4800–5100K") not single points.

## 3. Recursive refinement (why this improves over time)

- Every new batch of examples re-runs §1–2 and tightens the ranges / raises
  confidence. The profile is versioned in git, so you can diff what changed.
- When the user later **corrects** an auto-edit (in the Produce stage), that
  correction is a new, high-value example — fold it back in.
- Optionally seed the bridge's own knowledge base so the `lightroom-bridge-pro`
  skill uses the profile automatically: copy the finished `profile/user-taste.md`
  into the bridge's `kb/styles/` (the bridge's user-taste card overrides genre
  defaults). Keep this repo's copy as the canonical, git-tracked source.

## 4. Hand back

Commit `profile/` and push. On the Produce machine, `git pull` brings the profile
over; new shoots are then edited to it and left in the catalog for review.
