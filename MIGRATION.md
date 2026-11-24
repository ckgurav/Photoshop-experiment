# MIGRATION — step by step

Move the "learn my style" capability to the laptop that holds your reference
edits, run it there, and bring the learned profile back.

- **Laptop A** = where this package was built; where you'll edit new shoots and
  want the "final product."
- **Laptop B** = the MacBook Pro that holds your past Lightroom **example edits**.

Only this small repo (markdown) travels. Your catalogs and RAWs stay on Laptop B.

---

# STEP 0 — Prepare Laptop B first (do this before anything else)

The other machine needs a full working environment before the bridge or the
skill will do anything. Set all of this up first.

### 0.1 Base tools

macOS (Apple Silicon or Intel), with your Lightroom catalogs present. Then:

```sh
# Homebrew (skip if installed): https://brew.sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install git node uv       # git, Node.js (for the MCP servers), uv (for the bridge)
```

- **Claude Code** — install from Anthropic and sign in. Verify: `claude --version`.
- **Adobe Lightroom Classic** — installed, with the catalog that holds your
  edits. Open it at least once.

Sanity check — all four should print a version:
```sh
git --version && node --version && uv --version && claude --version
```

### 0.2 MCP servers

This is the toolset Laptop A uses. Replicate the ones the learning task needs:

```sh
# open-computer-use — lets Claude SEE and drive the Lightroom app window
#   (used to set up an "all edited photos" view and scan the catalog).
npm install -g open-computer-use
claude mcp add open-computer-use -- open-computer-use mcp

# photoshop MCP — OPTIONAL, only if you also want to PRODUCE edits on Laptop B.
# Not needed just to learn your style. Skip unless you want it.
# claude mcp add photoshop -- npx -y @immersive-media-technologies/photoshop-mcp-imt@1.1.4
```

The **lightroom** bridge MCP is registered in STEP 2 (it needs the repo cloned
first). You do **not** need `acl-mcp` — that's an unrelated internal server on
Laptop A; leave it off.

### 0.3 macOS permissions for open-computer-use (critical)

open-computer-use cannot see or control Lightroom until you grant its host app
(the Terminal you run `claude` in, or the Claude Code app) both permissions:

- **System Settings → Privacy & Security → Accessibility** → enable your
  terminal / Claude Code.
- **System Settings → Privacy & Security → Screen Recording** → enable the same.

Quit and reopen the terminal/app after granting, or the permissions won't take.
Without these, the "scan the open Lightroom app" step silently does nothing.

### 0.4 Confirm the toolset

```sh
claude mcp list        # expect: open-computer-use ✔  (lightroom added in STEP 2)
```

---

# STEP 1 — Laptop B: clone this repo

```sh
cd ~
git clone https://github.com/ckgurav/Photoshop-experiment.git lrc-taste-transfer
```

---

# STEP 2 — Laptop B: install the Claude Bridge for Lightroom

The plug-in that lets Claude read your catalog's Develop settings (source-reviewed
on Laptop A; pin the **same v2.1.0**).

```sh
cd ~
git clone https://github.com/aslanoov/lightroom-ai-bridge
cd lightroom-ai-bridge && git checkout v2.1.0      # or download the v2.1.0 release

uv run --directory ~/lightroom-ai-bridge lrc install-plugin
#   → fully quit and reopen Lightroom Classic so it loads the plug-in
#     (watch for the "Claude Bridge running" bezel)

uv run --directory ~/lightroom-ai-bridge lrc ping                    # expect ok:true
claude mcp add lightroom --scope user -- uv run --directory ~/lightroom-ai-bridge lightroom-bridge
```

If `ping` fails: Lightroom → File → Plug-in Extras → Claude Bridge: Start /
Restart, then re-run `ping`.

---

# STEP 3 — Laptop B: install the skills

```sh
~/lightroom-ai-bridge/skills/install.sh                              # lightroom-bridge + -pro
ln -s ~/lrc-taste-transfer/skills/taste-transfer ~/.claude/skills/taste-transfer
```

(Use `cp -R` instead of `ln -s` for a copy rather than a symlink.)

---

# STEP 4 — Laptop B: learn your style

1. Open Claude Code in `~/lrc-taste-transfer` and say:
   **"Read README.md and get ready."** It will verify the bridge + computer-use
   are connected and the setup is complete.
2. Point it at your edits. Two ways to pick the examples to learn from:
   - **Curated (recommended to start):** in Lightroom, select **5–10 finished
     reference edits** across varied scenes (bright/dim, portrait/group, a detail
     shot). cmd-click in the grid.
   - **Scan everything:** ask Claude to scan all your edited photos. It uses
     open-computer-use to set up an "edited only" view in Lightroom (Library
     filter / smart collection with adjustments), selects them, and the bridge
     reads each one's exact settings. Better coverage; more time.
3. Say: **"Run taste-transfer on my selection."** It reads each edit, renders it,
   writes a record under `profile/examples/`, and synthesizes
   `profile/user-taste.md`.
4. Read `profile/user-taste.md`, correct anything wrong (it's plain English), and
   re-run with more examples to raise confidence. Every pass sharpens it.

---

# STEP 5 — Laptop B: push the learned profile back

```sh
cd ~/lrc-taste-transfer
git add profile && git commit -m "Learned taste profile from reference edits" && git push
```

---

# STEP 6 — Laptop A: pull the profile and produce

```sh
cd ~/lrc-taste-transfer && git pull
```

New shoots get edited to the profile and left **in the Lightroom catalog for you
to review** — not auto-exported. To wire the profile into the bridge's pro editor:

```sh
cp ~/lrc-taste-transfer/profile/user-taste.md ~/lightroom-ai-bridge/kb/styles/user-taste.md
```

Then `/lightroom-bridge-pro` on a selected photo edits to your taste.

---

## Round-trip

Bidirectional: when you review edits on Laptop A and fix things, those
corrections are new training signal — update `profile/` and push; Laptop B pulls
the sharper profile. The style converges instead of living in one head.

## What is NOT in this package (and why)

- **Your catalogs / RAWs** — too big and unnecessary; the profile is the portable
  part.
- **The Python editing engine / pipeline** — lives on Laptop A for the *Produce*
  stage. Laptop B only needs the bridge to *read* edits for learning.
- **Secrets / tokens** — none. The bridge generates its auth token locally per
  machine in `~/.claude-lrc-bridge/`.
- **`acl-mcp`** — unrelated internal server; not part of this workflow.
