---
name: aoe-faq
description: >-
  Answers internal questions about AOE (Agentic Outcome Engineer) engagements, EMA/EDS migration scope,
  partner boundaries, AO credits, commercial model, scoping terminology, and delivery expectations using
  the approved FAQ corpus on Confluence (canonical) or the synced aoe-faq.md in this folder. Use when the
  user asks about AOE fit, timeline, team size, GitHub handover, partner vs Adobe scope, self-service vs
  Adobe-led migration, credits, sales comp, templates/block variants, search in scope, custom blocks,
  methodology, or related acronyms (AOE, EMA, EDS). Always try Adobe Wiki / Confluence MCP first
  (get_wiki_content on the canonical AOE FAQ URL); on any MCP failure (unavailable, error, timeout, auth),
  fall back to aoe-faq.md and tell the user answers are from the local copy and may be stale. When the user
  asks to sync the wiki to the local repo (e.g. update aoe-faq.md if the wiki changed), use wiki MCP only
  (Easy MCP credentials in Cursor). When the user asks to post to Slack (e.g. #aoe-ise-internal), answer from
  wiki first, then use Slack MCP if configured; see README and docs/aoe-faq-slack-mcp-setup.md. Do not invent
  commercial or scope claims beyond the approved FAQ.
---

# AOE FAQ (internal)

## Source of truth (mandatory)

- **Canonical FAQ:** [AOE FAQ — Confluence (AEMSites)](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ). **Edits happen only on the wiki** (governance: Dave Fink and Charity; see `docs/aoe-faq-implementation-status-and-next-steps.md`).
- **Synced copy:** `aoe-faq.md` in this folder is updated **from** the wiki for Cursor, diffs, and offline use. It is **not** the master document.
- **Repo sync (maintainers):** When the user asks to refer to the wiki and sync the local repo (e.g. *“please refer the wiki and sync `aoe-faq.md` if there are any changes”*): use **Adobe Wiki / Confluence MCP** with the canonical [AOE FAQ](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ) URL, compare to `.cursor/skills/aoe-faq/aoe-faq.md`, and **update that file only if content differs** (normalize to clean Markdown). Uses Cursor’s wiki MCP configuration (`mcp.json` / Easy MCP). After editing, remind them to review the diff and commit or PR.

## Retrieval order (mandatory)

For **every** FAQ-style question (not only when the user says “check the wiki”):

1. **First:** Call **Adobe Wiki / Confluence MCP** (e.g. `get_wiki_content`) with the canonical FAQ URL: [AOE FAQ — AEMSites](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ). Base the answer on that body when the call succeeds. If the returned content indicates the page is in **Draft** (or similar), mention that when answering.
2. **Fallback:** If the tool is missing, returns an error, times out, or auth fails, read **`aoe-faq.md`** in this folder. **Tell the user explicitly** that the reply uses the **local synced file** because wiki MCP was unavailable or failed, and that content may be **stale** or differ from Confluence until MCP works or the file is re-synced.

Do **not** skip step 1 when MCP tools are present in the session—attempt the fetch first unless the user only asked for a local-file operation (e.g. “diff my `aoe-faq.md`”).

## Before answering

1. Complete the **Retrieval order** steps above.
2. **Match by intent**, not exact question text. Map to the closest FAQ section(s).
3. If **no section fits**, say so and suggest an AOE lead or a wiki FAQ update—do **not** extrapolate sensitive commercial, legal, or customer-specific details.

## How to respond

- **Stay faithful** to whichever source you used (wiki after a successful MCP fetch, or `aoe-faq.md` on fallback). You may **shorten**, **restructure**, or **combine** answers if meaning stays aligned with approved text.
- If only part of an answer applies, give the **relevant excerpt** instead of dumping the whole section.
- Prefer **plain language** and **acronym expansion** from §24 when first using AOE, EMA, or EDS.
- For **customer-specific** scenarios, keep replies **generic** and defer specifics to humans.

## Audience

- **Internal use only.** Do not present answers as customer-facing legal or contractual commitments unless governance approves external wording.

## Slack MCP (optional — aoe-faq project)

This project may configure the [Adobe Slack MCP](https://github.com/Adobe-AIFoundations/adobe-mcp-servers) so FAQ answers can be shared in the team channel **#aoe-ise-internal** (bot app name example: `aoe-faq`). Setup: `docs/aoe-faq-slack-mcp-setup.md` and [Cursor.ai - Adobe Slack MCP Setup](https://wiki.corp.adobe.com/pages/viewpage.action?pageId=3513057951&spaceKey=BPS&title=Cursor.ai%2B-%2BAdobe%2BSlack%2BMCP%2BSetup) (BPS wiki).

### When to use Slack MCP

- User explicitly asks to **post**, **share**, or **send** content to Slack (for example `#aoe-ise-internal`).
- User combines FAQ/wiki with Slack in one request (for example *“@aoe-faq … post 3 bullets to #aoe-ise-internal”*).

Do **not** post to Slack without a clear user request.

### Wiki + Slack workflow (mandatory order)

1. Complete **Retrieval order** (wiki MCP first; `aoe-faq.md` fallback with notice).
2. Draft the reply from approved sources only. You may shorten or bulletize for Slack; **do not** add commercial or scope claims not in the wiki/FAQ.
3. If the wiki page is **Draft**, say so in the Slack message when posting summaries.
4. Call **Slack MCP** `slack_post_message` with the channel ID for `#aoe-ise-internal` (resolve via `slack_list_channels` if unknown; project example channel ID is documented in `docs/aoe-faq-slack-mcp-setup.md`).
5. Confirm to the user with the posted text and a Slack permalink when the API returns success.
6. If Slack fails (`not_in_channel`, `missing_scope`), explain the fix using `docs/aoe-faq-slack-mcp-setup.md` (invite bot, bot vs user scopes, reinstall).

### Slack message guidelines

- **Internal team** tone; no customer PII or deal-specific terms.
- Prefer short bullets for FAQ summaries; include the **Confluence source URL** when posting wiki-derived content.
- Optional footer: *Ask @aoe-faq in Cursor for full approved FAQ wording.*
- Simple announcements (for example team sync) may be posted as-is when the user requests—no wiki fetch required unless they also ask for FAQ content.

### Slack-only requests

If the user only asks to post a message (no FAQ question), use Slack MCP only—do not fabricate FAQ content.

## Out of scope for this skill

- Step-by-step **block authoring** or **site code** (use project Edge Delivery patterns and official `aem.live` docs).
- **Customer identifiers**, deal-specific pricing, or terms not in the FAQ.
- **Easy MCP / Cursor / Slack app setup** — point people to `.cursor/skills/aoe-faq/README.md`, [Cursor integration with Easy MCP](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP), and `docs/aoe-faq-slack-mcp-setup.md`.

## Related project docs

- Strategy: `docs/aoe-faq-skill-strategy.md`
- Status / next steps: `docs/aoe-faq-implementation-status-and-next-steps.md`
- Slack MCP setup (aoe-faq project): `docs/aoe-faq-slack-mcp-setup.md`

## Pattern reference (structure only)

Same general shape as Adobe’s Edge Delivery **docs-search** skill ([SKILL.md](https://github.com/adobe/skills/blob/main/skills/aem/edge-delivery-services/skills/docs-search/SKILL.md)). This skill uses **Confluence + optional synced markdown**, not the aem.live doc search script.
