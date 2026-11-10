# CLAUDE.md

Handoff notes for any agent or account continuing work on Interlecto's projects.
This repository is the hub page at `ilecto.app`, and also the home for knowledge
that spans the projects. See `README.md` for this site and `docs/portfolio.md`
for cross-project ideas such as iOS portability.

These notes consolidate the local Claude Code memories as of 2026-11-06, checked
against the repositories that day. A project's own `CLAUDE.md` wins over this
file for that project; every project has one. Statuses move
fast, so check the repository before relying on one.

## This repository

The hub is a static, bilingual (ES/EN) page at `https://ilecto.app/` that links
to each project's own site.

- `public/` holds the static site: `index.html`, `styles.css`, `_headers`, icons,
  and one image per linked project in `public/projects/`.
- `src/worker.js` is a small Worker that only redirects non-canonical hosts to
  `https://ilecto.app/`. The page itself is static.
- `wrangler.jsonc` configures the Worker: name `ilecto-site`, assets from
  `./public`, custom domain `ilecto.app`, `workers_dev` and preview URLs off.
- There is no build step and nothing to install. Deploy with `npx wrangler deploy`
  from the repo root. That publishes to production, so only deploy when asked.
- Use plain HTML and CSS with system fonts. Keep both languages in step when
  editing page text.

## Owner and conventions

- The owner is Carlos Thompson (Chlewey; `chlewey` on GitHub). Reply in the
  language they write in, usually Spanish or English. Commits and most documents
  are in English, and public-facing content is bilingual (ES/EN).
- Before pushing or deploying, follow what the owner asked in the current
  conversation. They usually name the steps, for example "deploy, commit, push".
- Never commit secrets, real release keystores or local configuration. This repo
  is public, so don't add email addresses or account IDs either. The exception is
  an ID already shown in a public store URL, such as the Google Play developer
  page. Console and dashboard URLs stay out.

## Projects

| Project | Local path | GitHub repo | Visibility | Default branch | Site | Worker |
| --- | --- | --- | --- | --- | --- | --- |
| Hub | `C:\temp\ilecto` | `Interlecto/ilecto` | Public | `master` | `ilecto.app` | `ilecto-site` |
| Novenario | `C:\Temp\Novenario` | `chlewey/Novenario` | Private | `master` | `novenario.ilecto.app` | `novenario-site` |
| ProHo | `C:\temp\proho` | `Interlecto/proho` | Public | `master` | `proho.ilecto.app` | `proho-site` |
| Contarte | `C:\Temp\Contarte` | `chlewey/Contarte` | Private | `main` | `contarte.ilecto.app` | `contarte-landing` |
| Geo-Utils | `~/geoutils` | `chlewey/geoutils` | Public | `main` | `geoutils.ilecto.app` | `geoutils-site` |
| QuickFixes | `C:\Temp\quickfixes` | `chlewey/quickfixes` | Private | `master` | `quickfixes.ilecto.app` | `quickfixes-site` |
| Agenda Unificada | `C:\Temp\agenda` | `Interlecto/agenda` | Public | `master` | `agenda.ilecto.app` | `agenda-site` |
| Dox-Mut | `C:\Temp\doxmut` | `Interlecto/doxmut` | Private | `main` | `doxmut.ilecto.app` | `doxmut-landing` |
| Extension Icons | `C:\Temp\extension-icons` | `chlewey/extension-icons` | Public | `master` | `extension-icons.ilecto.app` | `extension-icons-site` |
| Key Assign | `C:\Temp\keyassign` | `chlewey/keyassign` | Public | `master` | None yet (`keyassign.ilecto.app` planned) | None yet |

Key Assign (Windows keyboard layout editor, started 2026-11-10) has no site yet,
so it isn't on the hub page or in `README.md`. Add it with "Adding a project to
the hub" below once its landing page is live.

Many repos are under the personal `chlewey` account rather than the `Interlecto`
org, so check the table rather than guessing. Local checkouts are often on a
feature branch, so read `origin/<default>` when you need the current state.

The Geo-Utils worker name comes from earlier notes; every other name was read
from that repo's `wrangler` config.

## Git workflow

- Work on `develop/<topic>` branches cut from the default branch, which merge
  straight back. There is no long-lived integration branch. Propose sub-branches
  in a plan before creating them.
