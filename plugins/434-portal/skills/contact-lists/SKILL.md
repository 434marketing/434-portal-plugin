---
name: contact-lists
description: "Segment a website's contacts into 434 Portal contact lists (by service, source, location or recency), and export contacts only on explicit request. Use when the user wants to group, tag, segment or export contacts."
---

# 434 Portal Contact Lists

> **Before you start.** This connection is tied to one client organization: every website (project) and contact you see belongs to that client, so stay within it. Creating lists, adding contacts and exporting are changes, and changes obey the user's role in 434 Portal: read-only roles are refused. If a tool is refused, say so plainly instead of retrying. Contacts are personal data.

## Goal

Put the right contacts into clearly named lists the team can act on, without surprising bulk changes.

## Required inputs

- `projectId` (from `list_projects`; ask which website if there are several)
- The segment the user wants, such as "everyone who asked about termites", "leads from Google Ads this quarter" or "contacts in 22901"

## Tools

- `list_contact_lists`: existing lists and member counts. Reuse a list instead of creating a near-duplicate.
- `search_contacts`: contacts, newest activity first, matching a name, email or phone, or one list. Each row carries first source (UTM, referrer, landing page), postal code and last submission date. Page with `nextCursor`.
- `get_contact`: one contact's lists and submissions.
- `list_form_submissions`: answers, when the segment depends on what people asked for.
- `create_contact_list`: a new, empty list. Names are unique per website.
- `add_to_contact_list`: up to 100 contact ids per call; existing members are left alone.
- `export_contacts`: CSV of contacts, optionally one list or a search. Recorded in the organization's audit log.

## Workflow

1. Agree the rule for the segment in one sentence, and the list name.
2. Find the matching contacts: `search_contacts` for name, email, phone or list rules; page through `list_form_submissions` and match by `contactId` for answer-based rules; filter the rows yourself for source, postal code or date rules.
3. Show the result before changing anything: the count, the rule, a sample of up to five contacts (name and one contact detail), and anything ambiguous you left out.
4. **Confirm before bulk changes.** Ask before creating a list and before adding contacts. For more than 100 contacts, say how many calls it will take.
5. On a clear yes: `create_contact_list` if needed, then `add_to_contact_list` in batches of 100. Report added and already-member counts.

## Exports

- Run `export_contacts` only when the user explicitly asks for an export.
- Before exporting, warn that the file contains personal data (names, emails, phone numbers, answers), that it should be stored and shared carefully, and that the export is recorded in the audit log.
- Inline exports stop at 2,000 rows; when `truncated` is true, tell the user to export the full list from the portal's Contacts page.
- Don't paste a large CSV into chat. Offer to save it as a file if the client supports that, otherwise give a short preview and the row count.

## Guardrails

- Never remove contacts from lists or delete lists with this skill; send the user to the Contacts page.
- Never move contacts between websites; each list belongs to one website.
- Don't infer sensitive traits (health, finances, ethnicity) to build a segment.
