# Customised Socratic Chatbot for MATH1205 — Set Theory Pilot

**Deliverable:** Report + working mockup chatbot
**Pilot unit:** Chapter 2 — Set Theory
**Target page:** `https://gorgeousregina.simonsays.hk/courses/math1205/chatbotMockup.html`

---

## 1. Executive summary

We fetched the shared Google AI Mode (Gemini) conversation with a headless Chrome
browser, analysed how the student and Gemini interacted, and used those findings to
design a **customised Socratic chatbot** for MATH1205 Discrete Mathematics.

The pilot is **Chapter 2 — Set Theory**, the exact chapter the student was previewing
in the shared conversation. The mockup (`chatbotMockup.html`) is a self-contained page
that:

- calls the **Poe API** to power a real LLM tutor,
- falls back to a **local rule-based Socratic tutor** when the network/API key is
  unavailable (so the demo always works),
- adds **learning analytics** (per-concept mastery, XP, streak, level, weak-spot
  review) and **gamification** (badges, progress bar, confetti) that Gemini AI Mode
  does not offer,
- renders maths cleanly and provides quick-reply chips for a smoother UX.

---

## 2. Source conversation analysis (Gemini AI Mode)

The shared link (`share.google/aimode/GoM14STJI3R54mGbe`) is a Google **AI Mode**
conversation. We rendered it with headless Chrome and extracted the full transcript.
The student shared the Set Theory chapter (`converted - Ch2.set.txt`) and asked Gemini
to act as a Socratic tutor before attending class.

### 2.1 What the student did (interaction patterns)

| # | Student behaviour | Example from transcript |
|---|-------------------|-------------------------|
| 1 | Shared a Google Drive link expecting the AI to read it | `review this and help me preview…` |
| 2 | Explicitly requested a Socratic, one-question-at-a-time flow | `ask me one question at a time to test my prior knowledge` |
| 3 | Asked for lecture focus suggestions | `suggest which concepts or points to focus on when I attend the lecture` |
| 4 | Gave terse answers | `1.F 2.T`, `1.F 2.F` |
| 5 | Asked for clarification of symbols | `could you explain the two symbols between elements and set` |
| 6 | Asked deep, conceptual questions | `should we assume that a set should contain elements of equal nature…` |
| 7 | Fell into the "subset trap" and was corrected | answered `{3,4} ⊆ S` as True; Gemini explained it is False |

### 2.2 How Gemini responded (interactive styles)

1. **Socratic scaffolding** — one question at a time, difficulty escalating
   (cardinality → `∈` vs `⊆` → subsets of nested sets).
2. **Analogy-driven teaching** — the "cardboard box" and "paper bag" analogies for
   sets containing sets.
3. **Praise-then-correct** — "Spot on!", "You nailed it", followed by gentle
   correction of the trap.
4. **Lecture focus areas up front** — empty set vs `{∅}`, De Morgan's laws & set
   identity proofs, equivalence relations.
5. **Explicit next-step offers** — "Move on to Power Sets or Cartesian Products next?"
6. **Emoji for warmth** — 🌟, 🧠, 💡, 📦.

### 2.3 Limitations & pain points observed

| # | Limitation | Consequence |
|---|------------|-------------|
| 1 | **Cannot open Google Drive files** | Student had to paste the whole chapter manually |
| 2 | **No learning analytics** | No tracking of which concepts the student struggles with, no mastery signal |
| 3 | **No gamification** | No XP, levels, streaks, or badges to sustain motivation |
| 4 | **Clunky maths rendering** | AI Mode rendered `S = {1,2,{3,4}}` as a verbose spoken string |
| 5 | **Ad-hoc curriculum** | Tutor improvised; no structured lesson plan or spaced repetition |
| 6 | **No visual aids** | No Venn diagrams, no interactive set widgets |
| 7 | **No persistence** | Progress lost between sessions; no per-student history |

---

## 3. Design proposal: the customised chatbot

### 3.1 Architecture

```
┌────────────────────────────────────────────────────────────┐
│  chatbotMockup.html  (single self-contained page)          │
│                                                            │
│  UI layer   → chat, quick-reply chips, mastery panel,      │
│               badges, progress bar, confetti               │
│  Tutor core → Socratic state machine (concept → question   │
│               → evaluate → next)                           │
│  LLM layer  → Poe API (streaming)  ──fails──▶ local        │
│               rule-based Socratic tutor (fallback)         │
│  Analytics  → per-concept mastery, XP, streak, level,      │
│               weak-spot queue (localStorage)               │
└────────────────────────────────────────────────────────────┘
```

