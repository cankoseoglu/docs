# Hostaway Connect 5-Step Walkthrough Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Embed a screenshot-driven 5-step "How to Connect Hostaway" walkthrough into `docs/integrations/hostaway.mdx`, sourced from the Cendra Hostaway Integration Guide docx.

**Architecture:** Insert a new `## How to Connect Hostaway` H2 directly under the existing "How It Works" section, rendered with Mintlify's `<Steps>` + `<Frame>` components. Optimize the 4 source PNGs and place them under a new `docs/images/hostaway/` subfolder. Add `superpowers/` to `.mintignore` so internal spec/plan docs are not published. Work happens on the `hostaway-connect-walkthrough` branch in the docs repo; no push to `main` without explicit approval (Mintlify auto-deploys from `main`).

**Tech Stack:** Mintlify (theme: mint), MDX, `<Steps>` + `<Step>` + `<Frame>` components, `pngquant` for image optimization, `npx mintlify@latest dev` for local preview.

**Spec:** `docs/superpowers/specs/2026-04-28-hostaway-connect-5-steps-design.md`

---

## File Structure

| Path | Action | Responsibility |
|------|--------|----------------|
| `docs/.mintignore` | Modify | Exclude `superpowers/` from published site |
| `docs/images/hostaway/01-signup.png` | Create | Step 1 screenshot (PMS selection page) |
| `docs/images/hostaway/02-company-details.png` | Create | Step 2 screenshot (Auth0 Create Account) |
| `docs/images/hostaway/03-choose-hostaway.png` | Create | Step 3 screenshot ("Tell us about yourself") |
| `docs/images/hostaway/04-enter-api-key.png` | Create | Step 5 screenshot (Hostaway connect form) |
| `docs/integrations/hostaway.mdx` | Modify | Convert "Connect" bullet to anchor link; add new H2 with `<Steps>` walkthrough |

All work occurs in `/Users/efe/Desktop/BotelAI/docs/` (its own git repo). Branch: `hostaway-connect-walkthrough` (already created and contains the spec commit).

---

## Pre-flight Check (one-time, before Task 1)

Verify the working state matches assumptions before starting.

- [ ] **Step 1: Confirm working directory and branch**

Run:
```bash
cd /Users/efe/Desktop/BotelAI/docs && git rev-parse --abbrev-ref HEAD && git status --short
```

Expected:
```
hostaway-connect-walkthrough
```
(no uncommitted changes — spec is already committed)

If branch is not `hostaway-connect-walkthrough`, run: `git checkout hostaway-connect-walkthrough`

- [ ] **Step 2: Confirm extracted source PNGs exist**

Run:
```bash
ls -la /tmp/hostaway_docx_extract/image{1,2,3,4}.png
```

Expected: 4 files listed, sizes roughly: image1 ~1.9MB, image2 ~470KB, image3 ~2MB, image4 ~1.6MB.

If missing, re-extract from the docx:
```bash
python3 -c "
import zipfile, os, shutil
src = '/Users/efe/Downloads/Cendra & Hostaway Integration Guide.docx'
out = '/tmp/hostaway_docx_extract'
os.makedirs(out, exist_ok=True)
with zipfile.ZipFile(src) as z:
    for n in sorted(n for n in z.namelist() if n.startswith('word/media/')):
        with z.open(n) as f, open(os.path.join(out, os.path.basename(n)), 'wb') as g:
            shutil.copyfileobj(f, g)
print('done')
"
```

- [ ] **Step 3: Install `pngquant` if not present**

Run:
```bash
which pngquant || brew install pngquant
```

Expected: a path like `/opt/homebrew/bin/pngquant`, or fresh brew install completing successfully.

Verify:
```bash
pngquant --version
```

Expected: `2.x.x` or similar.

---

## Task 1: Add `superpowers/` to `.mintignore`

**Files:**
- Modify: `docs/.mintignore`

This excludes the internal `superpowers/` directory (specs + plans) from the published Mintlify site so commits to `main` don't accidentally publish design docs.

- [ ] **Step 1: Read current `.mintignore`**

Run:
```bash
cat /Users/efe/Desktop/BotelAI/docs/.mintignore
```

Expected current content:
```
# Mintlify automatically ignores these files and directories:
# .git, .github, .claude, .agents, .idea, node_modules,
# README.md, LICENSE.md, CHANGELOG.md, CONTRIBUTING.md

# Draft content
drafts/
*.draft.mdx
```

