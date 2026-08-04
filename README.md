# Alu Niu ✦

An interactive, memory-unlocked journey map. Instead of a static timeline,
personal milestones are **locked behind trivia** and revealed one at a time —
answer correctly and a new icon drops onto the map, a memory card unfolds, and
a glowing line draws itself forward to the next locked destination.

> A gamified, narrative-driven experience where personal history is unlocked
> like a map in an adventure game.

The journey spans **15 memories, split into 3 chapters of 5**. Each chapter is
locked behind a **code** — you enter a code to begin, and after every 5
memories the next chapter locks again until you enter its code.

## Features

| Component | What it does |
|---|---|
| **Chapter Code Gates** | The 15 milestones are grouped into 3 chapters of 5. A code must be entered to open each chapter — including one at the very start — and the next chapter re-locks after every 5th memory until its code is entered. |
| **Gated Logic Engine** | Within an open chapter, milestones must be solved in strict order. Step _N_ stays hidden/locked until step _N-1_ is solved (`state = unlocked when input == correct`). |
| **Interactive Canvas / Map** | A stylized SVG journey map. Nodes glow and change color — gray/locked → gold/active → mint/unlocked — and spawn a title + date label on success. |
| **Path Interpolation** | An animated line "walks" from one milestone to the next `(x₁,y₁) → (x₂,y₂)` as progress continues. |
| **Feedback Overlay** | Instant correct/incorrect banners, an input shake on a wrong guess, a gentle hint, and a confetti burst on every unlock (with a big finale). |

Both answers and chapter codes are matched case-insensitively, ignoring
punctuation/accents, so near-misses still count. Progress (solved milestones
and unlocked chapters) is saved in the browser (`localStorage`), so the journey
resumes where you left off.

## Chapter codes (demo)

The sample content ships with three codes — swap them in `data.js`:

| Chapter | Code |
|---|---|
| I — First Light (memories 1–5) | `BEGIN` |
| II — Adventures (memories 6–10) | `JOURNEY` |
| III — Forever (memories 11–15) | `FOREVER` |

## Run it

No build step and no dependencies. Just open the file:

```bash
# from the project root
open index.html            # macOS
xdg-open index.html        # Linux
# …or serve it (avoids any browser file:// quirks):
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Make it your own

Everything you'd want to change lives in **`data.js`**.

**Chapters & codes** — `CHAPTERS` holds one entry per group of `CHAPTER_SIZE`
milestones (default 5):

```js
window.CHAPTER_SIZE = 5;
window.CHAPTERS = [
  { name: "Chapter I — First Light", code: "BEGIN", intro: "Enter the opening code…" },
  // …one entry per chapter. A chapter with an empty code opens automatically.
];
```

**Milestones** — each entry in `JOURNEY`:

```js
{
  id: "meet",
  x: 10, y: 18,          // position on the map, as % of the canvas (0–100)
  icon: "☕",             // emoji shown when unlocked
  title: "Where It All Began",
  date: "Autumn, 2015",
  photo: "☕",            // emoji, OR an image URL / path (e.g. "img/cafe.jpg")
  blurb: "A rainy afternoon, two strangers…",
  question: "In which cozy place did we first meet?",
  hint: "You order a flat white there.",       // shown after a wrong guess
  answers: ["cafe", "coffee shop", "the cafe"] // any of these count as correct
}
```

Milestones unlock in array order, and the map path is drawn between
consecutive entries automatically. Every `CHAPTER_SIZE` milestones map to the
next `CHAPTERS` entry (entries 0–4 → chapter 0, 5–9 → chapter 1, …). Add,
remove, or reorder freely — just keep `CHAPTERS` long enough to cover them.

## Files

- `index.html` — markup and layout
- `styles.css` — theme, node/path animations, responsive layout
- `data.js` — **your memories** (the only file you normally edit)
- `app.js` — gated logic engine, SVG rendering, path animation, confetti

## Tech

Vanilla HTML/CSS/JS. SVG for the map and animated paths, a `<canvas>` for
confetti, CSS transitions for node state changes. Works fully offline.