- **Contarte and QuickFixes:** each roadmap phase or correction gets its own
  `develop/<name>` branch. Commit and push it without asking, but don't merge or
  open a PR unless asked.
- **Geo-Utils:** from milestone 1 on, roadmap work goes on the `develop` branch.
  Merging into `main` is the owner's call.
- **ProHo:** Phase 0 sub-branches are named `develop/phase0/<topic>`, because a
  `develop/foundation` branch already blocks `develop/foundation/...`. The owner
  reviews the domain model and accounting rules before they merge.

## Hosting and deployment

- Every site is a Cloudflare **Worker** with a static-assets binding. There are
  no Cloudflare Pages projects, so don't look for one.
- Deploy with `npx wrangler deploy` from the project's `site/` folder (`landing/`
  for ProHo, the repo root for this hub). Each worker claims one custom domain in
  its own `wrangler.jsonc`.
- **Accounts:** run `npx wrangler whoami` first. A new machine or account needs
  access to the Cloudflare account holding the `ilecto.app` zone, and push access
  to both `chlewey` and `Interlecto` repos (`gh auth status`).
- **Auto mode:** the permission classifier may block `wrangler deploy`, even when
  it allowed one earlier. Don't work around it. Ask the owner to run
  `! npx wrangler deploy`, or to confirm explicitly.
- **First deploy of a new worker:** it can fail with
  `[code: 10007] This Worker does not exist on your account` right after the
  asset upload. That is a Cloudflare propagation delay. Wait about 20 seconds and
  run it again, with no config change.
- **DNS:** `ilecto.app` uses Cloudflare nameservers, moved from Porkbun around
  2026-10-24. Local resolvers cache failures, so don't conclude a site is down
  from the default resolver. Verify with `nslookup <host> 1.1.1.1`, then
  `curl --resolve <host>:443:<ip> https://<host>/`.

## Adding a project to the hub

1. Read the repo's `site/wrangler.jsonc` for the real subdomain and worker. Check
   that the site is live, since a landing page may exist only on an unmerged
   branch.
2. Copy the project's favicon from its live site into `public/projects/`, and read
   any SVG to confirm it has no scripts or links.
3. In `public/index.html`, add an `<a class="card">` with the icon and ES and EN
   descriptions, and add the project to the meta description.
4. Add a row to the project table in `README.md`.
5. Deploy, confirm on the live page, then commit and push as the owner asks.

Hub icons are copies, so they need refreshing when a project's identity changes.

## Interlecto visual identity

- Use the blue speech bubble with a white lowercase "i", in brand blue `#3399cc`
  (`--accent`; `#5bb8e8` in dark mode). The wordmark is "Interlecto" in bold.
- For new Interlecto-branded pages, copy `public/icon.svg` from this repo instead
  of inventing a color or mark. Rasterized icons were made with ImageMagick.
- The source is the 2015 `Interlecto/ilecto-php` repo, not `il-font` or `il-ui`.
  Don't use its `root/ui/ilicon-*.png` files, an abandoned concept. Its
  "IlectoLogo" font is unfinished, so pages use system fonts.
- Individual projects have their own identities, kept in their own repos.

## Android projects

- Commit a shared `keystore/debug.keystore` (alias `androiddebugkey`, passwords
  `android`, the standard debug values), wired into `signingConfigs.debug` through
  `rootProject.file(...)`. Do this from the first commit, so test devices accept
  builds from any machine as the same app. Release keys never go in a repo.
  Contarte, QuickFixes, Agenda Unificada and Dox-Mut share one key (SHA-1
  `EA:38:F3:95:…:F7:BF:FD:D2`), which is also the one registered for Agenda's
  OAuth clients. Geo-Utils and ProHo each have their own. For a new app, copy
  Contarte's.
- The owner works from two Windows machines, so SDK paths and Git identity can
  differ. `local.properties` is not in git. Novenario's `CLAUDE.md` has the details.
- One Google Play developer account, "Interlecto", publishes all the apps.
  `docs/portfolio.md` tracks its public page, each app's application ID and its
  store descriptions. Update it when a listing changes. Profile verification was
  completed on 2026-11-06.
- Before publishing an Android app on Google Play, read
  `docs/google-play-release.md`: the lessons from Novenario's first release
  (closed-test timing, application ID, signing, bundle language splits, policy
  declarations, listing).

## ProHo and Contarte

