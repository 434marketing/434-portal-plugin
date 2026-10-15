---
name: form-builder
description: "Plan, build and publish a 434 Portal lead form for a website through a short interview, then explain how to put it on the WordPress site. Use when the user wants a new contact, quote, booking or inspection form, or wants to change an existing form's fields."
---

# 434 Portal Form Builder

> **Before you start.** This connection is tied to one client organization: every website (project) you see belongs to that client, so stay within it. Building forms is a change, and changes obey the user's role in 434 Portal: read-only roles are refused by `create_form`, `save_form_draft` and `publish_form`. If a tool is refused, say so plainly and suggest asking their 434 team, instead of retrying.

## Goal

Leave the user with a published form that captures what the business needs, fills the contact record correctly, notifies the right people, and is ready to paste into the site.

## Required inputs

- `projectId` (from `list_projects`; ask which website if there are several)
- What the form is for and where it goes on the site

## Tools

- `list_forms`, `get_form`: check for an existing form before creating a new one.
- `create_form`: a draft with starter fields (first name, last name, email, phone, message). Returns the form id and the shortcode.
- `save_form_draft`: replaces the draft's fields and changes any settings, notification recipients and template auto-reply. Returns `publishProblems`.
- `publish_form`: publishes the saved draft. Returns `problems` instead when something is missing.
- `set_form_status`: pause, resume or archive a published form.
- `list_contact_lists`: when the user wants new contacts added to a list.

## Interview

Ask in one or two short messages, offering sensible defaults:

1. Purpose and placement: what the visitor is asking for, and on which page.
2. What the team needs to know to follow up: usually name, email, phone, plus one or two questions specific to the business (service needed, address for an on-site visit, preferred time). Keep it short; every extra field costs leads.
3. Who on the team gets an email for each lead.
4. What the visitor sees after submitting: a thank-you message (default) or a redirect to a thank-you page (an `https://` address).
5. Whether the visitor gets an automatic reply, and whether new contacts join a contact list.

## Drafting fields

- Field keys are lowercase snake_case and unique, such as `first_name` or `service_needed`.
- Map fields to the contact (`contactMapping`) so leads become usable contacts: an `email` field → `email`, a `phone` field → `phone`, short text name fields → `first_name` and `last_name`, an `address` field → `address`, a ZIP field → `postal_code`. One field per mapping; everything else is `none`.
- Require only what the team can't follow up without, usually email or phone plus name.
- Use `select` or `choice` for a fixed list of answers, `checkbox` for several, `long` for open questions, `date` for a preferred date. Options are short and unique.
- An auto-reply needs a field mapped to `email`. Keep it plain: thank them, say when to expect a reply. Merge tags such as `{first_name}` and `{business_name}` are filled in.

## Workflow

1. Run the interview. Check `list_forms` for a form with the same purpose; if one exists, offer to change it instead.
2. `create_form` with a clear name (e.g. "Free Inspection Request"), unless changing an existing form.
3. `save_form_draft` with the full field list and the settings, recipients and auto-reply from the interview.
4. If `publishProblems` is not empty, fix what you can and save again; ask the user about anything you can't decide.
5. Show the draft summary: each field (label, type, required, contact mapping), what happens after submit, who is notified, the auto-reply, and the list. **Ask for confirmation before publishing.** Publishing changes what the live site shows.
6. On a clear yes, call `publish_form`. If it returns `problems`, explain them in plain words, fix with `save_form_draft`, and confirm again.
7. Explain how to put it on the site (below), with the shortcode from the tool output.

## Putting it on the WordPress site

1. Install and activate the 434 Portal WordPress plugin on the site (the 434 team can provide it).
2. In 434 Portal, open Settings → Website and connect the site, following the plugin's connect step.
3. Paste the shortcode, for example `[434_form id="frm_…"]`, into the page, or add the 434 Form block and pick the form.
4. Submit a test lead and check that the notification email arrives.

Changes made later go live only after another publish; the shortcode stays the same.

## Guardrails

- Never publish without the user's explicit confirmation of the draft summary.
- Don't add fields the user didn't ask for beyond the agreed defaults.
- Archiving a live form removes it from the site; confirm first.