- [ ] **Step 2: Append `superpowers/` exclusion block**

Use the Edit tool to append to `/Users/efe/Desktop/BotelAI/docs/.mintignore`. Add a blank line then a new block at the end of the file. Final content should be:

```
# Mintlify automatically ignores these files and directories:
# .git, .github, .claude, .agents, .idea, node_modules,
# README.md, LICENSE.md, CHANGELOG.md, CONTRIBUTING.md

# Draft content
drafts/
*.draft.mdx

# Internal design specs and implementation plans (not for publishing)
superpowers/
```

- [ ] **Step 3: Verify the file**

Run:
```bash
tail -3 /Users/efe/Desktop/BotelAI/docs/.mintignore
```

Expected:
```
# Internal design specs and implementation plans (not for publishing)
superpowers/
```

- [ ] **Step 4: Commit**

```bash
cd /Users/efe/Desktop/BotelAI/docs
git add .mintignore
git commit -m "$(cat <<'EOF'
chore(docs): exclude internal superpowers/ folder from published site

Spec and implementation plan files live under superpowers/ and should not
be rendered as Mintlify pages.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

Expected: `[hostaway-connect-walkthrough <hash>] chore(docs): exclude internal superpowers/ folder from published site` with `1 file changed`.

---

## Task 2: Add and optimize Hostaway screenshots

**Files:**
- Create: `docs/images/hostaway/01-signup.png`
- Create: `docs/images/hostaway/02-company-details.png`
- Create: `docs/images/hostaway/03-choose-hostaway.png`
- Create: `docs/images/hostaway/04-enter-api-key.png`

The 4 source PNGs from the docx are large (~6 MB total). `pngquant` produces near-lossless ~2 MB output that's perceptually identical and renders much faster on the live site.

- [ ] **Step 1: Create destination folder**

Run:
```bash
mkdir -p /Users/efe/Desktop/BotelAI/docs/images/hostaway
```

Verify:
```bash
ls -d /Users/efe/Desktop/BotelAI/docs/images/hostaway
```

Expected: the path is listed.

- [ ] **Step 2: Optimize and copy each image with semantic filename**

`pngquant` flags used:
- `--quality 80-95` — target quality range (0–100)
- `--strip` — remove metadata
- `--force` — overwrite output if it exists
- `--output <file>` — explicit output path

Run all four optimizations:
```bash
DEST=/Users/efe/Desktop/BotelAI/docs/images/hostaway
SRC=/tmp/hostaway_docx_extract

pngquant --quality 80-95 --strip --force --output "$DEST/01-signup.png"           "$SRC/image1.png"
pngquant --quality 80-95 --strip --force --output "$DEST/02-company-details.png"  "$SRC/image2.png"
pngquant --quality 80-95 --strip --force --output "$DEST/03-choose-hostaway.png"  "$SRC/image3.png"
pngquant --quality 80-95 --strip --force --output "$DEST/04-enter-api-key.png"    "$SRC/image4.png"
```

Expected: no errors, no output (pngquant is silent on success).

If pngquant fails on an image with "image too small or low quality" or similar, re-run that file with a wider quality range:
```bash
pngquant --quality 65-95 --strip --force --output "$DEST/<file>.png" "$SRC/<source>.png"
```

- [ ] **Step 3: Verify all 4 files exist with reduced size**

Run:
```bash
ls -lh /Users/efe/Desktop/BotelAI/docs/images/hostaway/
```

Expected: 4 PNG files, each significantly smaller than its source. Combined size should be roughly 1.5–3 MB (down from ~6 MB).

- [ ] **Step 4: Visual sanity check (manual)**

Open each file and confirm it shows the correct content:
```bash
open /Users/efe/Desktop/BotelAI/docs/images/hostaway/01-signup.png
open /Users/efe/Desktop/BotelAI/docs/images/hostaway/02-company-details.png
open /Users/efe/Desktop/BotelAI/docs/images/hostaway/03-choose-hostaway.png
open /Users/efe/Desktop/BotelAI/docs/images/hostaway/04-enter-api-key.png
```

Expected:
- `01-signup.png` → Cendra "Select your PMS to get started" page
- `02-company-details.png` → Auth0 "Create Your Account" form
- `03-choose-hostaway.png` → Cendra "Tell us about yourself" form
- `04-enter-api-key.png` → Hostaway "Securely connect your Hostaway account" form with API Token field

(Note: filenames follow the docx authoring order — see spec section "Screenshot-to-step mapping".)

If any image is wrong, re-check the source mapping in the spec and rerun Step 2 with the corrected source.

- [ ] **Step 5: Commit**

```bash
cd /Users/efe/Desktop/BotelAI/docs
git add images/hostaway/
git commit -m "$(cat <<'EOF'
docs(hostaway): add 5-step Connect walkthrough screenshots

