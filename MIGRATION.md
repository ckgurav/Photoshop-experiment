# MIGRATION — step by step

This is the manifest for moving the "learn my style" capability to the laptop
that holds your reference edits, running it there, and bringing the learned
profile back.

- **Laptop A** = this one (where the package was built; where you'll edit new
  shoots and want the "final product").
- **Laptop B** = the MacBook Pro that holds your past Lightroom **example edits**.

The only thing that travels is this small repo (markdown). Your catalogs and RAWs
stay on Laptop B.

---

## STEP 1 — Laptop A: push this repo to git

A local git repo has already been committed for you at `~/lrc-taste-transfer`.
Create an **empty** repo on your git host (GitHub, private), then:

```sh
cd ~/lrc-taste-transfer
git remote add origin <YOUR_EMPTY_REPO_URL>     # e.g. git@github.com:you/lrc-taste-transfer.git
git branch -M main
git push -u origin main
```

(Make it **private** — it will hold your personal editing style, and later your
profile.)

---

## STEP 2 — Laptop B: clone it

```sh
cd ~
git clone <YOUR_REPO_URL> lrc-taste-transfer
```

---

## STEP 3 — Laptop B: install the Claude Bridge for Lightroom

This is the third-party plug-in that lets Claude read your catalog's Develop
settings (source-reviewed on Laptop A; pin the **same v2.1.0** release). Needs
`uv` and Claude Code installed.

```sh
# 1. Get the bridge (same release we vetted)
cd ~
git clone https://github.com/aslanoov/lightroom-ai-bridge
cd lightroom-ai-bridge && git checkout v2.1.0    # or download the v2.1.0 release

# 2. Install the Lightroom plug-in into the Modules folder
uv run --directory ~/lightroom-ai-bridge lrc install-plugin

# 3. Restart Lightroom Classic so it loads the plug-in (watch for the
#    "Claude Bridge running" bezel). If LrC was open, fully quit and reopen.

# 4. Verify the socket handshake
uv run --directory ~/lightroom-ai-bridge lrc ping     # expect {"ok": true, ...}

# 5. Register it as an MCP server for Claude Code (user scope)
claude mcp add lightroom --scope user -- uv run --directory ~/lightroom-ai-bridge lightroom-bridge
```

If `ping` fails: in Lightroom, File → Plug-in Extras → Claude Bridge: Start /
Restart, then re-run step 4.

---

## STEP 4 — Laptop B: install the skills

```sh
# The bridge's own skills (editing + the pro one-shot editor)
~/lightroom-ai-bridge/skills/install.sh

# This repo's taste-transfer skill
ln -s ~/lrc-taste-transfer/skills/taste-transfer ~/.claude/skills/taste-transfer
```

(Use `cp -R` instead of `ln -s` if you prefer a copy over a symlink.)

---

## STEP 5 — Laptop B: learn your style

1. Open Claude Code in `~/lrc-taste-transfer` and say:
   **"Read README.md and get ready."** It will confirm the bridge and the setup.
2. In Lightroom, **select 5–10 of your finished reference edits** (Library grid,
   cmd-click). Mix scene types — bright/dim, portrait/group, a detail shot.
3. Tell Claude: **"Run taste-transfer on my selection."**
   It reads each edit, renders it, writes a record per photo under
   `profile/examples/`, and synthesizes `profile/user-taste.md`.
4. Read `profile/user-taste.md`. Correct anything that's wrong — it's plain
   English. Re-select more examples and re-run to raise confidence; the profile
   improves every pass.

---

## STEP 6 — Laptop B: push the learned profile back

```sh
cd ~/lrc-taste-transfer
git add profile
git commit -m "Learned taste profile from reference edits"
git push
```

---

## STEP 7 — Laptop A: pull the profile and produce

```sh
cd ~/lrc-taste-transfer
git pull
```

Now `profile/user-taste.md` is here. To edit new shoots to your style, point
Claude at them and have it apply the profile through the bridge, leaving the
edits **in the Lightroom catalog for you to review** (not auto-exported). To wire
the profile into the bridge's pro editor automatically, copy it into the bridge's
knowledge base:

```sh
cp ~/lrc-taste-transfer/profile/user-taste.md ~/lightroom-ai-bridge/kb/styles/user-taste.md
```

Then `/lightroom-bridge-pro` on a selected photo will edit to your taste.

---

## Round-trip

It's bidirectional. When you review edits on Laptop A and fix things, those
corrections are new training signal — note them, update `profile/`, and push.
Laptop B (or any machine) pulls the improved profile. The style converges over
time instead of living in one person's head.

## What is NOT in this package (and why)

- **Your catalogs / RAWs** — too big and unnecessary; the profile is what's
  portable.
- **The Python editing engine / pipeline** — that lives on Laptop A for the
  *Produce* stage. Laptop B only needs the bridge to *read* edits for learning.
- **Secrets / tokens** — none. The bridge auth token is generated locally per
  machine in `~/.claude-lrc-bridge/`.
