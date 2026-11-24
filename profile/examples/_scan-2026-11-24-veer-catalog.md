# Scan evidence — Veer catalog, 2026-11-24

Evidence behind this run of `profile/user-taste.md`. Method: read develop
settings for **all 1,877 edited photos** (`hasDevelopAdjustmentsEx = 1.0`) from a
read-only snapshot of `Veer.lrcat` (values validated against the live `lightroom`
bridge on DSC_0915), parsed the Lua settings blob (top-level fields, ignoring
pre-2012 legacy residue), then **rendered ~11 representative frames via the
bridge** to confirm numbers against pixels. No photo was reset, re-edited, or
exported.

Catalog: `/Users/I335763/Pictures/Veer/Veer.lrcat` · 2,833 images, 1,877 edited.

## Edited photos by shoot (RAW present on disk?)
| Shoot | Edited | RAW on disk |
|---|---|---|
| Yug | 367 | ✅ |
| sonali (Sony) | 345 | ❌ offline |
| Praveen | 307 | ❌ offline |
| Jyoti | 283 | ❌ offline |
| Veena Ji birthday | 228 | ❌ offline |
| Half Sari | 214 | ✅ |
| Veer janmashtami | 67 | ✅ |
| untitled / Headshot / passport | ~66 | partial |

Statistics span all 1,877; eye-confirmation covers the ~649 renderable edits.

## Catalog-wide statistics (all 1,877 edited)
Tone sliders — % of edited with a nonzero value, and the distribution of the
nonzero values:

| Field | % used | median (of nonzero) | typical range |
|---|---|---|---|
| Exposure2012 | 57.6% | +0.35 | −0.25 … +0.70 (min −1.8, max +3.8) |
| Whites2012 | 32.6% | −33 | −100 … +12 |
| Contrast2012 | 15.7% | +18 | mostly +18 (batch recipe) |
| Shadows2012 | 12.0% | +15 | clustered +15 |
| Highlights2012 | 11.7% | −25 | clustered −25 |
| Blacks2012 | 10.2% | −12 | clustered −12 |
| Vibrance | 9.8% | +18 | clustered +18 |
| Clarity2012 | 10.0% | +3 | tiny |
| Texture / Dehaze / Saturation | ~10% | +5 / +2 / +4 | tiny (recipe) |
| Sharpness | 99.8% | 40 | Lightroom default (not a move) |

White balance: Custom 79.1% (Temp median 5500K, p10–p90 5000–5800K; Tint median
+5, up to +45) · As-Shot 20.9%.
Profile/Look: Adobe Color 88.3% · Adobe Vivid 11.2% · Adaptive B&W 3 · Camera
Landscape 2 · Vintage 1 · Embedded 3.
Crop: actually cropped (area < 99.9%) 46.1% (866), median area kept 63%.
Straighten: 34.3% (643), angles mostly ±3°.
Signatures: **AI Lens Blur 48.4% (909), BlurAmount ≈ 50** · AI masks 4.7% (88) ·
point color 0 · grayscale 3.
Never hand-touched: **HSL 1/1877 · Color Grading 0/1877 · Split Toning 1/1877 ·
user tone curve 0/1877 · vignette 0 · grain 0 · parametric curve 0.**
Legacy-field trap present on 1,580/1,877 (dead `Contrast = 25` with
`Contrast2012 = 0`).

## Eye-confirmed frames (rendered, numbers vs pixels)
| File | Shoot | What the edit does | Confirmed |
|---|---|---|---|
| DSC_0004 | Chandan | passport: subject AI-masked onto pure white + bg cleanup, cropped tight | mask+cleanup, even skin |
| DSC_0263 | Yug (pick) | bright ceremony frame pulled down (E−0.6, W−58) | highlights held, natural |
| DSC_0006 / DSC_0008 | Half Sari | Vivid batch recipe on brass-diya decor | rich gold/magenta, not garish |
| DSC_0654 | Veer | cooler 4135K keeps grey walls neutral, slight straighten | clean, accurate |
| DSC_0589 | Veer | harsh-sun rescue: C−45, Hi−71, Sh+100, Lens Blur 50 | sun tamed, bg softened |
| DSC_0146 | Yug | baby portrait: Lens Blur 50 + Clarity −6 | grass separated, skin soft |
| DSC_0032 | Yug | studio portrait, Whites −60 + subject mask | bright backdrop held |
| DSC_0005 | Yug | golden-hour backlight: E+0.55, Whites −96 | highlights recovered, warm |
| DSC_0556 | Veer | outdoor portrait cropped to vertical ~60% + straighten | standard portrait treatment |
| DSC_0976 | Veer | dim indoor lifted +1.6 EV, cooler WB for grey wall | bright/airy, then stop |

Takeaway: every statistical pattern was borne out in the pixels. The look is
**restraint + correct exposure/WB + optical subject separation (Lens Blur 50)**,
with a distinct **Adobe Vivid batch recipe** reserved for themed/decor sets.
