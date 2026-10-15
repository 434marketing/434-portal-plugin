---
name: monthly-client-report
description: "Write one client-ready monthly summary of a website's search performance and leads: Search Console and Analytics trends, rankings, site health, form leads and their sources, and the next priorities. Use when the user asks for a monthly report, a client update or a month-in-review."
---

# 434 Portal Monthly Client Report

> **Before you start.** This connection is tied to one client organization: every website (project) you see belongs to that client, so stay within it. What you may read or save follows the user's role in 434 Portal; if a tool is refused because a module is off or the role can't use it, leave that section out with a one-line note instead of retrying.

## Goal

One summary a business owner can read in two minutes: how search and leads did last month, why, and what happens next.

## Required inputs

- `projectId` (from `list_projects`; ask which website if there are several)
- The month. Default to the last full calendar month, compared with the month before and, when data allows, the same month last year.

## Project context

Call `get_project_context` first: the business, its services and locations, competitors and writing preferences shape what matters. If the project has a report template for monthly reports (`list_report_templates`), follow it when the user names it.

## Tools

Use what is connected; skip what isn't and say so in one line.

- Search: `get_search_console_performance` (clicks, impressions, top queries and pages), `get_google_analytics_organic_overview` and `get_google_analytics_key_events` when Analytics is connected.
- Rankings: `get_rank_tracker` for tracked keywords and movement; `get_local_rank_grid` for map rankings when the project tracks them.
- Site health: `list_site_audits` and `get_audit_issues` for the latest audit; don't start a new audit unless asked.
- Leads: `get_submission_counts` for the month and the prior month, per form; `list_form_submissions` for sources (UTM source, else referrer host, else direct); `get_attribution_report` only if the tool is listed (otherwise use the submission sources and don't mention it).
- Saving: `list_reports` and `save_report`, when available.

These reads make no paid provider calls, except rank or grid checks if you start new ones; don't.

## Workflow

1. Resolve the project and the month. Say the dates.
2. Gather search, rankings, site health and leads for the month and the comparison periods.
3. Connect them: did organic traffic and leads move together; which pages or queries drove leads; which source grew.
4. Pick at most three priorities for next month, each tied to evidence above.
5. Reply in chat with the verdict and priorities. Then offer to save it: if `save_report` is available, deliver through the `seo-report` skill, saving with `skill: "monthly-client-report"` and a title like "Monthly Report — Oct 1, 2026". If `save_report` isn't available, give the summary in chat only.

## Output format

- **The month in one line**: leads and organic clicks, each with the change.
- **Leads**: total, per form, top three sources with counts.
- **Search**: clicks, impressions, average position; the top pages and queries that gained or lost.
- **Rankings**: notable movers among tracked keywords.
- **Site health**: the latest audit's headline and any new critical issue.
- **What we're doing next**: up to three priorities with the expected effect.
- **How this report was made**: which tools reported what, and anything not connected.

## Writing rules

- Plain language for a business owner: explain any term of art in a few words (impressions, average position).
- Every number has its comparison. Distinguish measured numbers from your interpretation.
- No names, emails or phone numbers of leads in the report.
- Don't overclaim causes: "leads rose alongside organic clicks" unless the sources show the link.
