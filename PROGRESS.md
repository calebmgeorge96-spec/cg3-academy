# PROGRESS — CG3 Academy (cg3academy.com)

**Mode:** 2 — fix from audit (audit supplied inline by Caleb, 2026-09-26)
**Stack:** single static `index.html` + Tailwind Play CDN + vanilla JS (no build step)
**Hosting:** GitHub Pages, serving branch `main` of `calebmgeorge96-spec/cg3-academy` (public repo), custom domain via `CNAME`
**Working branch:** `claude/nice-mccarthy-p1pb8x`. Nothing is live until it is merged to `main`.
**Goal:** turn local-search and social traffic into booked sessions this week. Primary conversion is a text or call to (813) 351-0034, then the booking form.

> ⚠️ **This file is public on GitHub** (public repo), but it is **not** published on cg3academy.com: `_config.yml` excludes it from the GitHub Pages (Jekyll) build. Never write the name of the third-party academy (see Hard constraint) or Caleb's personal email address here, in commit messages or in code comments.

## Status legend
`Not started` · `In progress` · `Blocked` · `Done`

## Stack decision
- **Chosen:** keep the existing single-file static site and edit `index.html` in place. **Why:** the audit calls for a copy and plumbing fix, not a rebuild. Migrating to Astro/Vercel would add risk and delay in the same week traffic starts.
- **Correction to the audit's assumption:** the site is **not** Astro on Vercel. Evidence:
  - No `package.json`, framework or build config in the repo.
  - Vercel team "CG3's projects" has no cg3-academy project, and cg3academy.com is not among its domains.
  - GitHub reports `has_pages: true` for the repo, and `CNAME` = `cg3academy.com`.
