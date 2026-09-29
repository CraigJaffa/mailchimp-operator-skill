# Native content: edit and verify

## Decide whether the builder owns the content

Read the campaign's content and inspect its editor. Native drag-and-drop markup can contain `mcnTextBlock` or `mcnButtonBlock` while the API returns `template: null` and no `mc:edit` regions. Conversely, a non-zero `settings.template_id` does not prove that an API HTML replacement will remain editable. A whole-content `PUT` may return success yet leave builder blocks unchanged, or may flatten the design into code. Do not treat an HTTP 200 as proof of a native edit.

Preserve editable Image, Text and Button blocks, plus the intended footer. Clone a correctly styled block when appropriate, replace its copy or asset and inspect its actual position after every structural operation. Template assignment and earlier edits can carry hidden old blocks, stale copy, duplicate footers or wrong alignment. Verify the block tree after a reload. Do not build a new email from an old campaign without inspecting its inherited trigger, audience and design state.

Rich text Source within a native Text block may preserve editability; a whole Code block does not meet a native-editability requirement. Image selection is complete only after the chosen asset is inserted and its alt text and destination are read back. Buttons may default to the wrong colour, font, padding or corner radius. Set the real style controls and verify the saved rendered result; do not infer a round button from a design description.

### Images and GIFs

