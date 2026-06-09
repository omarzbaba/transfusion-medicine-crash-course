# Transfusion Medicine — One-Day Crash Course

An immersive, single-page educational **dashboard** that teaches the three pillars of transfusion medicine to medical students on a one-day rotation:

1. **Blood components & blood bank**
2. **Coagulation testing & interpretation**
3. **Apheresis medicine**

Built for **Chloe & Matt**. It’s a working teaching tool — interactive cases, a scored quiz center, a searchable glossary, pattern-recognition cards, and a clean PT/aPTT algorithm — not a static study guide.

> **Live site:** see the **GitHub Pages** URL in the repository’s **About** panel (or the deploy step below).

---

## Highlights

- **Three pillar sections** with clinical-language-first teaching cards (Why it matters · Clinical pattern · What to do next · Student pearl · Pitfall · Do not confuse).
- **Quiz Center** — 28 questions (easy → integrative), filterable by pillar, with instant feedback: correct answer, why each distractor is wrong, and a teaching pearl. In-session scoring with a live progress ring.
- **Case Lab** — progressive-disclosure cases (trauma bay, the pre-op panel that won’t make sense, the 2 a.m. TTP consult, and integrated cross-pillar cases).
- **Searchable glossary / cheat sheet** — 39 beginner-friendly terms; the must-know set is flagged.
- **Final recap** — 15 take-home points, “how to sound smart on rounds,” and good questions to ask on service.
- **Light & dark themes** (true-black dark, clean-white light), scroll-spy navigation, a reading-progress bar, and a restrained ambient background — a near-monochrome, Apple-grade aesthetic.
- **Accessible**: semantic HTML, keyboard navigable, `prefers-reduced-motion` respected, focus-visible states.

---

## Run it locally

It’s a single static file — no build step.

```bash
# Option 1: just open it
open index.html            # macOS

# Option 2: serve it (recommended; fonts/canvas behave best over http)
python3 -m http.server 8123
# then visit http://localhost:8123
```

---

## File structure

```
.
├── index.html      # the entire app — HTML, CSS, and JS in one file
├── PLANNING.md     # Phase 1: learner profile, objectives, content map, IA, blueprint
├── README.md       # this file (usage + implementation notes)
├── .gitignore
└── .claude/
    └── launch.json # local static-server config for previewing
```

Everything lives in **`index.html`**, organized top-to-bottom as:

1. `<style>` — design tokens (light/dark), layout shell, and every component style.
2. `<body>` — the app shell (sidebar, header, main) and the long-form teaching content as semantic HTML.
3. `<script>` — the data (questions, cases, glossary) and the engine (quiz, cases, glossary search, theme, scroll-spy, canvas).

---

## Libraries used

- **None at all.** All interactivity is vanilla JavaScript; every visual is pure CSS. No framework, no bundler, no runtime dependencies.
- **No web fonts.** It uses the native system UI stack (SF Pro on Apple devices, Segoe/Helvetica elsewhere) for an Apple-grade, dependency-free, instant-loading feel — nothing to download.

This keeps it fast, portable, and trivially hostable on GitHub Pages.

---

## How the animations work

- **Reveal-on-scroll:** elements with the `.reveal` class start at `opacity:0` and translate up 12px. An `IntersectionObserver` (rooted on the scrolling `<main>`) adds `.in` to fade them in once, then unobserves. Disabled under `prefers-reduced-motion`.
- **Ambient background:** a single, very soft pair of radial washes on a fixed `body::before` layer (`opacity` ~0.05) that drifts a few pixels over 26s, plus a barely-visible SVG grain overlay for editorial texture. No particles, no canvas — quiet depth that never competes with the text. The drift is paused under reduced-motion.
- **UI motion:** CSS transitions only, on compositor-friendly properties (`opacity`, `transform`), ~200–400 ms with an ease-out curve. Buttons respond with a subtle `scale(.98)` on press; cards lift a few pixels on hover.

---

## How to customize the content later

The repetitive teaching content is **data-driven** — edit plain arrays near the top of the `<script>` in `index.html`:

| What | Where | Shape |
|---|---|---|
| Quiz questions | `const QUESTIONS = [ … ]` | `{ id, pillar:'blood'\|'coag'\|'aph', diff:'easy'\|'moderate'\|'integrative', stem, opts:[…], answer:<index>, explain, wrong, pearl }` |
| Inline checkpoints | `const CHECKPOINTS = [ … ]` | same shape as a question |
| Cases | `const CASES = [ … ]` | `{ slot:'blood'\|'coag'\|'aph'\|'lab', tagClass, tagLabel, title, blurb, labs?:[{k,v,s}], steps:[{label, prompt, reveal}] }` |
| Glossary | `const GLOSSARY = [ … ]` | `{ term, abbr?, def, must?:true }` |

Add an object to an array and it renders automatically — the hero stat counters update too.

**Long-form teaching cards** (products, reactions, the coagulation framework, apheresis modalities) are semantic HTML inside each `<section>`; edit the markup directly. The labeled callouts use `class="callout callout--why|pattern|next|pearl|adv|pitfall|dnc"`.

**Rename the learners:** search `index.html` for `Chloe` and `Matt` (sidebar chip, hero welcome, recap) and the avatar initials `>C<` / `>M<` in the sidebar.

**Re-theme:** all colors live in the `:root` / `[data-theme]` blocks at the top of `<style>` (the pillar hues are `--blood`, `--coag`, `--aph`; the chrome accent is `--accent`).

---

## Deploy (GitHub Pages)

This repo is configured to publish from the `main` branch root. After pushing:

1. **Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `root`.**
2. Wait ~1 minute; the live URL appears at the top of the Pages settings and in the repo **About** panel.

---

## A note on accuracy

The content is an educational synthesis for medical-student teaching. It is **not** a clinical protocol and contains **no fabricated citations**. For authoritative detail, consult the source bodies named in [`PLANNING.md`](PLANNING.md) (AABB, ASFA, ASH, ISTH, BSH) and your institution’s own policies before acting on a patient.