- **Poe API** (`https://api.poe.com/v2/chat`, `Authorization: Bearer <key>`) powers
  free-form answers with a **system prompt** that enforces Socratic behaviour and
  grounds the tutor in the Set Theory chapter.
- A **local rule-based tutor** mirrors the same Socratic flow when the API is
  unreachable, so the mockup is always demonstrable.

### 3.2 Socratic tutoring engine

- **One question at a time**, escalating difficulty (as the student requested).
- **Lecture focus areas** surfaced first (empty set vs `{∅}`, De Morgan's laws &
  set-identity proofs, equivalence relations).
- **Analogy + praise-then-correct** response style, matching what worked in the
  original conversation.
- **Quick-reply chips** for common intents: *Start lesson*, *Test me*, *Explain ∈ vs ⊆*,
  *Give me an example*, *Power sets*, *Review my weak spots*.

### 3.3 Learning analytics

| Metric | What it measures | How it helps |
|--------|------------------|--------------|
| **Concept mastery** | % correct per concept (Sets & Elements, Subsets, Cardinality, Power Sets, Operations, Functions, Relations) | Shows what to revise |
| **XP / Level** | Points earned per correct answer | Progress & motivation |
| **Streak** | Consecutive correct answers | Encourages consistency |
| **Weak-spot queue** | Concepts with low mastery | Drives "Review my weak spots" |
| **Session log** | Every Q&A stored in `localStorage` | Persistence & teacher insight |

### 3.4 Gamification

- **XP, levels, and a 🔥 streak counter** in the header.
- **Badges** (e.g. *First Spark*, *Subset Slayer*, *Power Set Pro*, *Master of Sets*).
- **Progress bar** toward module completion.
- **Confetti** on mastery milestones.

### 3.5 Visual UI

- Reuses the existing MATH1205 design language (violet gradient, cards, rounded
  corners) for consistency with the course site.
- **Clean maths rendering** (KaTeX) instead of Gemini's verbose spoken notation.
- **Mastery panel** with per-concept bars and a **badge shelf**.
- **Responsive** layout for desktop and mobile.

---

## 4. Pilot: Set Theory (Chapter 2)

Scope drawn from `converted/Ch2.set.txt` and the existing `lesson/sets.html`:

1. **Sets & Elements** — definition, roster / set-builder, common sets (ℕ, ℤ, ℚ, ℝ), ∅ vs `{∅}`.
2. **Subsets** — `⊆`, `⊂`, `∅ ⊆ S`, the "unpack the box" subset trick.
3. **Cardinality** — `|S|`, duplicates don't count.
4. **Power Sets** — `P(S)`, `|P(S)| = 2^|S|`.
5. **Set Operations** — `∪`, `∩`, `−`, `⊕`, complement, inclusion–exclusion, De Morgan's laws.
6. **Functions** — domain/codomain/range, injective/surjective/bijective, inverse, composition.
7. **Relations** — reflexive/symmetric/transitive, equivalence relations & partitions.

The mockup's Socratic flow mirrors the original conversation: open with lecture focus
areas, then ask one escalating question at a time, using the same analogies and
trap-warnings that proved effective.

---

## 5. Deployment

| File | Purpose |
|------|---------|
| `courses/math1205/chatbotMockup.html` | The mockup chatbot (Poe API + fallback) |
| `courses/math1205/report.html` | This report, rendered for the web |
| `courses/math1205/report.md` | This report, Markdown source |

**To deploy:** push these files to the GitHub Pages repo that serves
`gorgeousregina.simonsays.hk`. The page will then be live at
`https://gorgeousregina.simonsays.hk/courses/math1205/chatbotMockup.html`.

> **Note:** The Poe API key is embedded in the page for this mockup. For production,
> move the key to a server-side proxy so it is never exposed in the browser.

---

## 6. Roadmap

1. **Pilot validation** — run the Set Theory pilot with a small group; tune the
   Socratic prompts and analytics.
2. **Roll out to other chapters** — Logic, Combinatorics, Modular Arithmetic,
   Cryptography, Boolean Algebra (materials already converted).
3. **Server-side proxy** — secure the Poe key and add per-student accounts.
4. **Spaced repetition** — schedule weak concepts for review on later days.
5. **Teacher dashboard** — aggregate analytics across students to spot common
   misconceptions.
6. **File upload** — let students drop PDFs/text directly (fixes the #1 pain point
   from the original conversation).