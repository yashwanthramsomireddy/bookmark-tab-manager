# Changelog

All notable changes to Bookmark Tab Manager.

## v1.5.0 — August 2026 (Latest)

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

