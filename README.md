# 🏖️ Beach Time

A Flutter app that tells you whether today is a perfect beach day, and the best hours to **swim**, **surf** or **chill**.

## Features

| | |
|---|---|
| **Beach Score 0-100** | Weighted per activity: air & water temperature, wind, waves, rain, sunshine, UV. Penalties for storms, gusts, heavy rain and darkness. Tap the score to see the breakdown. |
| **Best time window** | Finds the best 3-6 h stretch of daylight. |
| **Hour by hour** | Animated score bars for every hour. |
| **Sea** | Water temperature, wave height and period, **tide curve** with highs and lows. |
| **Sun & UV** | Animated sun path, sunrise/sunset, golden hour, SPF advice, estimated time to burn. |
| **Water safety** | Estimated flag colour, rip-current risk, air quality. |
| **Beach bag** | Smart packing checklist built from the forecast; ticks are saved per day. |
| **7-day outlook** | Daily scores, with the best day starred. |
| **Beach Buddy AI** | Chat with an **open-source LLM** (Llama, Qwen, Mistral...) that reads the live forecast. |
| **Places** | Search any beach worldwide, use GPS, save favourites, one-tap popular beaches. |
| **Units** | Metric or imperial. |

## Design

- **Palette:** warm sand paper, deep-sea ink, one sea accent, coral and sun used sparingly (`lib/theme/palette.dart`). One score colour ramp everywhere.
- **Type:** Bricolage Grotesque (headlines, numbers) + Manrope (text), bundled as variable fonts, so there is no network fetch and no font flash (`lib/theme/typography.dart`).
- **Icons:** Phosphor icon fonts bundled in `assets/fonts`; `lib/theme/ph_icons.dart` lists the glyphs used (the old `phosphor_flutter` package does not compile on Flutter 3.50). Unused glyphs are tree-shaken at build time.
- **Borrowed from the best apps:** Surfline's word ratings (EPIC / GOOD / FAIR / POOR), Windy-style direction arrows for wind and swell, Surf-forecast's "best day" stars, Apple Weather's temperature range bars.

Animations: looping ocean waves, a sky that follows the time of day, a sun or crescent moon with twinkling stars, a sweeping score ring that counts up, staggered card entrances, a sliding activity pill, tide and sun-path draw-ins, a waving flag, a shimmer loading state, chat bubble pops and a typing indicator.

## Data (free, no API key)

All from [Open-Meteo](https://open-meteo.com) (CC BY 4.0):
`api.open-meteo.com` (weather) · `marine-api.open-meteo.com` (waves, sea temperature, tides) · `air-quality-api.open-meteo.com` · `geocoding-api.open-meteo.com`.

## Beach Buddy AI setup

Open Settings (the sliders icon) and pick a preset. Any OpenAI-compatible server works.

- **Ollama (100% local and free):** install from ollama.com, run `ollama pull llama3.2`. Works on desktop/web builds at `http://localhost:11434/v1`. For a phone, use your PC's LAN IP and start Ollama with `OLLAMA_HOST=0.0.0.0`.
- **Groq / OpenRouter / Hugging Face:** host open models in the cloud. Create a free API key on their website and paste it into Settings.

## Website

The marketing site and the brand kit live in their own repo, [beach-time](https://github.com/annitarik0/beach-time).
It is live at https://annitarik0.github.io/beach-time/ and builds the web version of this app under `/app/`.
After changing the app, run the "Publish website" workflow in that repo to refresh `/app/`.

Brand identity case study: https://www.behance.net/gallery/258758635/Beach-Time-Brand-Identity

## Run

Flutter was installed to `%USERPROFILE%\flutter`. Add `%USERPROFILE%\flutter\bin` to your PATH (or install the Flutter extension in VS Code and point it there).

**Easiest:** in VS Code press `Ctrl+Shift+B` (task "Beach Time: run in Chrome"), or run `.\run_dev.ps1`. Then press `r` in that terminal to hot reload and `R` to restart. The script runs from `%USERPROFILE%\beach_time`, a link to this folder, because the Flutter web debug compiler fails on the space in "BEACH TIME".

```powershell
flutter pub get
flutter run -d chrome     # or: -d windows, or an Android device/emulator
flutter test
```

### On your iPhone (personal use, from Windows, free)

1. Push this project to GitHub. The workflow `.github/workflows/ios-ipa.yml` builds `BeachTime.ipa` on a macOS machine (GitHub "Actions" tab, then the run, then "Artifacts"). Start it again any time with "Run workflow".
2. On the PC install iTunes (from apple.com, not the Microsoft Store) and Sideloadly (sideloadly.io).
3. Plug in the iPhone, drop `BeachTime.ipa` into Sideloadly, sign in with your Apple ID and press Start.
4. On the iPhone: Settings > Privacy & Security > Developer Mode (on, then restart), and Settings > General > VPN & Device Management > trust your Apple ID.
5. A free Apple ID signature lasts 7 days: re-install with Sideloadly when the app stops opening.

App icon: `assets/icon/app_icon.png`, regenerate all sizes with `dart run flutter_launcher_icons`.

### iOS

iOS builds need macOS with Xcode (Apple's requirement). On a Mac run `flutter run -d ios` or open `ios/Runner.xcworkspace`. From Windows you can build in the cloud with Codemagic or GitHub Actions (macOS runners). The location permission text is already in `ios/Runner/Info.plist`.

## Code map

```
lib/
  main.dart                 app + theme
  models/                   Beach, HourlyConditions, DailySummary, BeachForecast
  services/                 Open-Meteo, AI (OpenAI-compatible), location, storage
  logic/                    beach_score.dart (scoring), insights.dart (safety, tides, UV, packing)
  state/beach_controller.dart
  screens/                  home, search, AI chat, settings
  widgets/                  animated cards and painters
test/                       scoring + widget tests
```

## Ideas for the next upgrades

Push notification when a perfect day is coming · home-screen widget · jellyfish/water-quality reports · crowd levels · beach photos & reviews · on-device model (llama.cpp) for offline AI · Apple Watch complication.
