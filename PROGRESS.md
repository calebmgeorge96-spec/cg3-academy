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
| 0 | Section 0 | Map repo, capability check, confirm stack, create PROGRESS.md | Done | [pre-purge commit] | Stack corrected: static HTML on GitHub Pages, not Astro/Vercel. |
| 0.1 | 🔴 Hard constraint | Remove the photo showing the third-party academy's kit. `CG3 Assets/Pictures/IMG_6632.jpg` was the 4th photo-strip cell ("Caleb coaching young players"). Both kids wore that academy's crest. Removed from the markup **and** the file deleted. | Done. **Live** 2026-09-26 (Pages run #53). Caleb verified on his phone: strip correct, old photo URL 404s. | [pre-purge commit] | Per Caleb (option b), the 4th cell is now the unused game photo `IMG_4069.JPG` (FGCU white kit, on the ball), `object-position:62% 40%`, alt "Caleb FGCU attacking with the ball". It's a temporary stand-in: Caleb will supply a real coaching photo in plain or CG3 kit within ~2 weeks. html-validate: 7 errors = baseline. Console clean, no overflow at 1440/390. |
| 0.1b | 🔴 Hard constraint (Caleb, same merge as 0.1) | Keep PROGRESS.md off cg3academy.com | Done. **Live**; Caleb verified `/PROGRESS.md` 404s. | [pre-purge commit] | Added `_config.yml` with `exclude: [PROGRESS.md]`. Pages builds this repo with Jekyll (every deploy runs `actions/jekyll-build-pages`, "Build with Jekyll"). Verified locally with the same `github-pages` 232 gem: without the exclude, Jekyll renders `PROGRESS.md` into a page; with it, the log shows `EntryFilter: excluded /PROGRESS.md`. `index.html`, `CNAME`, `robots.txt` and `sitemap.xml` are byte-identical in the output. **Expected live result:** `/PROGRESS.md`, `/PROGRESS.html` and `/PROGRESS` all return GitHub Pages' 404. |
| 0.2 | 🔴 Hard constraint | Purge `IMG_6632.jpg` from git history (Caleb approved the rewrite, 2026-09-26) | Done 2026-09-26. Force-pushed `main`, this branch and `cloudflare/workers-autoconfig`; Pages run #54 succeeded. | force-push (`main` [pre-purge commit] → [pre-purge commit]) | See "0.2 purge record" below. GitHub-side cached views still need Caleb's Support ticket. |
| 1.1 | 🔴 Phase 1 | Dead Gumroad "Buy Now" links (5 products + $119 bundle all point at bare `https://gumroad.com`) | Not started | — | **Caleb: the products don't exist yet.** Delete the whole Digital Programs section (`#products`, its `<style>` block and the commented-out "Programs" nav links in the header and FAB) from the source, not just hide it. Remove "or a digital program" from the booking intro. **To restore later:** the full section is in git history, e.g. `git show [pre-purge commit]:index.html` (lines ~682–820; `[pre-purge commit]` = the pre-audit `main` tip, formerly `[pre-purge commit]`), ready to re-add with real Gumroad product URLs. |
| 1.2 | 🔴 Phase 1 | Add (813) 351-0034: header nav link (visible on mobile too; the nav "Book Session" button is hidden below 640px), a line under the submit button, and the phone as the primary Contact-block row | Not started | — | The Contact-block part replaces the "Email" row, which is the only code change 1.3 and 1.4 need. Proposed: "call" wording → `tel:`, "text" wording → `sms:` (confirm at task time). |
| 1.3 | 🔴 Phase 1 | Kill the dead inbox everywhere | Not started | — | Repo-wide grep: 1 hit only (Contact block, plain text, not a `mailto`). None in meta, JSON-LD, comments, form config or history-relevant files. The form recipient is set in the **Formspree dashboard**, not the repo, so there is nothing to repoint in code → NEEDS CALEB to verify in Formspree. |
| 1.4 | 🔴 Phase 1 | Never display the personal Gmail; the phone replaces the Email row | Not started | — | Gmail currently has 0 hits in the repo, and that is the expected end state: the "form config" is on Formspree's side. If a 2nd row is needed, link to `#booking`, not an address. |
| 1.5 | 🔴 Phase 1 (added by Caleb) | Image optimization: resize and compress every image to its display size | Not started | — | Priority: `CG3 Assets/Client Testimonial/IMG_2758.JPG` (5.0 MB shown as a 96px avatar). Then the other on-page photos (hero, about, strip, booking, testimonials). Keep the originals out of the served path or replace them in place (decide at task time). Traffic is mostly phones. |
| 1.6 | 🔴 Phase 1 (added by Caleb) | Trust stats | Not started | — | Change "100% Satisfaction Guaranteed" so it no longer claims a guarantee. Remove "5★ Average Rating" until there are real public reviews. Leave "50+ Players Trained" and flag it for Caleb to confirm. The trust bar is a 3-column grid, so check the layout once a tile is gone. |
| — | Phase review | Phase 1 boundary review (`phase-review.md`) | Not started | — | |
| 2.1 | 🟠 Phase 2 | Replace "Tampa · St. Pete · Clearwater · Brandon" with the 6 GBP areas | Not started | — | Hits: meta description (L7), hero eyebrow (L381), training footnote (L675), Contact "Location" (L936), footer bottom line (L1093). og/twitter descriptions say "Tampa Bay" (handled in 2.2). The credential ticker's "St. Pete Aztecs UPSL" ×2 (L433, L443) **stays**: it's a credential, not a service area (Caleb, 2026-09-26), and it's excluded from the Phase 4 "St. Pete" check. Six areas in the hero eyebrow will wrap at 390px, so check layout. |
| 2.2 | 🟠 Phase 2 | Meta description rewrite (153 chars, given verbatim), plus matching og:description / twitter:description | Not started | — | |
| 2.3 | 🟠 Phase 2 | LocalBusiness JSON-LD (name, telephone, areaServed ×6, hours; **no email**) | Not started | — | None exists today. Service-area business: omit street address unless GBP shows one. Match GBP exactly. |
| — | Phase review | Phase 2 boundary review | Not started | — | |
| 3.1 | 🟠 Phase 3 | Dismissible tryout bar at the top of the hero ("High school tryouts start in October. Tryout prep blocks available now." → `#booking`), plus "Tryout Preparation" in the 1-on-1 list and in the form dropdown | Not started | — | Nav is `position:fixed` over the hero, so place the bar so it doesn't collide. No price for tryout prep (don't invent one). **Time-sensitive copy:** needs a removal date. |
| 3.2 | 🟠 Phase 3 | Weekday daytime / homeschool availability line in the training section | Not started | — | Real hours: Mon/Wed 9–3, Tue/Thu 11–3, Fri 9–8:30. Don't claim "9–3 every weekday". No availability copy exists anywhere today. |
| 3.3 | 🟠 Phase 3 | Replace "Contact for location & scheduling" with a structured slot for 2–3 park names, currently `[[TBD]]` | Not started | — | **Decided (Caleb, 2026-09-26):** show parents honest neutral wording (no invented park names). Keep a `[[TBD]]` marker in an HTML comment/data attribute at the slot, and in this file, so the park names drop in without a redesign. |
| 3.4 | 🟡 Phase 3 | Remove the hero scarcity line (L395–398); keep the booking-section one (L925) | Not started | — | |
| 3.5 | 🟡 Phase 3 | Replace "within 24 hours" promises with a text-first line (e.g. "Text for same-day response.") | Not started | — | Hits: Contact "Response Time" (L954), form subhead (L968), success message (L973). Also review L1020 ("…will reach out to confirm availability and location"). |
| — | Phase review | Phase 3 boundary review | Not started | — | |
| 4 | Phase 4 | Full review as a separate engineer: greps, links, `tel:`/`sms:`, form recipient, validation, console, 390px + desktop; list of noticed-not-fixed | Not started | — | Grep targets: `coachcaleb`, `@cg3academy.com`, "St. Pete" (except the "St. Pete Aztecs UPSL" credential, kept by Caleb), "Clearwater", "Brandon", `the personal Gmail` (the email, not the GitHub username `calebmgeorge96-spec`; expect 0 since the recipient lives in Formspree), the third-party academy name (case-insensitive), bare `href="https://gumroad.com"`. Scope: every file except this PROGRESS.md, which records the old city names on purpose. |

