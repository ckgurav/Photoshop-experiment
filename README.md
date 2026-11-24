# LRC Taste Transfer

A do-it-yourself version of Imagen-AI's "Personal AI Profile": learn a
photographer's personal editing style from their **existing Lightroom Classic
edits**, write it to a small, **readable** taste profile, and carry that profile
between machines. Unlike a black-box model, the profile is plain markdown you can
read, correct, and version in git.

## If you are Claude, read this first

You have been handed this repo on a machine that holds the user's **reference
edits** (their past finished work in Lightroom Classic). Your job is to **get
ready to learn their style**, not to make new edits. Concretely:

1. **Prepare the system first.** Work through `MIGRATION.md` **STEP 0** before
   anything else — it installs the full toolset and grants the macOS permissions
   the UI automation needs. Do not skip to the bridge; a half-prepared machine
   fails silently.
2. Then do STEP 1–3: clone, install the Claude Bridge for Lightroom (v2.1.0), and
   install this repo's `taste-transfer` skill.
3. Confirm the bridge is live (`mcp__lightroom__ping`) and `claude mcp list`
   shows both `lightroom` and `open-computer-use` connected.
4. Have the user pick examples — either **select ~5–10 finished reference edits**
   in Lightroom, or ask to **scan all edited photos** (you drive the Lightroom
   app with open-computer-use to build an "edited only" view, then the bridge
   reads each one).
5. Invoke the `taste-transfer` skill and follow it: extract each edit, judge it
   by eye, and synthesize the patterns into `profile/user-taste.md`.
6. Commit the updated `profile/` and push. That profile is the deliverable.

### Toolset this machine must have (full list in MIGRATION.md STEP 0)

- **Base:** macOS, Adobe Lightroom Classic (with the user's catalogs), Claude
  Code, git, Node.js, uv.
- **MCP servers:** `lightroom` (the Claude Bridge — reads catalog edits) and
  `open-computer-use` (sees/drives the Lightroom app window). `photoshop` is
  optional, only for producing edits here. `acl-mcp` is NOT needed.
- **macOS permissions:** open-computer-use needs Accessibility **and** Screen
  Recording granted to the terminal / Claude Code app, or it can't scan the app.
- **Skills:** this repo's `taste-transfer`, plus the bridge's `lightroom-bridge`
  and `lightroom-bridge-pro`.

Do **not** reset, overwrite, or re-edit the user's reference photos. You are
reading their edits, not changing them. Everything you do through the bridge is
non-destructive, but you still must not disturb their finished work.

## The three stages (and which machine runs each)

| Stage | Machine | What happens | Output |
|---|---|---|---|
| 1. Train | the laptop with the example edits | read each edit, diff vs. baseline, render, articulate what/why | per-example records |
| 2. Profile | same | synthesize the repeating patterns into rules | `profile/user-taste.md` |
| 3. Produce | the laptop where new shoots are edited | apply the profile to new photos, leave them in the catalog for review | reviewable edits |

The **catalogs and RAWs never move** — only the small `profile/` folder travels
(via git). That is the whole point: the learned *style* is portable, the gigabytes
are not.

## What's in here

- `MIGRATION.md` — the human step-by-step for both laptops (the manifest).
- `skills/taste-transfer/SKILL.md` — the extraction + synthesis procedure.
- `profile/user-taste.md` — the living taste profile (seeded, grows over time).
- `profile/examples/` — one record per analyzed edit (the evidence behind the profile).

## Interpretation of the plan (correct me if wrong)

Learning happens where the examples are; the profile comes back to where new work
is edited. If instead you want to *produce* finished edits on this same machine,
that works too — the bridge + `lightroom-bridge-pro` skill can apply the profile
here. The profile is the same either way.
