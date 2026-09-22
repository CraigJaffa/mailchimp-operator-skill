# Broadcast campaigns and reports

Before replicating a campaign, inspect its `type` and content structure. A `variate` A/B source replicates as an A/B campaign; its subjects may live in `variate_settings.subject_lines` rather than one `settings.subject_line`. Choose the campaign format deliberately. Native broadcast designs can have the same builder/API split as automation emails, so inspect actual `.mojoMcBlock` widgets and native editability rather than trusting `settings.template_id` or a raw count of nested `mcnTextBlock` strings. A clean source can save substantial deletion work.

Check all recipient exclusions after replication. Inline segment conditions may be visible under `recipients.segment_opts`, while a saved advanced segment can expose only `match: all` in the campaign readback. Inspect the underlying segment in Mailchimp before asserting who receives the send. A recipient-settings PATCH can reset an inherited `settings.to_name`; preserve and verify it when that field is in scope. Never infer delivery safety from `recipient_count` alone.

For analytics, do not assume `GET /reports?type=automation` or `GET /campaigns?status=sent` lists every classic automation email. In the observed account, those list routes omitted automation emails, while `GET /reports/{campaign_id}` returned individual automation-email reports. Discover workflow emails first, then fetch their reports individually if the current API behaves the same. Record coverage and time range; avoid treating a broadcast-only export as all Mailchimp sends.

These are observed account behaviours, not universal API guarantees. Verify campaign type, endpoint output and recipient fields in the current account before mutating or reporting.
