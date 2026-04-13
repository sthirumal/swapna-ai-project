<!--
  Source of truth (edit here first): https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ
  This file is a synced copy for Cursor/offline. Do not change FAQ wording only in git—update the wiki, then sync via wiki MCP (see .cursor/skills/aoe-faq/README.md) or Confluence UI.
-->
# FAQ – Pre-reviewed Q&A (Project skill)

Use this document to answer recurring questions about AOE engagements, EDS, and related topics. **Match by topic or intent**, not exact wording—questions may be asked differently. When appropriate, **tailor the response** to the specific phrasing or angle of the question while staying consistent with the approved content below. If the user's question only partly matches a Q&A, use the relevant part and adapt; don't force a full canned answer when a shorter, targeted reply fits better.

---

## 1. AOE fit and scope (page count, languages, source, headless)

**Q:** Is the AOE model available and appropriate for our scenario (e.g. ~3000 pages, 3 languages, OpenText source, existing headless content for a mobile app)?

**A:** This could be a good candidate. Do we have the site URL so we can run a scope analysis? If there are many complex blocks, it may extend beyond the standard 8-week timeline; we would need to review to assess complexity.

---

## 2. How AOE works: team, customer contribution, duration

**Q:** How does an AOE engagement work in practice? What is the team composition on Adobe's side, what does our team need to provide, and what is the typical duration for a site of our scale?

**A:** At ~3,000 pages, this is on the larger side. From a page-count perspective, it could align to an 8-week timeline; however, this depends heavily on template reuse and block complexity. Our standard team is 2 AOEs, and for larger engagements we may scale the team to maintain the target timeline. We would need the URL to perform a proper assessment.

---

## 3. Deliverables: EDS codebase, GitHub, handover

**Q:** What does Adobe deliver at the end of an AOE engagement? Specifically: do we receive the full EDS codebase (blocks, styles, configuration) in our own GitHub repository, fully operational and independently maintainable by our team?

**A:** We deliver the full EDS codebase in a GitHub repository, which can be transferred to a customer-owned GitHub account. EDS is currently supported on GitHub only, as our automation tooling operates within GitHub repositories.

---

## 4. Scope boundary: AOE vs partner (CF, translation, headless, "remaining 30–40%")

**Q:** What is the scope boundary between AOE and partner work? You confirmed that EMA does not handle Content Fragment modelling, translation workflow configuration, or headless integration. Does the AOE engagement cover any of these, or is it strictly scoped to what EMA automates, meaning we still need a partner or internal effort for the remaining 30–40%?

**A:** AOE will import all page content into EDS (DA/X-Walk) pages and develop the required block code (JS and CSS) ensuring all views are accounted for (desktop, tablet, and mobile). The engagement focuses on front-end delivery and does not cover content fragment modeling, translation workflows, workflow configuration, or other back-end integrations. Those areas would be handled by ACS or the partner team.

---

## 5. AOE vs partner: complementary, alternative, or sequential?

**Q:** How does AOE relate to your earlier recommendation to work with Adobe partners? Are these complementary (AOE for EMA-driven migration, partner for the rest), alternative (AOE replaces the partner), or sequential?

**A:** AOE complements the partner. A partner is required to handle integrations and backend configuration outside the frontend migration scope.

---

## 6. Commercial model

**Q:** What is the commercial model? Fixed-scope, time-and-materials, or outcome-based?

**A:** This is a fixed-scope engagement covering frontend migration deliverables.

---

## 7. Self-service (agentic) vs AOE delivery: the two paths

**Q:** What are the options for using the Experience Modernization Agent—self-service vs Adobe-led?

**A:** There are two main paths:

**1. Self-service (agentic path)**  
The customer's own developers use the Experience Modernization Agent directly through the console. The agent consumes DX Agent Orchestrator (AO) credits at **100 credits per page migrated**. Example: a 100-page site migration consumes 10,000 credits.

**2. AOE delivery (Adobe-led)**

