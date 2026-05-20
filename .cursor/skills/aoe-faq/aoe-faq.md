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

**Q:** Can I see the architecture?

**A:** Yes. Up-to-date diagrams and information are here: [Helix architecture docs](https://github.com/adobe/helix-home/blob/main/docs/architecture.md). The main content-bus to HTML rendering code is in [helix-html-pipeline](https://github.com/adobe/helix-html-pipeline).

---

**Q:** Is there any infrastructure redundancy?

**A:** From 2022-10-01, a second fully redundant delivery stack (CDN, compute, and storage) was built on Cloudflare, independent of the existing stack (Fastly, AWS Lambda, and S3). Delivery can be switched via a DNS CNAME change during a Fastly or AWS outage. Both stacks run in parallel at all times.  _(Source as in your notes.)_

---

**Q:** Does Franklin have a concept of Dev, Staging, and Prod environments?

**A:** "Dev" and "staging" are not fixed Franklin concepts. For code, every branch gets a unique URL. You can link to a branch like `[https://98-cookie-style--vg-volvotrucks-us--hlxsites.hlx.page/drafts/wingeier/](https://98-cookie-style--vg-volvotrucks-us--hlxsites.hlx.page/drafts/wingeier/)` and Helix rewrites it to the related site (e.g. `[http://volvotrucks.us/drafts/wingeier](http://volvotrucks.us/drafts/wingeier)`). You can also point `[stage.customerdomain.com](http://stage.customerdomain.com)` at the `.page` URLs if you want.

---

**Q:** Can we hide our `.page` (preview) content from the public?

**A:** That is the customer's responsibility. You could implement a frontend redirect (e.g. if the URL ends in `.page`, check a cookie and send users to login if missing), but the added complexity is often not worth it. Preview content is not indexed by default, which is the main concern.

---

**Q:** Can I have more than one Franklin project?

**A:** You can fork a project when there are heavy post-go-live dev changes, with the goal of merging again later. Linking across Franklin projects can be tricky: the pipeline rewrites links. Example: linking from [hlx.live block-collection docs](https://www.hlx.live/developer/block-collection/links) to `[https://main--helix-block-collection--adobe.hlx.page/block-collection/links](https://main--helix-block-collection--adobe.hlx.page/block-collection/links)` gets rewritten; a workaround is an intermediary link (e.g. Bitly) as in your example.

---

**Q:** What serverless frameworks can I use?

**A:** The platform is generally serverless-framework agnostic; you supply your own serverless layer. Cloudflare Workers are often very fast and easy to onboard. AIO, Lambda, Azure Functions, etc. are fine as long as Franklin can reach the endpoint and responses are fast. If there is no direct serverless endpoint but App Builder / IO is available, that can be used.  _(Source as in your notes.)_

---

**Q:** How can we handle pollers or periodic server-side preprocessing from other systems with Franklin (similar to AEM workflows/schedulers)?

**A:** Use Cloudflare Workers or any serverless endpoint for extra logic. Example repo: [hlxsites/wesco-cloudflare-worker](https://github.com/hlxsites/wesco-cloudflare-worker). External workflow tools (e.g. Workfront) can be involved. Preprocessing often goes through SharePoint / Google Drive APIs. Pattern: bring your own serverless, then consume results on the client (e.g. a custom block that fetches and uses the data) or on the authoring side (e.g. generated Excel/Word).  _(Source as in your notes.)_
  
  
  
## Authoring

**Q:** Can Adobe Express be used for content authoring?

**A:** No, as of 2023-03-01. A Sidekick plugin using the [Express Embed SDK](https://developer.adobe.com/embed-sdk/docs/) could appear later (e.g. for banners inside a Word doc).

---

**Q:** Can I build a Sidekick plugin (e.g. an authoring guide)?

**A:** Yes. Examples: [adobe/blog `tools/sidekick`](https://github.com/adobe/blog/tree/main/tools/sidekick), [adobe/express-website `tools/sidekick`](https://github.com/adobe/express-website/tree/main/tools/sidekick), [adobecom/milo `tools/sidekick`](https://github.com/adobecom/milo/tree/main/tools/sidekick) (Milo spreads Sidekick-related files across the repo).

---

**Q:** Can I use Chinese / Japanese / etc. characters in URLs?

**A:** No--only Latin characters. Non-ASCII paths were blocked because most such traffic was abusive; distinguishing good from bad was impractical.  _(Source as in your notes.)_

---

**Q:** Does AEM Franklin support workflows?

**A:** As of 2023-02-02, AEM Franklin supports access controls for publishing and defers workflows to the content source. For SharePoint (Word/Excel), Adobe Workfront is recommended for document review and approval.

---

**Q:** Are there page versions? What if someone accidentally deletes something?

**A:** _(Not provided in your text—add the official answer here when you have it.)_

---

### **Multi-Site Manager (MSM): Traditional AEM vs. EDS via Crosswalk vs. DA Authoring**
Below is a **precise, actionable comparison** for customers currently relying on MSM in traditional AEM who are evaluating continuing with MSM via Crosswalk (AEM-driven content for EDS), or migrating to Document Authoring (DA) leveraging the new DA MSM model.
* * *
#### **1\. MSM in Traditional AEM Sites**
  * **Architecture** : Deeply integrated--segment "blueprints," "live copies," and inheritance rules at the JCR/repository level, with built-in support for:
    * Rollouts, relinking, detachment, partial overrides
    * Automated/triggered updates (pre/post rollout hooks)
    * Content hierarchy, language-masters, Live Relationship UI
    * Synchronization managed via JCR and replication agents
  * **Strengths** :
    * Very fine-grained control
    * Mature, supports complex global content governance, deep overrides, partial inheritance
    * Tightly coupled to workflows and access controls in AEM
  * **Limitations** (in cloud and on-prem): Complexity, heavy authoring learning curve, can create orphaned or stale live copies, dependency on classic UI (being deprecated), slow rollouts for large webs.

* * *
#### **2\. MSM for EDS via Crosswalk (AEM as Content Source for EDS)**
  * **How it works** : Authors still create/manipulate MSM blueprints/live copies in AEM; EDS (via "Crosswalk") pulls content from these live copies and delivers it with block-based rendering.
  * **Feature Support** :
    * **Inheritance of structure and content** (as managed in AEM)
    * **Rollouts, detachment, partial override** , full or partial as per AEM MSM
    * **Workflow, translation, launches** : as managed in AEM, visible within EDS-rendered pages.
    * **Language copies, regionalization** : preserved exactly as in AEM
    * **Content governance** and permissions managed in AEM
  * **Limitations Compared to Traditional** :
    * **Experience Editor/Touch UI only** (classic UI deprecated)
    * **AEM-centric authoring** : Authors must use AEM for all MSM operations (including intense enterprise permissions and config)
    * **EDS-specific limitations** :
      * No real-time preview of MSM-edited or "rolled out" changes until published to EDS (the Crosswalk agent acts as a replication endpoint)
      * Some advanced replication hooks, "pre-rollout" and "post-rollout" custom logic may not execute in EDS context
    * **Still inherits all AEM MSM quirks** : Orphaning, (in)consistent rollout triggers, etc.

* * *
#### **3\. MSM for DA Authoring (Native to EDS/Document Authoring)**
  * **How it works** : Uses DA MSM, now available in Early Access (see [DA MSM docs](https://docs.da.live/about/early-access/multi-site-manager)). All content is managed using SharePoint/Drive/docs, and MSM-like inheritance and rollout is managed via DA interfaces and APIs--not JCR. [https://github.com/da-sites/da-msm](https://github.com/da-sites/da-msm)
  * **Feature Support** :
    * **Blueprints and live copies** managed at the document/folder level, not in JCR
    * **Copy and update strategies** : Inheritance patterns like in AEM, but for folder/doc structure, not node hierarchies
    * **Bulk rollout and localized overrides** : Supported in flat-file/doc/metadata patterns
    * **Automation** : Rollout jobs, locale-specific metadata, update propagation
  * **Strengths** :
    * **Much lighter, easier authoring** : Any approved user, no AEM login needed for day-to-day content changes or inheritance
    * **Self-service rollouts/updates** : Via doc moves, DA UI, or Power Automate flows. Easier for non-technical teams
    * **Global/locale variants** : Folder-level structure fits global content governance in a simpler model; overrides via file/folder copy, metadata "diffs"
  * **Limitations Compared to Traditional/AEM MSM** :
    * **No JCR-level hooks** : No classic AEM workflow/replication triggers, no before/after rollout hooks, no direct integration with AEM Launches
    * **Inheritance is simpler/saner, but less nuanced** : All-or-nothing file/folder overrides; less support for "partial component" overrides
    * **No access to AEM 's workflow and inline access controls**: Permissions controlled by doc repository (SharePoint, Google Drive) only
    * **Loss of fine-grained "Detach/Relink/Partial" features**: There's no JCR relationship tree--rollout is by file/folder; detaching means just breaking the association

  

### Some Q+A from the DA MSM show and tell in April 2026

**Q:** In the new DA MSM feature, Can we override block / part of page also at site level?

**A:** Effectively, yes. But it's accomplished differently. It's the same as how DA handles inheritance breaking with localization.

---

**Q:** So block-level inheritance is supported in xwalk, but it's only page-level inheritance in DA, correct?

**A:** Block-level inheritance is supported, it's just done slightly differently, and recommended against in both cases due to excess complexity. (Have you ever seen a successful implementation? I mean where authors were not confused...)

---

**Q:** In good olde AEM MSM, Page Editor displays notifications on inherited content.  Will there be similar notification when viewing inherited DA Authored Content in UE?

**A:** TBD, possibly will add a 'diff view' in Experience Workshop.
  

* * *
#### **Summary Table (from FluffyJaws)**
__
Blueprint/live copy structure| Full JCR deep node trees| Same as AEM (JCR)| Folder/file hierarchy  
---|---|---|---  
Rollout hooks, pre/post custom logic| Yes| Partially (AEM only)| No  
Partial inheritance/override (component level)| Yes| Yes (from AEM)| No (folder/file level only)  
Inheritance visualization| Live Relationship UI| In AEM only| Planned for DA UI  
Language master / translation integration| Yes| Yes| Yes (metadata/folder level)  
Access controls| Fine-grained in AEM| As in AEM| By doc repo (coarse)  
Custom rollout logic| Yes| Yes (from AEM) / No in EDS| No  
Authoring UX| Complex, deep| AEM only| Simpler, doc-based  
Self-service/automation| Limited, technical| As in AEM| High--Power Automate/doc workflows  
Orphan prevention / drift mgmt| Unsolved (still a challenge)| Unsolved| Simpler, no JCR inherits  
Rollout across hundreds of sites| Supported, but can be slow/error-prone| As in AEM| Efficient for simple structures  
* * *
#### **Lost Features**
  * **AEM Crosswalk MSM** loses nothing (all MSM capabilities are as in AEM), but is limited by the legacy JCR-centric patterns and still relies on AEM for every MSM/rollout action.
  * **DA MSM** trades granularity and deep control for **authoring agility and ease** --you'll **lose** :
    * "Partial" node override/inheritance at component/granule level
    * AEM-centric rollout triggers, before/after hooks, advanced workflow integration
    * Classic visualization of inheritance/detachment ("Live Relationship UI")
  * **Both options** : Some traditional on-premise/AMS-specific workflow customizations, or anything deeply dependent on the JCR event system, will **not** be portable.

### Any reason to still go with XWalk over DA, especially as it relates to customers with MSM requirements?
If you only need in-context authoring (Universal Editor) and not the benefits of JCR, we recommend DA + UE.
Regarding MSM: Pure DA is currently the only path to support "MSM-like" features natively. DA + UE does not support this, yet. If you want guided authoring + MSM, XWalk is the better path.
#### In bullet point form:
  * Bring existing Java stack but deliver through Edge Delivery = XWalk
  * MSM + in-context authoring = XWalk
  * da.live for everything else.

  

**Remember: MSM was not meant for translation/localization of content.**
  * MSM is about automation of page inheritance, not translation and localization first.
  * if you have a translation or localization need, you should be using the [translation and localization features of DA](https://docs.da.live/administrators/guides/translation-strategy). Imagine a global car company that has dealership satellite sites, or the US Army which has local recruitment sites. It's language-for-language.

  

* * *
### **References & Additional Reading**
  * [MSM in EDS Crosswalk - Experience League Docs](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/integrations/msm)
  * [DA MSM Early Access - DA Docs](https://docs.da.live/about/early-access/multi-site-manager)
  * [AEM EDS Playground - Project Types & Patterns](https://wiki.corp.adobe.com/spaces/ACSUI/pages/3826493894/AEM+EDS+Playground+%E2%80%93+Project+Types+Patterns)
  * [Hybrid/Incremental Migration Best Practices](https://adobe.sharepoint.com/sites/AEMSuccessEngineering/Shared%20Documents/AEM%20Transformation-Communication/3%20%E2%80%94%20Community,%20%20Communication%20&%20Enablement/Community%20Mgmt/AdatpTo/Adapto%2024/AdaptTo\(\)%2024%20Panel.docx)
  * [https://docs.da.live/administrators/guides/translation-strategy](https://docs.da.live/administrators/guides/translation-strategy)

From Dirk Rudolph, June 2026:

---

## DA FAQ (content merged on Confluence from docs.da.live)

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
  * Bring existing Java stack but deliver through Edge Delivery = XWalk
  * MSM + in-context authoring = XWalk
  * da.live for everything else.

### For spreadsheets that use Excel formulas (e.g. AEM Forms), is it still recommended to use Sharepoint/Google for this specific use case?
If you have complex sheet needs (formulas, conditional formatting, more than 5,000 rows), we recommend a content overlay architecture where you store. You can link out to your complex sheets in DA for a seamless experience. [https://da.live](https://da.live/) > browse > New > Link  

    
      
    
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
