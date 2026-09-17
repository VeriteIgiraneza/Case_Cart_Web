# CaseCart

A tool for matching numbered surgical case carts to the supply boxes that belong on them — built for a real workflow I do at work, in two versions: a web app and a native Android app.

**[[Live Demo](https://veriteigiraneza.github.io/Case_Cart_Web/)](#)** · **[Download APK Not Available](#)**

---

## The problem

In hospital surgical supply, each case cart is labeled with a 4-digit number. The boxes that need to be loaded onto those carts arrive on separate source carts, each box also carrying a 4-digit number. Someone has to hold thirty or so numbers in their head, read each box, and figure out which cart it goes to.

The existing method is a paper list and memory. It is slow, it is easy to lose your place when you get interrupted, and a misplaced box means a cart goes to the OR incomplete.

CaseCart replaces the paper list. You enter the cart numbers once, then call out box numbers as you go — the app tells you the cart position instantly and tracks what is left.

## What it does

- **Setup** — enter the 4-digit number for each cart. Entries are grouped into "cuts" (`A1`, `A2`, … `B3`, `B4`) matching how carts are physically staged.
- **Matching** — enter a box number and get an immediate answer: which cart position it belongs to, a warning if that cart is already filled, or a clear "no matching cart."
- **Two-stage completion** — matched carts turn green; pressing **Finish** batches them to gray. The next round of matches stands out against the ones already handled, which is what makes the tool usable across a shift rather than a single pass.
- **History** — every session auto-saves and can be reopened mid-task, because this work gets interrupted constantly.

## Built with

**Android** — Kotlin, Jetpack Compose, Material 3, SharedPreferences for persistence.

**Web** — a single self-contained HTML file: vanilla JS, no framework, no build step, no dependencies. Deployed on GitHub Pages.

## Engineering decisions

**No backend.** The data is a few dozen 4-digit numbers that matter for one shift. A server would have added authentication, network dependency, and a hospital IT review for no benefit. Both versions persist locally — Android via SharedPreferences, web via `localStorage`.

**Single-file web app.** The web version is one HTML file with everything inlined. It loads on a locked-down hospital browser, works offline, and can be handed to a coworker as a link with nothing to install. That constraint shaped the whole architecture: hand-rolled screen routing, direct DOM rendering, a single mutable state object.

**Positions are derived, not stored.** Cart position is computed from list order on load rather than persisted. Removing a cart mid-setup renumbers everything below it automatically, with no chance of the stored index drifting from reality.

**Two versions, one model.** The web app is a port of the Android app and shares its data shape and session format. Writing the same feature twice against different UI frameworks made the state model considerably clearer than the first version was.

**Built from the inside.** I work in hospital supply chain, so the requirements came from doing the task rather than from a spec. The "Finish" flow above exists because the first version was unusable after the first pass — everything was green and nothing stood out.

## Running it

**Web** — open `index.html`, or visit the demo link. Nothing to install.

**Android** — open in Android Studio and run, or install the signed APK from Releases.

## What's next

- Barcode scanning via camera, to remove manual entry entirely
- Session export for shift handoff
- Resolving a stubborn tap-highlight rendering bug on Android Chrome

---

Built by [Verite](#) · [Portfolio](#) · [LinkedIn](#)