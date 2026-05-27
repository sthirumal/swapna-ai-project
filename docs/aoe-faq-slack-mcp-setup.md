# AOE FAQ — Slack MCP setup for Cursor

This document supports the **aoe-faq** project: configuring the [Adobe Slack MCP server](https://github.com/Adobe-AIFoundations/adobe-mcp-servers) in Cursor so agents can read and post in team Slack channels (for example **#aoe-ise-internal**).

**Canonical reference (Adobe internal):** [Cursor.ai - Adobe Slack MCP Setup](https://wiki.corp.adobe.com/pages/viewpage.action?pageId=3513057951&spaceKey=BPS&title=Cursor.ai%2B-%2BAdobe%2BSlack%2BMCP%2BSetup)

The wiki remains the source of truth for Adobe-wide process. This file adds **project-specific lessons** from the aoe-faq setup: correct OAuth scope placement, workspace install approval, bot channel membership, and common errors.

---

## What the Slack MCP enables

- List and search channels
- Read channel history and thread replies
- Post messages and replies
- Add emoji reactions
- Look up users and profiles
- Search messages (requires a **user** token with `search:read`)

---

## Prerequisites

- [Cursor](https://cursor.com/) installed
- Node.js (latest LTS) — if building the MCP server from source
- Access to an Adobe Slack workspace (Enterprise Grid)
- Permission to **create a Slack app** (or request admin install)
- For **Easy MCP / Docker** path: Easy MCP configured per [Cursor integration with Easy MCP](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP)

---

## 1. Clone and build the Slack MCP server

From the [adobe-mcp-servers](https://github.com/Adobe-AIFoundations/adobe-mcp-servers) repository:

```bash
git clone git@github.com:Adobe-AIFoundations/adobe-mcp-servers.git
cd adobe-mcp-servers/src/slack
npm install
npm run build
```

For local runs, note the path to `dist/index.js` (wiki example uses Node directly). Easy MCP may use a Docker image instead (see §6).

---

## 2. Create a Slack app

1. Open [Slack API — Your Apps](https://api.slack.com/apps).
2. **Create New App** → **From scratch**.
3. Name the app (for example `aoe-faq`) and pick a **development workspace**.

### Adobe Enterprise Grid note

When **creating** the app, **do not** select the main **Adobe (`adobe.slack.com`)** workspace. Choose **another workspace** in Adobe’s Enterprise Grid (600+ workspaces) where you are allowed to develop apps.

When **installing** (§4), you can authorize the workspace where you need the bot to operate (for example the team channel workspace).

---

## 3. Configure OAuth scopes (critical)

Scopes must be split between **Bot Token Scopes** and **User Token Scopes**. Putting everything under user scopes is a common mistake and leads to missing bot tokens or `missing_scope` errors.

In the app: **OAuth & Permissions**.

### Bot Token Scopes

| Scope              | Purpose                                             |
| ------------------ | --------------------------------------------------- |
| `channels:history` | Read messages in public channels the bot has joined |
| `channels:read`    | List channels and channel metadata                  |
| `chat:write`       | Post messages as the app                            |
| `reactions:write`  | Add emoji reactions                                 |
| `users:read`       | Resolve user IDs and basic profile info             |

Optional but useful for channel membership listing via API:

| Scope           | Purpose                                                        |
| --------------- | -------------------------------------------------------------- |
| `channels:join` | Let the bot join public channels (or invite manually — see §5) |

Do **not** add `search:read` here; it is a **user** scope.

### User Token Scopes

| Scope         | Purpose                                            |
| ------------- | -------------------------------------------------- |
| `search:read` | Workspace message search (`slack_search_messages`) |

### Scopes to avoid unless required

| Scope                         | Note                                                                            |
| ----------------------------- | ------------------------------------------------------------------------------- |
| `admin.users:read`            | Not in the wiki guide; may trigger stricter admin review                        |
| `chat:write` on **user** only | Posting as user is optional; the MCP defaults to the bot for `chat.postMessage` |

### Correct vs incorrect layout

**Correct:**

```
Bot Token Scopes:     channels:history, channels:read, chat:write,
                      reactions:write, users:read

User Token Scopes:    search:read
```

**Incorrect (seen in practice):**

- All scopes under **User Token Scopes**, **Bot Token Scopes** empty → no `xoxb-` bot token or bot cannot post.
- `search:read` only on bot → search tools fail.

After **any** scope change, **Reinstall to Workspace** (§4) and update tokens in your env file if Slack rotates them.

---

## 4. Install the app to the workspace

1. On **OAuth & Permissions**, click **Install to Workspace** (or **Reinstall to Workspace**).
2. Approve the requested permissions.
3. Copy tokens from the same page:

| Token                | Prefix  | Environment variable |
| -------------------- | ------- | -------------------- |
| Bot User OAuth Token | `xoxb-` | `SLACK_BOT_TOKEN`    |
| User OAuth Token     | `xoxp-` | `SLACK_USER_TOKEN`   |

### If you see “Request to install to &lt;workspace&gt;”

You are **not** a workspace admin (or Adobe policy requires review). Slack shows a **Request to install to &lt;workspace&gt;** modal instead of installing immediately.

#### Step A — Submit the request (api.slack.com)

1. Fill **Reason (optional)** with purpose, scopes, and app ID (example below).
2. Click **Submit Request**.
3. Wait for **Slackbot** to post the outcome in your Slack workspace (you do not need to poll the app settings page).

#### Step B — Watch for Slackbot in your workspace

After you submit, **Slackbot** sends a direct message in the workspace you requested (for example **Adobe CXO** or **aoe-faq**). The outcome is one of the following.

**Cancelled (denied)**

Example message:

> Your request to install **aoe-faq** on &lt;workspace&gt; has been cancelled by @Slackbot.  
> This application contains a high-risk scope such as `remote_files` or `admin.*` and your request to install this app has been denied.

| What to do                                                                                                       |
| ---------------------------------------------------------------------------------------------------------------- |
| Open [api.slack.com/apps](https://api.slack.com/apps) → your app → **OAuth & Permissions**.                      |
| Remove **admin.\*** scopes (for example `admin.users:read`) and any other scopes not listed in §3.               |
| **Reinstall** is not enough — submit a **new** install request after scopes are fixed.                           |
| Questions: contact the group named in the Slackbot message (for example **Grp-ES*Architecture*&\_Engineering**). |

This matches a common mistake during first setup: putting scopes under **User Token Scopes** that include `admin.users:read`. The MCP guide in §3 does not require any `admin.*` scope.

**Approved**

Example message:

> Your request to install **aoe-faq** has been approved. You can now install it from the Slack Marketplace.

| What to do                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------------- |
| Click **Go to Slack Marketplace** in the Slackbot message (or open the link Slack provides).                                     |
| Complete installation for your workspace from the Marketplace flow.                                                              |
| Return to [api.slack.com/apps](https://api.slack.com/apps) → your app → **OAuth & Permissions**.                                 |
| Confirm **Bot User OAuth Token** (`xoxb-`) and **User OAuth Token** (`xoxp-`) are shown, then copy them into your env file (§6). |

You may see **cancelled first**, then **approved** after fixing scopes and submitting again — that is normal.

#### Step C — Optional: notify admins

If approval is slow, you can still ping your workspace Slack admin and reference app name **aoe-faq** and your app ID from **Basic Information**.

**Example request reason (paste into the Submit Request modal):**

```text
Requesting installation of my custom Slack app for Cursor.ai MCP integration on the aoe-faq workspace (internal wiki: Cursor.ai - Adobe Slack MCP Setup).

Purpose: Connect Cursor to Slack to list/search channels and messages, post updates, and support AOE FAQ / team workflow from the IDE. App used only by me; tokens stored locally in Cursor MCP config (not in git).

Bot scopes: channels:history, channels:read, chat:write, reactions:write, users:read
User scope: search:read (message search)

App ID: A0B4XXXXXXXX
```

Replace the app ID with yours from **Basic Information**.

### Redirect URL / token rotation warning

If you see: _“At least one redirect URL needs to be set before this app can be opted into token rotation”_ — you can **ignore** this for the wiki Cursor flow unless you are building a custom OAuth redirect server. Install-and-copy-tokens from the app UI is sufficient.

---

## 5. Register the bot in channels

Installing the app to the workspace does **not** add the bot to every channel.

For each channel (for example `#aoe-ise-internal`):

```
/invite @<your-app-name>
```

Without this step, `chat.postMessage` returns **`not_in_channel`** and `conversations.history` may fail for that channel.

---

## 6. Environment variables and `SLACK_TEAM_ID`

Create a local env file (never commit tokens to git). Example variables:

| Variable           | Description                                                |
| ------------------ | ---------------------------------------------------------- |
| `SLACK_BOT_TOKEN`  | Bot User OAuth Token (`xoxb-...`)                          |
| `SLACK_USER_TOKEN` | User OAuth Token (`xoxp-...`) for search                   |
| `SLACK_TEAM_ID`    | Workspace **Team ID** (`T...`) for the installed workspace |
| `SLACK_LOG_FILE`   | Optional path for API debug logs                           |

### What is `SLACK_TEAM_ID`?

The **workspace ID** where the app is installed — **not** the app ID (`A...`), user ID (`U...`), or channel ID (`C...`).

**How to find it** after install:

```bash
curl -s https://slack.com/api/auth.test \
  -H "Authorization: Bearer xoxb-YOUR-BOT-TOKEN"
```

Use the `team_id` field from the JSON response (for example `T02CAQ0B2`).

---

## 7. Configure Cursor MCP

### Option A — Wiki (Node, local build)

**Cursor Settings** → **MCP Servers** → add:

```json
{
  "mcpServers": {
    "slack": {
      "command": "node",
      "args": ["/absolute/path/to/adobe-mcp-servers/src/slack/dist/index.js"],
      "env": {
        "SLACK_BOT_TOKEN": "xoxb-your-bot-token",
        "SLACK_USER_TOKEN": "xoxp-your-user-token",
        "SLACK_TEAM_ID": "T0XXXXXXXX",
        "SLACK_LOG_FILE": "/path/to/slack-api.log"
      },
      "autoApprove": [],
      "disabled": false
    }
  }
}
```

Use absolute paths. Prefer env files or Cursor secrets instead of pasting tokens into committed `mcp.json`.

### Option B — Easy MCP (Docker)

This project may use Easy MCP with Docker (example from `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "Slack": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "--name",
        "slack-<instance>",
        "--env-file",
        "~/.easymcp/adobe-mcp-servers/src/slack/.env-<instance>",
        "-e",
        "REGION_NAME=local",
        "-e",
        "ENVIRONMENT_NAME=local",
        "slack"
      ]
    }
  }
}
```

Tokens live in the referenced `--env-file` on your machine, not in the repo.

Restart or reload MCP in Cursor after token or scope changes.

---

## 8. Verify the setup

| Check                | How                                                           |
| -------------------- | ------------------------------------------------------------- |
| Bot token works      | `auth.test` with `xoxb-` returns `"ok": true`                 |
| User search works    | Agent can run `slack_search_messages` without `missing_scope` |
| Bot can post         | `/invite` bot to channel, then post a test message            |
| Bot can read history | `slack_get_channel_history` on channel ID after invite        |
| Team ID              | `SLACK_TEAM_ID` matches `auth.test` → `team_id`               |

**aoe-faq project smoke test** (in Cursor agent chat):

- List last messages in `#aoe-ise-internal`
- Post: `Hello from the Slack MCP server! …` (test message)
- List channel members (may require direct API if MCP tool is not exposed; `conversations.members` + `users.info`)

---

## 9. Troubleshooting

| Symptom                                                           | Likely cause                                     | Fix                                                                     |
| ----------------------------------------------------------------- | ------------------------------------------------ | ----------------------------------------------------------------------- |
| `missing_scope` on `conversations.list` / `conversations.history` | Bot scopes missing or wrong token type           | Add bot scopes in §3; reinstall                                         |
| `not_in_channel` on `chat.postMessage`                            | Bot not in channel                               | `/invite @app` (§5)                                                     |
| `missing_scope` on post (user fallback)                           | `chat:write` not on bot token                    | Add `chat:write` under **Bot** scopes; reinstall                        |
| Only `xoxp-`, no `xoxb-`                                          | All scopes on user side                          | Move bot scopes to **Bot Token Scopes** (§3)                            |
| “Request to install” modal                                        | Not workspace admin / Adobe review               | Submit request (§4); wait for Slackbot approved/cancelled DM            |
| Slackbot: cancelled, `admin.*` / high-risk scope                  | Forbidden scopes on the app                      | Remove `admin.*` and extra scopes (§3); submit a new request            |
| Slackbot: approved                                                | Request accepted                                 | **Go to Slack Marketplace** from Slackbot, then copy tokens (§4 Step B) |
| Search works; history fails                                       | Bot not in channel or missing `channels:history` | Invite bot; add scope; reinstall                                        |
| `invalid_team_for_non_distributed_app`                            | Wrong workspace for app                          | Create/install app per Adobe grid guidance (§2)                         |

### Debug with MCP Inspector

From `adobe-mcp-servers/src/slack`:

```bash
npx @modelcontextprotocol/inspector node dist/index.js
```

---

## 10. Security practices

- Never commit `xoxb-` / `xoxp-` tokens to git or share in channels.
- Use `.gitignore` / `.hlxignore` for env files.
- Grant **minimum** scopes (§3).
- Rotate tokens if exposed; reinstall the app if needed.
- Remember MCP runs **client-side** with your credentials — treat as personal access to Slack.
- Follow Adobe corporate security policies for custom apps on Enterprise Grid.

---

## 11. Related links

| Resource                               | URL                                                                                                                                                                     |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Adobe Slack MCP wiki                   | [Cursor.ai - Adobe Slack MCP Setup](https://wiki.corp.adobe.com/pages/viewpage.action?pageId=3513057951&spaceKey=BPS&title=Cursor.ai%2B-%2BAdobe%2BSlack%2BMCP%2BSetup) |
| adobe-mcp-servers                      | [GitHub](https://github.com/Adobe-AIFoundations/adobe-mcp-servers)                                                                                                      |
| Slack OAuth v2                         | [api.slack.com/authentication/oauth-v2](https://api.slack.com/authentication/oauth-v2)                                                                                  |
| Installing with OAuth (MCP / IDE note) | [docs.slack.dev/authentication/installing-with-oauth](https://docs.slack.dev/authentication/installing-with-oauth/)                                                     |
| Slack scopes reference                 | [api.slack.com/scopes](https://api.slack.com/scopes)                                                                                                                    |
| Easy MCP + Cursor                      | [assetscollab wiki](https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=assetscollab&title=Cursor+integration+with+Easy+MCP)                                     |
| AOE FAQ skill README                   | [`.cursor/skills/aoe-faq/README.md`](../.cursor/skills/aoe-faq/README.md)                                                                                               |

---

## 12. Example: Wiki FAQ query → answer → post to Slack

This is the **aoe-faq** pattern: answer from **approved wiki content** (via `@aoe-faq` / Confluence MCP), then share a short summary in **#aoe-ise-internal** with the Slack MCP.

### Prerequisites

- **Adobe Wiki Confluence** MCP configured (Easy MCP).
- **Slack** MCP configured (this guide).
- Bot invited to `#aoe-ise-internal` (§5).

### Step 1 — Ask in Cursor (example prompt)

Use the project skill and ask for Slack posting in the same message:

```text
@aoe-faq

What is included vs not included in AOE delivery scope? Keep the answer short for Slack.

Then post a summary to #aoe-ise-internal with:
- Source link to the wiki page
- Bullet points only (internal team)
- End with: "Ask @aoe-faq in Cursor for full FAQ wording."
```

### Step 2 — What the agent does (behind the scenes)

| Step | MCP / skill | Action |
| ---- | ----------- | ------ |
| 1 | `@aoe-faq` skill | Fetch canonical FAQ or related wiki (Confluence `get_wiki_content` first). |
| 2 | Confluence MCP | Example page: [Experience Modernization - AOE Delivery Model](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3696778769/Experience+Modernization+-+AOE+Delivery+Model) — sections *Included in AOE Scope* / *Not Included*. |
| 3 | Agent | Summarize in **Slack-friendly** form (no customer-specific or deal terms; internal only). |
| 4 | Slack MCP | `slack_post_message` → channel `C08KEAVG5E3` (`#aoe-ise-internal`). |

### Step 3 — Example wiki-based answer (for you in Cursor)

**Source:** [Experience Modernization - AOE Delivery Model](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3696778769/Experience+Modernization+-+AOE+Delivery+Model) (Confluence; check [AOE FAQ](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3835056848/AOE+FAQ) for related Q&A).

**Included in AOE scope (summary):**

- Migration to **Edge Delivery Services** (not classic AEM Publish).
- Static content import, design tokens, practical design fidelity, responsive layouts.
- Document Authoring and/or Universal Editor as appropriate.
- AOEs operate the Experience Modernization tooling and apply EDS best practices.

**Not included / needs extra project effort:**

- Intranet, VPN, or password-protected sources.
- Headless / SPA-only delivery targets.
- Pixel-perfect redesign, custom blocks, integrations, commerce/search, MarTech data layer, MSM/CF complexity — typically **Adobe Consulting or partners**.

**Engagement boundary:** AOEs deliver **front-end migration**; back-end and integrations are partner/ACS scope.

### Step 4 — Example message posted to Slack

Text the bot would post (you can copy this pattern):

```text
:books: *AOE scope reminder* (from wiki)

*Included:* EDS migration, static content import, design tokens, responsive layouts, DA/UE authoring setup, AOE-led use of Experience Modernization tooling.

*Not in base AOE scope:* protected/intranet sources, headless-only targets, pixel-perfect/custom blocks, integrations, commerce/search, MarTech data layer, complex MSM/CF — usually ACS or partner effort.

*Boundary:* AOE = front-end migration; back-end/integrations → partners/ACS.

Source: https://wiki.corp.adobe.com/spaces/AEMSites/pages/3696778769/Experience+Modernization+-+AOE+Delivery+Model

Ask @aoe-faq in Cursor for full approved FAQ wording.
```

Slack will render `*bold*` and the link. The `aoe-faq` bot posts as the app (not as your user).

### Step 5 — Variations you can try

| Goal | Example Cursor prompt |
| ---- | ----------------------- |
| FAQ only (no Slack) | `@aoe-faq Does Franklin have dev/staging/prod environments?` |
| Slack only | `Post to #aoe-ise-internal: Standup in 10 minutes` |
| Wiki + Slack | `@aoe-faq Answer from wiki about AO credits, then post 3 bullets to #aoe-ise-internal` |
| Thread reply | `Reply in thread on my last message in #aoe-ise-internal with the wiki link` |

### Governance reminder

- Answers must follow **approved wiki / FAQ** text (`@aoe-faq` skill); do not invent commercial or legal commitments.
- Slack posts are **internal team** channels unless governance approves external sharing.
- If Confluence returns **Draft**, say so in Slack before the team treats the summary as final.

---

## 13. aoe-faq project context

| Item                                           | Value               |
| ---------------------------------------------- | ------------------- |
| Slack app name (example)                       | `aoe-faq`           |
| Team channel                                   | `#aoe-ise-internal` |
| Channel ID (example; verify in your workspace) | `C08KEAVG5E3`       |
| Team ID (example; use your `auth.test` result) | `T02CAQ0B2`         |

Channel and team IDs vary by workspace; always resolve via Slack API or channel details after install.

See **§12** for a full wiki → Slack example prompt and sample message.

---

_Last updated from aoe-faq project setup experience (scopes, admin install request, bot invite, Easy MCP Docker). Align with the BPS wiki when Adobe updates the canonical guide._