## 0.2 purge record (run 2026-09-26)
**What was purged:** the single blob `CG3 Assets/Pictures/IMG_6632.jpg`, added in old `[pre-purge commit]` (2026-04-28, "Add image assets so photos display on live site") and deleted in old `[pre-purge commit]`. The third-party academy's name appeared nowhere in the text of history (0 hits in diffs, paths or messages), so the photo was the only trace.

**Commands run** (fresh `git clone --mirror` in the scratchpad):
```
git bundle create ../pre-purge-backup.bundle --all          # 33 MB, verified; container-only
git filter-repo --invert-paths --path 'CG3 Assets/Pictures/IMG_6632.jpg'
git push origin --force-with-lease=main:<old> main
git push origin --force-with-lease=claude/nice-mccarthy-p1pb8x:<old> claude/nice-mccarthy-p1pb8x
git push origin --force-with-lease=cloudflare/workers-autoconfig:<old> cloudflare/workers-autoconfig
```
**Gates, all passed before any push:**
1. 0 photo objects left in history.
2. `main` tree unchanged (`[pre-purge commit]`), so the live site is byte-identical.
3. The Cloudflare branch differs from `main` by only `.gitignore` + `wrangler.jsonc`.

**Push results:** no branch-protection block.

| Ref | Old | New |
|---|---|---|
| `main` | [pre-purge commit] | [pre-purge commit] |
| `claude/nice-mccarthy-p1pb8x` | [pre-purge commit] | [pre-purge commit] |
| `cloudflare/workers-autoconfig` | [pre-purge commit] | [pre-purge commit] |
| `refs/pull/1/head` (moved by GitHub) | [pre-purge commit] | [pre-purge commit] |

