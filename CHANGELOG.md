# Changelog

All notable changes to Bookmark Tab Manager.

## v2.3.19 — September 2026 (Latest)

- **Improved:** Clarified the device-activation security note from v2.3.18 — the one-time email code applies from the 2nd device onward; a license's very first activation was always exempt, the wording just didn't say so clearly.

## v2.3.18 — September 2026

- **New (Security):** 🔐 Activating a license key on a new device — the 2nd device onward — now requires a one-time 6-digit code emailed to the address on file for that license, entered to finish activating. A key's very first-ever activation is never gated this way (nobody else has had the chance to see the key yet at that point), and reinstalling or reactivating on a device that's already activated is unaffected too. The reasoning: a license key is just text, so if one ever ends up somewhere it shouldn't (shared publicly, screenshotted, etc.), this stops a stranger from activating it on their own device, since they have no access to the inbox the code goes to. It doesn't stop a buyer who deliberately hands their key *and* the code to someone they know — no software-side check can — but that's a much narrower case than an anonymous leak.

## v2.3.17 — September 2026

- **New (Free):** ⓘ Added an info icon next to "Quick calculator (search box)" in Settings → 🛠 Tools — click it for a quick popup of example queries (math, unit conversion, and currency conversion) right where you're deciding whether to turn the feature on, instead of needing to already know it's explained in the FAQ.

## v2.3.16 — September 2026

- **Fix (Free):** 💱 Currency conversion could still fail for perfectly-supported pairs (e.g. "100 usd to inr") even after the v2.3.15 currency-list fix, still showing the misleading "Currency conversion failed — check your connection" message. Root cause: the free exchange-rate API this feature uses (Frankfurter, ECB reference rates) moved its public address from `api.frankfurter.app` to `api.frankfurter.dev`, and the old address now answers with an HTTP redirect instead of data directly — which a cross-origin `fetch()` from an extension page doesn't always follow the way a normal page navigation would, so the request quietly failed. Fixed by calling the new address (`api.frankfurter.dev/v1`) directly. No settings or behavior changed on your end — just paste an amount and two currencies as before.

## v2.3.15 — September 2026

- **Fix (Free):** 💱 Currency conversion (added in v2.3.14) could show "Currency conversion failed — check your connection" for AED, SAR, and RUB — that message was misleading, since it wasn't a connection problem: those three currencies were never actually available from Frankfurter, the free exchange-rate API this feature uses. RUB was dropped from ECB reference rates after the 2022 sanctions, and AED/SAR are USD-pegged Gulf currencies the ECB has never tracked. Removed those three and verified the remaining list against Frankfurter's own `/currencies` endpoint, adding several currencies we'd missed that ARE genuinely supported (CZK, HUF, IDR, ILS, MYR, PHP, RON, ISK) — 30 total now, all confirmed working.
- **New (Free):** ❓ Added an FAQ entry — in both the in-app Settings FAQ and this website's FAQ — explaining exactly how to use the quick calculator and currency conversion features in the search box, including the full list of supported currencies.
- **Fix (Free):** 🖱️ The v2.3.14 middle-click fix worked in Firefox but not Chrome. Root cause: Chrome has its own middle-click "autoscroll" gesture that can claim the mousedown before the page's own `auxclick` handler ever fires — a known Chromium quirk on non-`<a>` elements that Firefox doesn't share. Fixed by calling `preventDefault()` on the middle button's `mousedown` first, which suppresses Chrome's autoscroll takeover so the background-tab handler now fires reliably in both browsers.
- **Fix (Firefox-only, no user-facing change):** 🦊 The Firefox package's manifest had a `data_collection_permissions` entry (Mozilla's new data-collection-disclosure requirement) sitting at the top level of manifest.json, where Firefox doesn't recognize it — it belongs nested under `browser_specific_settings.gecko`. Moved to the correct location and filled in based on what BTM actually collects per the Privacy Policy (location for weather, bookmark titles/URLs for AI Auto-Categorize and Drive Backup, email for Pro purchases, and the anonymous install/update ping) — all listed as optional, since every one of those features can be turned off or is opt-in.

## v2.3.14 — September 2026

- **Fix (Free):** 🔗 Right-click → "Open in new tab" (single bookmark) and a folder's "Open all in new tabs" both used `window.open(url,'_blank')`, which steals browser focus to the newly opened tab(s) — you'd get yanked away from BTM's own new tab every time. Both now use `chrome.tabs.create({url, active:false})` instead, the same silent-background pattern already used elsewhere in the codebase (auto-filling a bookmark's title from its URL), so links open behind the scenes and you stay right where you were.
- **Fix (Free):** 🖱️ Middle-click and Ctrl/Cmd+click didn't work on any bookmark card. Root cause: bookmark cards are `<div>`s with a single left-click handler, not real `<a>` links, so they never got the browser's native new-tab-click behavior for free — middle-click fires a different event (`auxclick`) that nothing was listening for, and Ctrl/Cmd+click fired the same handler but nothing checked for the modifier, so it just navigated the current tab anyway. Fixed everywhere this pattern appeared: the main grid cards, the pinned bar, Most Visited, Recently Visited, and the search-preview dropdown all now open silently in a new background tab on middle-click or Ctrl/Cmd+click.
- **New (Free):** 🧮 Quick calculator in the search box — type a plain math expression (`12*7+3`) or a unit conversion (`10 km to miles`, `100 f to c`, `5 kg in lbs`) and see the answer as a highlighted top row in the search dropdown; click it to copy the result. Fully offline, no network call. Toggle it off in Settings → 🛠 Tools if you'd rather the search box only ever search bookmarks.
- **New (Free):** 💱 Optional currency conversion in the same search box (`100 usd to inr`) — looks up the live rate from the free Frankfurter API (ECB reference rates, no API key, no personal data sent, just the amount and two currency codes). **Off by default** since it's a new network call triggered from the search bar; turn it on in Settings → 🛠 Tools → "Currency conversion (uses network)". See the Privacy Policy for exactly what's sent.
- **New (Free):** 📋 A "Copy list as text" button now appears above your search results whenever you're searching — copies every currently-matching bookmark (folder heading + name + URL) across every matched folder to your clipboard in one action, extending the existing per-folder "Copy all links in folder" to the whole search result set at once.
- **New (Pro):** ↔️ Widget alignment controls in Settings → 🕐 Clock & Weather — set the clock (paired with the Dual Timezone Clock, since they already render as one row) to left/center/right, and set Weather independently. Stats and Pinned are unaffected by design.

## v2.3.13 — September 2026

- **Fix (Backend/Security):** 🔒 The Razorpay account behind BTM's payments is also used by another of our apps (RecruitLens). Razorpay fires the same `payment.captured`/`payment_link.paid` webhook events for every payment on the account regardless of which app it was for, and BTM's webhook (`razorpay-webhook.ts`) treated any such event that wasn't an explicit donation as a BTM Pro purchase — so RecruitLens's own payments were falling into that fallback and getting issued real BTM license keys + confirmation emails. Fixed by requiring every BTM payment to carry a `notes.product === 'BTM'` marker before a license is ever issued; the webhook now rejects (ignores, no DB write, no email) any captured payment that doesn't carry it. Every BTM-initiated Razorpay link — the static plan-purchase links, the discounted-price link, the trial-upgrade link, the renewal reminder/warning links, and the license-validation renewal URL — now stamps that marker so real BTM purchases are unaffected. No action needed by users; this only affects the license-issuing backend.
- **Fix (Backend, follow-up):** 🔁 While reviewing the fix above, found the license/renewal flow had no duplicate-webhook protection — unlike the donation flow, which already checks for a repeat within 60 seconds. A Razorpay retry (or duplicate delivery) of the same payment event could re-run the renewal path a second time for a purchase that was already processed (observed: a fresh license "renewed" itself ~2 minutes after being created, before ever being activated). Fixed by skipping processing entirely if a license already has that exact `razorpay_id` on record.

## v2.3.12 — September 2026

