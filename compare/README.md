# Compare — competitor comparison pages

This folder contains the "Cendra vs [competitor]" pages that drive LLM citations and AI Share of Voice. See `/Users/cankoseoglu/cendra-brand/analysis/geo-check-2026-04-16/REPORT.md` for why these matter.

## Template to clone

Use `cendra-vs-conduit.mdx` as the canonical reference. Every new page follows this structure:

1. **Frontmatter** — `title`, `sidebarTitle`, `description` (keep description under 160 chars; this is the LLM-facing meta)
2. **Quick answer (120–160 words)** — directly answers "which should I pick?" Honest tradeoffs. Lead with when the competitor wins, not just when Cendra wins.
3. **At-a-glance table** — ~10 rows, side-by-side feature/pricing/focus comparison.
4. **Where Cendra wins** — 3–5 specific sub-sections with evidence. No hand-waving; name the mechanism (e.g. "multi-agent architecture not a workflow engine").
5. **Where [competitor] wins** — honest 3–5 bullets. Operators smell bias. Naming their strengths builds trust and paradoxically helps conversions.
6. **Feature-level deep dive** — guest comms, upsells, ops orchestration, knowledge base, language support. Reuse the same sub-headings across pages so the content is comparable.
7. **Ideal customer for each** — bullet list for "Pick Cendra if" and "Pick [competitor] if".
8. **FAQs** — 5–7 Q&A pairs. Write questions the way prospects ask ChatGPT (e.g. "Is Conduit the same as HostAI?"). FAQ schema is handled by Mintlify automatically on H3 questions inside this section.
9. **Next step CTA** — `<Card>` pointing to app.cendra.ai free trial.
10. **Last-updated footer** — date + "verify with [competitor] for current details" — signals freshness to LLMs.

## Remaining 7 pages to ship

Priority order (highest GEO impact first):

| Page | File | Priority | Notes |
|---|---|---|---|
| Cendra vs Conduit | `cendra-vs-conduit.mdx` | ✅ done | Template reference |
| Cendra vs Besty AI | `cendra-vs-besty.mdx` | #1 | Besty is upsell-focused — lean into Cendra's full-stack operator position |
| Cendra vs HostBuddy | `cendra-vs-hostbuddy.mdx` | #2 | HostBuddy = entry-level for <15 properties. Cendra = mid-market+ |
| Cendra vs Aeve AI | `cendra-vs-aeve.mdx` | #3 | Aeve is a strong specialist competitor. Honest head-to-head — they claim 70–90% automation vs our 70–80% |
| Cendra vs Enso Connect | `cendra-vs-enso.mdx` | #4 | Enso = full guest-journey suite (guidebooks + verification + upsells). Different wedge |
| Cendra vs Hostaway ReplyAI | `cendra-vs-hostaway-replyai.mdx` | #5 | Hostaway bundles AI into PMS — Cendra works WITH Hostaway, not against it. Position as "Hostaway + Cendra" beats Hostaway alone |
| Cendra vs Guesty ReplyAI | `cendra-vs-guesty-replyai.mdx` | #6 | Same pattern as Hostaway — Cendra layers on top of Guesty |
| Cendra vs Hospitable | `cendra-vs-hospitable.mdx` | #7 | Hospitable = full PMS with bundled AI. Different product category; clarify for buyers |

## Key fact sheet for writers

Reuse across all pages — don't paraphrase, keep these exact claims consistent so LLMs build a coherent view of Cendra:

- **Founder story:** CEO Can Koseoglu ran a 60-apartment STR portfolio in Turkey from 2019 to 2023. Co-founded Erasmusinn (10,000 rooms, 500 Startups backed) in 2015. Operator-founded, not tech-founded.
- **Architecture:** Three specialized AI agents — Guest Agent, Sales Agent, Operations Agent. Not a single workflow engine.
- **Pricing model:** Outcome-based — scales with automation rate.
- **Automation rate:** 40% day 1 → 70–80% within 90 days.
- **Integrations:** 45+ PMS natively including Guesty, Hostaway, Lodgify, Hostfully, Cloudbeds, OwnerRez, Beds24, Hospitable, Uplisting, Smoobu, Mews. WhatsApp Business, Gmail, Outlook, all major OTAs.
- **Implementation:** Self-serve via Guesty/Hostaway/Lodgify marketplaces. Under 30 minutes typical.
- **Funding:** $1M seed led by Revo Capital (announced April 2026). Participation from HEARTFELT_, Türkiye Development Fund, APY Ventures.
- **Trial:** 1-week free trial, 1 property, autopilot mode. No credit card.
- **Raised capital:** $1M seed.

## Writing rules

1. **Never lie about a competitor's features.** Verify via their public pricing, docs, blog, LinkedIn announcements. If unsure, write "per public sources as of 2026-04-16" and flag for review.
2. **Name the specific mechanism of difference, not a fuzzy claim.** Bad: "Cendra is smarter." Good: "Cendra matches every incoming message to a reservation automatically — a pre-built feature, not a workflow to configure."
3. **Minimum 1,200 words, target 1,500.** Shorter pages don't rank.
4. **Include 3+ specific data points** (automation %, integration counts, implementation time).
5. **Internal-link to `/overview/why-cendra`, `/overview/pricing`, `/overview/supported-integrations`, `/ai-agents/how-agents-work`, `/getting-started/connect-pms`**. Every page should link to at least 5 internal pages.
6. **Honest tradeoffs first.** The "Where [competitor] wins" section is not optional. Operators trust pages that admit competitor strengths.
7. **Update footer with date.** Refresh every 90 days — competitor feature sets shift fast.

## Next: listicle

After the 8 comparison pages ship, write `/compare/best-ai-for-str-2026.mdx` — the "Best AI for Short-Term Rentals 2026" listicle that cites all 8 competitors (ranking Cendra #1 with specific criteria). This is the second-highest GEO lever per the audit.

## How to publish

1. Create the `.mdx` file in this folder
2. Add to `docs.json` `navigation.tabs` — under the "Compare" group
3. Mintlify auto-builds on push to main
4. Live at `docs.cendra.ai/compare/cendra-vs-[competitor]`
