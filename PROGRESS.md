# PROGRESS — CG3 Academy (cg3academy.com)

**Mode:** 2 — fix from audit (audit supplied inline by Caleb, 2026-09-26)
**Stack:** single static `index.html` + Tailwind Play CDN + vanilla JS (no build step)
**Hosting:** GitHub Pages, serving branch `main` of `calebmgeorge96-spec/cg3-academy` (public repo), custom domain via `CNAME`
**Working branch:** `claude/nice-mccarthy-p1pb8x`. Nothing is live until it is merged to `main`.
**Goal:** turn local-search and social traffic into booked sessions this week. Primary conversion is a text or call to (813) 351-0034, then the booking form.

> ⚠️ **This file is public.** The repo is public and GitHub Pages serves every file in it, so this file is reachable at `cg3academy.com/PROGRESS.md`. Never write the name of the third-party academy (see Hard constraint) or Caleb's personal email address here, in commit messages or in code comments.

## Status legend
`Not started` · `In progress` · `Blocked` · `Done`

## Stack decision
- **Chosen:** keep the existing single-file static site and edit `index.html` in place. **Why:** the audit calls for a copy and plumbing fix, not a rebuild. Migrating to Astro/Vercel would add risk and delay in the same week traffic starts.
- **Correction to the audit's assumption:** the site is **not** Astro on Vercel. Evidence:
  - No `package.json`, framework or build config in the repo.
  - Vercel team "CG3's projects" has no cg3-academy project, and cg3academy.com is not among its domains.
  - GitHub reports `has_pages: true` for the repo, and `CNAME` = `cg3academy.com`.