Optimized PNGs (pngquant, near-lossless) for the upcoming
"How to Connect Hostaway" walkthrough on the Hostaway integration page.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

Expected: `[hostaway-connect-walkthrough <hash>] docs(hostaway): add 5-step Connect walkthrough screenshots` with `4 files changed`.

---

## Task 3: Update `hostaway.mdx` — anchor link + new walkthrough section

**Files:**
- Modify: `docs/integrations/hostaway.mdx`

Two edits to the same file in one commit:
1. Convert the "Connect" item in "How It Works" into an anchor link to the new section.
2. Insert the new `## How to Connect Hostaway` H2 with `<Steps>` between "How It Works" and "What Cendra Can Do With Hostaway Data".

- [ ] **Step 1: Read the current page**

Use the Read tool on `/Users/efe/Desktop/BotelAI/docs/integrations/hostaway.mdx`. Confirm the file matches what the spec describes (44+ lines, includes `## How It Works` block with 4 numbered items and `## What Cendra Can Do With Hostaway Data` after it).

- [ ] **Step 2: Edit 1 — convert "Connect" bullet into an anchor link**

Use the Edit tool on `/Users/efe/Desktop/BotelAI/docs/integrations/hostaway.mdx`.

`old_string`:
```
1. **Connect** — Enter your Hostaway API credentials in Cendra's onboarding flow
```

`new_string`:
```
1. **[Connect](#how-to-connect-hostaway)** — Enter your Hostaway API credentials in Cendra's onboarding flow
```

- [ ] **Step 3: Edit 2 — insert new `## How to Connect Hostaway` section**

Use the Edit tool on `/Users/efe/Desktop/BotelAI/docs/integrations/hostaway.mdx` to insert the new H2 before `## What Cendra Can Do With Hostaway Data`.

`old_string`:
```
## What Cendra Can Do With Hostaway Data
```

`new_string`:
```
## How to Connect Hostaway

<Steps>
  <Step title="Sign Up to Cendra">
    Go to [app.cendra.ai](https://app.cendra.ai) to create your account.
    <Frame>
      <img src="/images/hostaway/01-signup.png" alt="Cendra PMS selection screen" />
    </Frame>
  </Step>

  <Step title="Fill Out Your Company Details">
    Enter your company name, email, and phone number to set up your workspace.
    <Frame>
      <img src="/images/hostaway/02-company-details.png" alt="Cendra account creation screen" />
    </Frame>
  </Step>

  <Step title="Choose Hostaway as Your PMS">
    Select Hostaway from the list of supported PMS providers.
    <Frame>
      <img src="/images/hostaway/03-choose-hostaway.png" alt="Cendra account setup form" />
    </Frame>
  </Step>

  <Step title="Get Your API Key from Hostaway">
    In your Hostaway dashboard, search **"Cendra"** in the marketplace and click **Connect** to generate your API key.
  </Step>

  <Step title="Enter Your Hostaway API Key">
    Paste your API key into Cendra's Hostaway connect form and click **Verify & Connect** to start the integration.
    <Frame>
      <img src="/images/hostaway/04-enter-api-key.png" alt="Hostaway connect form in Cendra" />
    </Frame>
  </Step>
</Steps>

## What Cendra Can Do With Hostaway Data
```

- [ ] **Step 4: Verify both edits via diff**

Run:
```bash
cd /Users/efe/Desktop/BotelAI/docs
git diff integrations/hostaway.mdx
```

Expected diff includes:
- One line removed: `1. **Connect** — Enter your Hostaway API credentials in Cendra's onboarding flow`
- One line added: `1. **[Connect](#how-to-connect-hostaway)** — Enter your Hostaway API credentials in Cendra's onboarding flow`
- A large block added before `## What Cendra Can Do With Hostaway Data` containing `## How to Connect Hostaway` and the `<Steps>` markup.

