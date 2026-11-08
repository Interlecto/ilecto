# Publishing an Android app on Google Play

Lessons from taking Novenario 0.9 to the Play Console in November 2026, written so the next Interlecto Android app (ProHo, Contarte, Geo-Utils, Agenda…) can follow the same path with fewer surprises. Novenario's own step-by-step list is `play/RELEASE-CHECKLIST.md` in that repository; this file keeps what applies to any app.

Checked: 2026-11-07. Google changes its policies and console often, so confirm each rule against the linked page before relying on it.

## 1. Plan backwards from the launch date

- **The closed test is the critical path.** Personal developer accounts created after 2023-11-13 must run a closed test with **at least 12 testers opted in for 14 continuous days** before they can apply for production access. Add Google's review of that application and of the production release, and a public release needs about four weeks after the first upload. For Novenario (deadline December 16) the closed test had to start by November 10.
- **Testers are the bottleneck, not the code.** Start recruiting 15 or more people with Google accounts and Android phones as soon as the app has a date. Novenario had 6 testers four days before the test was due to start.
- **Identity verification of the developer account takes days.** Start it before the app is ready. Interlecto's account was verified on 2026-11-06. Verification is per account, so later apps do not repeat it.
- The internal-testing track is optional and does not count toward the 14 days. Still use it once with the owner alone: it shows the real Play delivery (signing, configuration splits, installer check) in minutes, before testers see the app.

## 2. Decide the application ID before the first upload

- **The application ID becomes permanent with the first bundle uploaded**, not when the app is created in the console. Changing it later means publishing a different app.
- Use a reverse domain that Interlecto controls: `app.ilecto.<app>` matches the project sites at `<app>.ilecto.app`. Novenario started as `net.chlewey.novenario`, but someone else holds `chlewey.net`, so it moved to `app.ilecto.novenario` the day before its first upload.
- `com.example.*` IDs are rejected by Play. QuickFixes still has one (see `portfolio.md`).
- A full rename moves `applicationId`, `namespace`, the Kotlin packages and, with Room's `exportSchema`, the schema folder under `app/schemas/` (it is named after the database class). Debug builds then install as a new app, without the old one's data.

## 3. Build configuration

