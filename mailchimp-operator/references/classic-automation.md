# Classic Automation: inventory and audience

## Read-only snapshot

Capture the workflow name, type, audience and status; then list all emails in order. For each step capture its API and browser identity, campaign status, subject/preview, HTML and plain-text versions, native block types, trigger and relative delay, day/time schedule with timezone, filter, queue and next-send time, subscriber sends and post-send action. Record the timestamp of the snapshot. A workflow can change between sessions.

Compare stable settings field by field. A live `recipient_count` or display `segment_text` may drift without a deliberate filter change. Inspect the saved segment in the UI where API `segment_opts` is incomplete. An API `send_time` may be a historical send timestamp rather than the recurring delivery window. A runtime value such as `hours.type=automation` does not establish the clock time. Reload the workflow UI and inspect queue times after any timing change.

Do not assume a successful API PATCH can set a classic automation's relative delay. One observed `previous email sent` delay update returned HTTP 400; the UI was needed. Recheck current API documentation and validate a supported operation on the exact workflow type before using it.

## Who can actually receive it?

Start with the intended roster, not the automation's headline queue number. Compare intended contacts with entry criteria, current queue, previous-step recipients and exclusions. Use minimal identifiers or hashed joins in local evidence. Check whether confirmation tags are shared across cohorts, whether a person has already traversed the step and whether changing a schedule would leave someone behind. Do not replay a tag or enrol contacts to repair a gap without authorisation.

Mailchimp documents different paused behaviour by automation type: activity-based emails can hold contacts in a paused step's queue, while date-based contacts can miss a paused date and move on. The exact workflow's trigger and queue must decide the plan. Inspect the queue's people and scheduled times, not just its count. See [Edit Classic Automation Emails](https://mailchimp.com/help/edit-automation-emails/) and [Manage Subscribers in a Classic Automation](https://mailchimp.com/help/manage-subscribers-in-a-classic-automation/).

Do not encode “resume downstream first” as a universal rule. That order may be useful for a verified held queue, but it can also release people into stale downstream steps. Calculate the effect of each individual resume from the current queue, schedule and trigger, then observe the result before the next activation. Avoid repeatedly pausing and resuming a time-sensitive step; held contacts may become immediately eligible.

Duplicated workflows can retain prior event dates, entry groups, filters, links, images, offer terms and QR destinations. Diff each step, including the plain-text alternative, against the new event evidence. If recipients do not match the intended cohort, present a concrete remediation option and obtain separate authority for any audience, trigger, tag or group change.

## Duplicating a workflow for the next event

Observed 29 Sep 2026 on classic automations.

- **Replicate from the campaigns list.** Open the row's menu, choose "Replicate", keep the audience and click "Continue". Mailchimp opens the new workflow page, named "<original> (copy 01)".
- **What the copy carries.** Every email arrives as a draft (API status `save`), with the original's content, delays, send windows, runtime days, filters and post-send actions. Triggers still point at the old entry group. There is no API route for this: `/automations` has no replicate action.
- **Check the API immediately after replicating.** If the page does not navigate after "Continue", list `/automations` before clicking again, or you will make a second copy.
- **Rename the workflow** under "Edit Settings". The workflow name is the `title` field, and "Update Settings" is top right. Internal email titles, subjects and preview text can be changed by API (`PATCH /automations/{wf}/emails/{id}`) on draft and paused emails.
- **Trigger.** Use the email's "Trigger · Edit". The group dropdown (`event_data0`) uses the builder's own numbers, not API interest IDs. A group created by API appears in the dropdown straight away. Keep the delay unit on "immediately" and click "Update Trigger".
- **Post-send action.** The API cannot read it. Use "Post-send action · Edit":
  - The action select offers "Add to group" (`join_interest`) or "Add tag" (`add_segment`).
  - The group select uses builder numbers, and the tag select uses static-segment IDs.
  - Click "Update Action".
  - To read it back, reload the workflow page and click the "Details" link beside it. A tooltip shows, for example, `Add to group "..."` or `Add "..." Tag`.
- **A copy made days earlier by someone else can be stale in the same ways.** In one case the registration copy still added registrants to last month's group, and its email still carried last month's Zoom details and a "Watch the replay" button.

## Runtime days, send windows and when to start

- **Weekday runtime can fire weeks early.** An active email that triggers on a group join, with its runtime limited to one weekday, sends on the next matching weekday, not on the event date. With the event three weeks out, anyone who joins on an earlier matching weekday gets "starting in 2 hours" that same day. Keep such emails paused until the event week, or agree the risk with the owner explicitly.
- **Prefer a 15-minute window to an exact time.** In one workflow, an email set to "at 5:00pm" never sent: its whole queue was still waiting the next day. Its siblings with "between" windows sent normally.
- **A paused email with a group-join trigger held its queue** (266 contacts) and released it when resumed.
- **Expect a never-started (draft) workflow to ignore people who join its trigger group.** They are not picked up later. Start it while the entry group is still empty, then pause the individual emails if they must wait.
- **Some owners trigger the post-event workflow on the main registrant group.** The first email is paused, holding everyone from registration onwards, and is resumed once the replay is live. That avoids a separate "Post" group. One owner preferred this and removed the extra group. Ask which pattern the owner uses before adding groups.
- **The owner may start, pause or retrigger workflows between your steps.** Re-read workflow and email statuses before each write and before reporting. If you find something active that you would have paused, explain the risk and ask. Do not pause it yourself.
