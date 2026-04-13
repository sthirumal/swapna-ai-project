# AOE FAQ — Cursor project skill

Internal-only FAQ for **AOE / EMA / EDS** engagement questions. The agent follows **`SKILL.md`**. Answers must match the **approved FAQ** (see source of truth below).

---

## Canonical source of truth (wiki)

**All FAQ wording is authored and maintained in Confluence—not in git.**

| Role                         | Location                                                                                   |
| ---------------------------- | ------------------------------------------------------------------------------------------ |
| **Master FAQ (edit here)**   | [AOE FAQ — AEMSites](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ) |
| **Synced copy in this repo** | `aoe-faq.md` (for `@aoe-faq`, PR review diffs, offline use)                                |

**Governance:** Dave Fink and Charity (see `docs/aoe-faq-implementation-status-and-next-steps.md`).

---

## Syncing `aoe-faq.md` from the wiki

After the wiki page is updated, a **maintainer** refreshes `aoe-faq.md` so the repo matches Confluence.

Sync is done **in Cursor** using **Easy MCP** and the **Adobe Wiki / Confluence** MCP (credentials in your Cursor MCP config, e.g. `mcp.json`). There is **no** separate CLI or env file in this repo for sync.

**Example prompt** (adjust wording as you like):

> Please refer to the wiki and sync the local repo `aoe-faq.md` if there are any changes.

The agent should:

1. Use **wiki MCP** to fetch the canonical page: [AOE FAQ — AEMSites](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ).
2. Compare with `.cursor/skills/aoe-faq/aoe-faq.md`.
3. **Update the file only if** the wiki content differs (normalize to readable Markdown; keep or restore the HTML header comment that points to the wiki).
4. Tell you to **review the diff** and commit or open a PR.

Use **`@aoe-faq`** so the agent loads this skill’s rules for that workflow.

**Requirement:** wiki MCP must be connected and authorized in Cursor. If MCP is unavailable, use the fallback below.

### Fallback (no MCP)

- **Confluence UI:** export or copy the page body into `aoe-faq.md`, preserving structure.

Do **not** invent FAQ answers in git—**wiki first**, then sync.

---

## How to use in Cursor

1. Open this repository in Cursor.
2. Enable **Easy MCP** and the **Adobe Wiki / Confluence** MCP so the agent can read the live wiki when needed (see below).
3. In chat, use **`@aoe-faq`** (or ask the agent to follow the **aoe-faq** project skill).
4. **With wiki MCP:** Prefer fetching the [AOE FAQ](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ) page for the latest text when answering.
5. **Without wiki MCP:** Use `aoe-faq.md`; note it may be **stale** until the next sync.

---

## Cursor integration with Easy MCP (wiki access for everyone)

Other teammates need **Easy MCP** so Cursor can call internal tools such as the **Adobe Wiki Confluence** MCP (read wiki pages, search, etc.).

**Authoritative setup (screenshots, versions, troubleshooting):**

**[Cursor integration with Easy MCP](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP)**  
(Confluence — **assetscollab** space; Adobe corporate sign-in required.)

**Summary checklist** (details are on that page—follow the wiki, not this list alone):

1. **Prerequisites** — Corporate network/VPN if required, Adobe Confluence access, supported Cursor build per the wiki.
2. **Install / enable Easy MCP** — As documented on the wiki (per-OS steps if provided).
3. **Add the Adobe Wiki Confluence MCP** — So tools like **get wiki content** / search are available to the agent.
4. **Verify** — In Cursor, confirm the MCP server is connected (no auth errors); ask the agent to fetch a known internal page (e.g. [AOE FAQ](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ)) and confirm content returns.
5. **If tools fail** — Use the wiki’s troubleshooting section; confirm token/session and MCP allowlists.

If your org changes MCP names or install paths, treat the **assetscollab** wiki as current.

---

## Files

| File         | Purpose                                                                   |
| ------------ | ------------------------------------------------------------------------- |
| `SKILL.md`   | When to use the skill, wiki vs `aoe-faq.md`, intent matching, boundaries. |
| `aoe-faq.md` | **Synced** markdown mirror of the wiki FAQ (not the system of record).    |

---

## Related strategy / requirements

- [Idea: AOE FAQ Skill future state – any FAQ](https://wiki.corp.adobe.com/spaces/MSTeam/pages/3769377144/Idea+AOE+FAQ+Skill+future+state+-+any+FAQ) (MSTeam wiki)
- Project docs: `docs/aoe-faq-skill-strategy.md`, `docs/aoe-faq-implementation-status-and-next-steps.md`
