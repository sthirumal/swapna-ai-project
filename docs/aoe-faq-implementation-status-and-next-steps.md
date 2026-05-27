# AOE FAQ Skill — Implementation Status and Next Steps

This document records **decisions from the internal team meeting** (post–[strategy](aoe-faq-skill-strategy.md)), **maps them to the phased plan**, and lists **next steps** plus **information still needed**. It complements [aoe-faq-skill-strategy.md](aoe-faq-skill-strategy.md); update this file when governance, locations, or tooling change.

---

## 1. Meeting outcomes (recorded)

| Topic | Decision |
| ----- | -------- |
| **Governance — approver / maintainer** | **Dave Fink** and **Charity** (approver and/or maintainer roles per your internal RACI). |
| **Your role** | Focus on **understanding requirements** and **implementation** (skill wiring, repo structure, docs, wiki/MCP readiness). |
| **Audience** | **Internal team only** — not for customer-facing or public assistant use without a separate review and scope change. |
| **Content start** | **Proceed**: build the FAQ in a document, then build the Cursor skill on top of it. |
| **Canonical FAQ (master)** | **[AOE FAQ — AEMSites](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ)**. All wording changes happen in Confluence first. |
| **Repo copy** | `.cursor/skills/aoe-faq/aoe-faq.md` is **synced from the wiki** (not the editor-of-record). See `.cursor/skills/aoe-faq/README.md` for sync steps. |
| **Skill structure reference** | Adobe Edge Delivery **docs-search** skill as a **pattern** only: [docs-search SKILL.md](https://github.com/adobe/skills/blob/main/skills/aem/edge-delivery-services/skills/docs-search/SKILL.md). |
| **Cursor + wiki MCP** | Teammates follow **[Cursor integration with Easy MCP](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP)** (assetscollab wiki); summary + checklist live in `.cursor/skills/aoe-faq/README.md`. **Slack MCP** (scopes, bot invite, tokens): [aoe-faq-slack-mcp-setup.md](aoe-faq-slack-mcp-setup.md). |

---

## 2. Map to [aoe-faq-skill-strategy.md](aoe-faq-skill-strategy.md)

| Strategy phase | Status after meeting |
| ---------------- | -------------------- |
| **Phase 0 — Align** | **Largely satisfied**: named approver/maintainer pair, audience (**internal only**), direction to start Phase 1. **Still confirm** written v1 scope (AOE-only vs “any FAQ”), success metrics, and escalation (see §5). |
| **Phase 1 — MVP skill (Cursor)** | **Initial MVP in repo**: `.cursor/skills/aoe-faq/` (`aoe-faq.md`, `SKILL.md`, `README.md`). **Remaining:** approver review of corpus + skill text, internal pilot, feedback backlog. |
| **Phase 2 — Source of truth and refresh** | **Under way**: canonical **[AOE FAQ wiki](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ)**; `SKILL.md` prefers wiki when MCP available; `aoe-faq.md` + README document **MCP-based sync** and **Easy MCP** setup ([assetscollab guide](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP)). |
| **Phases 3–4** | Unchanged optional roadmap in the strategy doc. |

The strategy doc’s **internal-only** pillar aligns with the meeting; keep the FAQ corpus **generic** (no customer PII, no deal-specific detail), as in strategy §1 and §6.

---

## 3. Recommended next steps (ordered)

**Done (Phase 1 initial drop):**

1. **FAQ in repo** — `.cursor/skills/aoe-faq/aoe-faq.md` (24 Q&A sections + acronyms).
2. **Cursor project skill** — `.cursor/skills/aoe-faq/SKILL.md` (`name: aoe-faq`, internal-only + governance, intent matching, link to [Adobe docs-search pattern](https://github.com/adobe/skills/blob/main/skills/aem/edge-delivery-services/skills/docs-search/SKILL.md) for structure only).
3. **README** — `.cursor/skills/aoe-faq/README.md` (wiki as SoT, sync `aoe-faq.md` from wiki, `@aoe-faq`, [Easy MCP + Cursor](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP) with checklist pointing to assetscollab wiki).

**Still to do:**

1. **PR + approver pass** — Open a PR; **Dave or Charity** review `aoe-faq.md` and `SKILL.md` wording.

2. **Internal pilot**  
   - Small set of users; track wrong intent, missing topics, over-long answers; backlog for Dave/Charity-approved FAQ edits.

3. **Wiki / sync hardening (Phase 2 remainder)**  
   - **MCP sync:** maintainers use chat + wiki MCP to refresh `aoe-faq.md` (see `.cursor/skills/aoe-faq/README.md`).  
   - Confirm **every teammate** has Easy MCP + wiki MCP per [Cursor integration with Easy MCP](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP).

4. **Governance housekeeping**  
   - Document in one place (wiki or this repo): **who approves** text changes, **who merges** PRs, **escalation** for messaging conflicts (strategy Phase 0 optional RACI).

---

## 4. Information still needed (checklist)

Fill these in as they are decided; they unblock polish and long-term ops.

| Item | Why it matters | Owner / notes |
| ---- | -------------- | ------------- |
| **Exact RACI** | Is Dave vs Charity **approver vs maintainer**, or both approvers? Who merges FAQ PRs? | Confirm with Dave and Charity. |
| **v1 scope line** | Explicit: **AOE-only** for v1, or first step toward “any FAQ”? | One sentence in wiki or README. |
| **Success metrics (pilot)** | At least one measurable or qualitative goal (strategy §0 step 4). | Agree with team. |
| **Escalation path** | Who resolves conflicts (sales vs delivery vs MS messaging)? | Name + channel (ticket, email, wiki). |
| **Target wiki location** | **[AOE FAQ — AEMSites](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ)** (canonical). | **Done** — linked from skill + README. |
| **Refresh mechanism** | **Wiki MCP in Cursor** (Easy MCP): agent fetches [AOE FAQ](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ) and updates `aoe-faq.md` when prompted. **Fallback:** Confluence UI copy into repo. | See `.cursor/skills/aoe-faq/README.md` §Syncing. |
| **Repo policy for internal text** | This Edge Delivery repo may be **broader access** than internal-only FAQ; confirm whether FAQ should live in a **private** repo or restricted path. | Strategy §4.1; align with Dave/Charity. |
| **Legal/commercial review** | Even for internal use, confirm if any FAQ topics need **GCO/legal** review before wide internal pilot. | Per org norms (strategy §6). |

---

## 5. Phase 0 exit criteria — quick update

From [aoe-faq-skill-strategy.md](aoe-faq-skill-strategy.md) §0 exit criteria, after the meeting you can mark:

- [x] **Named approver and maintainer** — Dave Fink and Charity (clarify RACI).  
- [ ] **Written scope** for v1 (AOE-only vs any FAQ) — **still needed** (one explicit decision).  
- [ ] **At least one success metric** — **still needed**.  
- [ ] **Escalation path** — **still needed**.  
- [x] **Green light to start Phase 1** — yes, per meeting (“get started to build the FAQ… then built [skill]”).

---

## 6. Reference links (external / internal)

| Resource | URL |
| -------- | --- |
| Strategy (this project) | [docs/aoe-faq-skill-strategy.md](aoe-faq-skill-strategy.md) |
| Team presentation + demo script | [docs/aoe-faq-team-presentation.md](aoe-faq-team-presentation.md) |
| Wiki — AOE FAQ skill idea | [Idea: AOE FAQ Skill future state – any FAQ](https://wiki.corp.adobe.com/spaces/MSTeam/pages/3769377144/Idea+AOE+FAQ+Skill+future+state+-+any+FAQ) |
| Adobe skill pattern (structure only) | [docs-search SKILL.md](https://github.com/adobe/skills/blob/main/skills/aem/edge-delivery-services/skills/docs-search/SKILL.md) |
| Cursor + wiki (Easy MCP) | [Cursor integration with Easy MCP](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP) |

---

## 7. Document maintenance

| When to update this file |
| ------------------------- |
| RACI, wiki URL, or sync approach changes. |
| FAQ file path in repo changes. |
| Pilot completes and Phase 2 wiki cutover is scheduled or done. |

_Last updated: reflects internal meeting decisions and alignment with aoe-faq-skill-strategy.md._
