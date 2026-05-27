<!--
  Source of truth (edit here first): https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ
  Synced copy for Cursor/offline. Last synced from Confluence via Adobe Wiki MCP (get_wiki_content).
  If the wiki page is in Draft, treat answers as draft until published on Confluence.
-->

# FAQ – synced from wiki

This file mirrors the **AOE FAQ** Confluence page body. **Match by topic or intent** when answering from this document. When the wiki is in **Draft**, confirm wording on Confluence before external or customer-facing use.

---
This page is in Draft mode


## Architecture


---

**Q:** Can I see the architecture?

**A:** Yes. Up-to-date diagrams and information are here: [Helix architecture docs](https://github.com/adobe/helix-home/blob/main/docs/architecture.md). The main content-bus to HTML rendering code is in [helix-html-pipeline](https://github.com/adobe/helix-html-pipeline).


---

**Q:** Is there any infrastructure redundancy?

**A:** From 2022-10-01, a second fully redundant delivery stack (CDN, compute, and storage) was built on Cloudflare, independent of the existing stack (Fastly, AWS Lambda, and S3). Delivery can be switched via a DNS CNAME change during a Fastly or AWS outage. Both stacks run in parallel at all times. _(Source as in your notes.)_


---

**Q:** Does Franklin have a concept of Dev, Staging, and Prod environments?

**A:** “Dev” and “staging” are not fixed Franklin concepts. For code, every branch gets a unique URL. You can link to a branch like `[https://98-cookie-style--vg-volvotrucks-us--hlxsites.hlx.page/drafts/wingeier/](https://98-cookie-style--vg-volvotrucks-us--hlxsites.hlx.page/drafts/wingeier/)` and Helix rewrites it to the related site (e.g. `[http://volvotrucks.us/drafts/wingeier](http://volvotrucks.us/drafts/wingeier)`). You can also point `[stage.customerdomain.com](http://stage.customerdomain.com)` at the `.page` URLs if you want.


---

**Q:** Can we hide our `.page` (preview) content from the public?

**A:** That is the customer’s responsibility. You could implement a frontend redirect (e.g. if the URL ends in `.page`, check a cookie and send users to login if missing), but the added complexity is often not worth it. Preview content is not indexed by default, which is the main concern.


---

**Q:** Can I have more than one Franklin project?

**A:** You can fork a project when there are heavy post-go-live dev changes, with the goal of merging again later. Linking across Franklin projects can be tricky: the pipeline rewrites links. Example: linking from [hlx.live block-collection docs](https://www.hlx.live/developer/block-collection/links) to `[https://main--helix-block-collection--adobe.hlx.page/block-collection/links](https://main--helix-block-collection--adobe.hlx.page/block-collection/links)` gets rewritten; a workaround is an intermediary link (e.g. Bitly) as in your example.


---

**Q:** What serverless frameworks can I use?

**A:** The platform is generally serverless-framework agnostic; you supply your own serverless layer. Cloudflare Workers are often very fast and easy to onboard. AIO, Lambda, Azure Functions, etc. are fine as long as Franklin can reach the endpoint and responses are fast. If there is no direct serverless endpoint but App Builder / IO is available, that can be used. _(Source as in your notes.)_


---

**Q:** How can we handle pollers or periodic server-side preprocessing from other systems with Franklin (similar to AEM workflows/schedulers)?

**A:** Use Cloudflare Workers or any serverless endpoint for extra logic. Example repo: [hlxsites/wesco-cloudflare-worker](https://github.com/hlxsites/wesco-cloudflare-worker). External workflow tools (e.g. Workfront) can be involved. Preprocessing often goes through SharePoint / Google Drive APIs. Pattern: bring your own serverless, then consume results on the client (e.g. a custom block that fetches and uses the data) or on the authoring side (e.g. generated Excel/Word). _(Source as in your notes.)_


## Authoring


---

**Q:** Can Adobe Express be used for content authoring?

**A:** No, as of 2023-03-01. A Sidekick plugin using the [Express Embed SDK](https://developer.adobe.com/embed-sdk/docs/) could appear later (e.g. for banners inside a Word doc).


---

**Q:** Can I build a Sidekick plugin (e.g. an authoring guide)?

**A:** Yes. Examples: [adobe/blog `tools/sidekick`](https://github.com/adobe/blog/tree/main/tools/sidekick), [adobe/express-website `tools/sidekick`](https://github.com/adobe/express-website/tree/main/tools/sidekick), [adobecom/milo `tools/sidekick`](https://github.com/adobecom/milo/tree/main/tools/sidekick) (Milo spreads Sidekick-related files across the repo).


---

**Q:** Can I use Chinese / Japanese / etc. characters in URLs?

**A:** No—only Latin characters. Non-ASCII paths were blocked because most such traffic was abusive; distinguishing good from bad was impractical. _(Source as in your notes.)_


---

**Q:** Does AEM Franklin support workflows?

**A:** As of 2023-02-02, AEM Franklin supports access controls for publishing and defers workflows to the content source. For SharePoint (Word/Excel), Adobe Workfront is recommended for document review and approval.


---

**Q:** Are there page versions? What if someone accidentally deletes something?

**A:** _(Not provided in your text—add the official answer here when you have it.)_

### **Multi-Site Manager (MSM): Traditional AEM vs. EDS via Crosswalk vs. DA Authoring**

Below is a **precise, actionable comparison** for customers currently relying on MSM in traditional AEM who are evaluating continuing with MSM via Crosswalk (AEM-driven content for EDS), or migrating to Document Authoring (DA) leveraging the new DA MSM model.


---


#### **1\. MSM in Traditional AEM Sites**

- **Architecture**: Deeply integrated—segment “blueprints,” “live copies,” and inheritance rules at the JCR/repository level, with built-in support for:
- Rollouts, relinking, detachment, partial overrides
- Automated/triggered updates (pre/post rollout hooks)
- Content hierarchy, language-masters, Live Relationship UI
- Synchronization managed via JCR and replication agents
- **Strengths**:
- Very fine-grained control
- Mature, supports complex global content governance, deep overrides, partial inheritance
- Tightly coupled to workflows and access controls in AEM
- **Limitations** (in cloud and on-prem): Complexity, heavy authoring learning curve, can create orphaned or stale live copies, dependency on classic UI (being deprecated), slow rollouts for large webs.


---


#### **2\. MSM for EDS via Crosswalk (AEM as Content Source for EDS)**

- **How it works**: Authors still create/manipulate MSM blueprints/live copies in AEM; EDS (via “Crosswalk”) pulls content from these live copies and delivers it with block-based rendering.
- **Feature Support**:
- **Inheritance of structure and content** (as managed in AEM)
- **Rollouts, detachment, partial override**, full or partial as per AEM MSM
- **Workflow, translation, launches**: as managed in AEM, visible within EDS-rendered pages.
- **Language copies, regionalization**: preserved exactly as in AEM
- **Content governance** and permissions managed in AEM
- **Limitations Compared to Traditional**:
- **Experience Editor/Touch UI only** (classic UI deprecated)
- **AEM-centric authoring**: Authors must use AEM for all MSM operations (including intense enterprise permissions and config)
- **EDS-specific limitations**:
- No real-time preview of MSM-edited or “rolled out” changes until published to EDS (the Crosswalk agent acts as a replication endpoint)
- Some advanced replication hooks, “pre-rollout” and “post-rollout” custom logic may not execute in EDS context
- **Still inherits all AEM MSM quirks**: Orphaning, (in)consistent rollout triggers, etc.


---


#### **3\. MSM for DA Authoring (Native to EDS/Document Authoring)**

- **How it works**: Uses DA MSM, now available in Early Access (see [DA MSM docs](https://docs.da.live/about/early-access/multi-site-manager)). All content is managed using SharePoint/Drive/docs, and MSM-like inheritance and rollout is managed via DA interfaces and APIs—not JCR. [https://github.com/da-sites/da-msm](https://github.com/da-sites/da-msm)
- **Feature Support**:
- **Blueprints and live copies** managed at the document/folder level, not in JCR
- **Copy and update strategies**: Inheritance patterns like in AEM, but for folder/doc structure, not node hierarchies
- **Bulk rollout and localized overrides**: Supported in flat-file/doc/metadata patterns
- **Automation**: Rollout jobs, locale-specific metadata, update propagation
- **Strengths**:
- **Much lighter, easier authoring**: Any approved user, no AEM login needed for day-to-day content changes or inheritance
- **Self-service rollouts/updates**: Via doc moves, DA UI, or Power Automate flows. Easier for non-technical teams
- **Global/locale variants**: Folder-level structure fits global content governance in a simpler model; overrides via file/folder copy, metadata “diffs”
- **Limitations Compared to Traditional/AEM MSM**:
- **No JCR-level hooks**: No classic AEM workflow/replication triggers, no before/after rollout hooks, no direct integration with AEM Launches
- **Inheritance is simpler/saner, but less nuanced**: All-or-nothing file/folder overrides; less support for "partial component" overrides
- **No access to AEM’s workflow and inline access controls**: Permissions controlled by doc repository (SharePoint, Google Drive) only
- **Loss of fine-grained “Detach/Relink/Partial” features**: There’s no JCR relationship tree—rollout is by file/folder; detaching means just breaking the association


### Some Q+A from the DA MSM show and tell in April 2026


---

**Q:** In the new DA MSM feature, Can we override block / part of page also at site level?


**A:** Effectively, yes. But it's accomplished differently. It's the same as how DA handles inheritance breaking with localization.


---

**Q:** So block-level inheritance is supported in xwalk, but it's only page-level inheritance in DA, correct?


**A:** Block-level inheritance is supported, it's just done slightly differently, and recommended against in both cases due to excess complexity. (Have you ever seen a successful implementation? I mean where authors were not confused...)


---

**Q:** In good olde AEM MSM, Page Editor displays notifications on inherited content.  Will there be similar notification when viewing inherited DA Authored Content in UE?


**A:** TBD, possibly will add a 'diff view' in Experience Workshop.


---


#### **Summary Table (from FluffyJaws)**

Blueprint/live copy structure

Full JCR deep node trees

Same as AEM (JCR)

Folder/file hierarchy

Rollout hooks, pre/post custom logic

Yes

Partially (AEM only)

No

Partial inheritance/override (component level)

Yes

Yes (from AEM)

No (folder/file level only)

Inheritance visualization

Live Relationship UI

In AEM only

Planned for DA UI

Language master / translation integration

Yes

Yes

Yes (metadata/folder level)

Access controls

Fine-grained in AEM

As in AEM

By doc repo (coarse)

Custom rollout logic

Yes

Yes (from AEM) / No in EDS

No

Authoring UX

Complex, deep

AEM only

Simpler, doc-based

Self-service/automation

Limited, technical

As in AEM

High—Power Automate/doc workflows

Orphan prevention / drift mgmt

Unsolved (still a challenge)

Unsolved

Simpler, no JCR inherits

Rollout across hundreds of sites

Supported, but can be slow/error-prone

As in AEM

Efficient for simple structures


---


#### **Lost Features**

- **AEM Crosswalk MSM** loses nothing (all MSM capabilities are as in AEM), but is limited by the legacy JCR-centric patterns and still relies on AEM for every MSM/rollout action.
- **DA MSM** trades granularity and deep control for **authoring agility and ease**—you’ll **lose**:
- “Partial” node override/inheritance at component/granule level
- AEM-centric rollout triggers, before/after hooks, advanced workflow integration
- Classic visualization of inheritance/detachment (“Live Relationship UI”)
- **Both options**: Some traditional on-premise/AMS-specific workflow customizations, or anything deeply dependent on the JCR event system, will **not** be portable.


### Any reason to still go with XWalk over DA, especially as it relates to customers with MSM requirements?

If you only need in-context authoring (Universal Editor) and not the benefits of JCR, we recommend DA + UE.

Regarding MSM: Pure DA is currently the only path to support "MSM-like" features natively. DA + UE does not support this, yet. If you want guided authoring + MSM, XWalk is the better path.


#### In bullet point form:

- Bring existing Java stack but deliver through Edge Delivery = XWalk
- MSM + in-context authoring = XWalk
- da.live for everything else.

**Remember: MSM was not meant for translation/localization of content.**

- MSM is about automation of page inheritance, not translation and localization first.
- if you have a translation or localization need, you should be using the [translation and localization features of DA](https://docs.da.live/administrators/guides/translation-strategy). Imagine a global car company that has dealership satellite sites, or the US Army which has local recruitment sites. It's language-for-language.


---


### **References & Additional Reading**

- [MSM in EDS Crosswalk – Experience League Docs](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/integrations/msm)
- [DA MSM Early Access – DA Docs](https://docs.da.live/about/early-access/multi-site-manager)
- [AEM EDS Playground – Project Types & Patterns](https://wiki.corp.adobe.com/spaces/ACSUI/pages/3826493894/AEM+EDS+Playground+%E2%80%93+Project+Types+Patterns)
- [Hybrid/Incremental Migration Best Practices](https://adobe.sharepoint.com/sites/AEMSuccessEngineering/Shared%20Documents/AEM%20Transformation-Communication/3%20%E2%80%94%20Community,%20%20Communication%20&%20Enablement/Community%20Mgmt/AdatpTo/Adapto%2024/AdaptTo\(\)%2024%20Panel.docx)
- [https://docs.da.live/administrators/guides/translation-strategy](https://docs.da.live/administrators/guides/translation-strategy)

From Dirk Rudolph, June 2026:

# FAQ


### Can I use DA in production?

Yes. DA is an Edge Delivery Services document-based content provider just like SharePoint and Google Drive. You are encouraged to use it in production if it fits your needs.

**Note:** if you choose to use DA in production context:

1. AEM Edge Delivery Services still provides 99.99% uptime for the delivery of your site.
2. Please reach out on Slack, Discord, or Teams to ensure you are supported through the lifecycle of your project.


### Who is working on and supporting this?

Several teams across Adobe working in collaboration with Adobe partners. You can see the full list:

[https://github.com/adobe/da-live/graphs/contributors](https://github.com/adobe/da-live/graphs/contributors)

[https://github.com/adobe/da-admin/graphs/contributors](https://github.com/adobe/da-admin/graphs/contributors)


### Do I have to do another migration if I'm already on Google Drive or SharePoint?

It's a bit nuanced, but the short answer is no. DA stores content in the same format as AEM Edge Delivery Services. This has the following implications:

1. There's no transformation of content needed when using DA's importer.
2. No changes to your project codebase need to be done.
3. If you decided DA does not fit your project, your content can be moved back to SharePoint or Google Drive easily.


### Where is DA content stored?

DA content is stored within the same infrastructure as AEM Edge Delivery Services. It is stored free from other cloud providers like Java-based AEM (JCR), SharePoint, OneDrive, Google Drive, Dropbox, etc.


### Does this mean SharePoint or Google Drive go away?

Probably not.

First, both systems are almost always used when making content that funnels into your web content management system (WCMS).

Also, depending on the persona, the familiarity of SharePoint or Google Drive may still be the preferred authoring tool. If you have multiple projects, you may find SharePoint or Google Drive is a better fit for some authors.


### What is your roadmap?

The current roadmap is based on blocking issues for VIP customers and items that improve operational efficiency. You can visit: [https://da.live/roadmap](https://da.live/roadmap) to see what the team has committed to, and [https://da.live/ideas](https://da.live/ideas) to see what the community is thinking about, but we have not necessarily committed to.


### What are the licensing costs?

At the time of writing, DA is available to all Edge Delivery customers.


### Does this mean AEM authoring tools (Touch UI, Universal Editor, etc.) go away?

No.

We believe there's a lot of different use cases to cover, and DA is only one path. DA is not meant to be an SPA editor or an in-context (wysiwyg) editor. It's un-apologetically a document-based editor meant to be compatible with CMD + A, CMD + C, CMD + V workflows from Word or Google Docs. AEM's Universal Editor also supports content stored on DA.


### What started this?

While many projects can benefit from the approachability of known content management systems, other projects can benefit from the tools and technologies provided by the Adobe Experience Cloud ecosystem. DA was built to provide a familiar document authoring interface and pair it with content management APIs backed by Adobe services.


### Does DA plan to have approval workflows similar to what AEM Author provides?

Our current approval workflows are solved via AEM Snapshots. We hope to expand to more complex use cases (notifications, multi-step, etc.) via Workfront, but we are waiting on a customer to need this before building anything.


### Any reason to still go with XWalk over DA, especially as it relates to customers with MSM requirements?

If you only need in-context authoring (Universal Editor) and not the benefits of JCR, we recommend DA + UE.

Regarding MSM: Pure DA is currently the only path to support "MSM-like" features natively. DA + UE does not support this, yet. If you want guided authoring + MSM, XWalk is the better path.


#### In bullet point form:

- Bring existing Java stack but deliver through Edge Delivery = XWalk
- MSM + in-context authoring = XWalk
- da.live for everything else.


### For spreadsheets that use Excel formulas (e.g. AEM Forms), is it still recommended to use Sharepoint/Google for this specific use case?

If you have complex sheet needs (formulas, conditional formatting, more than 5,000 rows), we recommend a content overlay architecture where you store. You can link out to your complex sheets in DA for a seamless experience. [https://da.live](https://da.live/) \> browse > New > Link

If you have forms needs, you are encouraged to get off the deprecated "Helix 4" Forms solution and use something else instead.


### For customers with an 6.5 AMS license that are considering EDS + DA, can they still use the Assets Selector that points to their 6.5 instance?

There is research being done on this topic for a customer. We will update this when we have more to share.


### What translation providers do you support?

As of now, the only publicly available translation providers we support are: Google Translate & Smartling. If you have specific needs (including a different provider), please get in touch.


### Is there any documentation around configuring blocks component-model.json in the UE, specifically for DA?

You are encouraged to look at our reference implementation here: [https://github.com/aemsites/da-block-collection](https://github.com/aemsites/da-block-collection)


### I believe I found a bug with DA, can I open a Github issue for it (as well as a PR fix)?

Yes! [https://da.live/ideas](https://da.live/ideas).

If you wish to submit a PR, please reach out to the team first to ensure everyone is aligned on the feature or fix.


# Customer FAQs

Field-ready answers to the questions and objections that come up most often in EDS customer conversations. Each answer has a short version for quick use and a nuanced version for deeper discussions.

For the full feature comparison across authoring models, see the [Feature Support Comparison Matrix →](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/feature-support-comparison)


## Is EDS suitable for large enterprise projects?

**Short answer:** Yes. EDS supports both small and large global sites.

**Nuanced answer:** Enterprise fit depends on architecture and authoring choices, not on whether the site uses blocks. Large enterprises need to think carefully about governance, multi-site management, backend integration patterns, and authoring model selection. These are all solvable — but they require architecture conversations, not dismissal of EDS as "only for simple sites."

**What to recommend:** Discuss authoring model options early. For governance-heavy enterprises, AEM Authoring + Universal Editor provides the full AEM governance stack. For teams prioritising speed and simplicity, DA.live is a strong enterprise choice.

[Feature support comparison →](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/feature-support-comparison) | [Authoring decision guide →](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring)


## Which authoring option should we use?

**Short answer:** It depends on your team, your content, and your governance requirements — but there is a clear framework for making the decision.

**Nuanced answer:** Three models are available:

- **Document-based (SharePoint / Google Docs):** Best for teams already in Microsoft 365 or Google Workspace. Fastest time to first publish. Best for document-centric content and FedRAMP scenarios.
- **DA.live:** Adobe-native document authoring. Default for many new EDS builds and migrations from WordPress/Drupal. Growing governance capabilities. No third-party dependency.
- **AEM Authoring + Universal Editor:** Best for teams migrating from AEM Sites who need to retain full AEM governance — MSM, Launches, Content Fragments, AEM workflows. Highest setup cost.

**What to recommend:** When in doubt, default to the simpler option. DA.live or Document-based authoring gets customers to value faster. Universal Editor can be introduced later.

[Full authoring decision guide →](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring)


## Is DA.live only for simple content?

**Short answer:** No. DA.live is not a lightweight option — it is the default authoring model for many new EDS projects including complex, large-scale sites.

**Nuanced answer:** DA.live provides visual authoring, built-in versioning and collaboration, review and approval workflows, access controls, and growing feature parity with enterprise CMS expectations. It is actively being developed. The correct framing is not "DA.live vs. full CMS" but "DA.live for teams who want Adobe-native authoring without the full AEM governance overhead."

**What to recommend:** Evaluate specific governance requirements. Many requirements that seem to need full AEM governance are actually workflow and approval requirements that DA.live handles well.

[See governance capabilities by model →](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/feature-support-comparison)


## Can we still have governance and approvals?

**Short answer:** Yes — the mechanism depends on the authoring model chosen.

**Nuanced answer:**

- **DA.live:** Built-in review and approval workflows, access controls, and versioning
- **AEM Authoring + Universal Editor:** Full AEM workflow engine, granular user/group permissions, MSM, Launches
- **Document-based (SharePoint):** Approval workflows via SharePoint/Microsoft 365 tools

**What to recommend:** For customers with complex, regulated approval requirements, AEM Authoring + UE provides the most comprehensive governance stack. For most other cases, DA.live is sufficient and faster to implement.


## Can we use Adobe Target and run experiments?

**Short answer:** Yes — all three EDS authoring models support Target integration and EDS's built-in experimentation framework.

**Nuanced answer:** EDS has a built-in A/B experimentation framework that works across all authoring models. Adobe Target integrates via standard JavaScript. ContextHub (AEM's server-side personalisation engine) is AEM-specific and is not available in EDS delivery — personalisation in EDS is delivered client-side via Target, Adobe CDP, or custom edge-side integrations.

**What to recommend:** EDS experimentation is lightweight, fast, and built for Core Web Vitals safety. Lead with EDS experimentation for A/B testing. Bring in Target for more complex segmentation and personalisation scenarios.


## What happens to our backend business logic?

**Short answer:** It needs to be re-architected. Backend Java/OSGi/servlet logic does not carry forward to any EDS authoring model.

**Nuanced answer:** This is the most important expectation to set in any EDS migration conversation. EDS is a client-side, edge-delivered platform. There is no application server, no OSGi container, no Sling resolver on the delivery path. Backend logic must be re-architected to:

- **APIs** — REST or GraphQL endpoints called by block JavaScript
- **Adobe App Builder** — Adobe's recommended serverless platform for backend business logic and integrations
- **Edge Workers** — For logic that must intercept the request/response cycle at the CDN edge

**What to recommend:** Identify backend dependencies early — in the first architecture conversation, not during build. Position App Builder as a modernisation opportunity, not a workaround. Customers who discover this mid-engagement feel misled.

[What lives in blocks vs. backend →](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/architecture-and-key-concepts)


## What about forms, translations, and launches?

**Short answer:** All are supported — support levels vary by authoring model.

**Forms:** EDS Forms is available across all EDS authoring models. AEM Forms (full enterprise forms with backend workflows) is available with AEM Authoring + Universal Editor.

**Translations:** Full AEM translation framework is available with AEM Authoring + UE. DA.live translation support is growing. Document-based authoring typically requires manual or third-party translation connectors.

**Launches / scheduled publishing:** Full AEM Launches support is available with AEM Authoring + UE. DA.live has growing support — check the current roadmap. Document-based authoring requires manual publish timing.

**What to recommend:** If translation automation and Launches are critical, AEM Authoring + UE is the strongest choice today. Verify DA.live roadmap for current capabilities before committing.

[Full capability comparison →](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/feature-support-comparison)


## Can we use Content Fragments or Experience Fragments?

**Short answer:** Content Fragments and Experience Fragments are supported with AEM Authoring + Universal Editor. DA.live supports fragments via DA's own fragment model.

**Nuanced answer:** AEM Content Fragments and Experience Fragments are AEM-native constructs — they are fully supported when using AEM Authoring + Universal Editor as the authoring model. DA.live has its own fragment and shared content model that covers many of the same reuse scenarios. Document-based authoring supports shared sections via document includes.

**What to recommend:** If the customer is heavily invested in AEM Content Fragments or Experience Fragments, that is a signal toward AEM Authoring + UE. For new projects without existing AEM fragment libraries, DA.live fragments are a lighter-weight alternative.


## Can we keep SharePoint or Google Docs as our authoring tool?

**Short answer:** Yes — document-based authoring with SharePoint or Google Docs is a fully supported and recommended path for the right customer profile.

**Nuanced answer:** Document-based authoring is not a "starter" option — it is the right long-term model for teams who are already deeply embedded in Microsoft 365 or Google Workspace. It offers the fastest time to first publish, minimal author retraining, and strong alignment with document-centric content workflows. It is also the recommended path for regulated industries and FedRAMP scenarios where SharePoint's compliance posture is required.

**What to recommend:** If the team lives in SharePoint or Google Docs and the content is primarily document-like, document-based authoring is the best fit. Do not push a more complex model on a team that does not need it.


## EDS terminology — quick reference

These terms come up constantly in customer conversations and mean different things in different contexts. Know the distinctions.

**Term**

**What it means in EDS context**

AEM

Can refer to: (a) traditional AEM Sites, (b) AEM as a Cloud Service, or (c) the AEM authoring model for EDS (AEM Authoring + Universal Editor). Clarify which is meant in every conversation.

DA.live / Document Authoring

Adobe's native web-based authoring platform for EDS. Not the same as SharePoint/Google Docs authoring. A distinct Adobe product with its own URL (da.live).

Universal Editor

The in-context WYSIWYG editing interface used with AEM Authoring + EDS. Not the same as DA.live. Requires AEM Sites content structures behind it.

Crosswalk

An older internal name for the AEM Authoring + Universal Editor + EDS delivery combination. You may hear this term from partners or in older documentation. The current preferred term is "AEM Authoring + Universal Editor."

Document-based authoring

Using Microsoft Word/SharePoint or Google Docs/Drive as the content authoring source for EDS. Not the same as DA.live, even though both involve document-like authoring.

EDS / Edge Delivery Services

The delivery layer — the edge network, content bus, and Sidekick publish workflow. EDS is not an authoring tool — it is how content gets from any authoring source to end users at the edge.

Blocks

The EDS component model. Every visual element on an EDS page is a block. Blocks are not a limitation — they are a simplified, performant alternative to traditional CMS components.


# Choosing the Right Authoring in EDS

One of the most consequential decisions in any EDS engagement. A CSME's job is to guide customers to the authoring model that fits their team, their content, their governance needs, and their long-term goals — not just the most familiar or most impressive-looking option.

**The recommendation hierarchy:** **DA.live is the recommended default** for most new EDS projects. AEM + Universal Editor is the right fit when deeper AEM-native governance or mature AEM authoring workflows are needed. SharePoint / Google Docs are supported in constrained or transitional cases — but are less flexible and less preferred than DA.live. "Supported" does not mean equally recommended.

**Quick links:** [The three options](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring#the-three-options "The three options") · [Universal Editor explained](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring#universal-editor-explained "Universal Editor explained") · [Who should choose what](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring#who-should-choose-what "Who should choose what") · [Governance by model](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring#governance-by-model "Governance by model") · [Decision framework](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring#decision-framework "Decision framework") · [Running the workshop](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring#running-the-workshop "Running the workshop") · [Feature comparison](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/choosing-the-right-authoring#feature-comparison "Feature comparison")


## The three authoring options

These three paths are not equal choices — there is a clear recommendation hierarchy. Use the guidance below to lead customers to the right model, not just present all three as equivalent options.

-


### 🖊️ DA.live (AEM Document Authoring) — Recommended default

**Recommended default for most new EDS builds** and migrations from WordPress, Drupal, and other non-AEM CMSs. Adobe-native, web-based document authoring with no dependency on third-party platforms.

- Adobe-native — no third-party document platform dependency
- Built-in versioning, collaboration, and content hierarchy
- Familiar document-like editing experience for non-technical authors
- Growing governance capabilities — not a lightweight option
- Ideal for teams who want Adobe-native authoring without Universal Editor complexity
- Fastest path to value for most non-AEM migration scenarios

**When to choose this:** New EDS projects, migrations from non-AEM CMSs, or any scenario where a firm Microsoft 365 / Google Workspace dependency does not exist.

**When not to choose this:** When full AEM governance patterns (MSM, Launches, full DAM workflow) are non-negotiable.

[DA.live →](https://da.live/)

-


### 🖥️ AEM Authoring + Universal Editor — When stronger governance is needed

The right fit when customers need **deeper AEM-native governance or mature AEM authoring workflows**. In-context WYSIWYG editing on the rendered page, backed by AEM Sites structures or Content Fragments.

- Full visual editing — authors see exactly what the page will look like
- Strongest for enterprise governance, MSM, Content Fragments, and AEM-familiar teams
- Supports complex, design-driven page layouts
- Higher setup cost — requires developer involvement for component and content model configuration
- Ideal for teams migrating from AEM Sites who need to retain AEM governance patterns

**When to choose this:** AEM Sites migrations with complex component structures. Enterprise multi-site management (MSM/blueprints). When AEM translation framework or Launches are required. Teams with an existing AEM authoring team and capacity for content model setup.

**When not to choose this:** When speed to launch is the top priority. When the team has no AEM authoring background. Do not default here — the setup cost and complexity are significant.

[Universal Editor Docs →](https://www.aem.live/docs/aem-authoring#universal-editor)

-


### 📄 Document-based Authoring (SharePoint / Google Docs) — Supported, not the first recommendation

**Supported, but less flexible and less preferred than DA.live** for most new EDS builds. Acceptable for teams with a firm, existing Microsoft 365 or Google Workspace commitment, or for constrained and transitional migration scenarios.

- Zero CMS learning curve for authors already in Microsoft 365 or Google Workspace
- Instant content updates — publish in seconds
- Best for content-heavy sites with structured, repeating page types
- Strong choice for regulated/FedRAMP scenarios where SharePoint compliance is a hard requirement
- Limited governance — workflow, MSM, and Launches not natively supported

**When to choose this:** Teams with a firm existing Microsoft 365 or Google Workspace dependency and no appetite for new authoring tooling. Regulated/FedRAMP scenarios requiring SharePoint's compliance certification. Short-term transitional migrations.

**When not to choose this:** As a default for new EDS projects — DA.live is the better default. When authors need visual editing. When governance requirements exceed what a document model supports. Do not present this as equally recommended alongside DA.live.

[SharePoint Setup →](https://www.aem.live/docs/setup-adobe-sharepoint) | [Google Drive Setup →](https://www.aem.live/docs/setup-googledrive)


## Universal Editor — what it is and where it fits

Universal Editor is frequently misunderstood. Here is what every CSME should be able to say clearly:

-


### What it is

Universal Editor is an **in-context authoring interface** — authors edit content directly on the rendered, live-looking page rather than in a form or document. It is part of the AEM authoring experience and is used alongside AEM Sites content structures.

-


### Where it fits

Universal Editor is the right choice when customers need **full AEM governance**, are migrating from AEM Sites with complex component structures, or require the highest level of visual authoring fidelity. It is not a replacement for all legacy AEM authoring patterns — some patterns still require re-architecture.

-


### What it is not

Universal Editor is **not the default EDS authoring model**. It is not required for EDS delivery. Most new EDS projects — especially migrations from WordPress, Drupal, or non-AEM CMSs — are better served by **DA.live**, which is the recommended default. Present Universal Editor only when the governance or workflow case genuinely warrants it.

-


### AEM naming clarification

"AEM authoring" in an EDS context means **AEM Sites-backed content + Universal Editor** for the editing interface — sometimes called Crosswalk. This is distinct from DA.live (Document Authoring), which is a separate Adobe-native authoring platform and the recommended default for most new EDS builds.


## Who should choose what?

Use this as a field reference for customer conversations. The right choice depends on the customer's team, content strategy, and governance requirements. **When no strong signal points to Document-based or AEM + UE, default to DA.live.**

**Customer profile**

**Recommended model**

**Why**

New EDS build, migrating from WordPress / Drupal / other non-AEM CMS

🖊️ DA.live — Recommended default

Adobe-native, no third-party dependency, strong governance, good for content migration scenarios. This is the default recommendation for most new EDS projects.

Team wants Adobe-native authoring without Universal Editor complexity

🖊️ DA.live — Recommended default

DA.live provides visual authoring in a lighter-weight Adobe-native environment. No third-party platform dependency.

Time-to-go-live is the top priority

🖊️ DA.live (preferred) or 📄 Document-based

Both get teams to first publish quickly. Universal Editor setup takes days to weeks. DA.live is preferred over Document-based unless a firm Microsoft 365 / Google Workspace dependency exists.

Marketing team, high content velocity, simple page types

🖊️ DA.live (preferred) or 📄 Document-based

Fast publishing cycles. Low learning curve. Author empowerment without developer dependency. Prefer DA.live as it offers more flexibility and governance than Document-based.

Team migrating from AEM Sites, needs to retain AEM governance patterns

🖥️ AEM Authoring + Universal Editor

Familiar authoring paradigm. Full MSM, Content Fragments, Launches, DAM workflow. Hybrid AEM/EDS delivery.

Enterprise with complex governance, approval workflows, multi-site management

🖥️ AEM Authoring + Universal Editor

Full AEM governance stack. MSM/blueprints, Launches, structured content fragments, granular permissions.

Highly designed, component-driven pages requiring pixel-level precision

🖥️ AEM Authoring + Universal Editor

Full WYSIWYG in-context editing. Best fidelity for complex, design-driven page layouts.

Team already in Microsoft 365 or Google Workspace, firm platform dependency

📄 Document-based (SharePoint / Google Docs) — Supported, not the first recommendation

Zero retraining for authors already in these tools. Fastest time to first publish in this specific scenario. Only recommend here when the platform dependency is firm — otherwise DA.live is preferred.

Regulated industry, FedRAMP, or data residency requirements (hard SharePoint requirement)

📄 Document-based (SharePoint) — Supported, not the first recommendation

SharePoint's compliance posture and FedRAMP certification cover many regulated scenarios. This is a specific compliance-driven case, not a general recommendation.


## Governance by authoring model

Governance is one of the most common objections. Know what each model genuinely supports before the conversation starts. **DA.live covers the majority of governance needs for most new EDS projects.** Only escalate to AEM + UE when the governance gap is real and specific.

**Governance need**

**🖊️ DA.live (Recommended default)**

**🖥️ AEM Authoring + UE**

**📄 Document-based (Supported, less preferred)**

Approval workflows before publish

✅ Built-in review and approval

✅ Full AEM workflow engine

⚠️ Via SharePoint/Google Workspace workflow tools — less integrated

Role-based access control

✅ DA.live access controls

✅ Full AEM user/group permissions

⚠️ Via SharePoint / Google Drive permissions — less granular

Multi-site management (MSM / blueprints)

⚠️ Limited — use path-based multi-site structuring

✅ Full AEM MSM and live copy support

❌ Not supported

Scheduled publishing / Launches

⚠️ Growing — check current DA.live roadmap

✅ Full AEM Launches support

❌ Manual

Versioning and content history

✅ Built-in versioning

✅ Full AEM versioning

⚠️ Via SharePoint/Google version history — outside the authoring platform

Translation / localisation workflow

⚠️ Growing support

✅ Full AEM translation framework

⚠️ Manual or via third-party connectors — limited

Backend Java/OSGi/servlet logic

❌ Requires re-architecture to APIs / App Builder / Edge Workers

❌ Requires re-architecture to APIs / App Builder / Edge Workers

❌ Requires re-architecture to APIs / App Builder / Edge Workers

⚠️ **Important:** Backend Java/OSGi/servlet patterns do not carry forward to any EDS authoring model. They must be re-architected to APIs, Adobe App Builder, or Edge Workers. This is true regardless of which authoring model is chosen — set this expectation early.


## CSME decision framework

Use these questions to narrow down the right authoring choice. **Start from DA.live as the default and look for signals that justify choosing something different.**

**Question**

**Signal and guidance**

Is this a new EDS build or migration from WordPress / Drupal / a non-AEM CMS?

→ **Default to DA.live**. This is the recommended starting point for most new EDS projects and non-AEM migrations.

Does the customer want to stay in the Adobe ecosystem without third-party doc tools?

→ **DA.live** — Adobe-native, no SharePoint or Google Workspace dependency.

Is time-to-go-live the top priority?

→ **DA.live** (preferred) or Document-based — both are fast to first publish. Universal Editor setup takes significantly longer.

Does the customer have a firm, existing Microsoft 365 or Google Workspace dependency and no appetite for new tooling?

→ Document-based may be appropriate — but validate that DA.live has not already been evaluated. This is a constrained-case exception, not a default.

Is the customer migrating from AEM Sites with complex component structures?

→ **AEM Authoring + Universal Editor** (familiar paradigm) or consider DA.live if governance simplification is acceptable.

Does the customer require full MSM, Launches, or AEM translation framework?

→ **AEM Authoring + Universal Editor** — these features require the full AEM governance stack.

Is there a dedicated AEM development team with capacity for content model setup?

→ Universal Editor becomes viable — the setup cost is justified when the team already has AEM capacity.

Do authors need to see the final rendered page while editing?

→ Universal Editor (full WYSIWYG) — or DA.live for a growing visual authoring experience with lower setup cost.

Are there highly designed, component-driven pages requiring pixel-level precision?

→ Universal Editor — best in-context editing fidelity for complex, design-driven layouts.

Does the customer have backend Java/servlet logic they expect to carry over?

→ Architecture conversation needed — this requires re-architecture regardless of authoring model.


## How CSME runs the authoring decision workshop

For customers who are undecided, run a structured 1–2 hour workshop:

1. **Author persona mapping (20 mins):** Identify who the content authors are, what tools they use today, and what their comfort level is with new tools.
2. **Live demo of all three options (40 mins):** Show a real authoring workflow for each option — creating a page, adding a block, publishing. Let the audience react to what they see. Lead with DA.live.
3. **Decision criteria review (20 mins):** Walk through the governance table and decision framework above in the context of their specific site and team. Ask explicitly: what would prevent DA.live from being the right choice here?
4. **Recommendation and rationale (20 mins):** Present your CSME recommendation with clear reasoning. Be direct — customers value a confident recommendation over "it depends." Unless there is a clear signal for Document-based or AEM + UE, recommend DA.live.


### CSME tip

When in doubt, default to DA.live. It is the recommended default authoring model for most new EDS projects — Adobe-native, no third-party dependency, and growing fast in governance capability. Document-based (SharePoint/Google Docs) is supported but less flexible and less preferred. Universal Editor can always be introduced later as the team matures on EDS and a specific governance need emerges. The cost of starting with a more complex authoring model and abandoning it is high.


## Feature support comparison — at a glance

This comparison helps CSMEs recommend the right authoring model. The columns are ordered by recommendation priority: **DA.live is the recommended default** for most new EDS builds. AEM Authoring + UE is right when governance demands it. SharePoint / Google Docs are supported for constrained cases — not equal strategic peers to DA.live.


| Capability | Traditional AEM Sites | EDS + AEM Authoring / UE | EDS + DA.live | EDS + SharePoint / GDocs |
|---|---|---|---|---|
| Visual / in-context editing | ✅ Full | ✅ Full WYSIWYG via UE | ✅ Visual document editing | ❌ Document-only |
| MSM / Live Copy / Blueprints | ✅ Full | ✅ Full | ⚠️ Limited | ❌ Not supported |
| Approval workflows | ✅ Full AEM workflow | ✅ Full AEM workflow | ✅ DA.live workflows | ⚠️ Via SP/GDrive tools only |
| Launches / scheduled publishing | ✅ Full | ✅ Full | ⚠️ Growing | ❌ Manual |
| Content / Experience Fragments | ✅ Full | ✅ Full | ⚠️ Fragments via DA | ⚠️ Limited |
| DAM / Dynamic Media / Scene7 | ✅ Full | ✅ Full | ✅ AEM Assets integration | ⚠️ Via integration |
| Personalisation / Target / experiments | ✅ Full ContextHub + Target | ✅ Target + EDS experimentation | ✅ EDS experimentation + Target | ✅ EDS experimentation + Target |
| Translation / localisation | ✅ Full AEM translation | ✅ Full AEM translation | ⚠️ Growing | ⚠️ Manual / third-party |
| Forms | ✅ AEM Forms | ✅ AEM Forms / EDS Forms | ✅ EDS Forms | ✅ EDS Forms |
| Vanity URLs / redirects | ✅ Full | ✅ EDS redirects sheet | ✅ EDS redirects sheet | ✅ EDS redirects sheet |
| Backend Java/OSGi/servlet logic | ✅ Native | ❌ Re-architecture required | ❌ Re-architecture required | ❌ Re-architecture required |
| Lighthouse / Core Web Vitals | ⚠️ Variable | ✅ 100/100 by default | ✅ 100/100 by default | ✅ 100/100 by default |

**Reading this table:** DA.live meets the majority of governance needs for most new EDS projects. AEM + UE offers a fuller governance stack — use it when the gap is real and specific. SharePoint / Google Docs are supported but score consistently lower on governance capabilities — they are a constrained-case option, not a strategic peer to DA.live.

**[Full feature support comparison matrix with notes →](https://dev--csme-eds-coe--kailasnadh790.aem.page/prepare/feature-support-comparison)**
