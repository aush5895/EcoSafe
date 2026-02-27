# EcoSafe

> *Scan packaging. Learn the facts. Rescue the ocean. Your choices have consequences — and today, they can be good ones.*

---

I built EcoSafe because I got tired of standing in front of a recycling bin, holding a plastic container, genuinely not knowing where it goes. That 10-second hesitation is where most plastic loses. I wanted an app that actually teaches you — not by lecturing, but by making the right choice feel good.

EcoSafe is a Swift Playgrounds app that turns everyday recycling into something you actually want to do. You scan packaging, learn what to do with it, play a mini-game to intercept plastic before it hits the ocean, and watch your real-world impact grow over time. The ocean health on your dashboard? It reflects how consistently you show up.

Built as a submission for the **Apple Swift Student Challenge**.

---

## What it actually does

### Scan & Learn
Point your camera at any packaging and EcoSafe identifies the material type — plastic bottles, aluminium cans, cardboard, glass, and more. It tells you exactly where it goes, what the recycling number means, and one fact you probably didn't know about that material. No guessing. No wish-cycling.

### Ocean Rescue Game
A physics-based mini-game where trash items fall toward the ocean and you slide a bin to catch them. Miss one and it hits the water. Catch them all and you get a perfect game. It sounds simple, but the combo system gets genuinely tense. The items you catch in the game actually count toward your daily recycling goal — because the habit of paying attention is the whole point.

### Real Impact Tracking
Every scan and every game session adds to your numbers. Not vague numbers — actual grams of plastic intercepted, bottles diverted, bags prevented. Your dashboard shows a live weekly bar chart that grows as you scan throughout the day, and a monthly lifetime chart so you can see the habit building over time.

### Ocean Health System
Your ocean starts polluted. As you scan, play, and learn, it recovers. The background visuals shift — from dark and murky to clear and full of colour. Fish appear as health improves. The ambient sound changes. It's not just cosmetic: it's a direct reflection of how consistently you've been showing up.

### 12 Flashcards
Six categories, twelve cards covering everything from why Tetra Pak is almost impossible to recycle, to how many microplastic fibres your washing machine releases every cycle. Each card has a fact and an actionable tip. They're not trivia — they're things that will change how you behave at the bin.

### Streaks & Levels
Your streak counts consecutive days you've scanned or played. Your ocean level (1–50) tracks your total lifetime impact — from "Beach Cleaner" at level 1 to "Ocean Legend" at level 36. There's also a best-streak counter so you have something to beat.

### Achievements
Ten unlockable achievements including:
- **First Step** — your very first scan
- **Eco Scholar** — all 12 flashcards mastered
- **Reef Defender** — 3-day streak
- **Ocean Sentinel** — 7-day streak  
- **Flawless** — a perfect game with zero misses
- **Ocean Hero** — ocean health above 80%

### Cinematic Intro & Outro
A 5-panel opening sequence that tells you why this matters — real statistics, real stakes, written to actually land. On the final panel it asks your name, then greets you personally every session. After your first game, a 4-panel outro reminds you what your actions just meant. It plays once and never gets in the way again.

---

## Tech stack

| What | How |
|------|-----|
| Language | Swift 5.9 |
| UI Framework | SwiftUI |
| Game Loop | `TimelineView` with `.animation` cadence |
| Persistence | `UserDefaults` (v2 key prefix) |
| Audio | `AVFoundation` — ambient, SFX, haptics |
| Camera | `AVCaptureSession` with real-time material detection |
| Charts | Custom SwiftUI bar charts with `@State`-driven spring animations |
| Architecture | `ObservableObject` + `@EnvironmentObject` throughout |
| Target | Swift Playgrounds 4 / Xcode — iOS 17+ |

No third-party dependencies. No analytics. No ads. Everything runs on-device.

---

## File structure

```
EcoSafe/
├── MyApp.swift              # App entry, ContentView, all dashboard views,
│                            # charts, impact view, flashcards, about
├── AppState.swift           # Single source of truth — all @Published state,
│                            # scan logic, streak, achievements, ocean health
├── EcoConfiguration.swift  # All constants, TrashType enum, OceanPhase enum,
│                            # motivational quotes, streak messages
├── GameEngine.swift         # Ocean Rescue game logic — physics, rounds,
│                            # scoring, combo system, timing
├── OceanRescueGameView.swift # Game UI — HUD, trash, bin, waves,
│                            # summary screen, plastic-saved popup
├── SupportViews.swift       # CinematicIntroView, MotivationOutroView,
│                            # LearnCardView, RecycleTipsCarousel
├── LearningContentProvider.swift # All 12 flashcard definitions
└── SoundManager.swift       # AVFoundation audio — ambient ocean,
                             # health-reactive layers, SFX, haptics
```