Adobe's Agentic Outcome Engineer (AOE) team operates the agent on behalf of the customer and delivers the migrated Edge Delivery Services site. The customer purchases an AOE services engagement, and Adobe runs the agent as part of that delivery.

---

## 8. How partners fit in (credits and services)

**Q:** How do partners (e.g. Accenture) fit in? Is partner delivery a third option?

**A:** No. The two options are **self-service** (customer's team runs the agent, consumes AO credits) and **AOE delivery** (Adobe runs the agent). When a partner performs the migration, the **customer purchases the Agent Orchestrator (AO) credits**; the partner uses those credits while running the agent. The partner separately charges the customer for their professional services. So: customer buys AO credits from Adobe; partner consumes those credits during the migration and charges the customer for their own delivery work.

---

## 9. Partner using their own credits / charging separately

**Q:** Can a partner (e.g. Accenture) use their own credits and charge the customer separately?

**A:** Typically no. The customer should purchase the AO credits, and the partner consumes those credits during the project. The partner charges the customer for their services; the credit purchase is the Adobe product the customer buys.

---

## 10. Partner including credits in project estimate

**Q:** Does the partner need to account for credits in their project estimate?

**A:** Yes. Migration consumes 100 credits per page, so that consumption should be reflected in the overall project estimate (e.g. scope, pricing, or materials the customer sees).

---

## 11. Sales comp on the AI agent

**Q:** Does sales get comp on the AI agent / migration?

**A:** Sales comp applies to the **Agent Orchestrator (AO) credit purchase** by the customer, since that is the Adobe product being sold.

---

## 12. Total Templates (scope / dashboard)

**Q:** What does "Total Templates" mean in the scoping report?

**A:** In scope analysis, pages with similar patterns are grouped together; each group is a template (a reusable layout/structure). **Total Templates** is the number of these distinct template groups (page types) identified for the site.

---

## 13. Total Block Variants (scope / dashboard)

**Q:** What does "Total Block Variants" mean in the scoping report?

**A:** Block variants are unique implementations of a block type discovered across analyzed pages. Each variant is a distinct implementation (e.g. different structure or content pattern). **Total Block Variants** is the count of all such distinct block variants identified for the site, often determined by sampling a subset of pages.

---

## 14. Search component (AOE scope)

**Q:** Is the search component in scope for AOE?

**A:** If the search input simply posts to a results page using a URL/query parameter, then yes—this is considered front-end and is in scope for AOE. However, any functionality related to search indexing, retrieving results, or filtering is considered back-end integration and would be handled by a partner.

---

## 15. EMA inputs: live site vs design artifacts

**Q:** What inputs does Adobe need to run the Experience Modernization tool? Are Figma designs required?

**A:** Under the AOE engagement, migration is like-for-like: we reproduce what is already published on the site today. Because the current experience is live in production, design and styling are derived from the live site—rendered pages, structure, and presentation as users see them—not from separate design artifacts. Figma files, static style guides, and similar design deliverables are not required for this approach; the authoritative reference is the live site.

---

## 16. Block reusability, legacy components, and scope analysis

**Q:** What percentage of reusability does Adobe assume for block generation so that long-term maintenance remains manageable? For example, will components in the current ecosystem (e.g. ~180) translate one-to-one to Edge Delivery blocks, or will there be rationalization?

**A:** We do not assume a fixed reusability percentage. For each site we run a scope analysis of the live experience, which identifies the number of block variants (distinct implementations based on design patterns) that appear across analyzed pages. Those variants inform what we build during migration. We do not map one-to-one from the source system's component catalog (for example, a count of "180 components" in the legacy stack) to Edge Delivery blocks; the analysis is based on what is actually rendered on the site. The scope output includes totals and estimates derived from that analysis. The final count is usually close to the scoped estimate, but during build and migration we may consolidate variants that can share one implementation or add variants if the live site surfaces patterns the sample did not capture.

---

## 17. Custom blocks, scope limits, and partner notification

**Q:** What is the definition of custom blocks? At what stage will the Partner be informed about components the agent missed or cannot be created?

**A:** Custom blocks are **non-standard** Edge Delivery blocks—patterns **beyond the boilerplate** (e.g. columns, hero, cards, carousel, accordion, embed, fragment, quote, table, teaser, media/video).

We can develop **many** custom block types, but **not everything** can be fully delivered as **front-end-only** migration—for example **data-driven calculators**, **maps tied to third-party APIs**, **personalization or authentication tied to back-end services**, or **deep commerce flows**. We can often still provide **layout, base styling, and placement**; **complex behavior and integrations** are the **partner's** responsibility.

Custom or out-of-scope items are **surfaced early** as we **prioritize pages and templates** for migration. **If we encounter something beyond scope at any point, we raise it immediately** so the partner can plan follow-on work.

---

## 18. Responsive viewports after EMA migration

**Q:** After migration to EDS via the Experience Modernization Agent, will the site support the agreed viewports (desktop, tablet, and mobile), or only match today's live breakpoints?

**A:** **Yes.** We implement Edge Delivery blocks so **desktop, tablet, and mobile layouts are covered**—responsive **CSS/JS**, not a single fixed viewport. Fidelity follows the **live site** (like-for-like), with behavior adapted for those standard responsive views.

---

## 19. Validating migration from AMS to "cloud"

**Q:** What report or artifact will be shared to validate that content has been successfully migrated from AMS to the cloud?

**A:** **Terminology:** In migration programs, **"cloud"** may mean different targets. **If the destination is Edge Delivery (EDS)**, stakeholders receive **lists of pages migrated in batches** as work progresses, so coverage can be validated and pages reviewed in EDS. **If "cloud" refers to another environment** (e.g. AEM as a Cloud Service), reporting should be **defined in the SOW** for that model.

---

## 20. SI publishing after migration

**Q:** Is there an expectation that the systems integrator (SI) will publish pages after Adobe completes migration?

**A:** There is **no default expectation** that the SI must publish everything. The **AOE team can publish** migrated pages as part of delivery. If **post-migration page-level edits** are made in the authoring flow (for example in Document Authoring / the customer repository), **whoever owns those changes** typically **publishes** them—often the **SI or customer**—so the live site reflects the updates.

---

## 21. Author-only page properties and tags

**Q:** Are page properties and tags configured in Author but not visible in the live site's HTML included in automated migration?

**A:** **Generally, no.** Migration follows what appears in the **published/live page experience** and the content brought into Edge Delivery. **Author-only metadata** (such as **tags** and other properties that do not render in the HTML) is **not** carried over by standard automation. If those fields are **required in EDS**, plan **partner follow-up** (for example custom mapping or integration) to populate **page metadata** in the target model.

---

## 22. Partner ownership of "data content" and go-live

**Q:** Is it reasonable to plan that the partner will own data or content migration work needed for go-live?

**A:** It depends what you mean by "data content." The AOE team migrates the **static page content** in scope for the Edge Delivery migration and hands over the **import scripts** used to perform the import. Those scripts can be run again if pages need to be re-imported during the lifecycle.

Back-end data, integrations, and **anything that is not static content** are the **partner's responsibility**.

---

## 23. Delivery methodology: waterfall vs hybrid agile

**Q:** Does moving to Edge Delivery Services (EDS) change the expectation of waterfall versus hybrid agile delivery?

**A:** **No.** EDS does not dictate waterfall vs hybrid agile; **methodology follows the SOW and operating model** agreed between the **customer and systems integrator**. The platform's **Git/pull-request workflow, preview, and incremental publishing** support **fast iteration**, which many teams align with **agile or hybrid** delivery—but that remains **optional**, not a requirement. **Governance and sign-off** still determine how the program is run.

---

## 24. Acronyms

- **AOE** – Agentic Outcome Engineer
- **EMA** – Experience Modernization Agent
- **EDS** – Edge Delivery Services

---
