# Native content: edit and verify

## Decide whether the builder owns the content

Read the campaign's content and inspect its editor. Native drag-and-drop markup can contain `mcnTextBlock` or `mcnButtonBlock` while the API returns `template: null` and no `mc:edit` regions. Conversely, a non-zero `settings.template_id` does not prove that an API HTML replacement will remain editable. A whole-content `PUT` may return success yet leave builder blocks unchanged, or may flatten the design into code. Do not treat an HTTP 200 as proof of a native edit.

Preserve editable Image, Text and Button blocks, plus the intended footer. Clone a correctly styled block when appropriate, replace its copy or asset and inspect its actual position after every structural operation. Template assignment and earlier edits can carry hidden old blocks, stale copy, duplicate footers or wrong alignment. Verify the block tree after a reload. Do not build a new email from an old campaign without inspecting its inherited trigger, audience and design state.

Rich text Source within a native Text block may preserve editability; a whole Code block does not meet a native-editability requirement. Image selection is complete only after the chosen asset is inserted and its alt text and destination are read back. Buttons may default to the wrong colour, font, padding or corner radius. Set the real style controls and verify the saved rendered result; do not infer a round button from a design description.

UI actions that timeout can still have saved. Re-inspect the page and campaign before retrying. Derive a fresh locator or screen position after layout changes. Avoid undocumented page-script mutations as a portable operating method; prefer supported UI controls and the documented API. A scripted, campaign-scoped adapter is useful only when its preconditions, allowlist and post-write verification are explicit.

## Independent readback

Compare HTML and plain text separately with the approved source. Inspect text order, emphasis, spacing, block type/order, subject and preview, banner identity, image alt text and links, button appearance, footer and unsubscribe elements. Check links without opening a URL whose GET can record an RSVP or other action; automated Link Checker may also visit it. Verify actual price, eligibility, expiry, checkout and QR from current evidence before inserting an offer. Visual desktop/mobile review complements structural checks; neither replaces the other.

An exact before/after comparison should show only authorised changes. If a plain-text-only API update is necessary, back up first, then prove the HTML is byte-identical and the native builder still opens. Keep the original backup immutable. If another editor changes the campaign meanwhile, stop rather than overwriting their work with an old backup.