**Verified after:**
- A fresh clone of all branches has 0 photo objects and 0 commits touching the path.
- PR #1 is open with 2 files changed (`.gitignore`, `wrangler.jsonc`), base `[pre-purge commit]`.
- Pages run #54 for `[pre-purge commit]` succeeded (18:37 UTC).
- 57 of 59 commits got new IDs; the 2 commits before `[pre-purge commit]` kept theirs.
- The session checkout was reset to the new history, unshallowed, and its stale local `main` repointed and garbage-collected (0 photo objects locally).

**Old → new IDs referenced in this file:**

| Commit | Old | New |
|---|---|---|
| pre-audit `main` tip | [pre-purge commit] | [pre-purge commit] |
| Phase 0 | [pre-purge commit] | [pre-purge commit] |
| 0.1 | [pre-purge commit] | [pre-purge commit] |
| 0.1b | [pre-purge commit] | [pre-purge commit] |
| first changed (photo added) | [pre-purge commit] | [pre-purge commit] |

The full map is in `filter-repo/commit-map` of the scratch mirror, which is container-only and lost when the session ends.

## 🔴 NEEDS CALEB
- [x] ~~0.1 approve + confirm IMG_4069 is Caleb~~: done, live 2026-09-26.
- [x] ~~0.1b check~~: Caleb verified 2026-09-26 (strip correct; `/PROGRESS.md` and the old photo URL 404).
- [x] ~~0.2 go~~: run 2026-09-26 (see purge record).
- [ ] **0.2 re-clone:** every local clone of this repo (your laptop, other Claude sessions) must be re-cloned, or hard-reset with `git fetch origin && git reset --hard origin/main`. **Never `git pull` an old clone; that merges the old history, photo included, back in.** Any old clone with `[pre-purge commit]` or `[pre-purge commit]` on `main` is pre-purge.
- [ ] **0.2 GitHub Support ticket** (https://support.github.com/request): ask them to remove cached views and dereference/garbage-collect the old commits for `calebmgeorge96-spec/cg3-academy` after a sensitive-data history rewrite. Include:
  - Affected PR: #1.
  - First changed commit (old ID): `[pre-purge commit]`.
  - Old ref tips: `main` [pre-purge commit]; `claude/nice-mccarthy-p1pb8x` [pre-purge commit]; `cloudflare/workers-autoconfig` [pre-purge commit]; `refs/pull/1/merge` [pre-purge commit].
  - No LFS objects.
  - Until Support acts, the old commits stay viewable on github.com to anyone who has their exact IDs.
- [ ] **0.1 follow-up (~2 weeks):** send a real coaching photo from your own sessions (plain or CG3 kit, no third-party crests) to replace the strip's 4th cell.
- [x] ~~0.1 history~~: Caleb approved rewriting `main` to purge the file → task 0.2.
- [x] ~~1.1~~: Products don't exist yet → remove the section (task 1.1).
- [ ] **1.1 later:** build the Gumroad products; then restore the section from git history with real product URLs.
- [ ] **1.3** (Caleb is doing this) Formspree dashboard → form `meevyrnw`: confirm the notification email is your current inbox, not the suspended domain inbox. After deploy, send one test submission from the live site and confirm it arrives.
- [ ] **1.6** Confirm "50+ Players Trained" is accurate.
- [ ] **3.3** Training park names (2–3) when decided.
- [ ] **3.1** When should the tryout bar come down? (Suggest the first week of November.)
- [ ] **Deploy.** Merge this branch to `main` for GitHub Pages to publish (Caleb approves each merge). Also decide on Cloudflare PR #1: it's clean now (2 files), so close it or intentionally move hosting, at your convenience.
- [ ] **Trust stats (rest):** confirm "10+ Years Competing" and the hero's "MLS Level Opposition" stat are how you want them worded.
- [ ] **Later.** Set up the proper domain email, then swap it back into the Contact block (and schema, if wanted).

## Open [[TBD]]
- [[TBD: training park names (2–3)]] (task 3.3)
- [[TBD: Lighthouse scores]] (not run yet; see capability check)

## Capability check (Section 0, 2026-09-26)
| Capability | Status | Substitute / note |
|---|---|---|
| git | ✅ | Branch `claude/nice-mccarthy-p1pb8x`, up to date with `origin/main` at start (`[pre-purge commit]`, now `[pre-purge commit]` after the 0.2 rewrite). Full (non-shallow) clone since 0.2. |
| Build | ➖ none exists | "Build green" = (1) `html-validate` (standard preset) shows **no new errors** vs baseline, (2) page loads with **zero console errors**, (3) **no horizontal overflow** at 390px and 1440px. Baseline: 7 pre-existing errors, all `<style>` inside `<body>` (L513, 534, 679, 820, 898, 1028, 1097). |
| Screenshots (1440 + 390) | ✅ with a workaround | Playwright + Chromium from the session scratchpad (not committed). `cdn.tailwindcss.com` is blocked by the sandbox network policy, so the harness compiles the same Tailwind v3 config locally and substitutes it for the CDN script. Google Fonts are fetched via Node and handed to the browser. **Caveat:** CSS cascade order may differ slightly from the live CDN. |
| Live site check | ❌ | cg3academy.com, formspree.io and *.github.io are blocked from this sandbox. Can't verify the live deploy or send a test submission; Caleb verifies. |
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
- 2026-09-26 (Caleb): approved rewriting `main` to purge `IMG_6632.jpg` (task 0.2), only after 0.1 is live and after a walkthrough of the commands.
- 2026-09-26 (Caleb): Digital Programs is removed from the source entirely (products not built yet). Restore from git history later.
- 2026-09-26 (Caleb): 3.3 uses honest neutral wording on the page; `[[TBD]]` stays in code + this file.
- 2026-09-26 (Caleb): keep the "St. Pete Aztecs UPSL" credential; excluded from the Phase 4 check.
- 2026-09-26 (Caleb): PROGRESS.md must not be served on cg3academy.com → `_config.yml` `exclude` (task 0.1b), in the same merge as 0.1.
- 2026-09-26 (Caleb): added tasks 1.5 (image optimization) and 1.6 (trust stats). I placed them at the end of Phase 1 because both affect paid mobile traffic right away.

## Gotchas & dead ends
- The Digital Programs section (`#products`) was already hidden (`display:none`) before this audit. Its dead Gumroad links still exist in source.
- The `#sticky-cta` element was removed earlier, but the JS still references it (harmless null-guarded).
- Mobile nav is the floating "BOOK NOW" FAB (`#fab`). The header "Book Session" button is `hidden sm:inline-block`, so below 640px the header shows only the logo.
- Line numbers above refer to `index.html` at commit `[pre-purge commit]` (pre-audit; formerly `[pre-purge commit]`) and will drift as tasks land.
- The sandbox proxy blocks cg3academy.com. Don't retry; it's policy, not an outage.
- The original session checkout was a **shallow** clone. It was unshallowed during 0.2, but a new session's checkout may be shallow again; use `git clone --mirror` for any history work.
- History was rewritten on 2026-09-26 (0.2). Commit IDs from before that date (in chat logs, old notes) no longer exist on the branches; see the old → new table in the purge record.
- GitHub Pages runs the `github-pages` gem's plugins, including `jekyll-optional-front-matter`, so **any** `.md` file in the repo becomes a published page unless excluded. Add new docs to `_config.yml` `exclude` (or give them a leading `_`).
- Setting `exclude` in Jekyll 3.10 replaces the default exclude list (Gemfile, node_modules, vendor…). None of those exist here; if any get added, list them in `_config.yml` too.

## Noticed but not in the audit (candidates, not scheduled)
- **Tailwind Play CDN in production:** render-blocking runtime JS, and it logs its own "should not be used in production" console warning on the live site.
- **Accessibility:** the 6 form `<label>`s aren't associated with their inputs (`for`/`id` missing).
- **Unused files served publicly:** 10 unused images, including 3 screenshots of older site versions (`Untitled.jpg`, `u.jpg`, `th.jpg`) that show outdated testimonial wording.
- `sitemap.xml` `lastmod` is 2026-04-30; bump it when changes ship.
- Footer blurb "Based in Tampa Bay" and the OG image text "Tampa Bay · Est 2023" are regional, not the old city list. Left as-is unless Caleb wants otherwise.

## Session log
- 2026-09-26: Phase 0 complete (stack mapped, capability check, Section 0 image audit found the 0.1 violation). Stopped for Caleb's go-ahead.
- 2026-09-26: Caleb answered the Phase 0 questions (see Decisions). 0.1 implemented on the branch (old [pre-purge commit], now [pre-purge commit]); stopped for approval.
- 2026-09-26: Caleb approved 0.1 and asked for PROGRESS.md to be excluded from the site in the same merge (0.1b). Fast-forwarded `main` [pre-purge commit] → [pre-purge commit] (old IDs; now [pre-purge commit] → [pre-purge commit]); Pages run #53 succeeded.
- 2026-09-26: 0.2 purge planned and rehearsed on a scratch mirror; waiting for Caleb's go.
- 2026-09-26: Caleb verified 0.1/0.1b live. Ran 0.2: all gates passed, 3 force-pushes succeeded, Pages run #54 green, PR #1 clean. Stopped before 1.1.
