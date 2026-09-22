# Mailchimp Operator Skill

This folder is a portable, account-neutral agent skill based on hands-on Mailchimp operations. Version 2 adds the wider campaign system around Mailchimp: registration pages, webhooks and imports, entry groups, reminder sequences, replay pages and expiry checks. The skill is in [`mailchimp-operator/`](mailchimp-operator/SKILL.md). It contains no client campaign IDs, subscriber lists, API keys, exported emails or offer terms.

## What it helps with

- Inventory and verify classic automations and regular campaigns.
- Preserve editable Image, Text and Button blocks.
- Check the actual audience, queues, consent and prior sends.
- Replicate campaigns without carrying old settings into a new event.
- Test emails sequentially and activate only approved steps.
- Map an event campaign from registration through replay and follow-up.

## Share with colleagues

1. Share this folder through a reviewed team repository or archive. Each colleague copies `mailchimp-operator/` into their agent's skills directory, usually `~/.codex/skills/` for Codex or `~/.claude/skills/` for Claude Code, and starts a new session.
2. Invoke it as `$mailchimp-operator` for a Mailchimp job, or let the agent select it when the request matches its description. Have each operator supply their own account access and the campaign-specific brief. Credentials and campaign backups belong outside the shared skill.
3. For a public GitHub repository, use this `share/` directory as the repository root. Review the public diff and repository visibility before pushing. Keep private client playbooks and campaign-specific scripts in a separate access-controlled repository.

The skill teaches a process; it does not grant Mailchimp access or authority to send. Recheck Mailchimp's current UI and API documentation for the target account. The observed API/native-builder behaviour comes from particular classic automations in September 2026 and should be validated before applying it elsewhere.

Future operator tooling and acceptance tests are tracked in [`ROADMAP.md`](ROADMAP.md).
