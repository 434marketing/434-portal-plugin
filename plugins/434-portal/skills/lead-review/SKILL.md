---
name: lead-review
description: "Summarize a website's form leads for a period: volume against the prior period, top sources, forms and leads that may go unanswered, and what to do next. Use when the user asks how leads are doing, wants a lead check-in, or asks about recent form submissions."
---

# 434 Portal Lead Review

> **Before you start.** This connection is tied to one client organization: every website (project) you see belongs to that client, so stay within it. What you may read or change follows the user's role in 434 Portal; if a tool is refused because the Forms or Contacts module is off or the role can't use it, say so plainly instead of retrying. Lead data is personal data: quote names, emails and phone numbers only when the user needs them to act.

## Goal

Tell the user, in a few lines, whether lead flow is healthy for a period, where leads came from, and which leads or forms need attention now.

## Required inputs

- `projectId` (from `list_projects`; ask which website if there are several)
- A period. Default to the last 30 days ending today, compared with the 30 days before it.

## Tools

- `get_submission_counts`: totals per day, week or month and per form, in the organization's timezone. Call it for the period and for the prior period of the same length.
- `list_form_submissions`: the leads themselves, newest first, with answers and source (page, referrer, UTM, Google Ads click). Page with `nextCursor`; stop after 200 rows and say the review covers the newest 200.
- `list_forms` and `get_form`: which forms are live, and who on the team is notified of each.
- `search_contacts`: contact records (first source, last submission) when the user asks about people rather than submissions.
- `get_attribution_report`: only if the tool is listed. When it is, use it for "how did you hear about us" answers and source totals. When it isn't, build the source breakdown from the submissions' UTM source, then referrer host, then "direct"; never mention the missing tool.

All of these are reads and make no paid provider calls.

## Workflow

1. Resolve the project and the period. Say the dates you used.
2. Call `get_submission_counts` for both periods. Compute the change in total and per form.
3. Page through `list_form_submissions` for the period. Group by source (UTM source, else referrer host, else direct) and by landing or form page.
4. Call `list_forms`, then `get_form` for each live form, to read its notification recipients.
5. Flag, only when true:
   - **Unusual volume**: a period total or a single form up or down by more than half against the prior period (and by at least 5 leads), or any day with more than three times the period's daily average. A sudden spike from one source or with repeated answers can be spam; say so as a possibility, not a conclusion.
   - **Leads that may go unanswered**: a live form with no notification recipients, and leads that left neither an email nor a phone. 434 Portal does not record whether the team replied, so ask the user to confirm follow-up on the newest leads rather than claiming they were missed.
   - **Silent forms**: a live form with no leads in the period.
6. Reply in chat with the output below. If the user wants something to share, deliver through the `seo-report` skill, saving with `skill: "lead-review"`.

## Output format

- **Verdict**: one sentence: up, down or steady, by how much, with both totals and dates.
- **Top sources**: up to five, with counts and share of the total.
- **By form**: count per form, with the change against the prior period.
- **Needs attention**: the flags from step 5, each with the next step (add a recipient, check the form on the site, call these leads first).
- **Next step**: one recommended action.

## Guardrails

- Read-only. This skill changes nothing; to change a form, use `form-builder`.
- Never paste full answer sets for many leads into chat. Summarize, and show individual leads only when the user asks or a flag needs it.
- Counts come from the tools; label any estimate as one.