- An open, unmerged Cloudflare Workers PR exists: https://github.com/calebmgeorge96-spec/cg3-academy/pull/1 (bot-generated; adds `wrangler.jsonc` + `.gitignore`). It is not part of the current deploy. See 🔴 NEEDS CALEB.

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
| 0 | Section 0 | Map repo, capability check, confirm stack, create PROGRESS.md | Done | (this commit) | Stack corrected: static HTML on GitHub Pages, not Astro/Vercel. |
| 0.1 | 🔴 Hard constraint | Remove the photo showing the third-party academy's kit. `CG3 Assets/Pictures/IMG_6632.jpg` is the 4th photo-strip cell ("Caleb coaching young players"). Both kids wear that academy's crest, visible at desktop and 390px, and the 6 MB original is publicly downloadable. Remove from markup **and** delete the file. | Not started | — | Needs a decision on the 4th strip cell (see NEEDS CALEB). Git history still holds the file (public repo). |
| 1.1 | 🔴 Phase 1 | Dead Gumroad "Buy Now" links (5 products + $119 bundle all point at bare `https://gumroad.com`) | Blocked | — | Waiting on Caleb: do the listings exist? The section is **already** `display:none` and its nav links are commented out, but the dead `href`s and prices still ship in the HTML (crawlable). The booking intro still says "or a digital program". If the products don't exist, remove the section from the rendered DOM (HTML comment) and drop the "digital program" mention. |
| 1.2 | 🔴 Phase 1 | Add (813) 351-0034: header nav link (visible on mobile too; the nav "Book Session" button is hidden below 640px), a line under the submit button, and the phone as the primary Contact-block row | Not started | — | The Contact-block part replaces the "Email" row, which is the only code change 1.3 and 1.4 need. Proposed: "call" wording → `tel:`, "text" wording → `sms:` (confirm at task time). |
| 1.3 | 🔴 Phase 1 | Kill the dead inbox everywhere | Not started | — | Repo-wide grep: 1 hit only (Contact block, plain text, not a `mailto`). None in meta, JSON-LD, comments, form config or history-relevant files. The form recipient is set in the **Formspree dashboard**, not the repo, so there is nothing to repoint in code → NEEDS CALEB to verify in Formspree. |
| 1.4 | 🔴 Phase 1 | Never display the personal Gmail; the phone replaces the Email row | Not started | — | Gmail currently has 0 hits in the repo, and that is the expected end state: the "form config" is on Formspree's side. If a 2nd row is needed, link to `#booking`, not an address. |
| — | Phase review | Phase 1 boundary review (`phase-review.md`) | Not started | — | |
| 2.1 | 🟠 Phase 2 | Replace "Tampa · St. Pete · Clearwater · Brandon" with the 6 GBP areas | Not started | — | Hits: meta description (L7), hero eyebrow (L381), training footnote (L675), Contact "Location" (L936), footer bottom line (L1093). og/twitter descriptions say "Tampa Bay" (handled in 2.2). **Conflict:** the credential ticker has "St. Pete Aztecs UPSL" ×2 (L433, L443). That is a team name, not a service area, but it breaks the Phase 4 zero-hit grep → ask Caleb. Six areas in the hero eyebrow will wrap at 390px, so check layout. |
| 2.2 | 🟠 Phase 2 | Meta description rewrite (153 chars, given verbatim), plus matching og:description / twitter:description | Not started | — | |
| 2.3 | 🟠 Phase 2 | LocalBusiness JSON-LD (name, telephone, areaServed ×6, hours; **no email**) | Not started | — | None exists today. Service-area business: omit street address unless GBP shows one. Match GBP exactly. |
| — | Phase review | Phase 2 boundary review | Not started | — | |
| 3.1 | 🟠 Phase 3 | Dismissible tryout bar at the top of the hero ("High school tryouts start in October. Tryout prep blocks available now." → `#booking`), plus "Tryout Preparation" in the 1-on-1 list and in the form dropdown | Not started | — | Nav is `position:fixed` over the hero, so place the bar so it doesn't collide. No price for tryout prep (don't invent one). **Time-sensitive copy:** needs a removal date. |
| 3.2 | 🟠 Phase 3 | Weekday daytime / homeschool availability line in the training section | Not started | — | Real hours: Mon/Wed 9–3, Tue/Thu 11–3, Fri 9–8:30. Don't claim "9–3 every weekday". No availability copy exists anywhere today. |
| 3.3 | 🟠 Phase 3 | Replace "Contact for location & scheduling" with a structured slot for 2–3 park names, currently `[[TBD]]` | Not started | — | Open question: literal `[[TBD]]` shown to parents on the live site, or honest neutral copy with the `[[TBD]]` marker kept in code? |
| 3.4 | 🟡 Phase 3 | Remove the hero scarcity line (L395–398); keep the booking-section one (L925) | Not started | — | |
| 3.5 | 🟡 Phase 3 | Replace "within 24 hours" promises with a text-first line (e.g. "Text for same-day response.") | Not started | — | Hits: Contact "Response Time" (L954), form subhead (L968), success message (L973). Also review L1020 ("…will reach out to confirm availability and location"). |
| — | Phase review | Phase 3 boundary review | Not started | — | |
| 4 | Phase 4 | Full review as a separate engineer: greps, links, `tel:`/`sms:`, form recipient, validation, console, 390px + desktop; list of noticed-not-fixed | Not started | — | Grep targets: `coachcaleb`, `@cg3academy.com`, "St. Pete", "Clearwater", "Brandon", `the personal Gmail` (the email, not the GitHub username `calebmgeorge96-spec`; expect 0 since the recipient lives in Formspree), the third-party academy name (case-insensitive), bare `href="https://gumroad.com"`. Scope: every file except this PROGRESS.md, which records the old city names on purpose. |

## 🔴 NEEDS CALEB
- [ ] **0.1 photo.** What goes in the 4th photo-strip cell once the offending coaching photo is removed? Options: (a) 3-photo strip; (b) an unused game photo already in the repo (`IMG_9893.JPG`, `IMG_4069.JPG`, `IMG_4070.JPG`); (c) a new coaching photo in plain or CG3 kit from you. There is no other coaching photo in the repo.
- [ ] **0.1 history.** The repo is public, so the removed photo stays in git history. Options: leave it (low discoverability); make the repo private (Pages from a private repo needs a paid GitHub plan); or rewrite history on `main` (destructive force-push, needs your explicit OK).
- [ ] **1.1** Do the 5 Gumroad products and the bundle exist as live listings? If yes, send the 6 product URLs.
- [ ] **1.3** Formspree dashboard → form `meevyrnw`: confirm the notification email is your current inbox, not the suspended domain inbox. After deploy, send one test submission from the live site and confirm it arrives.
- [ ] **3.3** Training park names (2–3) when decided.
- [ ] **3.1** When should the tryout bar come down? (Suggest the first week of November.)
- [ ] **2.1** Keep the "St. Pete Aztecs UPSL" credential? It's a team name, but it contains "St. Pete".
- [ ] **Deploy.** Merge this branch to `main` for GitHub Pages to publish. Also decide on Cloudflare PR #1 (close it, or intentionally move hosting).
- [ ] **Trust stats.** Confirm these are true and defensible before paid traffic sees them: "50+ Players Trained", "100% Satisfaction Guaranteed" (reads as a refund promise), "5★ Average Rating" (from which source?), "10+ Years Competing".
- [ ] **Later.** Set up the proper domain email, then swap it back into the Contact block (and schema, if wanted).