From the accounting point of view the two are the same: ProHo is not a general
accounting app but an app that uses accounting, optimized for its tasks. Frame
differences as ProHo's use of a shared accounting, not as a second model.

- **Upstream:** Contarte's ADR 0002 makes it upstream for shared primitives. If
  ProHo needs a primitive Contarte lacks, add it in Contarte first. Domain rules
  specific to ProHo stay in ProHo.
- **One API:** ProHo's accounting module API is the same contract as Contarte's,
  for ProHo's own clients too. Don't design a separate API or an adapter. Billing
  keeps its own API. Any ProHo community ledger is an accounting server from
  Contarte's point of view.
- **External access:** ProHo will expose a ledger API to Contarte clients through
  AAA (authentication, authorization, accounting) agreements. This is not
  scheduled; the agreement's contents, approver and write scope are open.
- **Contract folder:** `docs/api-contract/` on Contarte's `main` is authoritative;
  if ProHo's docs differ, the folder wins. From a ProHo session, write concerns
  only in `proho-requirements.md` and treat everything else there as read-only.
  `requirements-coverage.md` is the official response. Read it before relying on
  anything: items are agreed one by one, and agreement commits to the requirement
  rather than its wire-level shape.
- **Money:** integer minor units plus a currency code, never floating point.
- **Country scope:** both must comply with Colombian rules without being limited
  to them. Keep PUC codes, Ley 675, NIIF groups and withholding out of
  `core/accounting`; they belong in docs, templates and seed data. Country or
  sector starter packages are out of MVP scope, but Contarte's chart-of-accounts
  template mechanism must stay able to take them.

### ProHo decisions (don't re-ask)

- **Accounting:** Colombian *propiedad horizontal* (Ley 675/2001, NIIF for small
  entities, PUC-style codes as a configurable template). An accountant has yet to
  confirm this, so flag dependent choices in docs rather than guessing.
- **Identity:** Spring Authorization Server inside the backend monolith (OIDC
  tokens), with Google later.
- **Hosting:** undecided. Build container images and pick a host before Phase 4.
- **Local tools:** Node LTS through winget is allowed. There is no Docker, so use
  embedded PostgreSQL locally and Testcontainers in CI.
- **Modularity:** features are modules enabled per community kind (residential
  first; clubs and associations later) and plan.
- **Plans:** the free plan is for communities under 20 units and under 60 SMMLV of
  annual budget, with platform-held keys. The first paid plan gives complete data
  ownership and customer-held keys. This needs legal review.
- **iOS:** built only in CI on macOS runners, since the dev machines run Windows.

## Geo-Utils

The owner's older repos `chlewey/globe-prj`, `kml2svg` and `maps` are sources of
features to reimplement in Kotlin, marked *(from …)* in its roadmap.
`kml2svg/Continents.kml` is the owner's own CC0 work and may be bundled. Don't
bundle third-party data from those repos (GADM boundaries, other country KMLs of
unknown origin). The fixmap KML editor comes last (Milestone 8).

## Continuing from another machine or account

Each project's `CLAUDE.md` links here and holds that project's own rules.
Claude Code memories stay on the machine that wrote them, so record lasting
rules in a `CLAUDE.md`, not only in memory. Switching Claude accounts on the same
machine keeps the repos, memories and local tool logins, but connectors added on
claude.ai have to be connected again in the new account.

Access only the owner can grant:

- **GitHub:** push to both `chlewey` and `Interlecto`. Novenario, Contarte,
  QuickFixes and Dox-Mut are private. Check with `gh auth status`.
- **Cloudflare:** the account holding the `ilecto.app` zone. Check with
  `npx wrangler whoami`.
- **Google Play Console:** an invitation to the "Interlecto" developer account.
- **Agenda Unificada OAuth:** the Google and Microsoft client IDs go in
  `local.properties`; see its `docs/SETUP_OAUTH.md`.

Files outside git, to pass over a secure channel: Novenario's
`keystore.properties`, release key, `play-account.properties` and `private/`
(see its `CLAUDE.md`), and each project's `local.properties`.

Tools: JDK 17, the Android SDK and Node LTS for all projects; .NET 8 and Inno
Setup for Extension Icons; .NET 8 for Key Assign. iOS builds need a Mac or CI macOS runners. Clone the
repos to the paths in the project table, or update the table.

## Open items

- `docs/portfolio.md` lists the open iOS questions. No Apple Developer enrollment
  is on record, and individual or organization is undecided.
