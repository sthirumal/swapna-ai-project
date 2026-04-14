<!--
  Source of truth (edit here first): https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ
  Synced copy for Cursor/offline. This snapshot reflects the wiki page as returned by MCP (including when the page is in **draft** for testing). Restore the published AOE FAQ on the wiki and re-sync when experimentation ends.
-->

# FAQ – synced from wiki (draft testing)

This file mirrors the **AOE FAQ** Confluence page body. The wiki is intentionally in **draft** with placeholder / alternate Q&A for local testing—**not** the production AOE engagement FAQ corpus. **Match by topic or intent** when answering from this document.

---

## Architecture

**Q:** Can I see the architecture?

**A:** Yes. Up-to-date diagrams and information are here: [Helix architecture docs](https://github.com/adobe/helix-home/blob/main/docs/architecture.md). The main content-bus to HTML rendering code is in [helix-html-pipeline](https://github.com/adobe/helix-html-pipeline).

---

**Q:** Is there any infrastructure redundancy?

**A:** From 2022-10-01, a second fully redundant delivery stack (CDN, compute, and storage) was built on Cloudflare, independent of the existing stack (Fastly, AWS Lambda, and S3). Delivery can be switched via a DNS CNAME change during a Fastly or AWS outage. Both stacks run in parallel at all times. _(Source as in your notes.)_

---

**Q:** Does Franklin have a concept of Dev, Staging, and Prod environments?

**A:** “Dev” and “staging” are not fixed Franklin concepts. For code, every branch gets a unique URL. You can link to a branch like [https://98-cookie-style--vg-volvotrucks-us--hlxsites.hlx.page/drafts/wingeier/](https://98-cookie-style--vg-volvotrucks-us--hlxsites.hlx.page/drafts/wingeier/) and Helix rewrites it to the related site (e.g. [http://volvotrucks.us/drafts/wingeier](http://volvotrucks.us/drafts/wingeier)). You can also point [stage.customerdomain.com](http://stage.customerdomain.com) at the `.page` URLs if you want.

---

**Q:** Can we hide our `.page` (preview) content from the public?

**A:** That is the customer’s responsibility. You could implement a frontend redirect (e.g. if the URL ends in `.page`, check a cookie and send users to login if missing), but the added complexity is often not worth it. Preview content is not indexed by default, which is the main concern.

---

**Q:** Can I have more than one Franklin project?

**A:** You can fork a project when there are heavy post–go-live dev changes, with the goal of merging again later. Linking across Franklin projects can be tricky: the pipeline rewrites links. Example: linking from [hlx.live block-collection docs](https://www.hlx.live/developer/block-collection/links) to [https://main--helix-block-collection--adobe.hlx.page/block-collection/links](https://main--helix-block-collection--adobe.hlx.page/block-collection/links) gets rewritten; a workaround is an intermediary link (e.g. Bitly) as in your example.

---

**Q:** What serverless frameworks can I use?

**A:** The platform is generally serverless-framework agnostic; you supply your own serverless layer. Cloudflare Workers are often very fast and easy to onboard. AIO, Lambda, Azure Functions, etc. are fine as long as Franklin can reach the endpoint and responses are fast. If there is no direct serverless endpoint but App Builder / IO is available, that can be used. _(Source as in your notes.)_

---

**Q:** How can we handle pollers or periodic server-side preprocessing from other systems with Franklin (similar to AEM workflows/schedulers)?

**A:** Use Cloudflare Workers or any serverless endpoint for extra logic. Example repo: [hlxsites/wesco-cloudflare-worker](https://github.com/hlxsites/wesco-cloudflare-worker). External workflow tools (e.g. Workfront) can be involved. Preprocessing often goes through SharePoint / Google Drive APIs. Pattern: bring your own serverless, then consume results on the client (e.g. a custom block that fetches and uses the data) or on the authoring side (e.g. generated Excel/Word). _(Source as in your notes.)_

---

## Authoring

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

---