- [ ] **Step 5: Sanity check — anchor target matches H2 slug**

Mintlify generates the anchor slug by lowercasing the heading and replacing spaces with hyphens. `## How to Connect Hostaway` → `#how-to-connect-hostaway`. Confirm by inspecting the diff that:
- Anchor in bullet: `(#how-to-connect-hostaway)`
- New H2 text: `## How to Connect Hostaway`

If they don't match exactly (case, hyphens), fix one to match the other before continuing.

- [ ] **Step 6: Commit**

```bash
cd /Users/efe/Desktop/BotelAI/docs
git add integrations/hostaway.mdx
git commit -m "$(cat <<'EOF'
docs(hostaway): add 5-step Connect walkthrough section

Adds a new "How to Connect Hostaway" H2 under the existing How It Works
overview, rendered with <Steps> + <Frame> components and four
screenshots from docs/images/hostaway/. The "Connect" bullet in the
overview now anchors to this new section.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

Expected: `[hostaway-connect-walkthrough <hash>] docs(hostaway): add 5-step Connect walkthrough section` with `1 file changed`.

---

## Task 4: Local Mintlify preview verification

No commit. Manual visual + functional check. If issues, fix then return to the relevant task.

- [ ] **Step 1: Start Mintlify dev server**

```bash
cd /Users/efe/Desktop/BotelAI/docs
npx mintlify@latest dev
```

When prompted "Need to install the following packages: mintlify@..." answer **yes**.

Expected output (after install): `Preview is running at http://localhost:3000` (or similar). The server keeps running — leave it running and verify in a browser, then stop with Ctrl-C when done.

If `npx mintlify` exits with `Error: docs.json not found` or similar, confirm the working directory is `/Users/efe/Desktop/BotelAI/docs` (it must contain `docs.json`).

- [ ] **Step 2: Visit the Hostaway page**

Open in browser: <http://localhost:3000/integrations/hostaway>

- [ ] **Step 3: Visual checks**

Confirm all of:
- [ ] Page renders without an MDX parse error.
- [ ] `## How It Works` shows the original 4-bullet list, with "Connect" rendered as a link (underlined or color-distinct).
- [ ] `## How to Connect Hostaway` appears as a new H2 directly below "How It Works".
- [ ] `<Steps>` renders as a numbered, vertically-stacked list of 5 steps with the titles:
  1. Sign Up to Cendra
  2. Fill Out Your Company Details
  3. Choose Hostaway as Your PMS
  4. Get Your API Key from Hostaway
  5. Enter Your Hostaway API Key
- [ ] All 4 screenshots load (no broken-image icons). Steps 1, 2, 3, 5 each have a screenshot; Step 4 has none.
- [ ] Light theme renders cleanly. Toggle to dark theme and confirm `<Frame>` borders + screenshots still look correct.

- [ ] **Step 4: Anchor link check**

In the rendered page, click the **Connect** link in the "How It Works" list. Confirm:
- The browser scrolls down to the `## How to Connect Hostaway` heading.
- URL fragment becomes `#how-to-connect-hostaway`.

- [ ] **Step 5: Stop the dev server**

Press Ctrl-C in the terminal running `npx mintlify dev`.

- [ ] **Step 6: Final summary check**

Run:
```bash
cd /Users/efe/Desktop/BotelAI/docs
git log --oneline hostaway-connect-walkthrough ^main
git status
```

Expected log (most recent first):
```
<hash> docs(hostaway): add 5-step Connect walkthrough section
<hash> docs(hostaway): add 5-step Connect walkthrough screenshots
<hash> chore(docs): exclude internal superpowers/ folder from published site
<hash> docs(spec): add Hostaway Connect 5-step walkthrough design
```

Expected status: `nothing to commit, working tree clean`.

---

## Stop here — do not push

The branch `hostaway-connect-walkthrough` is local-only. Mintlify auto-deploys from `main`, so the user must explicitly approve the merge/push. Do **not** run `git push`, `git checkout main`, `git merge`, or any operation that could land this on `main` until the user says so.

Hand back to the user with:
- Branch name: `hostaway-connect-walkthrough`
- Commit list (from Step 6 above)
- A note that they can review the rendered preview locally any time with `npx mintlify@latest dev` from `/Users/efe/Desktop/BotelAI/docs`.
