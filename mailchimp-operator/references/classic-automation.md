# Classic Automation: inventory and audience

## Read-only snapshot

Capture the workflow name, type, audience and status; then list all emails in order. For each step capture its API and browser identity, campaign status, subject/preview, HTML and plain-text versions, native block types, trigger and relative delay, day/time schedule with timezone, filter, queue and next-send time, subscriber sends and post-send action. Record the timestamp of the snapshot. A workflow can change between sessions.

Compare stable settings field by field. A live `recipient_count` or display `segment_text` may drift without a deliberate filter change. Inspect the saved segment in the UI where API `segment_opts` is incomplete. An API `send_time` may be a historical send timestamp rather than the recurring delivery window. A runtime value such as `hours.type=automation` does not establish the clock time. Reload the workflow UI and inspect queue times after any timing change.

Do not assume a successful API PATCH can set a classic automation's relative delay. One observed `previous email sent` delay update returned HTTP 400; the UI was needed. Recheck current API documentation and validate a supported operation on the exact workflow type before using it.

## Who can actually receive it?

Start with the intended roster, not the automation's headline queue number. Compare intended contacts with entry criteria, current queue, previous-step recipients and exclusions. Use minimal identifiers or hashed joins in local evidence. Check whether confirmation tags are shared across cohorts, whether a person has already traversed the step and whether changing a schedule would leave someone behind. Do not replay a tag or enrol contacts to repair a gap without authorisation.

Mailchimp documents different paused behaviour by automation type: activity-based emails can hold contacts in a paused step's queue, while date-based contacts can miss a paused date and move on. The exact workflow's trigger and queue must decide the plan. Inspect the queue's people and scheduled times, not just its count. See [Edit Classic Automation Emails](https://mailchimp.com/help/edit-automation-emails/) and [Manage Subscribers in a Classic Automation](https://mailchimp.com/help/manage-subscribers-in-a-classic-automation/).

Duplicated workflows can retain prior event dates, entry groups, filters, links, images, offer terms and QR destinations. Diff each step, including the plain-text alternative, against the new event evidence. If recipients do not match the intended cohort, present a concrete remediation option and obtain separate authority for any audience, trigger, tag or group change.