- **Fix (Pro):** 🖌 Fixed the new "Space tab" text-color picker (added in v2.3.11) appearing to do nothing. The CSS only wired the custom color into the *inactive*-tab and hover rules (`.tab{color:var(--tab-text,...)}`, `.tab:hover:not(.active){color:var(--tab-text-hover,...)}`) — the currently selected/active tab had `color:#000` hardcoded directly on `.tab.active`, a more specific rule that always won regardless of the picker. On a single-space setup, or whichever tab happened to be active while testing, this made the picker look completely broken. Fixed by also driving the active tab's text through the same pref via a new `--tab-text-active` CSS variable (`R.style.setProperty('--tab-text-active', prefs.tabTextColor || '#000')` in `applyTheme()`), so the picker now visibly affects every tab, active or not.

## v2.3.11 — September 2026

- **New (Free):** 🙈 Added a "Hide page title" toggle in Settings → Widgets, directly below the Page Title/Subtitle text fields. Uses `visibility:hidden` rather than `display:none` on the `.header` box specifically so its layout height is preserved — the fixed-position settings gear icon and the space-tabs row's right-corner buttons (+/⊞/⇅) never shift or collide when the title is hidden.
- **New (Free):** ↺ The per-space "Space Background" popup now has an always-visible "Reset to default" button. Previously the popup only let you clear a custom color (via a "none" swatch) or a custom image (via a conditional "Remove background image" button that only appeared once an image was set) — clearing one never touched the other, so it was easy to end up unable to fully revert a space back to the global theme. The new button clears both `data.spaceBgColors[spaceId]` and `data.spaceBgImages[spaceId]` in one click.
- **New (Pro):** 🎨 Added a "Space tab" color picker in Settings → 🖌 Colors & Font, directly under Accent — lets you set a global custom text color for your space tab labels, wired the same way as the existing Accent/Text/Secondary pickers (live preview, saved on change, included in that section's "↺ Reset" button). See the v2.3.12 fix above — this shipped with a bug where it didn't visibly affect the active tab.

## v2.3.10 — September 2026

- **UI (Free):** 🎨 Changed the Donate popup's disclaimer text ("100% voluntary. You're not purchasing anything.") and the dynamic payment-method caption above the orange donate button from a low-contrast gray to white, for readability against the popup's pitch-black background.

## v2.3.9 — September 2026

- **Fix (Free):** 💙 Fixed the Donate popup's PayPal link always billing in USD even when an ₹ (INR) amount was selected. `paypal.me` amount links bill in the receiving account's default currency unless the amount is suffixed with an explicit ISO currency code (e.g. `/25INR` vs `/25`) — the code built the amount without that suffix, so an INR selection still opened PayPal showing a $ amount. Fixed by appending the correct currency suffix based on the selected amount's currency.
- **UI (Free):** ☕ The bottom orange "Donate" button silently duplicates whichever payment method matches your region (Razorpay for India, PayPal otherwise) without saying so. Added a caption above the button naming which method it will use, so this isn't a surprise.
- **Changed (Free):** ⏳ Consolidated the "please wait" hold/disable logic (added for Razorpay in v2.3.8) into one shared helper (`withDonateHold`) covering all four donate buttons (Razorpay, UPI, PayPal, and the bottom orange button) for consistent behavior — a real wait for Razorpay's payment-link creation, and a short ~500ms cosmetic wait for UPI/PayPal's otherwise-instant paths, so every donate path gives the same "please wait" feedback instead of some buttons reacting instantly and others visibly pausing.

## v2.3.8 — September 2026

- **New (Free):** ☕ Added the same "please wait" hold/disable pattern already used on the Upgrade popup's plan buttons to the Donate popup's Razorpay button — it now shows "Please wait..." and disables itself while its payment link is being created, so it can't be double-clicked into opening two payment tabs.

## v2.3.7 — September 2026

- **New (Free):** ❓ Added a new FAQ entry (in both the in-app Settings FAQ and this website's FAQ) explaining exactly what "Restore JSON backup" vs "Restore HTML backup" each bring back: JSON is a full restore (Spaces, bookmarks, and settings), while HTML restore only brings back bookmarks/folders, since the standard browser bookmark HTML format has no way to store BTM settings or multiple Spaces.

## v2.3.6 — September 2026

- **Fix (Free):** 📤 Fixed `_buildBookmarkHTML()` (used by HTML export/backup) flattening nested sub-folder structure — it ignored each bookmark/folder's `parentId` and wrote every category out at the same indent level. Rewritten to recurse through the real `parentId` chain (with a depth guard against malformed/cyclic data) so exported HTML now preserves real nested sub-folder structure, matching what a real browser bookmarks export looks like.
- **Fix (Free):** 🔗 Fixed `dedupNode()` in the onboarding HTML-import flow silently dropping a bookmark if its URL already existed anywhere else across *any* folder, even in a completely different folder than the one being imported into — so importing a real Chrome bookmarks HTML export could lose bookmarks that legitimately appeared in more than one folder. Removed the overly-aggressive cross-folder dedup entirely.
- **New (Free):** 📥 Added a dedicated "Import Bookmarks HTML (from file)" button, and split the previous single auto-detecting restore button on the onboarding/reset screen into two explicit buttons — "Restore JSON backup" and "Restore HTML backup" — so it's clear up front which kind of backup file to pick, instead of relying on the app to guess correctly.

## v2.3.5 — September 2026 (website copy fix Sep 18)

- **Fix (Website):** 📝 Removed a stale, contradictory FAQ entry ("How are my Chrome/Firefox sub-folders handled during import?") on the website that still described the old pre-nesting "Work / Project A" name-flattening behavior and said "True nested sub-folders inside BTM are planned for a future version" — directly contradicting the correct, up-to-date FAQ answer right above it about the current 5-level Pro-only nesting. Caught right before a store release.

- **Fix (Admin):** 📊 Fixed the new Admin Dashboard OTP login popup (`#adminOtpPopup`, added in v2.3.4) opening invisibly underneath the Admin Dashboard itself. The dashboard's own popup (`#adminPopup`) has an id-specific `z-index:9998`, far above the shared `.modal-overlay` class's `z-index:100` that the new OTP popup used — so when the OTP flow triggered from inside the already-open dashboard, it technically opened (the `.open` class was added correctly) but rendered completely behind the dashboard, unclickable. This also explains why no email arrived: the "Send code to my email" button lives inside that hidden popup, so the request to actually send the code was never triggered. Fixed with the same override already used for `#forgotPinPopup` for an identical reason — `#adminOtpPopup{z-index:9999;}` — so it now renders on top of the dashboard.
- **UI (Free):** 🧹 Shortened every in-app changelog entry from v2.2.1 through v2.3.4 down to a single plain-language line each, since the full technical root-cause writeups (kept below, unchanged) were originally written for this file but were also being displayed verbatim in the app's own "What's New" popup — several paragraphs long per item. The in-app popup now shows short one-liners; this file remains the detailed technical record.

## v2.3.4 — September 2026

- **Fix (Admin):** 📊 Fixed the Admin Dashboard's email login code failing to appear, with a console warning that a `window.prompt()` dialog "was suppressed because this page is not the active tab of the front window." The OTP flow requested the emailed code with an `await fetch(...)` call, then immediately called the native `window.prompt()` to collect it — Chrome suppresses `window.prompt()`/`alert()`/`confirm()` whenever they fire from a background tab, or after enough async delay since the last click that it no longer counts as a direct response to a user gesture, which is exactly what a "request a code by email, then get prompted for it" flow runs into. Replaced it with a proper in-page popup (a small two-step modal, mirroring the existing "Forgot PIN" flow's design) that has no such restriction — same behavior otherwise: request the code, enter it, unlock the dashboard for 2 hours.

## v2.3.3 — September 2026

- **Fix (Free):** 👁️ Fixed the Cancel button on the delete-folder "Are you sure?" popup being nearly invisible — it used a muted gray text color on a near-transparent background that could blend into the popup depending on theme, so it read as blank. Gave it the same visual treatment as the red Delete button next to it (a solid tinted background + border), just in a neutral white shade instead of red, so it's clearly readable in every theme.

## v2.3.2 — September 2026

- **Fix (Free):** 📜 Really fixed sub-folder scrolling — a real Chrome bookmarks import + manual drag-nesting test showed folders getting visibly cut off mid-row (a favicon/name chopped in half) with no scrollbar in sight, specifically when several sub-folder rows were stacked one under another. Root cause: each sub-folder card is a flex *item* inside its parent's `.subfolder-wrap` (`display:flex;flex-direction:column`), and every `.cat-card` (including sub-folder cards) sets `overflow:hidden` for its own blur effect. Per the CSS Flexbox spec, an element with `overflow` other than `visible` counts as a "scroll container", and a flex item that's a scroll container gets an *automatic minimum main-size of 0* instead of one based on its content — so when several stacked sub-folder rows didn't all fit under the wrap's height cap, the browser's flex-shrink math was actually allowed to squeeze these cards smaller than their real content (clipped by their own `overflow:hidden`) instead of leaving them full-size and letting the wrap's `overflow-y:auto` take over and scroll. This is a *different* mechanism from the backdrop-filter compositing quirk that v2.2.3/v2.3.0/v2.3.1's `contain:paint` fixes correctly addressed — which is why those earlier rounds never fully closed this out. Fixed by adding `flex-shrink:0` to `.cat-card.subfolder-card`, so a sub-folder row can never be compressed below its true height; once a stack of them genuinely exceeds the cap, the wrap itself overflows and scrolls as intended. Verified with a real WebKit render, before and after, reproducing the exact "one folder shows fully, the next one is chopped off" symptom and confirming the fix.
- **UI (Free):** 📏 Sub-folder scroll height reduced from ~15 visible bookmark rows (360px) down to 5 rows (120px) — applies consistently to both a sub-folder's own bookmark list and the stack of sibling sub-folders inside a parent, per request.
- **Fix (Free):** 🗑️ Delete-folder confirmation ("Are you sure?", shown for both a top-level folder's own 🗑️ Delete and a sub-folder row's ✕ delete when it has nested sub-folders under it) now closes if you click anywhere on the dark background outside the popup box, matching every other popup in the app. It already had a labeled Cancel button that safely closes it without deleting anything — the missing piece was specifically the click-outside-to-dismiss, since this one popup hadn't been wired up to the same outside-click pattern (`el.onclick=e=>{if(e.target===el)…}`) the rest of the app's popups already use.

## v2.3.1 — September 2026

- **Fix (Free):** 🖱️ Fixed drag-and-drop folder nesting: dragging a whole top-level folder (with its nested sub-folder chain along for the ride) and dropping it onto a *different* top-level folder now actually nests it there. Root cause: the drop handler in `setupCardDrag()` (`actions.js`) only ever did a flat array reorder (splice the dragged category out, splice it back in at the target's index) — it never touched `parentId`, so "convert to sub-folder via drag" was never actually wired up as a drop target; the only real path to nesting was the "Move into folder…" menu action. A folder card dropped onto a different, eligible folder card now calls `convertToSubfolder()` — the same function the menu action uses — gated by the same 5-level depth cap, Pro-at-every-level rule, and self/descendant cycle guard. Hold Shift while dropping for the old plain-reorder behavior.
- **Fix (Free):** 📁 Fixed the depth cap appearing not to hold (a test screenshot showed nesting reaching level 11). The 5-level cap itself was correctly enforced everywhere it's checked — the real bug was in `eligibleParentFolders()`, behind the "Move into folder…" picker: it only checked the *candidate parent's* own depth, never how deep the folder actually being moved would land once its own existing sub-folder chain came along too. The picker could therefore offer a target that `convertToSubfolder()` would then silently reject, with only a fading 3.5s toast as feedback — leading straight into building a second, independent nesting chain (via a fresh top-level folder) that visually read as a continuation past depth 5. Fixed the picker to use the exact same depth-plus-subtree math as `convertToSubfolder()`, and replaced the fading toast with a persistent `alert()` explaining exactly why a move was rejected.
- **Fix (Free):** 👁️ Fixed manually-added deep sub-folders being unreachable ("3 sub-folders inside a sub-folder, no way to see or add to them"). Recursive rendering itself has no depth limit and always drew every level correctly — the actual gap was that "Add sub-folder" only ever appeared in a top-level folder's own ⋮ menu, and always attached a new folder as *that top-level folder's* own direct child; a sub-folder card had no menu at all (removed entirely in v2.2.0), so there was no way to add a new child directly under an already-nested folder. Added a minimal "＋" icon directly on sub-folder rows, wired to `addSubfolder()` with that card's own id (not a top-level ancestor's), so every level 1-5 is directly reachable and extendable.
- **UI (Free):** 🎨 Fixed sub-folder rows still stair-stepping deeper the more they were nested, despite v2.2.2's attempted fix. v2.2.2 zeroed the nested `.subfolder-wrap`'s own left *margin*, but each sub-folder card is a real DOM child recursively wrapped inside its parent's own `.cat-card` box, and `.cat-card.subfolder-card` carries its own fixed 4px left *padding* at every level — that per-level padding kept compounding regardless of the wrap's margin. Fixed with a matching `-4px` left margin on every nested wrap that exactly cancels one ancestor's padding per level, so depth 1 through depth 5 rows all sit at the identical horizontal position now.
- **UI (Free):** 📜 Replaced the arbitrary 220px/260px scroll-height guesses with a height computed from the actual rendered row size: a `.bm-item` row is 24px tall (5px+5px padding around a 14px favicon), so both a sub-folder's own bookmark list and the list of sibling sub-folders now cap at 360px — roughly 15 rows visible before scrolling, consistently at every level.
- **Fix (Free):** 🗑️ Sub-folders can be deleted again: brought back a single minimal ✕ delete icon on sub-folder rows (not the full ⋮ menu) — v2.2.0 removed sub-folder rows' entire action row, including any way to delete one directly.
- **Fix (Free):** 🌳 Deleting a parent folder now deletes its entire sub-folder tree with it, instead of promoting its children to top-level. Rewrote `deleteCategory()` to recursively collect every descendant category at any depth and remove them all together with the parent (bookmarks included, no promotion), with a confirmation showing exactly how many sub-folders and total bookmarks will be deleted, full metadata cleanup for every deleted id, and full undo support restoring the whole subtree. The new sub-folder ✕ delete button uses this same cascading path.

## v2.3.0 — September 2026

- **Fix (Free):** 🖱️ Really fixed drag-and-drop of sub-folders — the v2.2.3 "fix" broke it completely instead. Root cause: v2.2.3 added `e.stopPropagation()` to `setupCardDrag()`'s `dragstart`/`dragend` handlers to stop a nested sub-folder's `dragstart` bubbling into its parent card's own listener (which was overwriting the shared `dragCat` global with the parent's id). But calling `stopPropagation()` inside a native `dragstart` handler doesn't just stop DOM bubbling — it interrupts the browser's own internal bookkeeping for that drag session whenever other listeners for the same event type exist further up the same element chain, which is always true here since every ancestor card wires its own `dragstart`/`dragend` via `setupCardDrag()`. The drag session was getting silently aborted right after starting, so `dragover`/`drop` never fired again for ANY card — total breakage, not just wrong targeting. Fixed properly: removed `stopPropagation()` from `dragstart`/`dragend` entirely and replaced it with an identity check (`if(e.target!==card)return;`) so only the listener belonging to the actual card that was grabbed acts on the event, with zero interference with the native drag session. Dragging a leaf or nested sub-folder now works again and only ever affects that one specific folder.
- **Fix (Free):** 📜 Really fixed the missing scrollbar on a sub-folder with 10+ of its own bookmarks — the v2.2.3 fix targeted the wrong element. `contain:paint` on `.subfolder-wrap-scroll` only bounds height for the list of *sibling* sub-folders inside a parent, which was already working; a single sub-folder's own bookmark list lives in that sub-folder's own `.cat-body`, which had no height cap or overflow rule at all, so a long list just grew past the wrap's 260px instead of being clipped. Added `max-height:220px;overflow-y:auto` directly on `.cat-card.subfolder-card .cat-body` in `newtab.html`, so each sub-folder's own expanded bookmark list scrolls independently. `contain:paint` is kept on the outer wrap since it's still relevant for the many-siblings case.
- **New (Pro):** 📁 Max sub-folder nesting depth raised from 3 to 5 levels — real-world usage of the redesigned flat-row sub-folder UI showed it comfortably handles deeper structures, and fewer than 0.1% of users would even reach it.
- **Changed (Pro):** 🔒 Sub-folder nesting is now Pro-only at every level. Previously the first level of nesting was free for everyone and only levels 2-3 required Pro; free plans can no longer create ANY sub-folder — even one level requires Pro. Applies to "Add sub-folder", "Move into folder…", "Split into sub-folder", dragging into a new parent, and every import path (HTML file import, Settings → Chrome Bookmarks Sync, first-run onboarding import, and the Import Folder widget).
- **New (Free):** 🛡️ Existing sub-folder structures are preserved even if your Pro plan lapses — nothing at app load or render time checks your plan against existing folders, so a structure built while Pro stays exactly as it is. The Pro gate only runs at the moment of a new action. A fresh import with nested folders on a non-Pro plan now demotes ALL nesting to flat top-level folders (not just levels 2+), with a popup explaining why.
- **New (Free):** ❓ FAQ updated to reflect the 5-level depth cap and the Pro-only-at-every-level nesting rule.