---

## How to run it

**In Swift Playgrounds 4 (iPad or Mac):**
1. Clone or download the repo
2. Open the `.swiftpm` package in Swift Playgrounds
3. Hit Run

**In Xcode:**
1. Open the `.swiftpm` as a package
2. Select an iOS 17+ simulator or your device
3. Build and run

> **Note:** Camera scanning requires a physical device. The game, flashcards, dashboard, and all tracking work fine in the simulator.

---

## How the ocean health system works

Ocean health is a value between 0.0 and 1.0, persisted across sessions.

| Action | Effect |
|--------|--------|
| Scanning an item | +0.015 |
| Catching trash in game | +0.020 per item |
| Missing trash in game | −0.010 per item |

Health determines the `OceanPhase`:

| Phase | Health range | What you see |
|-------|-------------|--------------|
| Polluted | 0 – 25% | Dark background, no fish |
| Stabilizing | 25 – 45% | Background lightens |
| Recovering | 45 – 65% | First fish appear |
| Flourishing | 65 – 80% | Colour returns, coral visible |
| Thriving | 80 – 100% | Full ecosystem, bright ambient |

The ambient sound changes with health too — ominous low rumble when polluted, bright layered tones when thriving.

---

## The level system

Every **25 grams** of plastic intercepted (scans + game catches) earns one level. The XP bar on the dashboard animates in real time.

| Levels | Title |
|--------|-------|
| 1–3 | Beach Cleaner |
| 4–7 | Shore Guardian |
| 8–12 | Reef Protector |
| 13–18 | Ocean Steward |
| 19–25 | Sea Champion |
| 26–35 | Marine Hero |
| 36–49 | Ocean Legend |
| 50 | Ocean Master |

---

## Some decisions I made along the way

**Why UserDefaults instead of SwiftData or CoreData?**  
Swift Playgrounds compatibility. SwiftData requires Xcode. UserDefaults works everywhere, and with a `v2.` prefix to version the keys, migration is clean.

**Why TimelineView for the game instead of SpriteKit?**  
Keeping everything in SwiftUI means no framework switching, no scene files, and the game feels native rather than embedded. `TimelineView` with a `Date`-driven tick function gives consistent frame timing without a game loop thread.

**Why is the daily goal 25g (5 scans)?**  
Early versions had 50g with 1.2g per scan — you'd need 42 scans to hit the goal. Nobody does that. 5 scans is achievable in a single lunch break. The goal should feel winnable every day so the streak stays intact.

**Why does the intro play every time?**  
Because this is a Challenge submission and judges need to see it. In a real release, you'd only show it once.

---

## What I learned building this

- SwiftUI's `.animation(..., value:)` on `.frame(height:)` doesn't reliably animate — you need `@State` height variables driven by `withAnimation {}` calls
- `@EnvironmentObject` is essential for reactive chart updates — passing data as `let` props breaks `onChange` detection
- `TimelineView` is genuinely underused for game loops — it's clean, SwiftUI-native, and the pause behaviour is built in
- Haptics make a bigger difference than you'd expect for something this small

---

## Roadmap / ideas I didn't have time for

- [ ] Barcode scanning for actual product lookup
- [ ] Local council recycling rules by postcode/zip
- [ ] Multiplayer ocean — share a health bar with friends
- [ ] AR mode — point at a bin and see what goes in it
- [ ] Notification at bin time ("You scanned 0 things today")
- [ ] Widget showing daily progress ring on home screen

---

## Acknowledgements

Built entirely with Swift and SwiftUI. No external libraries, no design assets — every colour, animation, and layout is code.

The environmental statistics used in the flashcards and intro are sourced from IUCN, UNEP, and peer-reviewed marine biology research. I tried to be accurate and not sensationalise — the real numbers are alarming enough.

---

## Licence

MIT. Use it, fork it, learn from it. If you build something with it, I'd love to see it.

---

*The ocean doesn't ask for much. Just your consistency.*