## Open [[TBD]]
- [[TBD: training park names (2–3)]] (task 3.3)
- [[TBD: Lighthouse scores]] (not run yet; see capability check)

## Capability check (Section 0, 2026-09-26)
| Capability | Status | Substitute / note |
|---|---|---|
| git | ✅ | Branch `claude/nice-mccarthy-p1pb8x`, up to date with `origin/main` at start ([pre-purge commit]). |
| Build | ➖ none exists | "Build green" = (1) `html-validate` (standard preset) shows **no new errors** vs baseline, (2) page loads with **zero console errors**, (3) **no horizontal overflow** at 390px and 1440px. Baseline: 7 pre-existing errors, all `<style>` inside `<body>` (L513, 534, 679, 820, 898, 1028, 1097). |
| Screenshots (1440 + 390) | ✅ with a workaround | Playwright + Chromium from the session scratchpad (not committed). `cdn.tailwindcss.com` is blocked by the sandbox network policy, so the harness compiles the same Tailwind v3 config locally and substitutes it for the CDN script. Google Fonts are fetched via Node and handed to the browser. **Caveat:** CSS cascade order may differ slightly from the live CDN. |
| Live site check | ❌ | cg3academy.com, formspree.io and *.github.io are blocked from this sandbox. Can't verify the live deploy or send a test submission; Caleb verifies. |
| Vercel preview | n/a | The site isn't on Vercel. Preview = local server + screenshots. |
| Deploy | 🔴 Caleb | GitHub Pages publishes on merge to `main`. |
| Lighthouse | ⚠️ not run | Could be installed from npm and run against the local server, but the scores would be skewed by the CDN/font substitution. Will report as `[[TBD]]` rather than invent numbers. |
| Form test submission | ❌ | Formspree is unreachable from the sandbox, and a real submission emails Caleb anyway. Verify manually (steps in task 1.3). |

## Decisions
- 2026-09-26: Keep the static single-file stack; edit `index.html` directly (see Stack decision).
- 2026-09-26: 1.3 and 1.4 need no separate code change beyond 1.2's Contact-row swap; they become verification tasks (grep + Formspree).
- 2026-09-26: This file is public, so the third-party academy's name and the personal Gmail are never written here, in commits or in comments.

## Gotchas & dead ends
- The Digital Programs section (`#products`) was already hidden (`display:none`) before this audit. Its dead Gumroad links still exist in source.
- The `#sticky-cta` element was removed earlier, but the JS still references it (harmless null-guarded).
- Mobile nav is the floating "BOOK NOW" FAB (`#fab`). The header "Book Session" button is `hidden sm:inline-block`, so below 640px the header shows only the logo.
- Line numbers above refer to `index.html` at commit [pre-purge commit] and will drift as tasks land.
- The sandbox proxy blocks cg3academy.com. Don't retry; it's policy, not an outage.

## Noticed but not in the audit (candidates, not scheduled)
- **Image weight (big mobile win):** `IMG_2758.JPG` 5.0 MB shown as a 96px avatar; `IMG_1635.JPG` 6.7 MB (booking photo, hidden on mobile but still downloaded); `IMG_6632.jpg` 6.1 MB; `IMG_9903.JPG` 3.9 MB. Social traffic is mostly mobile.
- **Tailwind Play CDN in production:** render-blocking runtime JS, and it logs its own "should not be used in production" console warning on the live site.
- **Accessibility:** the 6 form `<label>`s aren't associated with their inputs (`for`/`id` missing).
- **Unused files served publicly:** 10 unused images, including 3 screenshots of older site versions (`Untitled.jpg`, `u.jpg`, `th.jpg`) that show outdated testimonial wording.
- `sitemap.xml` `lastmod` is 2026-04-30; bump it when changes ship.
- Footer blurb "Based in Tampa Bay" and the OG image text "Tampa Bay · Est 2023" are regional, not the old city list. Left as-is unless Caleb wants otherwise.

## Session log
- 2026-09-26: Phase 0 complete (stack mapped, capability check, Section 0 image audit found the 0.1 violation). Stopped for Caleb's go-ahead.
