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
- **Path A (Firebase live mode):** the `firebaseConfig` at the top of the
  `<script>` in `index.html` is set to the project's real config, so the tool runs
  **LIVE** out of the box. Detection is automatic from `firebaseConfig.apiKey`:
  a real key → **LIVE**; a placeholder (contains `…`) or empty → **OFFLINE**
  (seeded demo). To force offline, set `apiKey` to `"…"`. The aggregate screen
  shows a small badge — `● live session` vs `○ demo data`.
  - **Students** get a 4-character **session code** field on the intro screen
    (prefilled from a `?code=CODE` URL). On finishing, their run is submitted once
    via `submitResult(code, avoidance, rtDelta, errDelta)` — `avoidance` is a
    `0..1` fraction, written to `sessions/<CODE>/results`.
  - **Instructors** open **"Instructor / show session"**, generate or type a code,
    and project it large with the shareable `?code=` link; the console calls
    `listenToSession(code, …)` and the histogram grows as students finish.
  - Init is lazy + dynamic-imported and every Firebase call is `try/catch`-wrapped;
    any error falls back to OFFLINE and `console.warn`s rather than blocking the UI.
    Nothing loads while OFFLINE, so a placeholder config runs fully on seeded data
    with no console errors.

  **To test live:** open the tool in two tabs. In tab 1, enter code `TEST` on the
  intro and complete a run. In tab 2, click **Instructor / show session**, type
  `TEST`, and a bar appears within ~1s; further runs add/grow bars live.

## Accessibility

Keyboard-operable throughout, visible `:focus-visible` rings, `prefers-reduced-motion`
honoured, WCAG-AA contrast. Google Fonts load via `<link>` with a full system
fallback stack, so the tool stays legible if the CDN is blocked.
