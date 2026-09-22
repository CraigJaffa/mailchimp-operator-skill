# Roadmap

The skill documents the verified operating method. These tools would make repeated campaign work faster while keeping the same checks.

## 1. Campaign manifest generator

Create one current record of registration pages, integrations, groups, campaigns, schedules, queues and destinations. It must exclude credentials and unnecessary contact data.

Acceptance: the manifest detects a stale date, wrong group or missing downstream workflow before an invite can be marked ready.

## 2. Queue-aware activation planner

Read current triggers, schedules and queues, then predict which contacts would send immediately after an individual resume.

Acceptance: the planner blocks activation when the predicted audience or time differs from the approved plan.

## 3. Registration integration verifier

Test a page-to-integration-to-Mailchimp path with an authorised internal contact. Cover duplicated pages whose webhook or form destination has been cleared.

Acceptance: confirm the expected group, workflow queues and receipt, or return the exact broken link in the chain.

## 4. Consent-safe hosted-event import

Prepare hosted-event registrants for Mailchimp without changing an unknown consent status to subscribed. Preserve unsubscribes, deduplicate contacts and report missing names or permission evidence.

Acceptance: imported contacts match the registration source, while every unknown-consent contact remains excluded from marketing delivery.

## 5. Page and destination checker

Verify public access, event details, add-to-calendar data, replay embeds, checkout status, expiry timezone and redirects on desktop and mobile. Avoid opening links whose GET request records an RSVP or purchase action.

Acceptance: produce a page-by-page result with the exact failing field and no side-effect requests.

## 6. Replication diff

Compare a copied campaign with its source and require an explicit decision for every inherited field.

Acceptance: cover campaign type, title, subject, preview, sender, `to_name`, recipients, exclusions, schedule, social card, HTML, plain text, images and links.

## 7. Exclusion latency check

Measure whether registration groups and buyer tags update soon enough to exclude people from later reminders or sales emails.

Acceptance: show the update delay and compare the final Mailchimp recipient set with the approved source roster.

## 8. Performance feedback loop

Record each email's angle, send time, recipients, clicks and resulting registrations or sales where attribution is available.

Acceptance: separate observed results from hypotheses and avoid declaring a winning angle from an underpowered sample.