- Cloudflare: a bot-generated Workers PR (https://github.com/calebmgeorge96-spec/cg3-academy/pull/1) was never merged, and the site is not hosted there. Caleb deleted the `cg3-academy` Worker on 2026-09-26. Its workers.dev production URL had still been serving an April build that included the 0.1 photo, because builds from `main` had been failing. Caleb's reviewer verified that the production, commit-preview and branch-preview URLs now return 404. PR #1 closed 2026-09-26.

## Key values
| Key | Value | Where it lives |
|---|---|---|
| Public phone (call/text) | (813) 351-0034 → `tel:+18133510034` / `sms:+18133510034` | page markup |
| Form endpoint | `https://formspree.io/f/meevyrnw` | `index.html` (form `action` + fetch in script) |
| Form recipient | Caleb's personal inbox. **Never rendered on the page.** | Formspree dashboard only, not in the repo. There is no `.env`. |
| Dead inbox (remove everywhere) | the suspended "coach…" inbox on the cg3academy.com domain (exact string is in Caleb's audit; grep `coachcaleb`) | only hit: `index.html` Contact block "Email" row |
| Service area (GBP) | Carrollwood, Northdale, Lutz, Citrus Park, Westchase, Odessa (all FL) | |
| Hours (GBP) | Sun 09:00–14:00 · Mon 09:00–15:00 · Tue 11:00–15:00 · Wed 09:00–15:00 · Thu 11:00–15:00 · Fri 09:00–20:30 · Sat 08:00–13:00 | |
| Training parks | `[[TBD]]`. Not decided. Never invent. | |

## Hard constraint
Nothing on the site may reference, name, imply or link to the third-party academy Caleb coaches for, or its owner. The name is deliberately **not** written in this public repo; Caleb has it. Check text **and photos** (kits, crests, banners). Phase 0 found a violation; see task 0.1.

## Tasks

| # | Tier | Audit finding → task | Status | Commit | Note |
|---|------|----------------------|--------|--------|------|
| 0 | Section 0 | Map repo, capability check, confirm stack, create PROGRESS.md | Done | 1ca05b9 | Stack corrected: static HTML on GitHub Pages, not Astro/Vercel. |
| 0.1 | 🔴 Hard constraint | Remove the photo showing the third-party academy's kit ("the 0.1 photo"). It was the 4th photo-strip cell ("Caleb coaching young players"), and both kids wore that academy's crest. Removed from the markup **and** the file deleted. | Done. **Live** 2026-09-26 (Pages run #53). Caleb verified on his phone: strip correct, old photo URL 404s. | 752cd88 | Per Caleb (option b), the 4th cell is now the unused game photo `IMG_4069.JPG` (FGCU white kit, on the ball), `object-position:62% 40%`, alt "Caleb FGCU attacking with the ball". It's a temporary stand-in: Caleb will supply a real coaching photo in plain or CG3 kit within ~2 weeks. html-validate: 7 errors = baseline. Console clean, no overflow at 1440/390. |
| 0.1b | 🔴 Hard constraint (Caleb, same merge as 0.1) | Keep PROGRESS.md off cg3academy.com | Done. **Live**; Caleb verified `/PROGRESS.md` 404s. | 679f956 | Added `_config.yml` with `exclude: [PROGRESS.md]`. Pages builds this repo with Jekyll (every deploy runs `actions/jekyll-build-pages`, "Build with Jekyll"). Verified locally with the same `github-pages` 232 gem: without the exclude, Jekyll renders `PROGRESS.md` into a page; with it, the log shows `EntryFilter: excluded /PROGRESS.md`. `index.html`, `CNAME`, `robots.txt` and `sitemap.xml` are byte-identical in the output. **Expected live result:** `/PROGRESS.md`, `/PROGRESS.html` and `/PROGRESS` all return GitHub Pages' 404. |
| 0.2 | 🔴 Hard constraint | Purge the 0.1 photo from git history (Caleb approved the rewrite, 2026-09-26) | Done 2026-09-26. Force-pushed `main`, this branch and `cloudflare/workers-autoconfig`; Pages run #54 succeeded. | force-push (later rewritten again by 0.3) | See "0.2 purge record" below. GitHub-side cached views still need Caleb's Support ticket (details in chat, not here). |
| 0.2b | 🔴 Hard constraint (Caleb) | Close PR #1 ("Not hosting on Cloudflare. Closing.") and delete `cloudflare/workers-autoconfig` before 0.3 | Done. PR closed 2026-09-26 19:05 UTC (by me). Branch deleted by Caleb (my deletes got HTTP 403 from the session proxy; Caleb's first attempt didn't go through either). Confirmed gone in a fresh mirror at 19:31:46 UTC. | — | Ref deletion is blocked for this session, so Caleb deletes branches himself. `refs/pull/1/head` survives the close by design (goes on the Support ticket). GitHub stopped advertising `refs/pull/1/merge` once the PR closed; before that it still pointed at the pre-purge merge commit (it wasn't recomputed after 0.2). |
| 0.3 | 🔴 Privacy (Caleb, 2026-09-26) | Scrub the personal Gmail (author/committer emails on 52 commits, one commit message, the fragment in PROGRESS.md) and the MacBook-local author email, plus all pre-purge commit IDs, from history. Rewrite `main` + this branch only. | Done 2026-09-26. Gates a–f passed on the rehearsal, on the real run, and again on a fresh clone after the push. Force-pushed `main` → `679f956` and this branch; Pages run #55 succeeded 19:34 UTC. | force-push (`main` → 679f956) | Mailmap → `Caleb George <277883012+calebmgeorge96-spec@users.noreply.github.com>`. Text/message replacements: Gmail → "Caleb's inbox", Gmail fragment → "the personal Gmail", each old ID (9 pre-0.2, 8 post-0.2, plus 2 later branch tips as no-op safety) → the literal placeholder "[pre-purge commit]". The mailmap and expressions files live in the scratchpad only (never committed). Gates a–f per Caleb's spec. |
| 1.1 | 🔴 Phase 1 | Dead Gumroad "Buy Now" links (5 products + $119 bundle all point at bare `https://gumroad.com`) | Done. **Live** (merged 2026-09-26) | 7637fc0 | **Done:** removed the `#products` section (5 products + bundle, 6 dead Gumroad links), its `<style>` block and the 2 commented-out "Programs" nav links (header + FAB): 146 lines. Booking intro now reads "Whether you're looking for private sessions or group training, reach out and we'll find the right fit for your goals." (dropping the phrase left a sentence fragment, so the period became a comma). `IMG_0745.jpg` (the bundle background) is now unused. html-validate 7 → 6 errors (one baseline `<style>` gone). **Caleb: the products don't exist yet.** Delete the whole Digital Programs section (`#products`, its `<style>` block and the commented-out "Programs" nav links in the header and FAB) from the source, not just hide it. Remove "or a digital program" from the booking intro. **To restore later:** the full section is in git history, e.g. `git show 61bcab0:index.html` (lines ~682–820; `61bcab0` = the pre-audit `main` tip), ready to re-add with real Gumroad product URLs. |
| 1.2 | 🔴 Phase 1 | Add (813) 351-0034: header nav link, a line under the submit button, and the phone as the primary Contact-block row | Done. **Live** (merged 2026-09-26) | 6858187 | **Header** (all widths): phone icon + "(813) 351-0034" → `tel:+18133510034`, plus a "Text" pill → `sms:+18133510034`. **Under submit:** "or text me at (813) 351-0034 for the fastest response" → `sms:` (Caleb's exact wording). **Contact block:** the Email row (the dead inbox) is replaced by a first-position "Call or Text" row: number → `tel:`, "Send a text →" → `sms:`. Rows are now Call or Text, Location, Response Time. All 5 links measure ≥44px tall at 390px (and ≥44 at 1440). **Also pinned:** header "Book Session" hidden below 640px via `#nav .nav-book` (the markup always said `hidden sm:inline-block`, but whether `.btn-primary` overrode it depended on where the Tailwind CDN injects its styles). The FAB covers mobile booking. Whether the live mobile header showed that button before this change is UNVERIFIED. |
| 1.3 | 🔴 Phase 1 | Kill the dead inbox everywhere | Done. **Live** (merged 2026-09-26) | da2b516 | Code change landed in 1.2 (the Contact-block Email row was the only occurrence). **Verified** across all 36 tracked files except this one: 0 hits for `coachcaleb` (any case), 0 `@cg3academy.com` addresses, 0 `mailto:` links, no JSON-LD or meta email, and 0 hits in the SVG/PNG brand assets. The only email-like string on the page is the form placeholder `you@example.com`. The form config holds only the Formspree endpoint `meevyrnw` (2 places: form `action` + fetch). **Formspree recipient:** Caleb switched "Send to" to his personal inbox on 2026-09-26 (same form ID, no code change). The suspended address is only his Formspree login. Delivery is UNVERIFIED until Caleb's live test submission. The dead inbox still appears in old versions of `index.html` in git history; it's a suspended business address, not personal data, so it's not scrubbed. |
| 1.4 | 🔴 Phase 1 | Never display the personal Gmail; the phone replaces the Email row | Done. **Live** (merged 2026-09-26) | daa77fe | **Verified (rendered page, 390 + 1440):** the full DOM after scripts run contains no email address except the form placeholder `you@example.com` (not visible text), and no Gmail fragment. Contact rows are Call or Text, Location, Response Time, so no second contact row is needed. What Formspree itself shows or sends is UNVERIFIED (unreachable from the sandbox). The personal Gmail has 0 hits in the current files **and** in git history as of 0.3 (commit metadata, messages and every past file version; checked on a fresh clone after the 0.3 push). The earlier "0 hits" in Phase 0 was a current-files-only check. The form's recipient lives on Formspree's side. If a 2nd row is needed, link to `#booking`, not an address. |
| 1.5 | 🔴 Phase 1 (added by Caleb) | Image optimization: resize and compress every image to its display size | Done. **Live** (merged 2026-09-26) | 81bbe92 | **11 JPEGs the page uses, replaced in place, 18.5 MB → 1.2 MB.** Target = 2× the largest rendered box measured at 7 viewports (390–1920), never upscaled. Re-encoded q80, progressive, optimized. The 2 Display P3 photos (`IMG_3879.jpg`, `IMG_3890.jpg`) were converted to sRGB first. **All metadata stripped:** 0 APP1 (EXIF/XMP), APP2 (ICC), APP13 (IPTC) or COM segments left. That includes GPS on the 4 used photos that had it (`IMG_1635`, `IMG_4063`, `IMG_4066`, `IMG_4069`). Details: avatars `IMG_2758` 4032×6048/4890 KB → 184×276/13 KB, `IMG_8428` → 276×184/7 KB, `UNADJUSTEDNONRAW_thumb_455` → 276×184/13 KB; booking `IMG_1635` 3933×3146/6550 KB → 1136×909/147 KB; strip `IMG_9903` 2625×3280/3798 KB → 960×1200/142 KB, `IMG_4063`/`IMG_4069` → 1021×680, `IMG_4066` same size q80 (395 → 222 KB); about `IMG_3879` same size (361 → 172 KB), `IMG_3890` → 554×465; hero `IMG_9896` same size (415 → 223 KB; full-bleed, so 2× would exceed the source). Before/after screenshots identical in crop, orientation and colour. **Untouched per Caleb:** 11 unused photos (`IMG_0745` (unused since 1.1), `IMG_1045`, `IMG_1118`, `IMG_4070`, `IMG_4071`, `IMG_9893`, `ed.jpg`, `th.jpg`, `Untitled.jpg`, `u.jpg`, `d43e4400-….JPG`) and all `Brand_assets` PNG/SVG. **Still GPS-tagged:** unused `IMG_1118`, `IMG_4070`, `IMG_4071` (publicly served, since Pages publishes every repo file), plus the original versions of the 4 used photos in git history. |
| 1.6 | 🔴 Phase 1 (added by Caleb) | Trust stats | Done. **Live** (merged 2026-09-26) | c51294d | "100% Satisfaction Guaranteed" → "100% / Satisfaction": only the word "Guaranteed" removed, the most literal way to drop the guarantee claim. "5★ Average Rating" tile removed. "50+ Players Trained" kept (Caleb confirmed it's true). Trust bar changed from 3 to 2 columns on desktop (stacks to 1 column ≤900px as before). **Flag:** "100% Satisfaction" is still an unsourced percentage claim; the wording is Caleb's call. |
| — | Phase review | Phase 1 boundary review (`phase-review.md`) | Done. Approved by Caleb 2026-09-26; `main` fast-forwarded `679f956` → `4cc1c87`; Pages run #56 green 20:02:30 UTC; live check by Caleb's reviewer (UNVERIFIED by me). | 4cc1c87 | Re-checked as a separate engineer. **Pass:** 1.1–1.6 each against the audit; full diff vs `main` read line by line (Location/Response Time rows byte-identical to `main`); all 5 `#` anchors resolve; all 16 local asset references exist; 2 `tel:` + 3 `sms:` links, all ≥44px tall at 390; html-validate 6 errors (all baseline `<style>`-in-body; was 7); 0 console errors and 0 horizontal overflow at 390/1440; GitHub Pages Jekyll build (github-pages 232) green, `index.html` published byte-identical, PROGRESS.md not published; simulated form submit with Formspree mocked in the browser (nothing sent) → success state shows, form hides. Page-used photo weight 18.07 MB → 1.19 MB. **Fixed in review:** "Send a text →" `aria-label` didn't contain its visible text (WCAG 2.5.3 Label in Name), now "Send a text to (813) 351-0034". **UNVERIFIED:** live site after merge, real Formspree delivery, real-device `tel:`/`sms:` behaviour, Lighthouse scores, how the live Tailwind CDN orders styles. |
| 2.1 | 🟠 Phase 2 | Replace "Tampa · St. Pete · Clearwater · Brandon" with the 6 GBP areas | Done (branch) | b8a8974 | Now "Carrollwood · Northdale · Lutz · Citrus Park · Westchase · Odessa" in the hero eyebrow (after "Private 1-on-1 Coaching"), the packages footnote, the Contact "Location" row and the footer bottom line (4 places). The meta/og/twitter descriptions are 2.2. "St. Pete Aztecs UPSL" ×2 kept (credential). The hero eyebrow wraps to 3 lines at 390px; readable, existing style. Left as-is: footer blurb "Based in Tampa Bay" and the OG image text "Tampa Bay" (regional, not the old list), plus the Rowdies/Tampa Bay United credentials. |
| 2.2 | 🟠 Phase 2 | Meta description rewrite, plus matching og:description / twitter:description | Done (branch) | b474c43 | `meta description` = Caleb's sentence verbatim (153 chars; it names 4 of the 6 areas). `og:description` + `twitter:description` = the same sentence with all 6 areas (177 chars), so Caleb's "use all 6 in og/twitter" is met without new copy. Titles unchanged. Old cities now appear only in "St. Pete Aztecs UPSL" ×2. Social preview rendering (Facebook/iMessage/X caches) is UNVERIFIED. |
| 2.3 | 🟠 Phase 2 | LocalBusiness JSON-LD (name, telephone, areaServed ×6, hours; **no email, no street address**) | Done (branch) | 82bb14d | One `application/ld+json` block in `<head>`: `@type` LocalBusiness, name "CG3 Academy", telephone "+1-813-351-0034", `areaServed` = 6 `Place`s ("Carrollwood, FL" … "Odessa, FL"), and 7 `OpeningHoursSpecification` entries (one per day) matching Caleb's hours exactly (checked programmatically). No other properties. Parses as JSON; html-validate unchanged. **Heads-up:** Google's rich-result docs list `address` as required for LocalBusiness, so their Rich Results Test may flag it. Not added: Caleb said no street address; a city-level address would be his call. Google Rich Results Test / schema.org validator not run (unreachable from the sandbox): UNVERIFIED. |
| 3.1 | 🟠 Phase 3 | Dismissible tryout bar at the top of the hero, plus "Tryout Preparation" in the 1-on-1 list and in the form dropdown | Done (branch) | 0103ea6 | Gold bar directly under the fixed header, exact text "High school tryouts start in October. Tryout prep blocks available now." linking to `#booking`, with a × close button (44×44). Rendered `hidden`. An inline script right after it shows it only if the America/New_York date is before 2026-11-08 and the visitor hasn't dismissed it (localStorage key `cg3-tryout-bar-dismissed`, wrapped in try/catch). If JS or Intl fails, the bar stays hidden. Because the script runs before the hero content renders, the bar adds 0 layout shift (CLS identical to `main`). **Tested with a pinned clock:** visible on 2026-09-26 and at 2026-11-07 23:59 ET; hidden at 2026-11-08 00:00 ET and later; hidden after dismiss + reload. "Tryout Preparation" is first in the 1-on-1 feature list and first in the dropdown's 1-on-1 group (value `1on1-tryout-prep`, **no price**). Existing prices unchanged. Delete markup/CSS/script after 2026-11-08 (reminder in NEEDS CALEB; the code comments say so too). |
| 3.2 | 🟠 Phase 3 | Weekday daytime / homeschool availability line in the training section | Done (branch) | f54354c | New line under the training heading: "**Daytime weekday sessions available** for homeschool players and flexible schedules: Mon & Wed 9am–3pm, Tue & Thu 11am–3pm, Fri 9am–8:30pm." These are Caleb's GBP weekday hours (no "9–3 every weekday" claim; weekends not mentioned). No em dashes (matches the site's earlier copy cleanup). It's the only availability copy on the page. |
| 3.3 | 🟠 Phase 3 | Replace "Contact for location & scheduling" with a structured slot for 2–3 park names | Done (branch) | c43a24e | The packages footnote now ends at the 6 areas. Below it, a new line `<p id="training-parks">` reads "Sessions at local parks in these areas. Your exact field is confirmed when you book." (honest, no park names). An HTML comment right above it carries `[[TBD]]` with drop-in instructions: replace the sentence with "Sessions at Park One, Park Two and Park Three.", keep the element and id. Verified that `[[TBD]]` appears only inside HTML comments and never in rendered text. |
| 3.4 | 🟡 Phase 3 | Remove the hero scarcity line; keep the booking-section one | Done (branch) | f985ff7 | Removed the hero "Limited spots available this month — only 4 sessions open" block. The booking-section "Only 4 spots remaining this month" stays (now the only one). The hero paragraph's bottom margin went 12px → 40px so the gap above the CTA stays open (the removed line used to supply its 40px margin). |
| 3.5 | 🟡 Phase 3 | Replace "within 24 hours" promises with text-first wording | Done (branch) | a6fe68c | All 3 replaced; 0 "24 hours" left. (1) Contact "Response Time" → "Text for same-day response" as an `sms:` link (44px tall; aria-label contains the visible text). (2) Form subhead → "Fill out the form and Caleb will reach out to confirm your spot. For a same-day response, text (813) 351-0034." (number = `sms:` link). (3) Success message → the same wording with the `sms:` link, because the "or text me" line hides with the form after submit. "Same-day response" is Caleb's own example wording and is used only for texts; no window is promised for the form. The small print "No commitment required. Caleb will reach out to confirm availability and location." is unchanged (no time promise). Links now: 6 `sms:` + 2 `tel:`, all `+18133510034`. Simulated submit shows the new success text. The subhead/success inline links sit inside sentences, so WCAG 2.5.8's inline exception applies. |
| 3.6 | 🟡 Phase 3 extra (Caleb) | Floating BOOK NOW button: `aria-label` must match its visible text | Done (branch) | ccfebec | `aria-label="Menu"` → `aria-label="Book now"` (visible text "BOOK NOW"; WCAG 2.5.3 now passes). `aria-expanded` still toggled by the existing script. No visual change. |
| 3.7 | 🟡 Phase 3 extra (Caleb) | Bump `sitemap.xml` `lastmod` | Done (branch) | a5ba8ab | `2026-04-30` → `2026-09-26`; still well-formed XML. Bump again whenever the page content changes. |
| A | 🟡 Phase 2+3 (Caleb, mid-run) | Trust bar: replace "100% / Satisfaction" and go back to 3 tiles | Done (branch) | 8e5ca86 | Tiles now: "50+ / Players Trained" (unchanged), "D1 / NCAA Division I", "Semi-Pro / Tampa Bay Rowdies U-23" (U-23 kept in the label so it never reads as the first team). Grid back to 3 columns (stacks ≤900px as before). No overflow and single-line labels at 390, 901 (narrowest 3-column width, 272px tiles), 1024 and 1440. |
| B | 🟡 Phase 2+3 (Caleb, mid-run) | Delete the 11 unused photos (`git rm`), no history rewrite | Done (branch) | (B commit) | Deleted: `IMG_0745.jpg`, `IMG_1045.JPG`, `IMG_1118.JPG`, `IMG_4070.JPG`, `IMG_4071.JPG`, `IMG_9893.JPG`, `ed.jpg`, `th.jpg` (Pictures) and `Untitled.jpg`, `u.jpg`, `d43e4400-726f-4791-be2e-3843ed6fcd47.JPG` (Client Testimonial). **Before:** 0 references in all 35 served files (every tracked file except PROGRESS.md), matched on full filename, URL-encoded filename and distinctive stems with boundaries. A first loose pass that also matched the 1–2 letter stems `ed`/`th`/`u` produced false hits and was discarded. **After:** all 17 local file paths referenced by `index.html` + `robots.txt` resolve. Page loads with 0 console errors / failed requests. The 3 unused brand SVGs (`cg3_logo.svg`, `logo-primary-inverse.svg`, `palette-and-type.svg`) are kept. 11 photos remain, all used by the page. No history rewrite, so the deleted files (including GPS on `IMG_1118`/`IMG_4070`/`IMG_4071`) stay in git history. |
| — | Phase review | Phase 2+3 combined boundary review (one stop at the end) | Not started | — | |
| 4 | Phase 4 | Full review as a separate engineer: greps, links, `tel:`/`sms:`, form recipient, validation, console, 390px + desktop; list of noticed-not-fixed | Not started | — | Grep targets: `coachcaleb`, `@cg3academy.com`, "St. Pete" (except the "St. Pete Aztecs UPSL" credential, kept by Caleb), "Clearwater", "Brandon", the personal Gmail address (exact string is in Caleb's audit; not the GitHub username `calebmgeorge96-spec`; expect 0 since the recipient lives in Formspree), the third-party academy name (case-insensitive), bare `href="https://gumroad.com"`. Scope: every file except this PROGRESS.md, which records the old city names on purpose. |

## 0.2 purge record (run 2026-09-26)
**What was purged:** the 0.1 photo, a single file that was added to the repo on 2026-04-28 and deleted by task 0.1 on 2026-09-26. The third-party academy's name appeared nowhere in the text of history (0 hits in diffs, paths or messages), so the photo was the only trace.

**Method** (fresh `git clone --mirror` in the scratchpad): a verified 33 MB `git bundle` backup (container-only); `git filter-repo --invert-paths --path <the 0.1 photo>`; then one `git push --force-with-lease=<branch>:<old tip>` per branch for `main`, this branch and `cloudflare/workers-autoconfig`.

**Gates, all passed before any push:**
1. 0 photo objects left in history.
2. `main` tree unchanged, so the live site is byte-identical.
3. The Cloudflare branch differs from `main` by only `.gitignore` + `wrangler.jsonc`.

**Push results:** no branch-protection block. All three branches force-pushed; GitHub moved `refs/pull/1/head` to the rewritten Cloudflare commit. (Those post-0.2 tips were superseded by 0.3; their IDs are deliberately not recorded here.)

**Verified after:**
- A fresh clone of all branches has 0 photo objects and 0 commits touching the path.
- PR #1 showed 2 files changed (`.gitignore`, `wrangler.jsonc`) on the rewritten `main`. **This check covered only the PR diff.** It missed that `refs/pull/1/merge` still pointed at the pre-purge merge commit, and that a Cloudflare Worker was still serving an old build with the photo. Both were found afterwards (see Stack decision and 0.2b).
- Pages run #54 succeeded (18:37 UTC).
- 57 of 59 commits got new IDs; the 2 oldest commits (before the photo was added) kept theirs.
- The session checkout was reset to the new history, unshallowed, and its stale local `main` repointed and garbage-collected (0 photo objects locally).

Pre-purge and post-0.2 IDs are deliberately **not** recorded in this repo. Caleb has them (chat, 2026-09-26).

## 0.3 scrub record (run 2026-09-26)
**What was scrubbed** (from `main` and this branch; the Cloudflare branch was deleted first):
- the personal Gmail as author/committer email on 52 commits;
- the MacBook-local author/committer email on 1 commit;
- the Gmail in 1 commit message (now "…emails now deliver to Caleb's inbox");
- the Gmail fragment in past versions of this file;
- 17 old commit/tree IDs quoted in past versions of this file: 9 from before 0.2, and 8 from between 0.2 and 0.3.

**Method** (fresh `git clone --mirror` in the scratchpad; confirmed first that `cloudflare/workers-autoconfig` was gone):
- Verified 28 MB `git bundle` backup (container-only).
- `refs/pull/*` dropped from the local mirror, since they can't be pushed.
- `git filter-repo --mailmap <scratch mailmap> --replace-text <scratch expressions> --replace-message <same>`: all identities go to `Caleb George <277883012+calebmgeorge96-spec@users.noreply.github.com>`, and each old ID regex eats a full SHA to `[pre-purge commit]`. The mailmap and expressions files were never committed.
- Rehearsed on a copy, then run for real. Then `--force-with-lease` pushes for `main`, then this branch.

**Gates** (passed on the rehearsal, on the real run, and on a fresh clone after the push):

| Gate | Result |
|---|---|
| a | 0 hits for the Gmail, its fragment, the MacBook hostname and 19 old IDs, in `git log --all --format=fuller -p` (38.6M chars) and in all 170 tree objects. The same patterns hit 2–111 times in the pre-0.3 history, so the check is live. |
| b | 0 copies of the 0.1 photo; 0 commits touching its path. |
| c | All 35 files on `main` other than PROGRESS.md have the same blob SHA as before 0.3: `index.html`, `_config.yml`, `CNAME`, `robots.txt`, `sitemap.xml`, 8 brand assets and 22 photos. |
| d | Across all 59 rewritten commits, PROGRESS.md is the only path whose content changed; 0 binary files changed. |
| e | This branch differs from `main` only in PROGRESS.md (at push time). |
| f | 59 commits before and after; author and committer dates identical on every commit; exactly 1 message changed (the Formspree one). |

After 0.3: `Caleb George <277883012+calebmgeorge96-spec@users.noreply.github.com>` on 53 commits, `Claude <noreply@anthropic.com>` on 6.

**Push results:** no branch-protection block. `main` → `679f956`, this branch → `4603b47`. `refs/pull/1/head` still points at the pre-0.3 Cloudflare commit (it survives a closed PR; it's on the Support ticket).

**Verified after:**
- Fresh clone: gates a–f pass (branches only).
- Pages run #55 for `679f956` succeeded (19:34:42 UTC).
- Session checkout reset to the new history; stale refs pruned, reflog expired, gc'd: 0 Gmail/MacBook hits, 0 photo objects, 59 commits, no pre-0.3 commits present.
- Live site unchanged: **UNVERIFIED** (every published file is byte-identical and PROGRESS.md is excluded from the build, but I can't reach cg3academy.com).

**Current (post-0.3) IDs:**

| Commit | ID |
|---|---|
| root "Initial commit" | `40be245` |
| pre-audit `main` tip | `61bcab0` |
| Phase 0 | `1ca05b9` |
| 0.1 | `752cd88` |
| 0.1b (`main`) | `679f956` |
| 0.2 PROGRESS records | `f1c85f0`, `5b0fd91` |
| 0.3 step 1 | `4603b47` |

## 🔴 NEEDS CALEB
- [x] ~~0.1 approve + confirm IMG_4069 is Caleb~~: done, live 2026-09-26.
- [x] ~~0.1b check~~: Caleb verified 2026-09-26 (strip correct; `/PROGRESS.md` and the old photo URL 404).
- [x] ~~0.2 go~~: run 2026-09-26 (see purge record).
- [x] ~~0.2b~~: Caleb deleted `cloudflare/workers-autoconfig`; confirmed gone 2026-09-26 19:31 UTC.
- [ ] **Re-clone after 0.3:** every local clone of this repo (your laptop, other Claude sessions) must be re-cloned, or hard-reset with `git fetch origin && git reset --hard origin/main`. **Never `git pull` an old clone; that merges old history (photo and Gmail) back in.** Any clone fetched before the 0.3 push is stale.
- [ ] **One GitHub Support ticket after 0.3** (https://support.github.com/request): remove cached views and garbage-collect the old commits after the 0.2 and 0.3 history rewrites. The ref tips, first changed commits, PR #1 refs and the no-LFS confirmation were given to Caleb **in chat only**. Until Support acts, old commits stay viewable on github.com to anyone who has their exact IDs, including everything reachable from PR #1's surviving head ref.
- [ ] **0.1 follow-up (~2 weeks):** send a real coaching photo from your own sessions (plain or CG3 kit, no third-party crests) to replace the strip's 4th cell.
- [x] ~~0.1 history~~: Caleb approved rewriting `main` to purge the file → task 0.2.
- [x] ~~1.1~~: Products don't exist yet → remove the section (task 1.1).
- [ ] **1.1 later:** build the Gumroad products; then restore the section from git history with real product URLs.
- [x] ~~1.3 Formspree recipient~~: Caleb switched "Send to" to his personal inbox (2026-09-26).
- [ ] **1.3 live test:** after Phase 1 merges, send one test submission from the live site and confirm it arrives (UNVERIFIED until then).
- [ ] **3.3** Training park names (2–3) when decided.
- [ ] **3.1 reminder:** the tryout bar auto-hides from 2026-11-08 (America/New_York). After that date, delete its markup, CSS and JS from `index.html`.
- [x] ~~Cloudflare~~: Worker deleted by Caleb (reviewer verified 404s); PR #1 closed 2026-09-26.
- [ ] **Deploy.** Merge this branch to `main` for GitHub Pages to publish (Caleb approves each merge).
- [ ] **Trust stats (rest):** confirm "10+ Years Competing" and the hero's "MLS Level Opposition" stat are how you want them worded.
- [x] ~~GPS in served files~~: the 3 GPS-tagged unused photos were deleted in task B, so once merged no served file carries GPS. **Remaining (Caleb chose no history rewrite):** those 3 files and the pre-1.5 originals of the 4 used GPS photos stay in git history on GitHub.
- [ ] **2.3 decision (optional):** Google may flag the missing `address` on the LocalBusiness schema. Leave it (service-area business, matches GBP), or add a city-level address without a street (e.g. locality + FL) to match GBP. Run Google's Rich Results Test after merge.
- [ ] **Later.** Set up the proper domain email, then swap it back into the Contact block (and schema, if wanted).

## Open [[TBD]]
- [[TBD: training park names (2–3)]] (task 3.3)
- [[TBD: Lighthouse scores]] (not run yet; see capability check)

## Capability check (Section 0, 2026-09-26)
| Capability | Status | Substitute / note |
|---|---|---|
| git | ✅ | Branch `claude/nice-mccarthy-p1pb8x`, up to date with `origin/main` at start (pre-audit tip, now `61bcab0` after the 0.3 rewrite). Full (non-shallow) clone since 0.2. Ref **deletion** is blocked by the session's GitHub proxy (HTTP 403). |
| Build | ➖ none exists | "Build green" = (1) `html-validate` (standard preset) shows **no new errors** vs baseline, (2) page loads with **zero console errors**, (3) **no horizontal overflow** at 390px and 1440px. Baseline: 7 pre-existing errors, all `<style>` inside `<body>` (L513, 534, 679, 820, 898, 1028, 1097). |
| Screenshots (1440 + 390) | ✅ with a workaround | Playwright + Chromium from the session scratchpad (not committed). `cdn.tailwindcss.com` is blocked by the sandbox network policy, so the harness compiles the same Tailwind v3 config locally and substitutes it for the CDN script. Google Fonts are fetched via Node and handed to the browser. **Caveat:** CSS cascade order may differ slightly from the live CDN. |
| Live site check | ❌ | cg3academy.com, formspree.io, *.github.io and *.workers.dev are blocked from this sandbox. Anything not reachable from here is reported as **UNVERIFIED** until Caleb checks it. |
| Vercel preview | n/a | The site isn't on Vercel. Preview = local server + screenshots. |
| Deploy | ✅ with approval | GitHub Pages publishes on push to `main` (branch deploy, Jekyll build). Caleb approves each merge. Deploy runs are visible in GitHub Actions as "pages build and deployment". |
| Pages build check | ✅ | `gem install github-pages -v 232` into the scratchpad (`GEM_HOME`), then `bundle exec jekyll build` with `LANG=C.UTF-8 NO_NETWORK=1` on a copy of the repo. Needs a Gemfile with `gem "github-pages", "232", group: :jekyll_plugins` (scratch only, **not** committed). Rendering any `.md` page through the default theme fails offline (the GitHub metadata lookup is blocked in the sandbox); that's sandbox-only. |
| Lighthouse | ⚠️ not run | Could be installed from npm and run against the local server, but the scores would be skewed by the CDN/font substitution. Will report as `[[TBD]]` rather than invent numbers. |
| Form test submission | ❌ | Formspree is unreachable from the sandbox, and a real submission emails Caleb anyway. Verify manually (steps in task 1.3). |

## Decisions
- 2026-09-26: Keep the static single-file stack; edit `index.html` directly (see Stack decision).
- 2026-09-26: 1.3 and 1.4 need no separate code change beyond 1.2's Contact-row swap; they become verification tasks (grep + Formspree).
- 2026-09-26: This file is public, so the third-party academy's name and the personal Gmail are never written here, in commits or in comments.
- 2026-09-26 (Caleb): 0.1 goes first, as its own commit, and merges to `main` as soon as it's approved, ahead of the other phases. The 4th strip cell uses an unused repo game photo until a real coaching photo arrives.
- 2026-09-26 (Caleb): approved rewriting `main` to purge the 0.1 photo (task 0.2), only after 0.1 is live and after a walkthrough of the commands.
- 2026-09-26 (Caleb): Digital Programs is removed from the source entirely (products not built yet). Restore from git history later.
- 2026-09-26 (Caleb): 3.3 uses honest neutral wording on the page; `[[TBD]]` stays in code + this file.
- 2026-09-26 (Caleb): keep the "St. Pete Aztecs UPSL" credential; excluded from the Phase 4 check.
- 2026-09-26 (Caleb): PROGRESS.md must not be served on cg3academy.com → `_config.yml` `exclude` (task 0.1b), in the same merge as 0.1.
- 2026-09-26 (Caleb): added tasks 1.5 (image optimization) and 1.6 (trust stats). I placed them at the end of Phase 1 because both affect paid mobile traffic right away.
- 2026-09-26 (Caleb): approval pace is **per phase**, starting with Phase 1 (replaces "per task"):
  - Still one commit per task, each with 390px + 1440px screenshots and the build-green checks.
  - Don't stop between tasks inside a phase. Stop at the end of the phase with one report listing every commit, what it changed, and everything UNVERIFIED.
  - Stop immediately, mid-phase, for: anything touching the hard constraint, any price change, any decision Caleb hasn't already given, or any failed check. Don't guess.
  - Nothing merges to `main` until Caleb approves the phase.
- 2026-09-26 (Caleb): 1.2 approved as proposed. "Text" wording → `sms:+18133510034`, "call" wording → `tel:+18133510034`. The line under the submit button is exactly: "or text me at (813) 351-0034 for the fastest response".
- 2026-09-26 (Caleb): 1.5 covers only the images the page uses: in place, ~2× largest display size, JPEG q≈80, all EXIF stripped; unused images untouched and listed.
- 2026-09-26 (Caleb): 1.6 keeps "50+ Players Trained" (confirmed true).
- 2026-09-26 (Caleb): Phase 1 approved and merged. His clarification: fixing my own in-task mistakes before commit is fine (keep reporting them); the stop rule covers problems I can't fix inside a task or anything needing his decision. Phase 1 choices approved (header number + Text pill, "Call or Text" row first, Book Session hidden <640px, hero at source size).
- 2026-09-26 (Caleb): **Don't touch** "100% Satisfaction" or the unused images until he sends answers.
- 2026-09-26 (Caleb): Phases 2 and 3 run as one combined phase: one commit per task, one stop at the end. Extras: floating button `aria-label` (3.6) and sitemap `lastmod` (3.7).
- 2026-09-26 (Caleb): 3.1 auto-hides from 2026-11-08 America/New_York; 3.3 has no park names yet, and `[[TBD]]` goes only in an HTML comment and in this file.
- 2026-09-26 (Caleb): anything I can't reach from the sandbox (the live site, workers.dev, Formspree, GitHub-side caches) is reported as **UNVERIFIED**, never as passed. "Checked" means checked from here.
- 2026-09-26 (Caleb): close PR #1 and delete its branch before 0.3; 0.3 rewrites only `main` and this branch.
- 2026-09-26 (Caleb): task 0.3 approved (Gmail + pre-purge IDs scrub). Old IDs and Support-ticket details go to Caleb in chat only, never into a file.

## Gotchas & dead ends
- The Digital Programs section (`#products`) was already hidden (`display:none`) before this audit. Its dead Gumroad links still exist in source.
- The `#sticky-cta` element was removed earlier, but the JS still references it (harmless null-guarded).
- Mobile nav is the floating "BOOK NOW" FAB (`#fab`). The header "Book Session" button is `hidden sm:inline-block`, so below 640px the header shows only the logo.
- Line numbers above refer to `index.html` at commit `61bcab0` (pre-audit tip) and will drift as tasks land.
- The sandbox proxy blocks cg3academy.com. Don't retry; it's policy, not an outage.
- The original session checkout was a **shallow** clone. It was unshallowed during 0.2, but a new session's checkout may be shallow again; use `git clone --mirror` for any history work.
- History was rewritten on 2026-09-26 (0.2, and 0.3 once it runs). Commit IDs from before a rewrite no longer exist on the branches, and pre-purge IDs are intentionally not recorded here.
- A green check on one surface isn't a green check on all of them. For anything containing the removed photo, the surfaces are: branches, PR refs (`refs/pull/*/head` and `/merge`), other hosts built from the repo (e.g. a Cloudflare Worker), and GitHub's caches.
- GitHub Pages runs the `github-pages` gem's plugins, including `jekyll-optional-front-matter`, so **any** `.md` file in the repo becomes a published page unless excluded. Add new docs to `_config.yml` `exclude` (or give them a leading `_`).
- Setting `exclude` in Jekyll 3.10 replaces the default exclude list (Gemfile, node_modules, vendor…). None of those exist here; if any get added, list them in `_config.yml` too.

## Noticed but not in the audit (candidates, not scheduled)
- **Floating mobile button label:** `#fab-btn` has `aria-label="Menu"` but shows "BOOK NOW" (WCAG 2.5.3 Label in Name). Pre-existing, not fixed.
- **Commit message of the 0.1 commit** names the removed photo's filename (a filename only; the file isn't retrievable from any branch).
- **Tailwind Play CDN in production:** render-blocking runtime JS, and it logs its own "should not be used in production" console warning on the live site.
- **Accessibility:** the 6 form `<label>`s aren't associated with their inputs (`for`/`id` missing).
- **Unused files served publicly:** 10 unused images, including 3 screenshots of older site versions (`Untitled.jpg`, `u.jpg`, `th.jpg`) that show outdated testimonial wording.
- `sitemap.xml` `lastmod` is 2026-04-30; bump it when changes ship.
- Footer blurb "Based in Tampa Bay" and the OG image text "Tampa Bay · Est 2023" are regional, not the old city list. Left as-is unless Caleb wants otherwise.

## Session log
- 2026-09-26: Phase 0 complete (stack mapped, capability check, Section 0 image audit found the 0.1 violation). Stopped for Caleb's go-ahead.
- 2026-09-26: Caleb answered the Phase 0 questions (see Decisions). 0.1 implemented on the branch (now `752cd88`); stopped for approval.
- 2026-09-26: Caleb approved 0.1 and asked for PROGRESS.md to be excluded from the site in the same merge (0.1b). Fast-forwarded `main` (now `61bcab0` → `679f956`); Pages run #53 succeeded.
- 2026-09-26: 0.2 purge planned and rehearsed on a scratch mirror; waiting for Caleb's go.
- 2026-09-26: Caleb verified 0.1/0.1b live. Ran 0.2: all gates passed, 3 force-pushes succeeded, Pages run #54 green, PR #1 diff clean (see the caveat in the purge record). Stopped before 1.1.
- 2026-09-26: Caleb deleted the Cloudflare Worker, set the UNVERIFIED rule, approved closing PR #1 and task 0.3. PR #1 closed; branch deletion blocked (403). 0.3 step 1 (this file) done; rehearsal run; waiting on the branch deletion before the real 0.3 push.
- 2026-09-26: Caleb deleted the Cloudflare branch; approved 0.3 with the post-0.2 IDs included; switched to per-phase approval. Ran 0.3: gates a–f passed on the rehearsal, the real run and a post-push fresh clone; `main` → `679f956`; Pages run #55 green. Starting Phase 1.
- 2026-09-26: Phase 1 (1.1–1.6) done on the branch, one commit per task, then the boundary review (one a11y label fix). Stopped at the Phase 1 boundary for Caleb's approval; nothing merged to `main`.
- 2026-09-26: Caleb approved Phase 1; `main` fast-forwarded to `4cc1c87` (no merge commit); Pages run #56 green. Started the combined Phase 2+3.
