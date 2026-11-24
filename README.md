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

1. Read `MIGRATION.md` and do the one-time setup it describes (install the
   Claude Bridge for Lightroom + this repo's `taste-transfer` skill).
2. Confirm the bridge is live (`mcp__lightroom__ping`).
3. Ask the user to **select ~5–10 of their finished reference edits** in
   Lightroom (varied scenes help).
4. Invoke the `taste-transfer` skill and follow it: extract each edit, judge it
   by eye, and synthesize the patterns into `profile/user-taste.md`.
5. Commit the updated `profile/` and push. That profile is the deliverable.

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
