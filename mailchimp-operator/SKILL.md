---
name: mailchimp-operator
description: Inspect, edit and verify Mailchimp campaigns or classic automation emails through the native builder and Marketing API. Use for campaign maintenance, audience/queue checks, test sends and controlled activation.
---

# Mailchimp Operator

Work from the user's exact campaign and authorised scope. A content approval, test-send request, audience change and subscriber activation are distinct actions. Preserve the existing delivery state unless the user has authorised changing it. Mailchimp's current UI and documentation take precedence over the examples here.

## Choose the path

- For a classic automation, read [references/classic-automation.md](references/classic-automation.md) before editing timing, triggers, filters, queues or states.
- For content and native design changes, read [references/native-content.md](references/native-content.md).
- For tests or live delivery, read [references/delivery.md](references/delivery.md).
- For broadcast replication, recipient settings or performance reports, read [references/campaigns-and-reports.md](references/campaigns-and-reports.md).

## Operating loop

1. Identify the account, audience and exact campaign or workflow. Browser numeric IDs and Marketing API IDs may differ; map them using current read-only evidence. Inspect every relevant step, its content and plain text, subject, preview, native block structure, trigger, schedule, segment, queue, prior sends and status. Recheck immediately before a write; another editor may have changed it.
2. Confirm the real recipient cohort and event or offer facts from current evidence. A shared tag, a broad permission segment or a paused queue alone does not prove the intended audience will receive an email.
3. Record a bounded before/after plan. Back up the exact target's content and settings before editing. Change one email at a time, using native Image, Text and Button blocks where future Mailchimp editing matters. Avoid replacing a native design with a whole-HTML API write.
4. Save, reopen and independently compare the rendered HTML, plain text, subject, preview, images, links, block editability, trigger, schedule, filter, queue and final state. Preview desktop and mobile where practical. A success response or save notification is only evidence that the action was accepted.
5. Keep a compact receipt: target identity, time and timezone, approved field changes, backup location, readback result, test-recipient acceptance if applicable, final status and unresolved audience or delivery risks. Exclude secrets and unnecessary subscriber data.

Use the smallest available capability. Read-only API calls help inventory and verify; use Mailchimp's editor for native blocks. A campaign-specific, allowlisted adapter can handle supported metadata or plain-text fields when readback proves native HTML and unrelated settings unchanged. Do not assume one workflow's successful API operation generalises to another campaign type or builder.

Stop dependent writes if identity, recipient eligibility, editability, actual saved content or delivery state is uncertain. Do not blind-retry a test send, live send or a UI action that may have succeeded before a timeout. Inspect current state first.

## Sources

Mailchimp's [classic automation editing](https://mailchimp.com/help/edit-automation-emails/), [queue and subscriber guidance](https://mailchimp.com/help/manage-subscribers-in-a-classic-automation/) and [Marketing API content reference](https://mailchimp.com/developer/marketing/api/campaign-content/) describe product capabilities. The detailed tactics in the references are operator observations that must be revalidated in the current account.
