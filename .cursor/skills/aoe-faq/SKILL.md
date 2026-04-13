---
name: aoe-faq
description: >-
  Answers internal questions about AOE (Agentic Outcome Engineer) engagements, EMA/EDS migration scope,
  partner boundaries, AO credits, commercial model, scoping terminology, and delivery expectations using
  the approved FAQ corpus on Confluence (canonical) or the synced aoe-faq.md in this folder. Use when the
  user asks about AOE fit, timeline, team size, GitHub handover, partner vs Adobe scope, self-service vs
  Adobe-led migration, credits, sales comp, templates/block variants, search in scope, custom blocks,
  methodology, or related acronyms (AOE, EMA, EDS). Prefer live wiki content when Adobe Wiki MCP is
  available; otherwise read aoe-faq.md and note possible staleness. When the user asks to sync the wiki
  to the local repo (e.g. update aoe-faq.md if the wiki changed), use wiki MCP only (Easy MCP credentials in Cursor).
  Do not invent commercial or scope claims beyond the approved FAQ.
---

# AOE FAQ (internal)

## Source of truth (mandatory)

- **Canonical FAQ:** [AOE FAQ — Confluence (AEMSites)](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ). **Edits happen only on the wiki** (governance: Dave Fink and Charity; see `docs/aoe-faq-implementation-status-and-next-steps.md`).
- **Synced copy:** `aoe-faq.md` in this folder is updated **from** the wiki for Cursor, diffs, and offline use. It is **not** the master document.
- **Repo sync (maintainers):** When the user asks to refer to the wiki and sync the local repo (e.g. *“please refer the wiki and sync `aoe-faq.md` if there are any changes”*): use **Adobe Wiki / Confluence MCP** with the canonical [AOE FAQ](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ) URL, compare to `.cursor/skills/aoe-faq/aoe-faq.md`, and **update that file only if content differs** (normalize to clean Markdown). Uses Cursor’s wiki MCP configuration (`mcp.json` / Easy MCP). After editing, remind them to review the diff and commit or PR.

## Before answering

1. **If Adobe Wiki / Confluence MCP is available:** Fetch the canonical page above and base answers on that content (latest approved text).
2. **Else:** Read `aoe-faq.md` next to this `SKILL.md`. If the user needs certainty or the wiki may have changed, say that answers come from the **last synced file** and they should confirm on Confluence or run a sync.
3. **Match by intent**, not exact question text. Map to the closest numbered section(s).
4. If **no section fits**, say so and suggest an AOE lead or a wiki FAQ update—do **not** extrapolate sensitive commercial, legal, or customer-specific details.

## How to respond

- **Stay faithful** to the wiki / synced Q&A. You may **shorten**, **restructure**, or **combine** answers if meaning stays aligned with approved text.
- If only part of an answer applies, give the **relevant excerpt** instead of dumping the whole section.
- Prefer **plain language** and **acronym expansion** from §24 when first using AOE, EMA, or EDS.
- For **customer-specific** scenarios, keep replies **generic** and defer specifics to humans.

## Audience

- **Internal use only.** Do not present answers as customer-facing legal or contractual commitments unless governance approves external wording.

## Out of scope for this skill

- Step-by-step **block authoring** or **site code** (use project Edge Delivery patterns and official `aem.live` docs).
- **Customer identifiers**, deal-specific pricing, or terms not in the FAQ.
- **Easy MCP / Cursor setup** — point people to `.cursor/skills/aoe-faq/README.md` and [Cursor integration with Easy MCP](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP).

## Related project docs

- Strategy: `docs/aoe-faq-skill-strategy.md`
- Status / next steps: `docs/aoe-faq-implementation-status-and-next-steps.md`

## Pattern reference (structure only)

Same general shape as Adobe’s Edge Delivery **docs-search** skill ([SKILL.md](https://github.com/adobe/skills/blob/main/skills/aem/edge-delivery-services/skills/docs-search/SKILL.md)). This skill uses **Confluence + optional synced markdown**, not the aem.live doc search script.
