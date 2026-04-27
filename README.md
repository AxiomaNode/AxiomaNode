# Axioma

**Diagnostic platform for reasoning gaps in algebra.**

Axioma doesn't grade students. It finds exactly where their mathematical thinking breaks down — between understanding and formula, between formula and graph, between analysis and method choice — and builds a targeted path to fix it.

Built by a solo developer. 30+ users. 4.8 rating.

---

## The Problem

School math evaluates results: right or wrong. A student who misreads the discriminant and a student who applies the wrong factoring pattern both get the same red mark. Their feedback is identical. Their problems are completely different.

This creates a cycle:
- Students memorize formulas without understanding why they work
- Gaps in reasoning accumulate silently
- Accelerated tracks (A/B/C classes) widen the gap further
- The student who "almost gets it" and the student who's fundamentally lost receive the same remediation

## The Hypothesis

Most errors in quadratic equations come not from computation, but from **breaks between stages of reasoning**:

- Between meaning and formula
- Between formula and graph
- Between analyzing a problem and choosing a method

If you identify the specific break — not grade the student with a number — learning becomes more effective and psychologically safer.

**Research basis:** Preliminary study with ~50 students found that error patterns cluster around reasoning transitions, not computational difficulty. Students who scored similarly on traditional tests showed distinctly different reasoning profiles when analyzed by gap type.

## How It Works

### The Reasoning Chain

Solving a quadratic equation requires passing through these stages:

```
Interpretation → Representation → Analysis → Execution → Verification
```

1. **Interpretation** — Understanding that an equation describes a relationship, not just an expression
2. **Representation** — Moving between formula, graph, and verbal description
3. **Analysis** — Reasoning about the number of roots and existence conditions before computing
4. **Execution** — Correctly applying algorithms (formulas, transformations)
5. **Verification** — Evaluating whether the answer is reasonable and connects to the original problem

A "gap" is a break between any two adjacent stages. Axioma detects which transition fails.

### Five Gap Types

| Gap | What breaks | Example behavior |
|---|---|---|
| **Discriminant interpretation** | Reads D's sign but draws wrong conclusion about solutions | "D > 0 means both roots are positive" |
| **Double root recognition** | Finds one square root, forgets the other | Writes x = 3 for x² = 9, misses x = −3 |
| **Division by variable** | Divides both sides by x, silently loses x = 0 | Gets one root instead of two |
| **Factoring pattern confusion** | Applies difference of squares where perfect square is needed | Factors x²−6x+9 as (x−3)(x+3) |
| **Vieta's formula misapplication** | Uses b instead of −b for root sum | Flips the sign on every Vieta calculation |

### The Learning Loop

```
Diagnostic → Theory → Practice → Re-diagnose → Mastery Test
```

**Diagnostic** (once per day, ~10 min): 20 questions across all 5 gap types. Each gap gets 4 questions at 3 difficulty layers (direct → applied → transfer). A weighted scoring model with a transfer gate determines whether a gap is detected — transfer questions matter most because they reveal real reasoning, not memorization.

**Theory**: Step-by-step course with analogies, worked examples, formula cards, and section checks. Gap-specific recommendations send students to the exact sections that address their detected break.

**Practice**: 24 questions per session, targeted to detected gaps. Layered difficulty (4 direct, 8 applied, 10 transfer, 2 mixed). Session persistence with auto-save.

**Re-diagnose**: Run another diagnostic the next day. If the gap wasn't detected in 2 consecutive diagnostics, it's marked as closed. If it reappears, it reopens.

**Mastery Test**: Unlocks when all gaps for a topic are closed. 15 questions in formats the student hasn't seen before. Score ≥75% earns a mastery card (Proficient → Advanced → Expert → Flawless) saved to the student's public profile.

### Gap Detection Model

Each diagnostic question has a difficulty weight:

| Layer | Weight | Purpose |
|---|---|---|
| Direct | 1 | Can the student compute/recall? |
| Applied | 2 | Can the student evaluate and choose? |
| Transfer | 3 | Can the student reason in unfamiliar framing? |

A gap is **detected** when:
1. Weighted correct score < 70% of weighted total (with margin at scale)
2. AND the student fails the transfer condition (must pass ≥1 transfer question per gap)

A gap is **clean** only when both conditions pass. There is no uncertain state.

## Tech Stack

- **Frontend**: React (Vite)
- **Backend**: Firebase (Auth, Firestore, Hosting)
- **Scoring**: Custom diagnostic engine with layered difficulty model
- **State**: Firestore-persisted sessions, gap status tracking, XP/leveling

## Project Structure

```
src/
├── core/
│   ├── diagnosticEngine.js    # Gap detection + session builder
│   ├── masteryEngine.js       # Mastery gate, card logic, daily limits
│   └── scoringEngine.js       # XP, leveling, point awards
├── data/
│   ├── gaps.js                # Gap definitions per topic
│   ├── coreGaps.js            # Core gap taxonomy
│   ├── topics.js              # Topic registry
│   ├── theory.js              # Theory course content
│   └── templates/
│       ├── templateUtils.js   # Session generators, helpers
│       └── quadratic/
│           ├── quadratic.diag.js      # 60 diagnostic templates
│           ├── quadratic.practice.js  # ~120 practice templates
│           └── quadratic.mastery.js   # 14 mastery templates
├── services/
│   └── db.js                  # Firestore operations, gap status tracking
├── pages/
│   ├── diagnostics/           # Diagnostic flow (intro → questions → results)
│   ├── theory/                # Step-by-step theory reader
│   ├── practice/              # Targeted + free practice sessions
│   └── mastery/               # Mastery test + card system
└── hooks/
    └── useOnboarding.js       # First-run guided experience
```

## Running Locally

```bash
npm install
npm run dev
```

Requires a Firebase project with Firestore and Auth enabled. Create `src/firebase/firebaseConfig.js` with your project credentials.

## Adding New Topics

Axioma is designed to expand beyond quadratic equations. A complete guide for adding topics is included in `docs/adding_topics_guide.md`. The short version:

1. Define the topic in `topics.js`
2. Define reasoning gaps in `gaps.js` (3–6 gaps per topic)
3. Create diagnostic templates (≥10 per gap: 2 direct + 4 applied + 4 transfer)
4. Create practice templates (≥20 per gap)
5. Register in the template index
6. Add theory content

The diagnostic engine and gap tracking are fully dynamic — no engine code changes needed when adding topics.

## Current Status

**What works:**
- Full diagnostic → theory → practice → mastery loop
- Gap detection across 5 reasoning types with weighted scoring
- Daily diagnostic limits with spaced repetition logic
- Session persistence (resume where you left off)
- Mastery card system with public profiles
- First-run onboarding (Focus & Reveal spotlight model)

**What's next:**
- Russian/Uzbek translation (prerequisite for target audience)
- Linear equations topic (gap definitions complete, templates in progress)
- Template expansion via structured content pipeline
- Teacher dashboard for class-level gap analytics

## Research Context

This project started with a question: *why do students who "know the formula" still fail on non-standard problems?*

Preliminary research with ~50 students showed that errors cluster not around difficulty level, but around specific **transitions in reasoning** — the jump from reading a problem to choosing a method, from knowing a formula to interpreting its output, from finding a numerical answer to evaluating whether it makes sense.

Axioma tests this hypothesis by replacing score-based assessment with gap-based profiling. Early results (30+ users, 4.8/5 rating) suggest that students engage more willingly when the system identifies specific reasoning breaks rather than assigning a grade.

The long-term goal is to validate whether gap-targeted remediation produces measurably better learning outcomes than traditional score-based feedback.

## License

MIT
