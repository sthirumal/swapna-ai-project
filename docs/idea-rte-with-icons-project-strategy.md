# Idea: RTE with icons — project strategy

This document captures the **problem**, **acceptance criteria**, and a **phased delivery strategy** for the **“Idea: RTE with icons”** initiative (MSTeam wiki), plus context from the **Crosswalk (Xwalk) / Universal Editor** thread and mockup.

**Source requirements (internal wiki):** [Idea: RTE with icons](https://wiki.corp.adobe.com/spaces/MSTeam/pages/3774314007/Idea+RTE+with+icons) (MSTeam space).

**In-repo sync:** Wiki body was not machine-readable from agents (403). The sections **Problem**, **Acceptance criteria**, and **Authoring example** below are **copied from the wiki** as provided by the project owner (April 2026).

---

## 1. Problem (from wiki)

In several projects, **customers want to replace traditional `UL > LI` bullets** with icons (for example checkbox, arrow, or other symbols).

They may also want **icons inline** with body text, not only at the start of list items.

### 1.1 Author context (thread + mockup)

- **Today:** Authors can insert icons by typing a **shortcode** in the RTE, for example `:icon-name:` (thread examples include `:handwave:`, `:green-check:`, `:red-x:`, `:notepad:`; wiki examples use `:icon-search:`). This is used in **headings** and **list items** (including replacing the visual bullet).
- **Pain:** Remembering and typing icon names is error-prone and slow.
- **Desired enhancement:** An RTE **toolbar control** (for example a dropdown labeled like **“Add icon”**) that **inserts the chosen shortcode at the caret**, so authors do not have to memorize names—conceptually the same token whether inserted by hand or by the extension.
- **Product signal:** Colleague feedback in the thread: a **dropdown-style RTE extension** is a reasonable enhancement direction; **at the time of the discussion, nothing was supported in that direction** for this use case (confirm current roadmap with engineering / `@svinod` or present owner).
- **Reference doc (AEM):** [Configuring the RTE for the Universal Editor | Adobe Experience Manager](https://experienceleague.adobe.com/) — understand how the RTE is configured in **Universal Editor** and what extension points exist or are planned.

**Visual mockup:** Toolbar with lists/formatting plus an **“add icon”** control opening a menu of insertable codes (see screenshot attached to the wiki / MSThread). Local copy may exist under Cursor project assets as `image-76e33b72-bded-4c1a-9d81-a23ce6e79877.png` if you saved it from chat.

---

## 2. Acceptance criteria (from wiki)

**Authoring path is intentionally agnostic:** whether the author picks the icon from an **RTE extension / dropdown** or **types the shortcode manually**, the **published behavior** must satisfy the same rules.

| #       | Criterion                                                                                                                                                                                                                                                                 |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **AC1** | If a **list item (`LI`) starts with an icon** (the icon shortcode / token at the beginning of the item content), the **default list bullet must be replaced** by that icon (visually the icon acts as the list marker).                                                   |
| **AC2** | When the line wraps, **text must not sit under the icon** as if the icon were a full-width block. Wrapped lines should align like a normal bulleted list: **continuation lines align with the text start** of the first line (hanging indent / “bullet column” behavior). |

These two criteria are the **definition of done** for list rendering. Inline icons in running text (outside “icon-as-bullet” cases) should be specified separately if the wiki adds them; the **problem statement** already covers inline use for authors.

---

## 3. Authoring example (from wiki)

Authors may write list items such as:

```text
:icon-search: Here is LI number one
:icon-search: Second item is here
```

The **stored model** might be HTML, JSON, or another serialization; the contract is that **the renderer** (and any preview) can detect “icon at start of `LI`” and apply **AC1** and **AC2**.

---

## 4. North star

| Goal                      | Description                                                                                                                                  |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Predictable lists**     | Any `LI` that **starts with** a supported icon token renders with **no duplicate bullet** and **correct wrapping** (wiki AC).                |
| **Author ergonomics**     | Optional **RTE UI** inserts the **same tokens** authors could type—no second, parallel representation unless product explicitly requires it. |
| **Single token contract** | One agreed syntax (for example `:icon-key:`) from authoring → persistence → site/app render (and EDS block output if applicable).            |

**v1 suggestion:** Implement **rendering + CSS (or equivalent) for `LI` leading icon** first (proves AC1/AC2 on real content), then add **toolbar dropdown** if the program owns the RTE extension surface; split reduces dependency on Universal Editor release cadence.

---

## 5. Strategic pillars

Quick reference:

| Pillar                    | What to establish                                                                                                                                               |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Token registry**        | Canonical map `:icon-search:` → asset (SVG or sprite), **deprecation** rules, and **unknown token** behavior (show raw, strip, or fallback).                    |
| **Where rendering runs**  | Universal Editor preview vs **published page** (AEM, EDS, app shell). AC1/AC2 must hold wherever customers see the list.                                        |
| **RTE extension reality** | Confirm **allowed extension points** for Crosswalk/UE (insert string at caret, plugin lifecycle, sanitization). Align with Experience League RTE configuration. |
| **Accessibility**         | Icons replacing bullets: **decorative vs meaningful** list markers; how **assistive tech** exposes the list (role, text alternative, list semantics).           |
| **Governance**            | Who adds icons, who reviews customer-specific sets, and how projects stay on **approved** names.                                                                |

Full explanations of each pillar are in **§10 — Strategic pillars (detailed reference)** at the end of this document.

---

## 6. Phased plan

### Phase 0 — Align (short; mandatory)

| Activity               | Outcome                                                                                        |
| ---------------------- | ---------------------------------------------------------------------------------------------- |
| **Stack confirmation** | Exact surfaces: **UE RTE only**, **published HTML pipeline**, **EDS blocks**, or customer SPA. |
| **Token spec**         | Regex or parser rules for “start of `LI`”; whitespace handling; case sensitivity.              |
| **AC test content**    | Fixture pages using the wiki **Authoring example**; visual + narrow-width wrap tests.          |
| **RTE dropdown**       | Decision: **in scope for v1 or v1.1** based on extension feasibility.                          |

**Exit:** Signed scope, owner, and test fixtures agreed.

---

### Phase 1 — Rendering (proves acceptance criteria)

| Activity                    | Outcome                                                                                                                                                                                  |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Detect leading icon**     | On render (or server transform), if first meaningful content in `LI` is a known token, treat as **list marker icon**.                                                                    |
| **Suppress default bullet** | `list-style: none` (or equivalent) for those items or the parent `ul`/`ol` strategy you choose.                                                                                          |
| **Hanging layout**          | CSS (typical patterns: `display: flex` on `li` with fixed-width icon column, or `padding-inline-start` + absolutely positioned / `::before` marker) so **AC2** holds across breakpoints. |
| **Cross-browser QA**        | Safari/Chrome/Firefox; RTL if in scope.                                                                                                                                                  |

---

### Phase 2 — Authoring (optional for v1 per scope)

| Activity              | Outcome                                                                       |
| --------------------- | ----------------------------------------------------------------------------- |
| **Dropdown / picker** | Inserts **exact shortcode** at caret; respects RTE sanitization.              |
| **Docs for authors**  | Short internal page: how to use dropdown + manual syntax + which icons exist. |

---

### Phase 3 — Harden

| Activity        | Outcome                                                                                                |
| --------------- | ------------------------------------------------------------------------------------------------------ |
| **Edge cases**  | Nested lists, mixed icon/non-icon items, `LI` with block children, links before icon, paste from Word. |
| **Performance** | Sprite vs many inline SVGs; caching.                                                                   |
| **Operations**  | Process to add/rename icons without breaking existing pages.                                           |

---

## 7. Technical notes (implementation-facing)

| Topic                        | Guidance                                                                                                                                              |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **“LI starts with an icon”** | Define “start” after optional whitespace? after a single inline element? Document explicitly to avoid parser drift between author, RTE, and renderer. |
| **One visual bullet**        | Ensure the **native marker** is hidden and only **one** icon shows (avoid icon + `::marker`).                                                         |
| **Wrapping (AC2)**           | Prefer a **flex row** (`icon` + `content` flex child with `min-width: 0`) or a **grid** template; avoid floats for robustness.                        |
| **Ordered lists**            | Wiki emphasizes `UL/LI`; if `OL` + leading icon appears, decide same rules or explicit non-support.                                                   |
| **Inline icons**             | Not covered by AC1/AC2; render as inline replacements without list-marker logic.                                                                      |

---

## 8. Risks and mitigations

| Risk                       | Mitigation                                                                                               |
| -------------------------- | -------------------------------------------------------------------------------------------------------- |
| **No OOTB RTE dropdown**   | Lead with **render path**; partner or backlog **UE extension** once API is confirmed.                    |
| **Sanitizer strips `:…:`** | Validate persisted HTML/JSON from UE; adjust allowlist or use data-attribute representation if required. |
| **Duplicate semantics**    | Screen readers may announce list + icon oddly; a11y review early.                                        |
| **Scope creep**            | Lock v1 to **AC1+AC2** + token registry; defer fancy picker search until base is stable.                 |

---

## 9. Immediate next steps

1. **Confirm ownership** with engineering (thread referenced confirmation with `@svinod`—update to current DRI).
2. **Spike:** Given your delivery stack (AEM only vs EDS vs app), prototype **one `ul` with two `LI`s** from the **Authoring example** and verify **AC2** at small viewport widths.
3. **Parallel:** Read UE RTE configuration on Experience League and record **what is officially extensible** for “insert at caret” vs post-processing at save.
4. Keep this file as the **in-repo mirror** of wiki requirements; when the wiki changes, update **§1–3** (and the **§5** summary table if pillars shift), keep **§10** detailed notes in sync, then bump **§11** (revision log).

---

## 10. Strategic pillars (detailed reference)

This section expands the **§5** summary table: token registry, rendering surfaces, RTE extension points, accessibility, and governance. Use it for onboarding, reviews, and scope discussions.

---

### 10.1 Token registry

A **token registry** is the **official catalog** that answers: _When stored content contains this text token, which icon do we show, and what do we do when we do not recognize it?_ It is the single **dictionary** from authoring **codes** to **assets and rules**.

**Canonical map (`:icon-search:` → asset)**  
**Canonical** means the one **approved** definition—not “whatever each team guesses.”

| Piece     | Meaning                                                                            |
| --------- | ---------------------------------------------------------------------------------- |
| **Token** | The string in content, e.g. `:icon-search:` (typed or inserted by an RTE control). |
| **Asset** | What actually draws the icon: often **SVG**, **SVG sprite**, or a font glyph.      |

For every supported token there should be **exactly one** agreed visual (and usually metadata: size, optional accessible name). That prevents `:icon-search:` looking different on Project A vs Project B.

**Deprecation rules**  
**Deprecation** means an icon or token is **still honored for old content** but **should not be used for new content**. Rules should cover:

- Whether an **old token** maps to the same asset as a **new token** for a transition period.
- **Sunset** expectations (no new pages using the old token; optional auto-rewrite on save/migration vs leave until edited).
- How authors are **notified** (docs, RTE warnings).

Without deprecation rules you either **break old pages** or accumulate **duplicate tokens forever**.

**Unknown token behavior**  
An **unknown token** matches your pattern (or is meant as an icon) but is **not in the registry**—typo, removed icon, or content pasted from another system. Pick and document a default:

| Option       | Effect                                                                        |
| ------------ | ----------------------------------------------------------------------------- |
| **Show raw** | Render `:typo-icon:` as literal text so authors notice mistakes.              |
| **Strip**    | Remove from output (content can disappear—usually risky for published prose). |
| **Fallback** | Show a generic “missing icon” or default marker so layout stays stable.       |

You may choose **different defaults** for **authoring preview** (often show raw or warn) vs **public site** (often fallback for polish).

---

### 10.2 Where rendering runs

This pillar asks: **which software turns stored content** (with `:icon-search:` tokens) **into final HTML/CSS** that people see. Authoring preview and the live site are **not automatically the same code path**; icons and list layout can work in one place and fail in another.

| Surface                      | What it usually is                                                                                                                                                                   |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Universal Editor preview** | What authors see **while editing** in AEM Universal Editor (preview iframe or equivalent). Lists should respect **AC1/AC2** here so authors trust publish output.                    |
| **Published page (AEM)**     | Customer-facing HTML from **AEM** (Dispatcher, etc.) after publish.                                                                                                                  |
| **EDS (Edge Delivery)**      | Git-backed delivery; rich text may be processed in **blocks**, pipeline, or global CSS—not identical to UE unless you align implementations.                                         |
| **App shell**                | A **separate front-end** (SPA, mobile WebView) that consumes content/APIs and renders lists **itself**. Tokens only work if that app implements the **same** token and layout rules. |

**Requirement:** **AC1** and **AC2** must hold **everywhere end users (and authors judging final output) see that list**—otherwise you get “fine in UE, wrong on `.live`” or “fine on web, broken in the app.”

**Practical implication:** Decide between **one shared renderer** (shared lib, shared CSS, shared post-process) vs **per-channel implementations**; if per-channel, run the **same fixture content** (wiki authoring example) through **each** surface in QA.

---

### 10.3 RTE extension reality

**Before committing** to a toolbar **“Add icon”** dropdown in **Crosswalk / Universal Editor**, confirm what the **RTE actually allows** and what the platform **strips or rewrites** (sanitization).

- **Crosswalk (“Xwalk”)** here means the **AEM Sites → Edge Delivery** style path from the thread context.
- **Universal Editor (UE)** is the **authoring UI** where the **RTE** lives—the rich text field authors use—not the public EDS site alone.

**Extension points to validate**

| Topic                      | What to confirm                                                                                                                                                                                         |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Insert string at caret** | Can an extension insert text (e.g. `:icon-search:`) **at the cursor** like typing? Some stacks only allow whitelisted commands or structured nodes, not arbitrary strings.                              |
| **Plugin lifecycle**       | **When** custom code loads, updates, and is destroyed; how it registers with the editor; behavior across **inline vs dialog** fields, navigation, and multiple RTEs—avoid leaks and fragile init order. |
| **Sanitization**           | On save or round-trip, does the platform **remove or normalize** patterns you rely on? If `:icon-search:` disappears, the feature fails unless you use another **supported** representation.            |

**Experience League** documents how the RTE is **configured** for Universal Editor. **Align** implementation plans with **documented, supported** configuration—not only undocumented DOM behavior—so work does not break on the next product update.

**Plain takeaway:** **Prove on a real UE RTE field** that tokens can be **inserted and persisted**, and understand **supported customization** vs **unsupported internals** before treating the dropdown as v1 scope.

---

### 10.4 Accessibility

When a **visual bullet** becomes an **icon**, you must decide what that icon **means for people who do not see the screen** (screen readers and other assistive technology). Poor choices make lists confusing or noisy.

**Decorative vs meaningful**

| Type           | Idea                                                                                    | Typical handling                                                                                                                                                            |
| -------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Decorative** | Icon is **visual styling only**; the **words** carry the meaning (like a styled dot).   | Often **hide the icon** from the accessibility tree (`aria-hidden="true"` on the SVG/wrapper) so users hear a **normal list** without repeated “graphic, …” for every line. |
| **Meaningful** | Icon **adds information** not fully in the text, or **replaces** wording (e.g. status). | Usually needs a **text alternative** (visible text, `aria-label`, or visually hidden text) so non-visual users get that meaning.                                            |

The team should **decide per design** (and document): is the list marker **only** replacing a bullet next to self-explanatory text (**often decorative**), or is it the **primary** status indicator (**often meaningful**)?

**Assistive technology: list exposure**

Assistive tech uses **browser semantics** (list, listitem, etc.).

- **List semantics:** Prefer real **`ul` / `ol` / `li`** (or a correct ARIA list pattern if markup must change). Breaking list structure for layout hacks can stop “list with N items” announcements.
- **Roles:** Native elements expose **list** and **listitem** reliably; custom widgets may need explicit roles and more testing.
- **Text alternative for meaningful icons:** Provide a short **accessible name** without duplicating the whole line awkwardly or spamming announcements.

**Link to AC1/AC2:** Those criteria are **visual** (marker + wrapping). Accessibility asks: after the change, does a screen reader user still get correct **list structure**, sensible **item** reading, and any **status** the icon conveys—without redundant noise?

**Practical default to discuss:** Treat icon-as-bullet as **decorative** when line text is self-explanatory; preserve native list markup; **exception** when the icon encodes **state**—then treat as **meaningful** and expose state accessibly **once**, with a11y sign-off.

---

### 10.5 Governance

**Governance** is the **people and rules** that keep icon tokens from becoming chaos: duplicate names, incompatible customer forks, or unreviewed assets.

**Who adds icons**

Define:

- **Who may request** a new token and asset.
- **Who implements** it (central team vs project).
- **Where the registry lives** (repo, design tokens package, Confluence, etc.).

Without ownership, every project adds ad hoc icons and **renderers** or **support** diverge.

**Who reviews customer-specific sets**

When customers get **custom** icons (logos, product marks):

- **Who approves** (brand, legal, a11y, security as needed).
- **Namespacing** to avoid clashes with global tokens (e.g. `:acme-logo:` vs `:icon-search:`).
- **Quality bar:** SVG safety, size, contrast, licensing.

**How projects stay on approved names**

- **Authoring:** RTE dropdown fed only from the **approved registry** reduces typos and invented codes.
- **Documentation:** Published list of allowed tokens.
- **Optional enforcement:** Lint or CI warnings on unknown `:…:` patterns in content repos.
- **Deprecation:** Communicated migrations so teams **update** instead of forking the token list.

**Why it matters:** AC1/AC2 assume a **stable meaning** for “`LI` starts with an icon.” If `:check:` maps to different SVGs on two sites, or `:green-check:` vs `:greencheck:` diverge, behavior is **unpredictable** and **unmaintainable**.

---

## 11. Revision log

| Date       | Change                                                                                                                                             |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-04-22 | Initial draft; wiki not fetchable (403).                                                                                                           |
| 2026-04-22 | Synced **Problem**, **Acceptance criteria**, **Authoring example**; added thread/mockup context, phased strategy, technical notes.                 |
| 2026-04-22 | Expanded **§5 Strategic pillars** with full reference text (token registry, rendering surfaces, RTE extension reality, accessibility, governance). |
| 2026-04-22 | Moved pillar **detailed** text from §5 to **§10** at end of document; §5 is summary only; revision log renumbered to **§11**.                      |
