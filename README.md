# PayDay

Real-time salary and shift tracking for Android, built for the Israeli labor market.

![Platform](https://img.shields.io/badge/platform-Android-3DDC84)
![Language](https://img.shields.io/badge/kotlin-100%25-7F52FF)
![Min SDK](https://img.shields.io/badge/minSdk-24-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

Most time-tracking apps log hours and leave you to do the math later. PayDay does the math while you're still on shift - base rate, elapsed time, and travel expenses are calculated live and shown down to the second, not recalculated after the fact.

## Why

I built this because I was tired of clocking out and then opening a calculator to figure out what I actually earned. Israeli wage structures - travel allowances, per-shift vs. monthly commuting rates - aren't well served by generic Western time trackers, so I wrote one that handles them natively instead of bolting them on as an afterthought.

## Features

- **Live earnings counter** - updates continuously during an active shift using a lightweight main-thread looper, no polling, no noticeable battery drain.
- **Clock-in / clock-out reminders** - `WorkManager`-backed background checks nudge you at your expected start time and flag it if you forget to clock out. Compatible with Android 13+ notification permissions.
- **Monthly cycle handling** - the app tracks calendar months on its own: shifts roll over and archive automatically at the start of each month, no manual reset required.
- **Travel expense modes** - set either a flat monthly travel allowance or a per-shift rate; both feed directly into the salary total.
- **In-app updates without the Play Store** - a small OTA layer checks a JSON manifest hosted alongside the repo, pulls the new APK via `DownloadManager`, and hands it off through `FileProvider` for installation. No re-sideloading required after the first install.

## Screenshots

<p> <img src="https://github.com/user-attachments/assets/e415df82-b30f-4e70-a108-5e58ba253171" width="200"> <img src="https://github.com/user-attachments/assets/7c71b522-c0e2-47a7-862e-6341d9ef5f8e" width="200"> <img src="https://github.com/user-attachments/assets/c2bf5d54-8a59-4c37-ad5d-1ca9e678bed0" width="200"> </p>


## Stack

| Layer | Choice |
|---|---|
| Language | Kotlin |
| UI | Native XML + Material Components |
| Concurrency | Handlers / custom threading for the live counter |
| Background work | `WorkManager` (`PeriodicWorkRequest`) |
| Storage | `SharedPreferences`, wrapped for typed key-value access |

No Compose, no Room - the data model is small enough that a full SQL layer would've been overhead rather than help. Might revisit that if the schema grows.

## Installation

PayDay isn't on the Play Store, so it's installed manually:

1. Download `PayDay.apk` from the [Releases](../../releases) page.
2. Transfer it to your device, or open the release link directly from your phone's browser.
3. Open the APK. Android will ask you to allow installs from your browser or file manager - that's expected for sideloaded apps.
4. From then on, updates are pulled automatically through the app's built-in OTA check.

## Localization

Calculation defaults, currency formatting (₪), and date/time formats target Israel out of the box. The underlying logic isn't hardcoded to it, but adapting it for another market currently means adjusting the constants directly - there's no settings-based locale switch yet.

## Roadmap

- [ ] Configurable locale / currency
- [ ] Export monthly summary (CSV / PDF)
- [ ] Home screen widget for live earnings

## Contributing

Issues and PRs are welcome. If you're proposing a larger change, open an issue first so we can talk through the approach before you sink time into it.

Developed and maintained by Liad Michaeli
