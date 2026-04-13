# AOE FAQ Skill — Strategy and Phases

This document captures the planned approach for the **AOE FAQ** project skill: pre-approved answers to recurring questions about AOE engagements, delivered consistently via an assistant when the skill is referenced (e.g. `@aoe-faq`).

**Source requirements (internal wiki):** [Idea: AOE FAQ Skill future state – any FAQ](https://wiki.corp.adobe.com/spaces/MSTeam/pages/3769377144/Idea+AOE+FAQ+Skill+future+state+-+any+FAQ) (MSTeam space).

---

## 1. North star

| Goal                       | Description                                                                                                                                                                   |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Consistent messaging**   | One approved set of answers for topics such as fit, deliverables, scope, commercial model, and AOE vs partner work.                                                           |
| **Intent-based use**       | The assistant matches **by topic or intent**, not exact wording; responses can be **tailored** (shorter/longer, combined/focused) while staying faithful to approved content. |
| **Single source of truth** | Canonical FAQ content is maintained in one place (today often a markdown file in the project; later possibly Confluence/wiki or another connected source).                    |
| **Safe content**           | Generic, reusable Q&A only—no customer-specific or sensitive detail in the skill corpus.                                                                                      |

The wiki describes the skill as the **“how to use that content”** layer on top of the maintained FAQ document.

---

## 2. Strategic pillars

| Pillar                 | What to establish                                                                                                                                 |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Content governance** | Owners, review cadence, and a single canonical store so messaging does not drift across teams or copies.                                          |
| **Retrieval behavior** | Skill instructions that emphasize intent matching, partial matches, and tailoring—not verbatim dumps every time.                                  |
| **Distribution**       | Start with Cursor (project skill); optional later channels (e.g. Slack, Teams, small web app, CLI, Outlook) using the **same** canonical FAQ.     |
| **Operations**         | A simple way to **run or refresh** content from a local Mac (script or one-off sync), especially if the master source moves to the internal wiki. |

---

## 3. Phased plan

### Phase 0 — Align

**Purpose:** Lock scope, people, and success criteria before building anything in Phase 1.

_Typical duration:_ about one to two weeks, depending on stakeholder availability.

| Activity                   | Outcome                                                                                                                              |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Identify **stakeholders**  | Who approves FAQ text (e.g. MS / AOE leads), who maintains the file, how disagreements are escalated.                                |
| Confirm **scope**          | v1 as **AOE-only** vs early steps toward “any FAQ” (wiki lists expansion as a follow-on).                                            |
| Define **success metrics** | Examples: faster answers to recurring questions, fewer inconsistent replies in pilot, no PII or customer-specific data in the skill. |

#### What you need going into Phase 0

| Input                         | Why it matters                                                                                 |
| ----------------------------- | ---------------------------------------------------------------------------------------------- |
| **Sponsor or decision-maker** | Can approve scope and where the FAQ may live (repo vs wiki).                                   |
| **Content authority**         | Someone who can say “this wording is approved for AOE messaging” (may be the same as sponsor). |
| **Wiki / requirements link**  | Single reference for intent (e.g. the MSTeam wiki page already describing the skill).          |
| **Your role**                 | Clear whether you are driver, contributor, or only implementing after others approve text.     |

You do **not** need the final `aoe-faq.md`, Cursor skill files, or automation in Phase 0—only agreements and names.

#### How to start Phase 0 (practical steps)

1. **Schedule a short kickoff** (30–45 minutes) with sponsor + content authority (and MS/AOE lead if different). Share the wiki page and this strategy doc in the invite.
2. **Walk through three decisions** in that meeting or a follow-up:
   - **Audience:** Internal only vs any caveat for customer-facing use of the assistant (affects tone and legal review).
   - **Scope:** AOE FAQ only for v1, or explicitly include “foundation for any FAQ.”
   - **Source of truth (directional):** Start in git in this repo vs wiki-first vs “we decide in Phase 2” (it is fine to leave wiki integration for Phase 2 if you only need a direction now).
3. **Assign roles** (can be one page in Confluence or email):
   - **Approver** — signs off on FAQ text before it ships in the skill.
   - **Maintainer** — day-to-day edits and PRs to the markdown (or wiki updates).
   - **Escalation path** — who resolves conflicts between sales, delivery, and MS messaging.
4. **Agree on success metrics** — pick one or two that are measurable in a pilot, for example:
   - Pilot users report fewer “I heard different things from different people” cases for covered topics.
   - Zero customer PII or deal-specific details stored in the skill corpus.
   - Time-to-answer for listed FAQ topics improves vs baseline (even qualitatively in v1).
5. **Record outcomes** — short written summary (half a page): scope, owners, metrics, and “Phase 1 may start when …” (e.g. when approver green-lights the initial FAQ draft).

#### Phase 0 exit criteria (definition of done)

Phase 0 is complete when all of the following are true:

- [ ] **Named approver and maintainer** (can be the same person if your process allows).
- [ ] **Written scope** for v1: AOE-only (yes/no) and whether “any FAQ” is explicitly out of scope for v1.
- [ ] **At least one success metric** agreed for the pilot.
- [ ] **Escalation path** documented in one place (wiki, email, or ticket).
- [ ] **Green light to start Phase 1** — e.g. approver agrees that building the skill + initial FAQ draft is authorized (content can still iterate in PRs).

Optional but useful: a **RACI** (who is Responsible, Accountable, Consulted, Informed) for FAQ updates, especially if multiple teams touch messaging.

---

### Phase 1 — MVP skill (Cursor)

**Purpose:** Ship a usable project skill wired to approved FAQ content.

| Activity                                                                        | Outcome                                                                                                                                                                                          |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Author or import **`aoe-faq.md`** (or equivalent)                               | Structured Q&A aligned with approved messaging (the wiki includes a starter covering fit, team/duration, deliverables/GitHub, scope vs partner, complementary vs alternative, commercial model). |
| Add a **Cursor project skill** (e.g. `SKILL.md` under a dedicated skill folder) | Instructions for **when** to use the skill (AOE / engagement FAQ-style questions), **how** to match intent and tailor answers, and **where** the canonical document lives.                       |
| **Version control**                                                             | FAQ changes go through git with PR review where your process requires it.                                                                                                                        |
| **Pilot**                                                                       | Small user set; collect misfires (wrong intent, over-long answers, missing topics).                                                                                                              |

_Typical duration:_ depends on content readiness; technical wiring is small if content exists.

---

### Phase 2 — Source of truth and refresh

**Purpose:** Align long-term ownership with wiki or other systems of record.

| Activity                     | Outcome                                                                                   |
| ---------------------------- | ----------------------------------------------------------------------------------------- |
| Decide **master source**     | Wiki as master, repo as master, or **generated** (e.g. export wiki → markdown in repo).   |
| Implement **refresh on Mac** | Script or documented one-off steps to update the file the skill reads.                    |
| If Confluence/wiki is master | Clarify access, export format, automation constraints (API vs manual export, scheduling). |

---

### Phase 3 — Multi-channel (optional)

**Purpose:** Reuse the same FAQ outside Cursor without forking content.

| Activity          | Outcome                                                                                     |
| ----------------- | ------------------------------------------------------------------------------------------- |
| Choose channels   | e.g. Slack, Teams, lightweight web FAQ, CLI, Outlook.                                       |
| **Single corpus** | Each channel is a thin layer; avoid per-channel copy that diverges without a merge process. |

---

### Phase 4 — “Any FAQ” pattern (optional)

**Purpose:** Reuse the same idea for other topic areas.

| Activity                | Outcome                                                                                |
| ----------------------- | -------------------------------------------------------------------------------------- |
| Generalize the template | Namespacing (e.g. per-topic skills), shared Q&A structure, governance per topic owner. |

---

## 4. Repository strategy

### 4.1 Building the skill in this repo vs a separate repo

| Approach                                            | Advantages                                                                                                                    | Disadvantages                                                                                                                                                             |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Same repo** (e.g. this AEM Edge Delivery project) | One clone for site work; simple PRs when FAQ and site are tightly coupled; no extra repo to create.                           | Mixed concerns (public site vs internal skill); broader repo access may be unsuitable for sensitive positioning text; harder reuse across many engagements without drift. |
| **Separate private repo**                           | Clear ownership; easier internal-only access; natural fit if FAQ is a **shared MS asset**; wiki as source + export fits well. | Two clones if you also work on a customer site repo.                                                                                                                      |

### 4.2 Starting here and moving later

You **can** implement the skill in this repository first and **relocate** it later.

**Practices that make migration easy:**

1. Keep the skill and FAQ in **one dedicated folder** (e.g. `.cursor/skills/aoe-faq/` with `SKILL.md` and `aoe-faq.md`).
2. Use **relative paths** inside the skill (e.g. `./aoe-faq.md`) so the folder copies intact.
3. Avoid hard-coding **repository-specific** names or URLs in the skill unless they are permanent.
4. After move: copy the folder to the new repo, reopen the project in Cursor, and update any **sync scripts** that assumed the old path.

---

## 5. Deliverables checklist

Use this as a lightweight project tracker.

- [ ] Approved FAQ source (markdown and/or wiki) and named owners.
- [ ] Cursor project skill wired to that source, with clear invocation (e.g. `@aoe-faq`).
- [ ] Refresh/sync approach for local Mac (and wiki, if applicable).
- [ ] Pilot feedback loop and backlog for missing intents or wording fixes.
- [ ] Optional roadmap: multi-channel delivery and generic “any FAQ” reuse.

---

## 6. Risks and mitigations

| Risk                                     | Mitigation                                                                                          |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Stale content                            | Owners + periodic review; changelog in the FAQ or wiki.                                             |
| Answers that go beyond approved text     | Skill instructions: use only approved Q&A; if no match, say so and defer to humans.                 |
| Sensitive or customer-specific questions | Explicit defer/refuse behavior; keep the corpus generic.                                            |
| Legal/commercial precision               | Clarify approved messaging vs examples; involve legal/commercial review where your org requires it. |

---

## 7. Document maintenance

| When to update this doc                                             |
| ------------------------------------------------------------------- |
| Phase dates or scope change materially.                             |
| Source of truth moves (e.g. from repo-only to wiki-first).          |
| Repository strategy changes (e.g. split to a dedicated skill repo). |

_Last aligned with wiki page concept: AOE FAQ Skill future state — any FAQ._
