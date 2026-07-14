# Demand-Selection Task — Habits Suite

A playable, single-file implementation of the cognitive **demand-selection task**
(Kool, McGuire, Rosen & Botvinick, 2010). Students freely choose between two
visually near-identical decks; one covertly demands more cognitive effort (more
task-switching). People reliably drift to the low-demand deck — usually without
knowing why. The tool then reveals each student's own **demand-avoidance index**
and aggregates the class.

## Run it

Everything is in **`index.html`** — no build step, no dependencies. Open the file
directly in a browser, or serve it via **GitHub Pages** (static hosting is all it
needs).

## How it works

- **Practice** (6 trials) teaches the two judgments: **parity** (odd/even, blue
  digit) and **magnitude** (`<5` / `>5`, orange digit). Respond with **F / J** or
  the on-screen buttons.
- **Task block** (80 free-choice trials): each trial starts at a centred **"Show
  the decks" gate**, then two decks appear and you pick one; the pick runs one digit
  micro-task and returns to the gate. The chosen deck secretly sets the probability
  that the judgment rule *switches* from the previous trial — **0.10** for the easy
  deck, **0.90** for the hard deck. Which visual patch is hard is randomised per
  participant (and re-randomised on "Play again") so the effect can't be a
  side-preference. No difficulty cues anywhere.
- **Why the gate + position shuffle:** together they stop a participant from
  "choosing" by parking the cursor and auto-firing clicks. The interactive target
  alternates centre (gate) → left/right (deck), so no single fixed coordinate can
  advance two trials in a row — a deliberate action is required each round — and the
  decks swap sides so location is never a stable stand-in for identity. Mindless
  repetition is therefore both effortful and unrewarding (it yields random decks,
  not a comfortable easy one). No client-side measure can *force* genuine
  engagement, but this removes the effortless lazy path.
- **Personal reveal**: hero % ("you chose the easier deck X% of the time"), the
  hidden mapping, the corroborating RT/error gap, and the student's own
  awareness answer.
- **Class aggregate**: live histogram of the demand-avoidance index with the class
  mean and 50% reference line.

## Delivery modes

Top-right toggle, persisted in `localStorage`:

- **● Solo** — the private play experience (choice must be private, or the free
  choice is contaminated).
- **▣ Project** — dark instrument variant for the projector: the aggregate console,
  large histogram, high-contrast accents legible from the back of a room.

## Class aggregation

- **Path B (default, no backend):** a clearly-labelled seeded demo class (~n=40,
  skewed toward avoidance) makes the aggregate teachable with zero setup, plus an
  "add my result" button and a manual instructor entry for called-out percentages.
  A **"Show illustrative demo class" checkbox** clears the seeded data from the
  histogram in one click (leaving only real results), and "Clear added results"
  removes the tally you've built up.
- **Path A (optional Firebase live mode):** paste a Firebase web config into the
  `FIREBASE_CONFIG` constant near the bottom of `index.html`. When present, the
  tool creates a session with a short **join code** + shareable URL, and student
  results stream into a live histogram. When absent — or if Firebase fails to load —
  it degrades gracefully to Path B. No external JS loads unless a config is set.

## Accessibility

Keyboard-operable throughout, visible `:focus-visible` rings, `prefers-reduced-motion`
honoured, WCAG-AA contrast. Google Fonts load via `<link>` with a full system
fallback stack, so the tool stays legible if the CDN is blocked.
