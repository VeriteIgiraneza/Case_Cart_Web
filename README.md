# CaseCart

Match numbered surgical case carts to the boxes that belong on them.

In the OR supply workflow, each surgical case cart carries a 4-digit number, and the boxes staged on source carts carry their own 4-digit numbers. CaseCart holds the list of cart numbers, then tells you — as you read each box number out loud or type it in — which cart position that box goes to.

## Two versions

| | Web | Android |
|---|---|---|
| Stack | Single-file HTML/CSS/JS, no frameworks | Kotlin + Jetpack Compose |
| Package / entry | `index.html` | `com.reflekt.casecart` |
| Storage | `localStorage`, key `casecart_history_v2` | SharedPreferences `casecart_prefs`, key `history_v2` |
| Distribution | GitHub Pages | Signed APK via GitHub Releases |

The web app is a port of the Android app and keeps the same core features.

## Workflow

1. **Carts (setup)** — type each 4-digit cart number and press **Next**. Each entry gets a position (`A1`, `A2`, …). Press **Cut A** to start a new cut group, so the next entries become `B3`, `B4`, and so on. **Done** moves to matching.
2. **Match Boxes** — type a box number and press **Check Box**. The result banner says which cart it belongs to, warns if that cart was already matched, or reports no match.
3. **Finish** — batch-marks every green (matched) cart as done/gray, so the remaining greens from the next round stand out. Repeatable: match more boxes, press Finish again.
4. **View (summary)** — the full cart list with match status.
5. **History** — every session is auto-saved. Tap to reopen one, long-press to select and delete.

## Cart states

| State | Appearance | How to get there / undo |
|---|---|---|
| Unmatched | Plain white card | — |
| Matched | Green card, green position label | Set by checking a box number. Long-press (or right-click) the check to unmatch. |
| Placed / done | Gray card, gray text | Set by **Finish**, or by tapping an individual check. Tap the check to undo. |

## Running it

**Web** — open `index.html` in a browser, or serve the folder. It is self-contained: no build step, no dependencies, no network calls. Everything lives in browser storage on that device.

**Android** — open the project in Android Studio and run. Dependency versions are pinned in `libs.versions.toml` (core-ktx 1.13.1, activityCompose 1.9.3, lifecycleRuntimeKtx 2.8.7, composeBom 2024.10.01) to avoid AGP conflicts.

## Data model

A session is a list of carts plus a timestamp and a stable id:

```json
{
  "id": 1757606400000,
  "timestamp": 1757606400000,
  "carts": [
    { "number": "4821", "group": "A", "matched": true, "placed": true }
  ]
}
```

Positions are not stored — they are derived from list order on load, so removing a cart renumbers the rest.

## Constraints

- The web app must stay a **single self-contained HTML file**. No frameworks, no build tooling, no external assets.
- Cart numbers are exactly 4 digits and must be unique within a session.

## Known issues

- Android Chrome sometimes leaves a stuck gray oval on a tapped button. Several CSS and JS workarounds have not resolved it.
- Session history is per-device and per-browser. Clearing site data clears it.