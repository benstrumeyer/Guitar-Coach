# Real Guitar Hero

A Guitar Hero-style game that teaches you to play **real guitar** using **interactive tablature**. Notes scroll down a highway (like Guitar Hero), but instead of 5 colored buttons, you see real fret numbers on 6 strings -- and you play them on an actual guitar.

## Core Concept

- **Interactive scrolling tabs** -- not sheet music, not simplified buttons
- **Real guitar input** -- plug in your guitar, the app hears what you play
- **Pitch detection** -- knows if you hit the right note, bend, slide, or chord
- **Gamified learning** -- score, streaks, difficulty progression, just like Guitar Hero

## Architecture Overview

```
┌─────────────┐    ┌──────────────────┐    ┌─────────────────┐
│  Real Guitar │───>│  Audio Interface │───>│  Desktop App    │
│  (any electric    │  (USB, ~5-10ms)  │    │                 │
│   or acoustic     └──────────────────┘    │  - Tab Highway  │
│   w/ pickup)                              │  - Pitch Detect │
│              │                            │  - Scoring      │
└─────────────┘                             └─────────────────┘
```

## Tech Stack (Planned)

| Layer              | Technology              | Why                                        |
|--------------------|-------------------------|--------------------------------------------|
| **Game Engine/UI** | Godot 4 or Electron     | Godot: fast 2D rendering. Electron: web tech flexibility |
| **Pitch Detection**| Aubio (YIN algorithm)   | Battle-tested, low-latency, open source    |
| **Tab Parsing**    | alphaTab / TuxGuitar    | Parse Guitar Pro (.gp5/.gpx) files         |
| **Audio Input**    | USB Audio Interface     | Scarlett Solo, Behringer UM2, etc. ($40-60) |
| **Song Format**    | Guitar Pro tabs (.gpx)  | Richest guitar data: bends, slides, hammer-ons, timing |

## Hardware Requirements

### MVP (Desktop App)
- Any guitar (electric preferred for clean signal)
- USB audio interface (1/4" jack input)
- Computer running Windows/Mac/Linux

### Future: Standalone Hardware Version
- **Bela** (~$150-300) -- BeagleBone-based, <1ms audio latency, runs Linux
- **Electro-Smith Daisy** (~$30-250 depending on variant) -- STM32-based audio platform
- Either could run pitch detection + drive a display independently

## How It Works

1. **Load a song** -- import a Guitar Pro tab file
2. **Tab highway scrolls** -- 6 horizontal lanes (strings), fret numbers scroll toward a hit zone
3. **Play along** -- the app listens via audio input
4. **Real-time feedback** -- hit/miss detection, score, streak counter
5. **Practice tools** -- slow down sections, loop difficult parts, increase speed gradually

## Input Detection Strategy

The app does **polyphonic pitch detection** on raw audio from the guitar:

- **Single notes**: YIN algorithm via Aubio -- fast and accurate
- **Chords**: Harmonic Product Spectrum or ML-based (Crepe/SPICE)
- **Techniques**: Onset detection for hammer-ons/pull-offs, pitch bend tracking for bends/slides
- **Timing**: Compare detected note onsets against expected timing from the tab

## Song/Tab Format

Guitar Pro files (.gp5/.gpx) are the primary format because they contain:
- Note-by-note fret/string assignments
- Timing and tempo information
- Technique annotations (bends, slides, vibrato, palm mute, etc.)
- Difficulty can be derived from the tab data
- Massive community library of existing tabs

## Project Structure (Planned)

```
Real-Guitar-Hero/
├── docs/
│   ├── README.md          # This file
│   ├── plan.md            # Development plan & milestones
│   └── ideas.md           # Feature ideas & brainstorming
├── src/
│   ├── audio/             # Pitch detection, audio input
│   ├── tabs/              # Tab parsing (Guitar Pro format)
│   ├── game/              # Highway renderer, scoring, game loop
│   └── ui/                # Menus, settings, song select
├── assets/
│   ├── songs/             # Guitar Pro tab files
│   └── themes/            # Visual themes for the highway
└── tests/
```

## Getting Started

> **Status: Planning phase** -- see [plan.md](./plan.md) for development roadmap.

## Inspiration

- Guitar Hero / Rock Band (gameplay feel)
- Rocksmith (real guitar input concept)
- Yousician (learning progression)
- Clone Hero (open-source community charts)

The gap: Rocksmith is discontinued/closed-source, Yousician is subscription-based and limited. There's no good **open-source, tab-based, Guitar Hero-style real guitar trainer**. This project fills that gap.
