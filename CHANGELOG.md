# Changelog

All notable changes to Bookmark Tab Manager.

## v1.24.5 — September 2026 (Latest)

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
- **Improved:** Device limits: Monthly = 3 devices, Yearly = 5, Lifetime = unlimited
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