- **Target API level.** New apps and updates must target the API level Google requires; from 2026-08-31 that is Android 16 (API 36). Check [the target API page](https://support.google.com/googleplay/android-developer/answer/11926878) each year.
- **Signing.** Keep the upload key out of git: a `keystore.properties` at the project root (with a committed `.example`) that `build.gradle.kts` reads, producing an unsigned bundle when the file is missing. **Back up the keystore and its passwords outside the computer.** Play App Signing enrollment cannot be undone: Google holds the app signing key, and the developer's key becomes only the upload key. Google can replace a lost upload key, but only on request.
- **Versions.** `versionCode` must increase with every upload and can never be reused, including builds that never left the internal track. Novenario called its first release 0.9 and kept 1.0 for the next milestone.
- **R8.** Test the *release* build on a device, because shrinking can break reflection-based code that works in debug. Keep `app/build/outputs/mapping/release/mapping.txt` and upload it with each bundle, so Play can deobfuscate crash reports.
- **Keep each uploaded bundle.** Copy the `.aab`, its `mapping.txt` and a note with the commit and SHA-256 to a folder per version outside `build/` (Novenario uses `~/.novenario/releases/<versionName>-vc<versionCode>/`). A `clean` deletes `build/`.

### App bundles and in-app language pickers

Play splits app bundles by language by default and installs only the device's languages. An app with its own language picker (`AppCompatDelegate.setApplicationLocales`) can then switch to a language whose resources are not installed: on Android 12 and older the strings fall back to the default language. Android 13 and later can fetch the missing split, which is why it did not show on the test phone.

For a small bilingual app, keep both languages in the base APK and drop library resources in other languages:

```kotlin
android {
    androidResources {
        localeFilters += listOf("es", "en")
    }
    bundle {
        language {
            enableSplit = false
        }
    }
}
```

To confirm, open `BundleConfig.pb` inside the `.aab` (a zip): an empty splits config means every split is on. `aapt2 dump badging app-release.apk` should list only the chosen locales. Google's `bundletool` can simulate Play's delivery, but auto mode blocks downloading it, so the owner has to install it.

## 4. Policy and console declarations

Play Console asks for these before the first release. Novenario's answers were all "no" because it is offline, free, without ads, accounts or data collection. Each new app should review them, and each version that adds network access, ads, accounts, payments or contact between users must answer them again.

| Declaration | What to know |
| --- | --- |
| Privacy policy | Required **even when no data is collected**. It must be on a public, non-geofenced web page (no PDF), be labeled as a privacy policy, name the app or developer, give a contact, and cover data handling, retention and deletion. Google's [User Data policy](https://support.google.com/googleplay/android-developer/answer/10144311) also asks for **a link or text inside the app**: Novenario links it from its About page, opened in the browser, so the app still needs no internet permission. Host it on the project site. |
| Data safety | "Collect" means sending data off the device. On-device storage is not collected. An app that collects nothing still fills in the form. |
| Advertising ID | Answer "no" when the merged manifest has no `AD_ID` permission (`aapt2 dump permissions`). |
| App access ("Detalles de acceso") | "No" when nothing needs an account, payment, code or second device. Reviewers cannot create accounts, so any restricted part needs test credentials. |
| Content rating (IARC) | "All other app types" for non-games. Text references in religious content (the Passion in the Rosary) did not count as violence. |
| Target audience | Selecting any age under 13 puts the app under the Families policy (extra forms, slower review). 13 and over was right for a family app not aimed at children; 16+ would have described the users less accurately without removing any obligation. |
| Trader status (EU) | Required for distribution in the EU. A trader's name, address, phone and email are shown on the listing. A non-trader listing tells EU users that consumer protection law does not apply. |
| US export laws | A checkbox when the app is created. Apps without their own encryption have nothing to file. |
| Advance notice to App Review | [A form](https://support.google.com/googleplay/android-developer/answer/6320428) for a few cases only, such as written permission to use third-party IP, government or health affiliations. Use it before releasing licensed texts, logos or audio; don't send anything else. |

The [Developer Policy Center](https://play.google/developer-content-policy/) is the index. Read the latest policy announcement before each release: the 2026-10-29 update was about local network, contacts and location permissions and subscription disclosures.

When the app is created, the console also offers **automatic protection** (an installer check added to the APKs Play serves). Leave it on: testers install from Play, and APKs signed outside Play are not affected.

## 5. Store listing

- **Default language and translations.** Novenario's default listing is Spanish (Latin America, es-419), with English (en-US) added as a translation. Write in the same register as the app: the Spanish listing first used *tú* while the app used *usted*, and was rewritten before upload.
- **Screenshots.** Phone screenshots must be no wider than 1:2. 1080 × 2160 crops below the status bar work well. Upload 2 to 8 per language, in order. Take a whole set on one device and theme: Novenario's set mixes an emulator and a phone, so the typeface differs. Set the device date to show the season (Novenario used December 18), clear personal data shown in the app (a greeting with the owner's name), and restore everything afterwards.
- **Release notes.** Up to 500 characters per language, pasted into one field with language tags: `<es-419>…</es-419>` and `<en-US>…</en-US>`. Keep the text in the repository with the listing. For an off-season test, tell testers how to reach the content (Novenario: "choose any day with the Día button").
- Category, contact email and website are required. A phone number is optional and public.

## 6. Content and intellectual property

- Record each text's source and license next to its files (Novenario keeps `source` and `license` in each `index.json` and a summary in `CONTENT.md`).
- **"Traditional" does not prove public domain.** In Colombia (Ley 23 of 1982), the term is the author's life plus 80 years, or 80 years from publication for an anonymous work. A popular lyric may be a recent arrangement with its own rights. Find a dated primary source; digitized books on Internet Archive, Wikisource and university repositories were enough for Novenario's carols.
- Research from AI assistants (Grok, ChatGPT) helped find leads, but both repeated unsourced claims and one reported a protected song as free. Open the primary source before relying on a claim.
- "Fair use" is a US doctrine. Colombia has specific exceptions instead, such as quotation.
- Play distributes worldwide, so a text free in Colombia may still be protected elsewhere.
- Novenario's policy: version 0.9 ships only verified public-domain texts; later versions may add licensed Bible translations and songs with written permission from the rights holders, sent to App Review through the advance-notice form.

## 7. Working with devices and agents

- **The owner's phone is in use.** Before every automated tap, check that the app is in front (`adb shell dumpsys window | grep mCurrentFocus`); a blind tap once landed in a chat. Prefer the emulator for anything that changes the app's data, and report any change made on the phone.
- The emulator needs several GB of free memory; on a busy machine it may not start at all. Stopping Gradle daemons (`./gradlew --stop`) frees some.
- Auto mode may block downloading tools from the internet (bundletool) or accepting third-party terms on the owner's behalf (IMSLP's disclaimer). Don't work around it; ask the owner.
- Keep a release checklist in the app repository and tick it as console steps are confirmed. Record decisions and their reasons in `CLAUDE.md`, such as why language splits are off.

## Sources

- [Testing requirements for new personal developer accounts](https://support.google.com/googleplay/android-developer/answer/14151465)
- [Target API level requirements](https://support.google.com/googleplay/android-developer/answer/11926878)
- [User Data policy](https://support.google.com/googleplay/android-developer/answer/10144311)
- [Data safety section](https://support.google.com/googleplay/android-developer/answer/10787469)
- [Developer Policy Center](https://play.google/developer-content-policy/)
- [Play App Signing terms](https://play.google/play-app-signing-terms/)
- [Advance notice to App Review](https://support.google.com/googleplay/android-developer/answer/6320428)
- [US export compliance](https://support.google.com/googleplay/android-developer/answer/113770)
- [Android App Bundles](https://developer.android.com/guide/app-bundle)
- [Per-app language preferences](https://developer.android.com/guide/topics/resources/app-languages)
