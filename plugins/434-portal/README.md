# 434 Portal agent plugin

The 434 Portal plugin connects your AI assistant to [434 Portal](https://app.434portal.com), the SEO workspace 434 Marketing runs for its clients. It is for 434 Marketing clients and staff.

It bundles two things:

- the 434 Portal MCP server (`https://app.434portal.com/mcp`), which gives the assistant live SEO data and your saved project context
- twelve guided SEO skills and four lead skills, so the assistant follows complete workflows instead of giving generic advice

You need a 434 Portal account. Accounts are invite-only: your 434 Marketing team sends the invitation.

## What you can do

- Find and evaluate keywords
- Research competitors and content gaps
- Audit a website and prioritize fixes
- Analyze backlinks and find link prospects
- Track organic and Google Maps rankings
- Work with Google Search Console and Analytics data
- Research AI prompts and track brand mentions, citations, and competitors
- Review form leads, build and publish lead forms, and segment contacts into lists

## Install

### Claude Code

The plugin is published from a separate public repository, `434marketing/434-portal-plugin`, which contains only this plugin and its marketplace manifest. Add it as a marketplace, then install:

```text
/plugin marketplace add 434marketing/434-portal-plugin
/plugin install 434-portal@434-portal
```

If the install summary says `Run /reload-plugins to activate.`, run that command. Claude Code should then ask you to sign in to 434 Portal; if it doesn't, run `/mcp` and approve the 434 Portal connection there.

Skills run as `/434-portal:<skill>`, for example `/434-portal:seo-audit`.

To update later:

```text
/plugin marketplace update 434-portal
/plugin update 434-portal@434-portal
```

### Claude.ai and Claude Desktop (custom connector)

Without the plugin you can still connect the MCP server; you get the tools but not the skills.

1. Open **Settings → Connectors** and choose **Add custom connector**.
2. Name it "434 Portal" and enter the URL `https://app.434portal.com/mcp`.
3. Click **Connect** and complete the sign-in below.

Claude Desktop can also install the full plugin: open **Customize → Plugins**, choose **Add marketplace**, enter `434marketing/434-portal-plugin`, then install **434 Portal**.

### Cursor

If your Cursor plan supports team or custom marketplaces, import `434marketing/434-portal-plugin` and install **434 Portal**. Otherwise add the MCP server to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "434-portal": { "url": "https://app.434portal.com/mcp" }
  }
}
```

Cursor opens the sign-in the first time it uses a 434 Portal tool.

### Codex

```sh
codex plugin marketplace add 434marketing/434-portal-plugin
codex plugin add 434-portal@434-portal
codex mcp login 434-portal
```

If `codex mcp login` reports that `434-portal` isn't found, restart Codex first: bundled MCP servers register only after a restart. Type `$` in Codex to see the skills.

## First connection

1. Your assistant opens a browser window to app.434portal.com. Sign in with your 434 Portal account.
2. The approval screen shows which app is asking for access and asks you to **choose the client organization** this connection may use. Pick one and approve.
3. Return to your assistant. It can now see that organization's projects.

## One connection per client

Each connection is tied to exactly one client organization. The assistant sees only that client's projects, and nothing in a conversation can switch it to another client. If you work with several clients (434 staff usually do), add one connection per client, or disconnect and reconnect to pick a different organization.

Websites (projects) are set up by the client's 434 Marketing team. Creating a project is staff-only, so for client users the assistant tells you your 434 team sets up websites instead of creating one.

## Costs and provider keys

434 Portal has no credits. Research tools call data providers (such as DataForSEO) with the provider keys configured for your organization in **Settings → Provider keys**. If a tool reports a missing key, add one there or ask your 434 team. Reading saved project context, reports, audits, and connected Search Console data makes no paid provider calls.

## Included skills

The skills are adapted from the open-source [OpenSEO](https://github.com/every-app/open-seo) project (MIT license). Each skill links its reports back to its folder under `skills/`.

- `ai-prompt-research`: AI prompt research
- `ai-visibility-audit`: AI visibility audit
- `competitive-landscape`: competitive landscape
- `competitor-analysis`: competitor analysis
- `keyword-clustering`: keyword clustering
- `keyword-research`: keyword research
- `link-prospecting`: link prospecting
- `local-seo`: local SEO
- `seo-audit`: SEO audit
- `seo-coach`: SEO coach
- `seo-project-setup`: SEO project setup (context interview; project creation is staff-only)
- `seo-report`: SEO report

434 Portal's own lead skills, for the Forms and Contacts modules:

- `lead-review`: lead review (volume, top sources, forms and leads that need attention)
- `form-builder`: form builder (interview, draft, confirm, publish, then the WordPress shortcode)
- `contact-lists`: contact lists (segment contacts into lists; exports only on request)
- `monthly-client-report`: monthly client report (search performance and leads in one summary)

Lead skills that change something (building forms, adding contacts to lists, exports) follow your role in 434 Portal: read-only roles are refused.

## Coming next

- Attribution report: tie leads back to the pages and searches that brought them

## Privacy

The plugin itself stores nothing. Your assistant reaches 434 Portal only through the OAuth connection you approve, limited to the one organization you chose; you can revoke it from your assistant's connector settings at any time. What the assistant does with the data it reads follows your AI provider's own policies. See the [434 Marketing privacy policy](https://www.434marketing.com/privacy-policy/) and [terms](https://www.434marketing.com/terms-and-conditions/).

<!-- TODO: add a local plugin icon (assets/icon.png) once the final 434 logo asset is chosen, then reference it from the Claude ("icon") and Codex ("composerIcon", "logo") manifests. Cursor uses the hosted logo URL. -->