## v2.2.3 — September 2026

- **Fix (Free):** 📜 Fixed the v2.2.0 internal sub-folder scroll container: expanding a top-level folder's sub-folder to reveal its own long bookmark list overflowed straight out of the card with no visible scrollbar. Root cause: `.cat-card` uses `backdrop-filter` for the card-blur effect, and Chromium/WebKit have a known compositing quirk where an `overflow:auto` box nested inside a `backdrop-filter`/`filter` ancestor stops actually clipping its painted content (`scrollHeight`/`scrollTop` still worked, the clip just never rendered on screen). Added `contain:paint` (plus `overflow-x:hidden`) to `.subfolder-wrap-scroll` in `newtab.html` so it establishes its own paint/clip boundary independent of the ancestor's filter, making the 260px scroll area — and its scrollbar — actually work whether many sub-folders or one sub-folder's own long bookmark list is what's overflowing.
- **Fix (Free):** 🖱️ Fixed drag-and-drop moving a whole sub-folder subtree even when only the deepest/leaf sub-folder was dragged. Root cause: every card (top-level AND sub-folder) wires `setupCardDrag()` to itself, and a sub-folder card is nested inside its parent's own `.cat-card` — HTML5 `dragstart`/`dragend` events bubble, so dragging a nested sub-folder fired its own `dragstart` (correctly setting the shared `dragCat` to the child's id) and then bubbled straight into the PARENT card's own `dragstart` listener, which unconditionally overwrote `dragCat`/`dragCatFromSpace` with the PARENT's id — so the drop always acted on the outermost ancestor (and everything still pointing at it via `parentId`) instead of the one sub-folder actually grabbed. Added `e.stopPropagation()` to `setupCardDrag()`'s `dragstart`/`dragend` handlers in `actions.js` (matching the `dragover`/`drop` handlers, which already had it) so a nested card's drag events stop at that card instead of re-triggering its ancestors' handlers.

