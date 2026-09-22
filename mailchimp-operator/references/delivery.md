# Test and delivery controls

## Test sends

Check the exact authorised recipients and target steps. For an ordered sequence, select one email at a time and wait for acceptance before selecting the next. Mailchimp's test dialog can retain a prior email checkbox or add account-user recipients; inspect all selections and the address field before every send. Never infer permission for a test from permission to edit copy, or permission for a subscriber send from a test.

Record each campaign, recipient, UTC time, UI/API result and any uncertainty. A success banner means Mailchimp accepted a test request. It does not prove inbox delivery or correct rendering there. Inspect the recipient's actual inbox only when access and scope permit. If a request times out or the result is unclear, inspect history before retrying to avoid duplicate tests.

Mailchimp's [campaign content API](https://mailchimp.com/developer/marketing/api/campaign-content/) is separate from [test-send operations](https://mailchimp.com/developer/marketing/api/campaigns/send-test-email/). Check the current campaign type and capability before applying either.

## Activation

Immediately before an approved activation, re-read status, queue membership, next-send times, filters and prior sends. Identify whether queued people would receive a now-stale message, and whether intended people are missing. Resume only the specifically authorised step. Avoid broad “Resume All” when the request is for individual emails. Do not silently change tags, groups, audience, trigger or schedule to make the queue look right. Verify the final state and the first expected delivery time after activation; log subscriber sends separately from test sends.

On failure, leave any unaffected paused steps paused. Report the exact unresolved state and safe next action. A previously active step controlled by a person must not be paused merely because an older local fixture expected it to be paused.
