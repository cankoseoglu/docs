# Hostaway "How to Connect" 5-Step Walkthrough — Design

**Date:** 2026-04-28
**Page:** `docs/integrations/hostaway.mdx` (Cendra Mintlify docs)
**Source material:** `/Users/efe/Downloads/Cendra & Hostaway Integration Guide.docx` (5 steps, 4 inline screenshots)

## Goal

Embed a professional, screenshot-driven 5-step "Connect Hostaway" walkthrough into the existing Hostaway integration page, immediately under the high-level "How It Works" section, without disrupting the rest of the page.

## Non-Goals

- No changes to other PMS integration pages.
- No restructure of the existing "How It Works", "What Syncs", "What Cendra Can Do", "Communication Channels", "Setup Time", or final CTA `<Card>` sections.
- No new Mintlify MCP setup. The Mintlify MCP at `https://docs.cendra.ai/mcp` is read-only against the published site and is not required for editing local `.mdx` files. It can be added later as a separate task if useful.

## Page Structure (After)

```
frontmatter            (unchanged)
intro paragraph        (unchanged)
## What Syncs From Hostaway          (unchanged)
## How It Works                      (small change — Connect bullet becomes anchor link)
## How to Connect Hostaway           (NEW H2 — 5-step <Steps> walkthrough)
## What Cendra Can Do With Hostaway Data   (unchanged)
## Communication Channels             (unchanged)
## Setup Time                         (unchanged)
<Card>...                             (unchanged)
```

## Edits to `docs/integrations/hostaway.mdx`

### Edit 1 — "Connect" bullet becomes an anchor link

In the existing `## How It Works` numbered list, item 1:

**Before:**
```mdx
1. **Connect** — Enter your Hostaway API credentials in Cendra's onboarding flow
```

**After:**
```mdx
1. **[Connect](#how-to-connect-hostaway)** — Enter your Hostaway API credentials in Cendra's onboarding flow
```

Mintlify auto-generates the anchor `#how-to-connect-hostaway` from the new H2 heading.

### Edit 2 — New `## How to Connect Hostaway` section

Inserted directly after the `## How It Works` block and before `## What Cendra Can Do With Hostaway Data`:

```mdx
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
```

Density: minimal — short title + 1 sentence + screenshot. No extra `<Note>`/`<Tip>` callouts. Step 4 is text-only (no screenshot in source).

## New Image Assets

Place under `docs/images/hostaway/` (new folder; sets a per-PMS pattern for future integrations):

| File | Source (in `.docx`) | Content |
|------|--------------------|---------|
| `01-signup.png` | `word/media/image1.png` | Cendra "Select your PMS to get started" |
| `02-company-details.png` | `word/media/image2.png` | Auth0 "Create Your Account" form |
| `03-choose-hostaway.png` | `word/media/image3.png` | Cendra "Tell us about yourself" form |
| `04-enter-api-key.png` | `word/media/image4.png` | Hostaway "Securely connect your Hostaway account" form |

**Screenshot-to-step mapping** follows the docx order as authored (decision: keep as-is, don't re-map by semantic intent).

**Optimization:** PNGs will be optimized before commit using `oxipng` (lossless) or `pngquant` (near-lossless, perceptually identical, ~50–70% smaller). Source PNGs total ~6 MB; expected post-optimization ~2 MB. Tool choice deferred to implementation step (use whichever is already available; install `oxipng` via Homebrew if neither is present).

## `.mintignore` Update

Add `superpowers/` so this spec folder is excluded from the published site:

```
# Internal design specs
superpowers/
```

## File Touch List

| Path | Action |
|---|---|
| `docs/integrations/hostaway.mdx` | Edit (anchor link + new H2 section) |
| `docs/images/hostaway/01-signup.png` | Create (optimized) |
| `docs/images/hostaway/02-company-details.png` | Create (optimized) |
| `docs/images/hostaway/03-choose-hostaway.png` | Create (optimized) |
| `docs/images/hostaway/04-enter-api-key.png` | Create (optimized) |
| `docs/.mintignore` | Edit (add `superpowers/`) |
| `docs/superpowers/specs/2026-04-28-hostaway-connect-5-steps-design.md` | Create (this spec) |

## Verification

1. **Mintlify dev preview** — run `mintlify dev` (or `npx mintlify dev`) from `docs/`. Confirm:
   - `## How to Connect Hostaway` renders as an H2.
   - `<Steps>` shows 5 numbered steps.
   - Each screenshot loads via `<Frame>` with no broken `<img>` paths.
   - The Connect bullet in "How It Works" links to the new section's anchor.
2. **Anchor link** — clicking "Connect" jumps to the new H2; URL fragment becomes `#how-to-connect-hostaway`.
3. **Visual check** — light and dark theme both render `<Frame>` cleanly.
4. **Lint / parse** — no MDX parse errors during `mintlify dev` startup.

## Risks & Considerations

- **Mintlify component naming.** `<Steps>` and `<Step title="...">` are the Mintlify standard. If the project's Mintlify version uses a different syntax, swap to the project's actual component (verify against existing `.mdx` pages during implementation).
- **Image filenames.** All-lowercase, hyphenated, prefixed with order index. Matches the project's existing `images/` naming convention (e.g., `01-agent-general.png`).
- **Anchor stability.** If the H2 title changes later, the anchor in the "Connect" bullet must change with it. Acceptable for this size of doc.
- **Screenshot freshness.** Screenshots will date as Cendra UI evolves. Acceptable for now; future PMS docs can adopt the same pattern and updates can be batched.
