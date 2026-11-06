# Portfolio notes

Ideas and decisions that span more than one Interlecto project. Each project's
own roadmap stays the source of truth for its schedule; this file only tracks
how the projects relate to each other.

Update the "Checked" date when a section is reviewed against the repositories,
and let the git history of this file carry the older versions.

## iOS portability

Checked: 2026-11-06, from each repository's docs, its branches, and the imports
in its Kotlin code. File counts are a snapshot and will drift.

### Plans in place

| App | Plan | Approach | Status |
| --- | --- | --- | --- |
| Novenario | In step with Android by December 2027, possibly much earlier (ROADMAP Phase 5) | Undecided; the roadmap recommends Kotlin Multiplatform for the logic and SwiftUI for the screens | Not started |
| ProHo | One of three clients on a shared backend, alongside web and Android | Native Swift + SwiftUI, explicitly not Kotlin Multiplatform | Scaffolded on `develop/ios-shell` (app shell, `ProHoKit` package), not merged to `master`. Builds run only in CI on macOS runners. TestFlight and App Store distribution is an open question in its roadmap |
| Agenda Unificada | ROADMAP Phase 8, once Android and the data model are stable | Swift + SwiftUI | `ios/` placeholder only |

### No plan yet

| App | Fit | Reasoning |
| --- | --- | --- |
| Geo-Utils | Good | The 15 files in `geometry/` import nothing from Android, so projections are ready to share. Bitmap reprojection and export need iOS graphics APIs. |
| Dox-Mut | Good | A conversion core (parsers and renderers around a document model) suits a shared Kotlin module. 5 of 18 Kotlin files import Android. The share target becomes an iOS share extension. |
| Contarte | Good, later | Local-first with no server, and 20 of 97 Kotlin files import Android. The screens need rewriting; the accounting logic could be shared. |
| QuickFixes | Weak | 14 of 18 Kotlin files import Android, so little is reusable. A small image utility is also the most exposed to Apple's minimum functionality rule. |
| Extension Icons | Not possible | Built on the Windows registry and shell. iOS apps can't control icons or file associations system-wide. |

### Suggested order

1. **Novenario:** a real deadline, shared content, no backend and no data
   collected, so it's a low-risk first pass through Apple's process.
2. **Agenda Unificada and ProHo:** when their Android or backend work
   stabilizes.
3. **Geo-Utils and Dox-Mut:** when there is capacity. Dox-Mut's shared-core
   decision is cheapest before its M2 starts.
4. **Contarte:** after v1, targeted for mid-May 2027.

### Shared prerequisites

- Apple Developer Program: US$99 a year, one account for all apps.
- A Mac with Xcode, owned or rented, since the dev machines run Windows. ProHo's
  CI builds on macOS runners, which covers compiling but not device testing.
- A decision on enrolling as an individual or an organization, which sets the
  seller name shown on the App Store. An organization needs a legal entity and
  a D-U-N-S number.

### Open questions

- **Shared Kotlin or native Swift?** Novenario leans towards Kotlin Multiplatform
  while ProHo and Agenda Unificada chose Swift. Mixing them means maintaining
  both toolchains.
- **Shared accounting code?** ProHo and Contarte already share an API contract
  (`docs/api-contract/` in Contarte), but ProHo's native Swift client limits how
  much code an iOS Contarte could share with it.
- **Does Agenda Unificada need Sign in with Apple?** It depends on whether Apple
  treats Google and Outlook sign-in as account login or as connecting calendars.
