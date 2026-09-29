# Native content: edit and verify

## Decide whether the builder owns the content

Read the campaign's content and inspect its editor. Native drag-and-drop markup can contain `mcnTextBlock` or `mcnButtonBlock` while the API returns `template: null` and no `mc:edit` regions. Conversely, a non-zero `settings.template_id` does not prove that an API HTML replacement will remain editable. A whole-content `PUT` may return success yet leave builder blocks unchanged, or may flatten the design into code. Do not treat an HTTP 200 as proof of a native edit.

Preserve editable Image, Text and Button blocks, plus the intended footer. Clone a correctly styled block when appropriate, replace its copy or asset and inspect its actual position after every structural operation. Template assignment and earlier edits can carry hidden old blocks, stale copy, duplicate footers or wrong alignment. Verify the block tree after a reload. Do not build a new email from an old campaign without inspecting its inherited trigger, audience and design state.

Rich text Source within a native Text block may preserve editability; a whole Code block does not meet a native-editability requirement. Image selection is complete only after the chosen asset is inserted and its alt text and destination are read back. Buttons may default to the wrong colour, font, padding or corner radius. Set the real style controls and verify the saved rendered result; do not infer a round button from a design description.

### Images and GIFs

Upload the asset to the account's file manager (the API `POST /file-manager/files` with a file name and base64 data returns a hosted `full_size_url`), then place it. An image inserted inside a native Text block's rich text stayed editable in the builder and rendered correctly in Gmail. Give it `width="564"` (or the image's own width if smaller), an inline `max-width: 100%; height: auto`, meaningful alt text and a link to the email's main destination, then read all four back. Prepare images at about twice display width for sharp rendering.

Keep animated GIFs short and small: a 10-second product screen recording at 6 fps, 560 px wide and a 64-colour palette came in under 1 MB. Some desktop clients show only the first frame, so make sure the first frame works on its own. A wide infographic becomes unreadable at email width on a phone. Crop it to the part that supports the paragraph around it, and let the copy carry the detail. An asset supplied by the campaign owner is usually the one to reuse; check for their latest version before cropping.

### When the builder stops responding

UI actions that timeout can still have saved. Re-inspect the page and campaign before retrying. Derive a fresh locator or screen position after layout changes. A screenshot's coordinate frame can differ from the page's own scale, so take block and icon positions from a zoom of the live surface rather than from page geometry alone.

Observed recovery patterns, in order of cost: if a Text block's editor will not open from its pencil icon, click the block body once or double-click its text; if neither opens it, close the tab and open the campaign in a fresh tab, where a single click usually works. A browser tab driven by an agent can stop responding entirely when its window is behind another application; ask the operator to bring it to the front rather than retrying. Long waits run inside the page right after navigation froze the renderer once; wait with the tool instead. Avoid undocumented page-script mutations as a portable operating method; prefer supported UI controls and the documented API. A scripted, campaign-scoped adapter is useful only when its preconditions, allowlist and post-write verification are explicit.

## Independent readback

Compare HTML and plain text separately with the approved source. Inspect text order, emphasis, spacing, block type/order, subject and preview, banner identity, image alt text and links, button appearance, footer and unsubscribe elements. Check links without opening a URL whose GET can record an RSVP or other action; automated Link Checker may also visit it. Verify actual price, eligibility, expiry, checkout and QR from current evidence before inserting an offer. Visual desktop/mobile review complements structural checks; neither replaces the other.

An exact before/after comparison should show only authorised changes. Builder edits did not update the plain-text version in any campaign observed, including regular broadcasts. After each copy change, update plain text: for a one-phrase edit, replace the phrase only after confirming it appears exactly once; for a rewrite, regenerate the body from the saved HTML (links as `label (url)`, images omitted) and keep the original footer section. If a plain-text-only API update is necessary, back up first, then prove the HTML is byte-identical and the native builder still opens. Keep the original backup immutable. If another editor changes the campaign meanwhile, stop rather than overwriting their work with an old backup.