## v2.2.2 — September 2026

- **Fix (Free):** 🔧 Fixed a 4th separate flat-import bug, found via A/B testing: the first-run onboarding modal's "📥 Import My Browser Bookmarks" flow (`onboarding.js` — `extractBrowserBookmarks()` / `performImport()`) had its own independent importer that flattened every nested Chrome/Firefox folder into a "Parent / Child / Grandchild"-named top-level folder — a bug distinct from (and found after) the already-fixed HTML-file import (`importChromeHTML()` in `actions.js`) and the Settings "🌐 Import Chrome bookmarks" button (`syncChromeBookmarks()` + `settings.js`). Rebuilt `extractBrowserBookmarks()` to keep each folder's real native children instead of collapsing them into name strings, and `performImport()` now reconstructs true `parentId`-based nesting up to depth 3 (flattening anything deeper into name-joined siblings, demoting Level 2/3 sub-folders to top-level with the same warning popup when the plan isn't Pro) — matching the other two import paths, for both "Import All to Home" and "Auto-Categorize" onboarding modes.
- **UI (Free):** 🎨 Sub-folder rows are now left-aligned at one fixed position regardless of nesting depth — removed the per-level `marginLeft` "staircase" indent in `render.js` so depth 1/2/3 sub-folders no longer creep further right, giving a flat list look instead of a tree. Collapse/expand is unchanged.

## v2.2.1 — September 2026

- **Fix (Free):** 🔧 Fixed a v2.2.0 regression where a locked+pinned sub-folder rendered as its own separate top-level folder instead of nesting inside its parent. Root cause: `buildCard()`'s locked-folder branch ended with `grid.appendChild(card); return;` instead of `return card;` — `grid` isn't even in scope inside `buildCard()`, and for a recursively-built sub-folder card this forced it straight into the top-level grid while returning `undefined` to the parent's sub-folder loop, which could also break rendering of that folder's remaining children. Now correctly `return card;`s so it's appended into its parent's sub-folder wrapper like every other sub-folder.

## v2.2.0 — September 2026

- **New (Free):** 🎨 Sub-folder visual redesign: nested folders no longer render as heavy stacked boxes with growing indent — they're now slim, integrated rows inside their parent folder, with just a title, bookmark count and the same expand/collapse arrow as before
- **Removed (Free):** 🧹 Sub-folder rows no longer show a ⋮ menu at all, and the "Sub-folder settings icons" toggle (added in v2.1.2) is removed entirely since it's no longer needed — every folder action stays available from the top-level ancestor's own menu
- **New (Free):** 📜 A folder with many or deeply nested sub-folders now scrolls internally instead of stretching the whole grid card
- **New (Free):** 📤 Share folder now recursively bundles a folder's own sub-folders (any depth) into the exported JSON, and importing that file rebuilds the same nested structure — respecting the usual depth-3 cap and Pro gating for 2-3 level nesting
- **Verified:** 🔄 Chrome Bookmarks Sync still mirrors sub-folders into the browser nested the same way they appear in BTM — unaffected by this visual redesign

## v2.1.3 — September 2026

- **Fix (Free):** 🔧 Fixed the real bug behind sub-folders showing as separate top-level folders: the "🌐 Import Chrome bookmarks" button (Settings) and the Firefox import button were each running their own older, flat, non-nested importer instead of the fixed one — both now rebuild real nesting up to 3 levels, same as Settings → Import & Backup

## v2.1.2 — September 2026

- **New (Free):** 🎨 Sub-folders now inherit their parent folder's own bg color/image/border color/font color by default — set your own color directly on a sub-folder any time to override it
- **New (Free):** ⚙️ New Settings → Bookmark Display toggle: "Sub-folder settings icons" — turn off to hide the ⋮ menu on nested sub-folder cards specifically, for a cleaner look (top-level folders are unaffected)

## v2.1.1 — September 2026

- **Fix (Free):** 🔒 Chrome/Firefox HTML import now correctly demotes Level 2/3 sub-folders to regular top-level folders when your Pro plan isn't active (including a lapsed Monthly/Yearly renewal) — with a popup explaining why, since Level 2/3 nesting is Pro-only everywhere else in the app too. Level 1 sub-folders still import nested, free for everyone
- **New (Free):** 🔄 Chrome Bookmarks Sync now mirrors sub-folders into Chrome/Firefox nested the same way they appear in BTM, instead of flattening every sub-folder into a sibling folder
- **UI (Pro):** 🎨 The Sub-folder Migration ("Split into sub-folder") popup now picks up that folder's own custom colors when set, instead of always showing the same fixed dark theme

## v2.1.0 — September 2026

- **New (Free):** ↳ Level-1 sub-folders (a sub-folder inside a top-level folder) are now FREE for everyone — no Pro required for that first level of nesting
- **New (Pro):** 📁 Sub-folder nesting extended to 3 levels deep (4 tiers total) — Levels 2 and 3 are Pro-exclusive, via "Add sub-folder"/"Move into folder…"/"Split into sub-folder" in any eligible folder's ⋮ menu
- **New (Free):** 📂 Chrome/Firefox HTML import now preserves up to 3 levels of your real nested folder structure (previously capped at 1 level before flattening the rest into named sibling sub-folders)

## v2.0.1 — September 2026

- **New (Pro):** 📁 Sub-folders with lots of children (4+) now start collapsed instead of stretching the whole folder card tall — one click still opens any of them
- **New (Pro):** 🖱️ Drag a bookmark over a collapsed sub-folder for a moment and it auto-expands, so you can drop straight into it without clicking to open it first
- **New (Free):** ✅ Bulk mode now auto-opens every sub-folder while it's on, so you can select and move bookmarks nested inside them — closes back to however you left it once Bulk mode is off

## v2.0.0 — September 2026

- **New (Pro):** ↳ True Sub-folders — nest a folder inside another folder (up to 2 levels), via "Add sub-folder" or "Move into folder…" in any folder's ⋮ menu; deleting a parent folder promotes its sub-folders back to top-level without touching their bookmarks
- **New (Pro):** 🪓 Sub-folder Migration — split a large flat folder into sub-folders with a checkbox picker, straight from that folder's ⋮ menu
- **New (Free):** 📂 Chrome/Firefox HTML import now rebuilds your real nested folder structure as true BTM sub-folders (previously flattened every sub-folder into a separate "Parent › Child" top-level folder)

## v1.25.0 — September 2026

- **New (Free):** 📥 Save All Open Tabs as a Space — one click in the toolbar popup (or "+ New Space" → Save All Open Tabs) captures every open tab into a brand-new space
- **New (Pro):** ➖ Bookmark Groups — add colored divider labels inside any folder to informally section your bookmarks, without needing a full sub-folder

## v1.24.7 — September 2026

- **New (Free):** ☁️ Google Drive Backup is now 1 backup/month on the Free plan — Pro plans get unlimited backups for as long as your plan is active (a lapsed Monthly/Yearly plan reverts to the 1/month free allowance until renewed)

## v1.24.6 — September 2026

- **New (Free):** ➕ Added an "Add bookmark" option directly to every folder's ⋮ menu — a faster way in than scrolling to the bottom of a long folder

## v1.24.5 — September 2026

- **Fix:** Toolbar popup — the title, "BTM Active" checkmark, and Open New Tab button were still green from the old logo; recolored to match the amber/orange rebrand
- **Fix:** Incognito/Private browsing workaround — wording now matches exactly between the extension's own FAQ and the teamexykings.in website FAQ

## v1.24.4 — September 2026

- **New (Free):** FAQ — added "Does Bookmark Tab Manager work in Incognito / Private browsing?" explaining the one-time "Allow in Incognito" toggle + Open New Tab workaround (a browser-wide rule that applies to every extension, not a BTM limitation)

## v1.24.3 — September 2026

- **New (Free):** The "TeamExyKings" bar now gives a little periodic shake to catch your eye — hover over it and it holds still

## v1.24.2 — September 2026

- **UI:** Fixed the odd-looking pale "Suggest a Feature" button — now a solid gold gradient matching the other big Settings buttons
- **New (Free):** The "TeamExyKings" bar at the bottom of Settings is now clickable — opens teamexykings.in in a new tab
- **New (Free):** FAQ — added a "Does TeamExyKings have other projects?" entry pointing to teamexykings.in

## v1.24.1 — September 2026

- **UI:** Moved the FAQ & Help button up next to What's New — Changelog and Suggest a Feature, styled to match, instead of being a small link tucked under About
- **Privacy:** Donate → Scan to Pay UPI no longer prints the raw UPI ID as visible text — the QR code and the Copy UPI ID button still work exactly the same

## v1.24.0 — September 2026

- **New (Free):** 😀 Space tabs can now have their own emoji (plain or animated) — click the new 😀 icon on any space tab, same picker already used for folders
- **New (Free):** 📦 Bulk-selected bookmarks can now be moved to a folder all at once — a new "Move to folder" button appears next to Tag/Delete once you've bulk-selected bookmarks, listing every folder across every space

## v1.23.0 — September 2026

- **New (Free):** ☁️ Google Drive Backup is now fully live on both Chrome and Firefox — sign in with Google from Settings → Import & Backup and push a backup (JSON + HTML, plus Markdown for Pro) straight to your own Drive
- **New (Free):** Drive backups are now organized automatically — each backup lands in a dated folder (e.g. "12-Sep-2026") inside your "Bookmark Tab Manager Backups" folder, with separate Chrome and Firefox subfolders underneath, so backups from both browsers stay cleanly separated instead of mixing together
- **Fix:** Drive backup filenames now include the browser name and exact date/time, so repeat backups on the same day no longer create confusing duplicate-looking files
- **UI:** Several long explainer paragraphs in Settings (the keyboard-shortcuts quick reference, the HTML export note, and all three Chrome Bookmarks Sync descriptions) were taking up a lot of scroll space — each is now a small ⓘ icon that opens a popup with the same info instead
- **Fix:** Removed a stale hardcoded "v1.4" from the Settings → About footer's copyright line — it was several releases out of date and served no purpose since the real version is already shown elsewhere in Settings

## v1.22.0 — September 2026

- **New (Free):** New app icon — refreshed from green to amber/orange, across the toolbar icon, the Chrome/Firefox store listing, and all promotional artwork (marquee, promo tile, social kit, YouTube assets, landing page). Purely visual — nothing about how BTM works has changed.

## v1.21.0 — September 2026

- **New (Free):** Chrome Bookmarks Sync — Detect: adding a bookmark directly in Chrome (or syncing one in from another device via your Google account) pops up a prompt in BTM offering to add it into a matching or new folder
- **New (Free):** Chrome Bookmarks Sync — Write-back: a new Settings toggle mirrors your BTM Spaces into a dedicated "Bookmark Tab Manager" folder in Chrome (one subfolder per Space) — your existing Chrome folders are never read from or touched. Deleting something in BTM removes its Chrome copy too, but only after a grace period you control (1-12 hours, default 12), so a mistaken delete can still be recovered from the still-present Chrome copy
- **New (Free):** Chrome Bookmarks Sync — locking a Space (or turning on Child Lock) immediately pulls that content's mirror back out of Chrome, and restores it the moment you unlock again, so locked content doesn't sit unprotected in Chrome's own bookmark manager. No new backend/Supabase setup needed — this feature is entirely client-side.

## v1.20.0 — September 2026

- **New (Free):** Suggest a Feature — a new button in Settings lets you send a title + description straight to the developer, free or Pro, no sign-up or email required
- **New (Pro/Admin):** Thank-You Message — a one-time celebratory message the admin can write and enable from the Admin Dashboard, shown once to every user (free and Pro) on their next new tab, dismissible after 10 seconds with a 30-second auto-dismiss
- **New (Pro/Admin):** Feature Suggestions inbox — submissions land in the Admin Dashboard (plus an instant email notification) so they can be triaged and marked reviewed. **Requires a one-time Supabase setup — see `backend/00_DEPLOY_CHECKLIST.md` before either of these two features works end-to-end.**

## v1.19.0 — September 2026

- **New (Free):** Taste Pro Backgrounds for Free — Custom Colors/Gradients, Video Background, and Unsplash Backgrounds can now each be turned on up to 2 times a month at no cost, so every user gets a taste of Pro styling (Custom Image Background keeps its existing, separate 2 times a week)
- **New (Pro):** 1-Hour Pro Trial — when your trial ends, a new popup asks you to Upgrade or Continue with Free; choosing Free resets Pro-only appearance settings (colors, video/Unsplash backgrounds, per-folder styling, Child Lock, and similar) back to free defaults so you start clean — your bookmarks, folders, and spaces are never touched
- **Fix:** Child Lock — removing or changing an existing Child Lock no longer requires an active Pro plan, so a lapsed trial or plan can never lock you out of your own bookmarks (matches how Space Lock already worked)
- **Fix:** Saved Background Presets — reloading a saved preset now correctly requires Pro, closing a gap where a free account that once saved presets (e.g. during a trial) could keep reapplying Pro-only backgrounds indefinitely
- **Fix:** Custom Colors/Gradients — switching to a Custom background no longer grants unlimited free use; it now draws from the same new 2x/month taste allowance as Video Background and Unsplash

## v1.18.0 — September 2026

- **New (Pro):** Master PIN Reset — forgot your Child Lock or Space Lock PIN? A new "Forgot your PIN?" link (in Settings, and directly on both lock screens) emails a one-time reset code to the address on file for your license; entering it removes Child Lock and every locked space on this device at once, so you're never permanently locked out of your own bookmarks. **Requires a one-time Supabase setup — see `server-setup/README.md` before this works end-to-end.**

## v1.17.0 — September 2026

- **New (Pro):** Child Lock & Space Lock — setting, changing, unlocking, or removing a PIN now uses a proper popup instead of the browser's native prompt boxes, with a new "Allow letters & symbols" toggle: off (default) keeps the original 4-6 digit PIN, on lets you use any 4-6 character PIN — letters, symbols, or a mix, like "&8%2" or "199/92"

## v1.16.0 — September 2026

- **Fix:** Space Lock — a locked space's bookmarks could still leak into Most Used, Recently Visited, On This Day, both Search bars, and the Weekly Report; all now skip locked spaces entirely. The lock screen's "switch to a different space instead" option now actually switches you to an unlocked space rather than just dismissing the lock, and moving a folder or changing a background can no longer target a locked space
- **Fix:** Export a Single Space — re-importing a JSON file that was exported from the ⬇ single-space option now correctly restores it as a new space, instead of being rejected
- **Fix:** Bookmark Templates for New Spaces — templates (and creating a blank new space) now respect the free plan's space and bookmark limits instead of bypassing them
- **Fix:** Bookmark Visit Goal — the weekly 🎯 progress badge now correctly disappears if your Pro trial or plan ends, instead of continuing to show on a free account
- **Fix:** Folder Icon from Favicon — picking a new folder emoji (or removing one) now also clears any favicon icon that was set, so the change actually takes effect instead of the old favicon still showing
- **Fix:** Curated 10 Background Gradients — fixed 7 of the 10 curated picks not actually using their guaranteed text color due to it matching the theme's own default
- **Fix:** Keyboard Shortcuts — moved out of the main Settings scroll into its own popup (⌨️ Keyboard Shortcuts → Customize), so Settings stays shorter and cleaner
- **Fix:** Settings → About — an already-expired license now shows its countdown chip in red instead of amber
- **Fix:** Bookmark Import from URL — fixed a background listener that could be left running if a page took longer than 8 seconds to load its title

## v1.15.2 — September 2026

- **Fix:** The top-right icon rail is more compact — the dark/light theme toggle now sits beside the Settings gear instead of stacked below it, so there's no chance of it (or the space-tabs row below) crowding the discount banner even when the clock is hidden and the page shifts up

## v1.15.1 — September 2026

- **Fix:** Settings → Clock & Weather — Bold clock, Milliseconds, Compact, Show seconds, and Clock size now hide together when "Show clock" is off, instead of staying visible with nothing to affect; Milliseconds also hides whenever Show seconds is off, since it never actually displayed without seconds shown
- **Fix:** Layout overlap when the clock is hidden, first pass — later refined in 1.15.2 with a cleaner fix

## v1.15.0 — September 2026

- **New (Pro):** Text Stroke/Outline — two new sliders in Settings → Colors & Font let you add a 1-5px outline (plus a color picker) to the header Title/Subtitle and, separately, to folder titles
- **New:** Curated 10 Background Gradients — a new "✨ Curated 10" row above the usual 60 shades in the Custom background picker, each one hand-picked with a guaranteed-readable text color instead of relying only on the live auto-contrast check
- **Fix:** Video background sound no longer keeps playing after switching to Unsplash or turning Video BG off — both settings now fully stop the video (pause + clear + unload) instead of only hiding it
- **Fix:** Unsplash widget text color no longer flickers back to the theme default after opening Settings or changing any preference — the photo-based color adjustment now persists
- **Fix:** Upgrade popup — the email field is now wrapped in a proper form so Chrome's native address/email autofill can fill it in reliably

## v1.14.0 — September 2026

- **New:** Google Drive Backup — new ☁️ Backup to Google Drive button in Settings → Import & Backup pushes a real backup straight to your Drive (JSON + HTML free, plus Markdown for Pro)
- **Fix:** Toolbar popup — removed the "⚙️ Settings" button, which opened a plain new tab and didn't actually open Settings; Disable/Enable now also refreshes any already-open BTM tab immediately instead of requiring a manual refresh
- **Fix:** Export as Markdown is now correctly Pro-only, matching what the README always said — it had no actual lock before this

## v1.13.0 — September 2026

- **New:** Save for Later — new 📚 option in the bookmark right-click menu copies it into a dedicated "Reading List" space, without duplicating URLs you've already saved
- **New:** Snooze a Bookmark — new ⏱️ right-click option to remind yourself tomorrow, in 3 days, or in a week, without typing an exact date
- **New:** Show Seconds toggle — new setting under 🕐 Clock & Weather lets you hide the seconds on the clock
- **New:** Folder Icon from Favicon — new 🖼️ folder action adopts the favicon of your most-visited (or first-added) bookmark as the folder's icon, as an alternative to an emoji
- **New:** Shareable Space Link — new 🔗 icon on each space tab creates a read-only preview link for that space; note this opens as a browser link rather than a permanently-hosted URL, so very large spaces should use Export instead
- **New:** Custom Keyboard Shortcuts — new ⌨️ Keyboard Shortcuts section in Settings lets you reassign Search, Quick Add, Add Bookmark, New Folder, Export Backup, and Scratchpad to your own keys if one conflicts with another extension
- **New:** Bookmark Health Digest — the weekly broken-link check now arrives as a single combined notification, adding a "top pick this week" highlight for Pro users instead of being a separate, manual-only report
- **New:** A one-time note now appears under Import & Backup explaining that your data lives locally in this browser and doesn't travel via Chrome/Firefox account sync — use Backup/Restore to move it between devices

## v1.12.0 — September 2026

- **New:** License Expiry Reminder — a small dismissible corner reminder now appears when your Monthly/Yearly plan is within 7 days of expiring, in addition to the existing renewal emails; the expiry date in Settings → About is also now clickable and shows the confirmed D/Mon/YYYY format (e.g. 8/Sep/2026)
- **New:** Renew / Check Status now opens the same Monthly/Yearly/Lifetime pricing popup used for new purchases — so you can switch plans on renewal, with any live discount applied, instead of always going straight to your current plan's payment link
- **New (Pro):** Bookmark Visit Goal — set a weekly visit target on any bookmark from its right-click menu; a 🎯 badge shows your progress for the week
- **Fix:** Manage Devices — deactivating a device now reliably refreshes the tab afterward for any device, not only when deactivating the one you're currently using
- **Fix:** Upgrade popup — clicking Monthly/Yearly/Lifetime now shows a "Please wait..." state on the plan buttons while your discount is checked, so it's no longer possible to open several duplicate Razorpay tabs by clicking more than once

## v1.11.0 — September 2026

- **New:** Bookmark Import from URL — paste a URL into Add Bookmark and its page title auto-fills the Name field once it loads
- **New:** Export a Single Space — new ⬇ icon on each space tab exports just that space as JSON or Markdown, instead of only the whole workspace
- **New:** Bookmark Templates for New Spaces — the + New Space button now offers Work, Travel, Job Hunt, and Student starter templates, or a blank space

## v1.10.0 — September 2026

- **New:** Sort Spaces — new ⇅ button next to your spaces lets you sort them by Name (A→Z), Most Used, or Recently Added
- **New:** Drag Folder Between Spaces — drag a folder card straight onto a space tab to move it there, instead of only using the 📦 Move to Space menu
- **New (Pro):** Per-Space Background — give each space its own background color or image (🎨 icon on the tab) so every space feels visually distinct at a glance
- **New (Pro):** Space Lock — lock just one sensitive space with its own PIN (🔒 icon on the tab) instead of locking the whole extension with Child Lock

## v1.9.2 — August 2026

- **Fix:** Tags per bookmark are now capped at 2 for real — previously there was no actual limit, only a display truncation (bookmark rows always showed just the first 2 even if more were saved underneath), which looked like a hard limit that wasn't really there. Both the single-bookmark tag editor and the Bulk Tag Editor now enforce this directly.

## v1.9.1 — August 2026

- **Fix:** Bulk Tag Editor: the 🏷️ Tag button did nothing (console error) — it was passing the click event into the popup instead of the button itself

## v1.9.0 — August 2026

- **New:** Bulk Tag Editor — new 🏷️ Tag button in bulk-select mode lets you add or remove a tag across every selected bookmark at once
- **New:** Multi-Bookmark Drag Select — in bulk-select mode, drag a selection box across the grid to select several bookmarks at once instead of clicking each checkbox individually

## v1.8.2 — August 2026

- **Fix:** Settings → Widgets → "Most visited" toggle now works the first time you switch it on — it used to silently do nothing until you also toggled Stats card, which happened to force the same refresh
- **Fix:** Unsplash "Change" row (Settings → Background) — the 🔀 shuffle button was stretching full-width and wrapping onto its own line below the dropdown; it's now a small icon that sits neatly next to the dropdown, and the "Change" text label was dropped since the dropdown options already say what it does

## v1.8.1 — August 2026

- **New:** The "What's New" red dot now also shows on the 📋 What's New — Changelog button inside Settings → About, not just the ⚙️ gear icon, so it's clear which button the gear dot was pointing you to

## v1.8.0 — August 2026

- **New:** Recently Visited — new collapsible 🕐 section on Home showing your last 8 opened bookmarks, next to Most Visited (Settings → Widgets to toggle)
- **New:** On This Day — a dismissible 📅 card surfaces on Home when a bookmark was added exactly 1 year ago today
- **New:** What's New dot — a small red dot now appears on the ⚙️ Settings icon after an update, until you open Settings → About → What's New

## v1.7.0 — August 2026

- **New:** Collapse All / Expand All Folders — new toggle button next to "+ Add space" collapses or expands every folder in the current space in one click
- **New:** Copy All Links in Folder — new 📋 folder action copies every bookmark URL in a folder to your clipboard as a list, alongside the existing "Open all in new tabs"
- **New:** Folder Notes — add a short note to a folder itself (e.g. "Check monthly") via the new 📝 folder action; shows as a hover tooltip once set
- **New:** Duplicate Bookmark Detector — new "Find duplicate bookmarks" tool (Settings → Tools) scans every space and folder for the same link saved more than once, and lets you remove the copies you don't need

## v1.6.1 — August 2026

- **New:** Search box opacity slider (Settings → Layout) — the Search Preview Dropdown was hard to read against a busy background; it now defaults to a solid, readable panel and you can adjust how see-through it is
- **Fix:** Tag filter chips (footer) now correctly show which tag is active when clicked — the highlight was never updating before, so a selected tag looked identical to an unselected one

## v1.6.0 — August 2026

- **New:** Search Preview Dropdown — a live floating preview of your top matches appears under the search bar as you type, click one to jump straight to it (the full search results grid still works exactly as before)
- **New:** Quick Jump (Ctrl+K) — a new command-palette overlay to jump straight to any space or folder by typing a few letters, separate from the search bar which only filters bookmarks
- **New:** Search by #tag — type #tagname in the search bar to filter bookmarks by tag instead of name/URL

## v1.5.0 — August 2026

- **Fix:** Discount slider (Admin) now allows 1%-99% instead of being capped between 10%-75%
- **Fix:** FAQ popup: question titles and search/section borders were still following the old theme colors, making them unreadable in white theme and custom backgrounds — now always white text on the pitch-black popup, irrespective of theme
- **Fix:** "Are you sure?" delete-confirmation popup now also uses the same pitch-black background, white text, and light-gray border as every other popup, irrespective of theme
- **Fix:** Manage Devices popup: device name labels were still following the old theme text color — now always white on the black popup

## v1.4.15 — August 2026

- **Fix:** Session, Changelog, Donate, Admin, FAQ, Manage Devices, Upgrade/Trial, Edit Bookmark, Quick Add, and Share popups now all show a consistent pitch-black background with white text and a light-gray border, no matter what theme is active — previously each popup had its own inconsistent background color

## v1.4.14 — August 2026

- **Fix:** Settings → About version number was a hand-typed "v1.4" that never updated — now pulled live from the actual build/manifest version every load, so it always matches the installed zip

## v1.4.13 — August 2026

- **Fix:** Fixed the Pro badge briefly flashing as "Free" on new tab open when Ambient Sound (especially a custom MP3) was on — badge now stays hidden until your real Free/Pro/Trial state is known, so it can never show the wrong one

## v1.4.12 — August 2026

- **Fix:** Donate popup now shows your real UPI QR code instead of the placeholder
- **Fix:** Admin Dashboard now correctly tracks devices separately per browser (Chrome/Firefox counted as 2 devices, not 1)
- **Fix:** Custom color section: the two "Color 1"/"Color 2" pickers were not wired up at all — picking a color now actually applies and saves it
- **Fix:** Custom color "Columns layout" buttons: unselected options were unreadable dark-on-dark text — now visible
- **Fix:** Ambient sound bar no longer appears when the Ambient Sound toggle is off, even if a track is selected
- **Fix:** Auto-lock timeout dropdown now only shows when Child Lock is enabled
- **Fix:** Changelog version date label contrast fixed — was hard to read
- **Fix:** Settings dropdowns switched to black background / white text for readability
- **Fix:** Edit Bookmark fields were black-text-on-black-background in some themes — fixed to a fixed dark/light scheme
- **New:** Particles: added a Thickness control, plus an Ultra Fast tier for both Speed and Density
- **New:** Pomodoro and Scratchpad widgets now sit side-by-side to save vertical space
- **New:** Streak, Heatmap and Stats widgets: added position ordering controls under ⚙️ Widgets
- **New:** Broken Links popup: added a per-link remove button and a short reason note (e.g. 404, Unreachable)
- **New:** Admin Dashboard: added Export All Data (licenses + donations as CSV)
- **New:** Browsing History: added Export to Excel (CSV)
- **Improved:** Keyboard Shortcuts toggle now has a short description of what it does

## v1.4.11 — August 2026

- **Fix:** Unsplash background no longer turns off when you click a black/white/custom bg preset — only turning it off directly, or switching to Video BG, replaces it
- **Fix:** Folder ⋮ menu, collapse arrow, drag handle and action icons now stay visible on custom card background colors (were a fixed dark gray)
- **Fix:** Donate popup and Admin Dashboard popup now use a solid theme-matched background instead of a see-through one — fixes poor contrast, especially in white theme
- **Fix:** Unsplash "Change" dropdown: added Weekly and Fixed options, fixed it overlapping the shuffle button
- **Fix:** Custom color popup: the browser's own native color-chooser (eyedropper/RGB box) now auto-confirms our popup the moment you close it — no more guessing what to click next
- **Fix:** Most Visited no longer looks up a per-bookmark custom favicon — always shows the auto-fetched one
- **Improved:** 60 gradient shades: Regenerate is more vibrant — raised the saturation floor and tightened the lightness range so shades read less washed out

## v1.4.10 — August 2026

- **Fix:** Auto Dark/Light: the 🌙/☀️ new-tab icon now directly turns Auto Dark/Light (by time) ON/OFF for Pro users, instead of only flipping the theme once
- **Fix:** Ambient local MP3: fixed the real autoplay-block cause — audio now starts muted then unmutes itself once playback begins, which browsers allow without a click
- **Improved:** Custom color popup: OK button now stays inside the popup and immediately saves as your "last custom" swatch, ready to select and Apply
- **Improved:** 60 gradient shades: Regenerate is back — genuinely fresh unique shades each click, capped at 5 per new-tab session
- **Fix:** Settings font size: existing installs stuck on the old "Small" default are migrated to Medium (S/M/L still all available)
- **Fix:** Upgrade popup: tightened spacing throughout so the license key field is reachable without scrolling, on top of the wider 4-per-row layout

## v1.4.9 — August 2026

- **Fix:** Space Tab ‹ › arrow buttons now actually switch spaces (previously only scrolled the tab bar)
- **Fix:** Auto Dark/Light hover label simplified to just ON/OFF, and hardened to always match the Settings toggle
- **Improved:** Per-folder color picker: OK button to confirm a custom color pick before Apply
- **Improved:** Ambient Sound: up to 5 saved local MP3s now show as a numbered list — click any to switch, ✕ to remove
- **Improved:** Settings font size default changed from Small to Medium (S/M/L still there)
- **Improved:** Upgrade popup widened with a 4-per-row benefit grid — no more scrolling to reach the license key field
- **Fix:** Removed the "Regenerate 60 gradient shades" feature — the curated default 60 shades are back to being fixed

## v1.4.8 — August 2026

- **Improved:** Space Tabs — arrow-key switching now claims real keyboard focus the moment you click anywhere on the page, fixing it never firing at all
- **Improved:** Quick Add (any website) now always lands in a "Saved" folder
- **Improved:** Stop Trial button on the Pro trial banner — end early and go straight to purchasing
- **Improved:** Settings font size (S/M/L) now also resizes description text and settings buttons, not just section titles
- **Fix:** Undo toast now gets its own popup — no longer gets overwritten by other notifications and closing early
- **Fix:** Ambient Sound (including a saved local MP3) now resumes automatically on every new tab, instead of only for the current one
- **Fix:** Ambient Local MP3 now actually persists — large files were silently failing to save past the storage limit
- **Fix:** Most Visited widget now shows your custom favicon instead of always the default
- **Fix:** Per-folder background/border/font color popup remembers your last custom color as its own swatch
- **Fix:** Regenerate (60 gradient shades) now generates genuinely new light-to-dark shades every click
- **Fix:** Discount banner no longer overlaps the settings icon; its upgrade popup now fits all 12 Pro highlights without extra scrolling

## v1.4.7 — August 2026

- **Improved:** Space Tabs — Left/Right arrow key switching rebuilt as its own listener, now actually fires
- **Improved:** Quick Add from any website now lands in a dedicated "New" folder every time, instead of whatever folder happened to be first
- **Fix:** Undo toast no longer shares its popup with other notifications — was getting overwritten and closing in under 2 seconds; now has its own 10-second window
- **Fix:** Regenerate (60 gradient shades) now varies the hand-picked default palette instead of generating from scratch — closer to the original look

## v1.4.6 — August 2026

- **Improved:** Space Tabs — Left/Right arrow keys now switch the active space
- **Improved:** Bulk select — real checkboxes on every bookmark, action bar moved under the search box
- **Improved:** Quick Add now works from any website — right-click any page or press Ctrl+Shift+Y (Cmd+Shift+Y on Mac)
- **Improved:** Search bar — clear (✕) button appears once you start typing
- **Improved:** Default theme is now dark on first install
- **Fix:** Undo button on the delete toast was unclickable — now works
- **Fix:** Right-click bookmark menu no longer gets cut off near the screen edge
- **Fix:** Removed the AI Suggest and duplicate Open in Incognito entries from the bookmark right-click menu
- **Fix:** Manage Devices no longer shows "No devices activated yet" for an active license — it now re-registers this device automatically
- **Fix:** Regenerate (60 gradient shades) now produces a visibly different palette every time

## v1.4 — August 2026

- **Improved:** First-run onboarding — Default Template, Import, or Start Fresh
- **Improved:** Default template with 3 spaces (Home/Work/Tech) and real bookmark links
- **Improved:** Import Chrome or Firefox HTML bookmark file — auto-sorted into 20 spaces
- **Improved:** Unknown folders create their own dedicated space on import
- **Improved:** Session tracker — floating popup, 30-day history, URL and duration
- **Improved:** Undo delete — 5 second undo toast after deleting folder or bookmark
- **Improved:** Global search ON by default — shows [Space] label in results
- **Improved:** Quick Add current tab — press Q or click + button
- **Improved:** Wide monitor — removed max-width, grid fills full screen width
- **Improved:** Export Chrome HTML and Firefox HTML with timestamp in filename
- **Improved:** WhatsApp and Telegram community links in About, Onboarding and FAQ
- **Improved:** FAQ popup — 14 searchable items with community links
- **Improved:** Settings font size control — S / M / L buttons in settings header
- **Improved:** Ambient sounds via Web Audio API — Rain, Ocean, Fireplace, Forest, LoFi
- **Improved:** Local MP3 file picker and default copyright-free ambient song
- **Improved:** Privacy mode hover-to-reveal — hover shows bookmarks, mouse away re-blurs
- **Improved:** Particles options row — Shape, Speed, Density, Color visible when toggled
- **Improved:** Renewal warning popup — 7 days before expiry, once per day
- **Improved:** Manage Devices popup — list all devices, deactivate any device
- **Improved:** License auto-validates on paste and auto-reloads after activation
- **Improved:** Device limits: Monthly = 3 devices, Yearly = 5, Lifetime = 10
- **Fix:** Admin dashboard browser breakdown — Chrome vs Firefox bar chart
- **Fix:** Admin-only endpoints now require a secret never shipped in the extension, closing an unauthenticated stats leak
- **Fix:** Admin dashboard flags license keys with unusually high device counts for review
- **Fix:** Admin login now uses a one-time code emailed on demand instead of a static secret — nothing long-lived to leak, session lasts 2 hours
- **Fix:** Admin dashboard install and uninstall tracking with trend sparkline
- **Fix:** Streak and heatmap cards now show and align like other folder cards
- **Fix:** Ambient toggle off now stops sound and resets to none
- **Fix:** Session and admin popup backgrounds match active theme
- **Fix:** Backup JSON and Restore JSON buttons properly wired
- **Fix:** Splash image Fixed mode — same image on every tab and every refresh

## v1.3 — August 2026

- **Improved:** Privacy mode — blur all bookmark folders for screen sharing
- **Improved:** Open in Incognito — any bookmark, 100/month free
- **Improved:** Filter by tag — click tag chip to filter across all folders
- **Improved:** Bulk select mode — select multiple bookmarks and delete all
- **Improved:** Broken link checker — scans up to 50 links for errors
- **Improved:** Folder emoji picker — 14 categories, 200+ emojis
- **Improved:** Date added — shows when bookmark was first saved
- **Improved:** Show/hide visit count toggle
- **Improved:** Compact clock toggle for more bookmark space
- **Improved:** Donation popup — UPI, Razorpay and PayPal
- **Improved:** Changelog popup — full version history
- **Improved:** Child Lock — master PIN locks all folders + export + settings
- **Improved:** Video background — local MP4 up to 30 seconds
- **Improved:** Save 5 background presets — save/load/delete
- **Improved:** Export as Markdown — Notion/Obsidian format
- **Improved:** Open in Incognito — unlimited for Pro
- **Improved:** Streak tracker card — daily usage streak
- **Improved:** Usage heatmap card — 7-day hour-by-hour grid
- **Improved:** Weekly report popup — top 5 most visited
- **Improved:** AI auto-categorize — Claude AI suggests best folder
- **Improved:** 50 gradient presets (added 20 new ones)
- **Fix:** All popups now follow theme colors
- **Fix:** Folder actions hidden by default — appear on 2s hover
- **Fix:** Reset clears all spaces, folders and bookmarks
- **Fix:** Unsplash and Video are mutually exclusive
- **Improved:** Voice search removed — planned for future version

## v1.2 — August 2026

- **Improved:** Razorpay payment integration — UPI, Cards, NetBanking
- **Improved:** Supabase license key system — BTM-XXXX key auto-generated
- **Improved:** Brevo email delivery — license key emailed after payment
- **Improved:** 5 device activation limit for Monthly and Yearly plans
- **Improved:** Lifetime plan — 999 device activations
- **Improved:** Bookmark names always visible
- **Improved:** Search is bookmark-only — no Google redirect
- **Fix:** Removed obfuscated code to comply with Chrome policy
- **Improved:** Removed Google search on Enter key

## v1.1 — August 2026

- **Improved:** Visit counter and last visited timestamp
- **Improved:** Bookmark notes, tags and reminders
- **Improved:** Duplicate bookmark detector
- **Improved:** Custom favicon upload per bookmark
- **Improved:** Hover actions after 2 seconds
- **Improved:** Right-click context menu — 13 options
- **Improved:** 25+ language support for clock and UI
- **Improved:** Unsplash backgrounds — random photos by category
- **Improved:** Ambient sounds — Rain, Ocean, Fireplace, Forest, LoFi
- **Fix:** Renamed from Bookmark Tab Manager to Bookmark Tab Manager

## v1.0 — August 2026

- **Improved:** Unlimited spaces, folders and drag & drop
- **Improved:** Import Chrome bookmarks with one click
- **Improved:** Live clock (25+ languages) and weather widget
- **Improved:** Dark and Light themes
- **Improved:** Backup and restore as JSON
- **Improved:** Import and export Chrome HTML
- **Improved:** Custom backgrounds — 50 gradients, solid, image
- **Improved:** Animated particles, per-folder colors
- **Improved:** Font picker, grid layout control
- **Improved:** Dual timezone clock, auto dark/light mode
- **Improved:** Pinned bar, Scratchpad, Pomodoro timer
- **Improved:** Productivity stats, QR code per bookmark