Upload the asset to the account's file manager (the API `POST /file-manager/files` with a file name and base64 data returns a hosted `full_size_url`), then place it. An image inserted inside a native Text block's rich text stayed editable in the builder and rendered correctly in Gmail. Give it `width="564"` (or the image's own width if smaller), an inline `max-width: 100%; height: auto`, meaningful alt text and a link to the email's main destination, then read all four back. Prepare images at about twice display width for sharp rendering.

Keep animated GIFs short and small: a 10-second product screen recording at 6 fps, 560 px wide and a 64-colour palette came in under 1 MB. Some desktop clients show only the first frame, so make sure the first frame works on its own. Name uploads with a campaign prefix (for example `cohort-oct-...`). Content Studio search matches the file name you gave it, not the hashed file name in the hosted URL, so an unnamed or hashed asset is hard to find again. Uploaded images appear in the builder's Content Studio straight away. Its toolbar shows two "Insert" buttons and a "Delete" button nearby: confirm the element under the pointer before clicking.

To restore an image that was deleted from a draft, use Content Studio's Upload menu, "Import from URL", with the original hosted URL. The re-imported file was byte-identical, with a new URL. Re-enter alt text and any link, and match the original width.

A third-party image placed inside a Text block without a width (for example a countdown timer GIF from a service such as Sendtric) can force the whole email wider than a phone screen. In the mobile preview every block's text runs off the right edge. Fix it in the Text block's Source: `width="400"` (the image's natural width) plus `style="width: 400px; max-width: 100%; height: auto;"` and alt text. After the fix, check the mobile preview again: text should wrap inside the frame.

Countdown GIFs are rendered when opened. To check one, fetch the image URL and compare the remaining time shown against the current clock; the result should land on the landing page's deadline. The timer's owner resets it in the timer service; the email keeps the same URL.

Screenshots shared in team chat usually arrive as WebP. Convert them to PNG or JPEG, resize to about 1,200 px wide, and blur the names and avatars of community members before using a member's post in an email. Ask whether the person has agreed to the quote being used. On a machine without an imaging library, macOS Core Image (a short Swift script using `applyingGaussianBlur` on cropped regions) did the blurring.

A wide infographic becomes unreadable at email width on a phone. Crop it to the part that supports the paragraph around it, and let the copy carry the detail. An asset supplied by the campaign owner is usually the one to reuse; check for their latest version before cropping.

### Inserting an image in the middle of existing copy

A Text block cannot hold a native Image block inside it. To put an image between two paragraphs: clone the Text block (its duplicate lands directly below and keeps its styling), cut the first block down to the copy before the image and the clone down to the copy after it (in Source mode, splice by unique text markers and confirm each marker appears once), then drag the palette's Image tile onto the top 40 to 50 px of the second block. The new Image block is inserted above it and its panel opens. Add the image, alt text and link, then Save & Close. The same pattern adds a new Text block anywhere: clone the nearest styled Text block and replace its Source.

A block's hover toolbar sometimes does not appear on the first hover. Move the pointer off the preview (for example over the side panel), then back onto the block, and read the toolbar icon's position from the page again before clicking. If the reported icon position is off-screen or negative, the toolbar is not showing.

### When the builder stops responding

UI actions that timeout can still have saved. Re-inspect the page and campaign before retrying. Derive a fresh locator or screen position after layout changes. A screenshot's coordinate frame can differ from the page's own scale, so take block and icon positions from a zoom of the live surface rather than from page geometry alone.

Observed recovery patterns, in order of cost: if a Text block's editor will not open from its pencil icon, click the block body once or double-click its text; if neither opens it, close the tab and open the campaign in a fresh tab, where a single click usually works. A browser tab driven by an agent can stop responding entirely when its window is behind another application; ask the operator to bring it to the front rather than retrying. Long waits run inside the page right after navigation froze the renderer once; wait with the tool instead. Avoid undocumented page-script mutations as a portable operating method; prefer supported UI controls and the documented API. A scripted, campaign-scoped adapter is useful only when its preconditions, allowlist and post-write verification are explicit.

## Independent readback

Compare HTML and plain text separately with the approved source. Inspect text order, emphasis, spacing, block type/order, subject and preview, banner identity, image alt text and links, button appearance, footer and unsubscribe elements. Check links without opening a URL whose GET can record an RSVP or other action; automated Link Checker may also visit it. Verify actual price, eligibility, expiry, checkout and QR from current evidence before inserting an offer. Visual desktop/mobile review complements structural checks; neither replaces the other.

An exact before/after comparison should show only authorised changes. Builder edits usually did not update the plain-text version, including on regular broadcasts. In one regular draft whose plain text had never been replaced through the API, a later builder edit did appear in the plain text. Read the plain text back after every edit rather than assuming either way. After each copy change, update plain text: for a one-phrase edit, replace the phrase only after confirming it appears exactly once; for a rewrite, regenerate the body from the saved HTML (links as `label (url)`, images omitted) and keep the original footer section. If a plain-text-only API update is necessary, back up first, then prove the HTML is byte-identical and the native builder still opens. Keep the original backup immutable. If another editor changes the campaign meanwhile, stop rather than overwriting their work with an old backup.

## Scripted edits in the builder (observed 29 Sep 2026)

These worked across 13 automation emails and 1 broadcast. Each edit was verified by API readback: `template` still null, native `mcn*Block` markup intact, and the builder reopened normally.

**Opening a block's editor**
- Hover over the block with the real pointer, then click its pencil icon with the real pointer.
- A script `click()` on the edit icon did not open the editor.
- Find the pencil's position from a fresh screenshot or zoom after every page load. The window size can change mid-session, which moves every toolbar.

**Changing the text**
- Once the editor is open, read the visible CKEditor instance with `getData()`, make the changes, write back with `setData()`, then click the visible "Save & Close".
- Use one exact-count check per change: count the pattern's matches first, and refuse the whole edit if any count differs from what is expected.
- CKEditor returns `<br />`, `&#39;` and `&nbsp;`, so patterns must allow for them.
- Use a function as the replacement, so `$` in the new text is not treated as a special pattern.
- Keep any `<style>` element that sits at the end of the block's data.

**Buttons**
- The panel has a label field and a "Web address (URL)" field.
- Triple-click the field, type, press Tab, then "Save & Close".
- Read back the `a.mcnButton` href in the preview frame.

**Image links**
- Select the image, then choose "Link" in the image panel.
- In the "Insert/Edit Link" dialog, fill "Web address (URL)" and click "Insert".

**Leaving the builder**
- For regular broadcasts, the builder URL is also `/campaigns/wizard/neapolitan?id=WEB_ID`. The top-bar "Save & Close" saves and returns to Home.
- For automation emails, leave through "Save and Return to Workflow".

**Browser behaviour**
- DOM reads and CKEditor writes run in a background tab, but clicks, panels and saves need the tab in front. Bring it forward before each batch of clicks, because the operator using the computer hides it again.
- The browser tool's output filter blocks results that contain URLs with query strings (calendar links, for example). Return counts or true/false checks instead.
- Gmail's compose window rejects `innerHTML` (TrustedHTML), so build the body from DOM text nodes and `<br>` elements. Then type one real keystroke so Gmail saves the draft, and confirm it appears under Drafts.

**Zoom invitations and calendar links**
- An owner pasting a Zoom invitation into the registration email put it below the sign-off, which duplicated the join link, and the old placeholders stayed above it. Rebuild the whole text block in one clean layout:
  - greeting and session title
  - date and time with UK and ET
  - join link and an "Add To Calendar" link
  - one-tap numbers and dial-in numbers
  - Webinar ID and the international numbers link
  - sign-off
- Build the calendar link as `https://calendar.google.com/calendar/render?action=TEMPLATE&text=...&dates=YYYYMMDDTHHMMSSZ/YYYYMMDDTHHMMSSZ&details=...&location=...`:
  - Use UTC times: in BST, 17:00 UK is `160000Z`.
  - URL-encode each value.
  - Write `&amp;` in the HTML.
- Verify by parsing the saved href's query back into fields.

**Countdown timers**
- When a timer image moves to a new timer, search every email and broadcast for the old timer code. Invites and reminders both carry it.
- Swap the image `src` in the Text block, and check the new URL returns a GIF.

**Plain-text regeneration**
- Build it from the saved HTML: links as `label (url)`, list items as `* `, image alt text kept, footer kept from the old plain text.
- Collapse a `<br>` followed by a newline into one line break, or dial-in lists come out double spaced.
- The plain-text-only PUT worked on draft (`save`) and paused automation emails. In both cases the HTML stayed byte-identical.
