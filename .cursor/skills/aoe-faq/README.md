# AOE FAQ — Cursor project skill

Internal-only FAQ for **AOE / EMA / EDS** engagement questions. The agent follows **`SKILL.md`**. Answers must match the **approved FAQ** (see source of truth below). **Order of operations:** try **wiki MCP first** for every FAQ question; if MCP fails, use **`aoe-faq.md`** and say so (see `SKILL.md` → *Retrieval order*).

This project also supports **Slack MCP** (optional): share FAQ summaries or team messages in **#aoe-ise-internal** via the `aoe-faq` Slack app. See [Slack MCP setup](#slack-mcp-aoe-faq-project) below.

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
2. Enable **Easy MCP** and these MCP servers (as needed):
   - **Adobe Wiki / Confluence** — read the live FAQ (required for wiki-first answers).
   - **Slack** — post to **#aoe-ise-internal** (optional; see [Slack MCP](#slack-mcp-aoe-faq-project)).
3. In chat, use **`@aoe-faq`** (or ask the agent to follow the **aoe-faq** project skill).
4. **Answering FAQ questions:** The agent should **always try wiki MCP first** (`get_wiki_content` on the [AOE FAQ](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ) URL). **If MCP is missing, errors, times out, or auth fails**, it should **fall back to `aoe-faq.md`** and **tell you** the answer is from the local file and may be stale or out of sync with Confluence.
5. **If you see that fallback notice:** Fix MCP in Cursor settings (see below) or confirm on Confluence; optionally ask for a wiki sync of `aoe-faq.md` when the wiki was updated.
6. **Post to Slack:** Ask in the same message when you want a channel post (examples below). The agent answers from wiki first, then posts via Slack MCP.

### Example prompts

| Goal | Example |
| ---- | ------- |
| FAQ only | `@aoe-faq Does Franklin have dev/staging/prod environments?` |
| Wiki + Slack | `@aoe-faq Answer from wiki about partner vs AOE scope, post 3 bullets to #aoe-ise-internal` |
| Slack only | `Post to #aoe-ise-internal: Team sync in 10 minutes` |

---

## Slack MCP (aoe-faq project)

Share **approved FAQ summaries** or **internal announcements** in **#aoe-ise-internal** using the Adobe Slack MCP server and the **`aoe-faq`** Slack app (configured per teammate).

| Role | Location |
| ---- | -------- |
| **Project setup guide (scopes, bot invite, tokens)** | [`docs/aoe-faq-slack-mcp-setup.md`](../../../docs/aoe-faq-slack-mcp-setup.md) |
| **Adobe canonical Slack MCP wiki** | [Cursor.ai - Adobe Slack MCP Setup](https://wiki.corp.adobe.com/pages/viewpage.action?pageId=3513057951&spaceKey=BPS&title=Cursor.ai%2B-%2BAdobe%2BSlack%2BMCP%2BSetup) (BPS) |
| **Easy MCP + Cursor** | [Cursor integration with Easy MCP](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP) |
| **Agent rules (wiki then Slack)** | `SKILL.md` → *Slack MCP* |

### Setup checklist (summary)

Full steps are in **`docs/aoe-faq-slack-mcp-setup.md`**. At minimum:

1. Build or run the Slack MCP server ([adobe-mcp-servers](https://github.com/Adobe-AIFoundations/adobe-mcp-servers)) via Easy MCP / Docker or Node.
2. Create a Slack app; set **bot** scopes (`channels:history`, `channels:read`, `chat:write`, `reactions:write`, `users:read`) and **user** scope `search:read`.
3. Install to workspace (or submit admin request; wait for **Slackbot** approved/cancelled message).
4. Invite the bot: `/invite @aoe-faq` in **#aoe-ise-internal**.
5. Set `SLACK_BOT_TOKEN`, `SLACK_USER_TOKEN`, `SLACK_TEAM_ID` in your local env (see project `.cursor/mcp.json` for Docker env-file path).
6. Verify: post a test message and fetch channel history.

### Governance

- Slack posts are **internal team** use only; wording must match **wiki/FAQ** when sharing FAQ content.
- Do not post customer PII, deal terms, or content not in approved sources.
- If Confluence returns **Draft**, mention that in Slack when sharing summaries.

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
| `SKILL.md`   | When to use the skill, wiki vs `aoe-faq.md`, Slack posting rules, boundaries. |
| `aoe-faq.md` | **Synced** markdown mirror of the wiki FAQ (not the system of record).    |

---

## Related strategy / requirements

- [Idea: AOE FAQ Skill future state – any FAQ](https://wiki.corp.adobe.com/spaces/MSTeam/pages/3769377144/Idea+AOE+FAQ+Skill+future+state+-+any+FAQ) (MSTeam wiki)
- Project docs: `docs/aoe-faq-skill-strategy.md`, `docs/aoe-faq-implementation-status-and-next-steps.md`, `docs/aoe-faq-slack-mcp-setup.md`
- Team presentation (slides + live demo script): `docs/aoe-faq-team-presentation.md`
