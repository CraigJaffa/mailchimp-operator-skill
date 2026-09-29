# Broadcast campaigns and reports

Before replicating a campaign, inspect its `type` and content structure. A `variate` A/B source replicates as an A/B campaign; its subjects may live in `variate_settings.subject_lines` rather than one `settings.subject_line`. Choose the campaign format deliberately. Native broadcast designs can have the same builder/API split as automation emails, so inspect actual `.mojoMcBlock` widgets and native editability rather than trusting `settings.template_id` or a raw count of nested `mcnTextBlock` strings. A clean source can save substantial deletion work.

Mailchimp states that campaign replication copies both content and settings. Recheck title, subject, preview, `to_name`, sender, recipients, exclusions, schedule, social card, plain text and every destination. Replicating an original is safer than repeatedly replicating copies because some fields may not carry over through later generations.

Check all recipient exclusions after replication. Inline segment conditions may be visible under `recipients.segment_opts`, while a saved advanced segment can expose only `match: all` in the campaign readback. Inspect the underlying segment in Mailchimp before asserting who receives the send. A copy of last cohort's email keeps last cohort's saved segment, even when the list view shows zero recipients; set the new audience deliberately before any send. Never infer delivery safety from `recipient_count` alone.

A campaign `settings` PATCH can clear `settings.to_name`, including a PATCH that only changes the internal title or the preview text. In the observed account this happened on every regular campaign patched, not only on recipient changes. Include the intended `to_name` (for example the first-name merge tag) in every settings PATCH, then read back subject, preview, sender, reply-to and `to_name` together and compare them with the pre-write values.

For a sequence that someone will send manually, rename each draft in send order, for example "Cohort X Offer - Email 2 of 5 (Tue 3 Nov)". The campaign list sorts by last edited, so a position and date in the title is what lets the sender find the next email. The title is internal; it does not change what subscribers see.

For analytics, do not assume `GET /reports?type=automation` or `GET /campaigns?status=sent` lists every classic automation email. In the observed account, those list routes omitted automation emails, while `GET /reports/{campaign_id}` returned individual automation-email reports. Discover workflow emails first, then fetch their reports individually if the current API behaves the same. Record coverage and time range; avoid treating a broadcast-only export as all Mailchimp sends.

These are observed account behaviours, not universal API guarantees. Verify campaign type, endpoint output and recipient fields in the current account before mutating or reporting.
