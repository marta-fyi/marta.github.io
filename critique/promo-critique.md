# Production Hub promo — motion critique

> **Status:** approved and fixed. Sections 1–6 are the original critique (evidence now under `before/`). Section 7 is the re-run after the fixes.

Branch `promo-critique`. Evidence captured 6 Oct 2026 against `http://127.0.0.1:8000/work/production-hub/`, desktop 1440×900 unless stated, element screenshots at deviceScaleFactor 2.

## 1. Verdict

The final frames tell the story well, the order is easy to recognise, and the engine is technically clean: no dropped frames, no long tasks, all motion done with transform and opacity. The transitions between stages break the piece: every hand-off morphs one card into another by scaling it, so for 0.3–0.6 s at a time text balloons to 1.9× or shrinks to 0.5×, and two semi-transparent cards stack on top of each other (15 double-exposure pairs, 66 of 603 frames where the order isn't clearly visible). Add a 30 s loop that starts mid-story, never stops, and moves against the rail's left-to-right direction, and the result pulls more attention than a case-study sidebar should while explaining less than its stills.

## 2. Scorecard

Scores: 5 excellent · 4 good · 3 acceptable · 2 poor · 1 broken. Evidence links are relative to this folder.

| # | Check | Result | Score | Evidence |
|---|---|---|---|---|
| 3.1 | Simultaneity (≤ 2 primary movers) | **Fail** — up to 5 at once; 2850–3200, 8550–8850, 10450–10950, 15750–16150, 22950–23250 ms | 2 | [checks.json › simultaneity](before/checks.json), [A2a-split](before/sheets/A2a-split.png) |
| 3.2 | Double exposure | **Fail** — 15 card pairs, e.g. `up+order` 2850–3100, `p1+p2+p3+tok` 10350–10600, `done+p3` 15750–16400, `track+done+ctok` 22950–23300 | 1 | [T1](before/sheets/T1-accounts-to-production.png), [T2](before/sheets/T2-production-to-operations.png), [T3](before/sheets/T3-operations-to-shipping.png) |
| 3.3 | Distortion | **Fail** — no non-uniform scale (pass), but text cards scaled 0.52–1.90 in 9 elements | 1 | [checks.json › distortion](before/checks.json), [A2a-split](before/sheets/A2a-split.png) |
| 3.4 | Continuity (#39208 + bar/art at ≥ 0.9) | **Fail** — 66/603 frames: 0–300, 1800–1850, 2850–3050, 3700, 10400, 16300–18100, 22950–23050, 29650–30100 ms | 2 | [checks.json › continuity](before/checks.json) |
| 3.5 | Direction (in from right, out to left; toasts from one slot) | **Fail** — 10 entries rise from below or scale in place, 9 exits go up/right/in place; toast slots differ by 28 px (desktop) and 58 px (mobile) | 1 | [checks.json › direction](before/checks.json) |
| 3.6 | Transform origin of popovers | **Fail** — Task actions scales about its own centre (`145px 131px`); no control opens it | 2 | [A3](before/sheets/A3-mark-done.png) |
| 3.7 | Timing tokens | **Fail** — entrances 400–1067 ms, exits ≈ entrances (median 400 = 400), the enter ease used for exits, travel not scaled to distance (48 px → 200 ms, 186 px → 667 ms, 384 px → 1200 ms) | 2 | [checks.json › timing](before/checks.json), [timeline.json](before/timeline.json) |
| 3.8 | Holds (≥ 1.5 s, last ≥ 2 s) | **Pass with caveat** — content still for 2.25 / 2.35 / 2.50 / 2.55 s, but the rail never stops, so no frame is fully still | 3 | [checks.json › holds](before/checks.json) |
| 3.9 | Rail sync with token travel | **Fail** — rail is a linear timer across each stage; the token moves on a different ease and schedule | 2 | [checks.json › railSync](before/checks.json) |
| 3.10 | Legibility (≥ 11 px) | **Fail** — 168 frames under 11 px; smallest 5.8 px ("Due 23/04/2024" at 15750 ms, mid-morph); the receded schedule renders 10.75 px for 4.5 s | 2 | [checks.json › legibility](before/checks.json) |
| 3.11a | Stage scaled with CSS transform | **Fail** — `.pm` has `matrix(0.9333…)` | 3 | `tools/content/production-hub.js:544` |
| 3.11b | box-shadow animated | **Fail (minor)** — rail node dots transition `box-shadow` | 4 | `tools/content/production-hub.css:52` |
| 3.11c | will-change left on | **Fail (minor)** — 4 captions keep `will-change: opacity` forever | 4 | `tools/content/production-hub.css:256` |
| 3.11d | Tabular numerals on changing numbers | **Pass** — total, due chips, tracker dates, time pill | 5 | [checks.json › rendering](before/checks.json) |
| 3.12a | Starts mid-stage when scrolled in | **Fail** — first view is Stage 1's final frame (upload opacity 0); the upload beat never plays first time | 1 | [checks.json › playback](before/checks.json) |
| 3.12b | Loops | **Fail vs standard** — loops every 30.1 s (stage log 0 → 1 → 2 → 3 → 0 → 1) | 2 | [checks.json › playback](before/checks.json) |
| 3.12c | Pause / Replay control | **Partial** — Pause/Play yes, no Replay | 3 | — |
| 3.12d | Reduced motion stops playback | **Pass** (desktop and mobile); shows Stage 1, standard prefers Stage 4 | 4 | [finals/reduced-*](before/finals/) |
| 3.12e | Rail nodes by keyboard | **Pass** — tab order Accounts → Shipping → Pause, Enter selects, 2 px focus ring | 5 | [checks.json › playback.keyboard](before/checks.json) |
| 3.13 | Performance | **Pass** — 3995 frames, p99 9.3 ms, 0 over 20 ms, 0 long tasks (headless Chromium, 120 Hz) | 5 | [perf-frame-deltas.json](before/perf-frame-deltas.json) |
| 5.1 | Story from final frames | **Partial** — 3 of 4 match; Stage 3 shows the new date but not that it changed | 3 | [finals](before/finals/) |
| 5.2 | Hierarchy in transitions | **Fail** — in every hand-off two cards compete, one ballooning, one ghosting | 2 | [T1](before/sheets/T1-accounts-to-production.png)–[T3](before/sheets/T3-operations-to-shipping.png) |
| 5.3 | Continuity by eye | **Partial** — easy in holds, lost in the split and both morphs | 3 | [A2a-split](before/sheets/A2a-split.png) |
| 5.4 | Rhythm | **Partial** — Stage 1 crams five beats into 7.3 s; Stage 2's middle idles; Stage 3 stacks menu, chip, phone and knob | 3 | [gantt](#current-timeline) |
| 5.5 | Polish | **Fail** — stretched text, captions cross-fading through each other, pill clipped by the stage edge for 0.7 s | 2 | [S1-upload](before/sheets/S1-upload.png), [T3](before/sheets/T3-operations-to-shipping.png) |
| 5.6 | Restraint next to long-form text | **Fail** — perpetual 30 s loop, constantly moving rail, sticky for 1.5 screens of prose | 2 | [video](before/video/promo-full-realtime.mp4) |

## 3. Issues by severity

File references are to `tools/content/production-hub.js` unless noted. Timestamps are real playback ms from the start of the loop; `t` is timeline seconds.

### P0 — looks broken or weird

**P0-1 · Cards morph into other cards by scaling, text balloons and shrinks**
- What: at every hand-off one card is scaled onto another's rectangle while cross-fading — order → token, token → three parts, part -03 → indigo card, indigo card → tracking card. Text renders between 0.52× and 1.90× its size.
- Where: order→tok 8550–9150 ms (`order`, `tok`, lines 409–424); split 10300–10950 ms (`p1`–`p3`, lines 426–430, scale up to 1.69); -03→done 15750–16400 ms (lines 429, 437, scale 0.52–1.90); done→track 22950–23550 ms (lines 437–439, 455).
- Evidence: [A2a-split](before/sheets/A2a-split.png) frames 10400–10800, [T2](before/sheets/T2-production-to-operations.png) frames 15817–16267, [T3](before/sheets/T3-operations-to-shipping.png) frames 23017–23217, [0.25× video](before/video/promo-transitions-0.25x.mp4).
- Why: it reads as a rendering glitch. Giant "#39208-03 Cutting" over a tiny indigo card is the most noticeable thing in the whole loop.
- Fix: remove all morphs (`from()`, line 368, is only used for this). One persistent token element — artwork (3:4), mint bar, #39208 — travels by translation only, scale within ±10 %. Cards exit and enter around it. Parts grow from the parent chip from scale 0.6 with origin at the chip, each a small card with no body text to distort.

**P0-2 · Double exposure: two semi-transparent cards in the same place**
- What: 15 pairs of overlapping cards both between 5 % and 95 % opacity, plus caption text cross-fading through the next caption.
- Where: `up+order` 2850–3100 ms (lines 397, 409); `quote+tok` 8550–8700; `p1/p2/p3+tok` 10350–10600; `p1/p2+menu` 16700–16950; `track+done+ctok` 22950–23300; captions at each stage start (line 468: out 0.3 s overlaps in 0.4 s).
- Evidence: [T1](before/sheets/T1-accounts-to-production.png) 8567–8717, [T3](before/sheets/T3-operations-to-shipping.png) 23017 ("Shipped and tracked" over "…der adapts"), [S1-upload](before/sheets/S1-upload.png) 2850–3050.
- Why: reads as two broken layers; the eye can't decide which card to read.
- Fix: strict sequence per transition — satellites out 0–180 ms, primary out left 150–400 ms, token travels 300–850 ms, next card in from the right 650–1100 ms. Captions: out 180 ms, then in 450 ms starting at 650 ms; never overlap.

### P1 — hurts understanding

**P1-1 · Starts mid-story on first view**
- What: when the visual first reaches 50 % in view it shows Stage 1's final frame and moves straight to Production; the upload beat is skipped first time.
- Where: line 536 (`u = uAt(FRAME_T[0])`) and line 572.
- Evidence: [checks.json › playback.firstView](before/checks.json) — `uploadOpacity: "0"`, `orderOpacity: "1"`.
- Fix: start at `u = 0` on the first 50 % intersection. Render the first frame before then.

**P1-2 · Loops forever; the rail never stops**
- What: 30.1 s loop, restarts indefinitely; the rail fill moves every frame, so nothing next to the prose is ever still.
- Where: line 557 (`% U_END`), `railFill` line 500.
- Evidence: stage log `[0,0] [1.81,1] [9.02,2] [16.21,3] [23.41,0] [31.94,1]`, [full video](before/video/promo-full-realtime.mp4).
- Why: perpetual motion beside long-form text competes with reading, and a loop makes it impossible to settle on the conclusion.
- Fix: play once and end on Stage 4's final frame. Replace Pause with Replay at the rail's right end once finished; keep Pause while playing.

**P1-3 · The order disappears or ghosts during transitions**
- What: #39208 plus its bar or artwork drops below 0.9 opacity in 66 frames.
- Where: see 3.4. Most are the morphs (P0-1). 16300–18100 ms is a technicality: `.done__id` is set at `opacity: .85` (`production-hub.css`, `.pm .done__id`).
- Fix: the persistent token (P0-1). Replace the `.done__id` opacity with `color: rgba(255,255,255,.85)`.

**P1-4 · Motion runs against the rail's direction**
- What: no card enters from the right or exits to the left. Entries rise from below (`up`, `order`, `menu`) or scale in place (`tok`, `done`, `track`); exits drift up (`up`), right (`quote` +24, `phone` +80) or nowhere (`track`, `ctok`).
- Where: lines 397, 409, 412, 437, 440, 445, 455.
- Evidence: [checks.json › direction](before/checks.json).
- Fix: incoming stage content enters from the right with 16 px drift; outgoing leaves left, 8–16 px. Toasts are the one exception (see P1-6).

**P1-5 · Too many things move at once**
- What: up to 5 primary movers in one frame (split: tok + p1 + p2 + p3 + sched).
- Evidence: [checks.json › simultaneity](before/checks.json).
- Fix: follows from P0-1/P0-2 — the transition sequence leaves at most the token plus one card moving.

**P1-6 · Toasts don't share a slot and move diagonally**
- What: approval toast at y 540, shipped toast at y 510 (desktop; mobile 498 vs 440). Both enter from (−24, +24); the shipped toast never exits.
- Where: lines 287/293 (desktop), 299/305 (mobile), 419, 464.
- Fix: one bottom-left slot for both; rise 12 px over 450 ms with the enter ease; exit 8 px down over 180 ms with the exit ease.

**P1-7 · Type gets too small**
- What: 168 frames under 11 px. Mid-morph text down to 5.8 px; the receded schedule (scale 0.96 at 18 % opacity) renders 12 px type at 10.75 px for all of Stage 3.
- Where: morphs (P0-1); recede at line 425.
- Fix: no text scaling (P0-1); replace the recede with a proper exit to the left.

**P1-8 · Rail isn't synced with the token**
- What: the rail is a linear timer; the token's moves use the enter ease on their own schedule.
- Fix: the rail holds at the current node, then moves to the next node in the same 550 ms window and with the same travel ease as the token.

**P1-9 · Stage 3 doesn't show the change**
- What: from the final frame you see a task marked done with "Due 22/04/2024", but not that the date moved. The explanation is in the phone thread, which needs reading.
- Evidence: [finals/desktop-stage3.png](before/finals/desktop-stage3.png).
- Fix: make the due-chip roll (old date out the top, new in from the bottom, 250 ms) a deliberate beat right after the Due date row highlights. Replace the phone with a single comment row under the card.

### P2 — polish

**P2-1 · Timing tokens off-standard**
- Entrances 600–1067 ms real (standard ≈ 450 ms); exits as long as entrances and on the enter ease (standard 250 ms, satellites 180 ms, `cubic-bezier(.55,0,1,.45)`); travel not scaled to distance. Partly caused by `SPEED = 0.75` (line 31) slowing everything uniformly.
- Fix: author in real seconds with the tokens (enter 450 ms `cubic-bezier(.22,1,.36,1)` + 16 px; exit 250/180 ms `cubic-bezier(.55,0,1,.45)`; travel `cubic-bezier(.65,0,.35,1)` at 300/450/550 ms by distance), drop SPEED, keep holds of 1.5 s (2 s on Stage 4).

**P2-2 · Task actions opens from nowhere**
- Scales 0.96 → 1 about its centre; no control on the indigo card opens it. Fix: add a ⋮ to the indigo card and set `transform-origin` to it.

**P2-3 · Stage scaled with `transform: scale(0.933)`**
- Text is rasterised at authored size and resampled. Fix: size the frame with container query units (`cqw`) instead of a transform (line 544).

**P2-4 · box-shadow transition and stale will-change**
- `transition: background .3s, box-shadow .3s` on rail dots (css:52); `will-change: opacity` on captions forever (css:256). Fix: swap the ring to an opacity-faded pseudo-element; add/remove will-change around moves only.

**P2-5 · File pill clipped by the stage edge**
- 450–1150 ms "AW_Campai…" hangs off the right edge. Fix: start it inside the stage, 24 px right of the drop zone, and use travel timing (≈ 450 ms).

**P2-6 · One-frame artwork hand-off**
- 3700 ms the flying artwork and the order card's thumbnail swap with a 50 ms dip. Disappears with the persistent token.

## 4. Timelines

### Current timeline

[timeline.json](before/timeline.json) holds all 100 tweens (target, properties, startTime, duration, ease, stage, role, real start and duration).

```
                │0   2   4   6   8   10  12  14  16  18  20  22  24  26  28  30
────────────────┼─────────────────────────────────────────────────────────────
  up            │▸▸   ◂◂
  pill          │▸▪═▪
  pfill         │   ▪▪▪
  fly           │     ▪═▪
  order         │     ▸▸▸         ═◂
  queued        │           ▸▸
  quote         │       ▸▸▸       ◂◂
  ql0           │       ▸▸
  qt            │        ▸▸
  sent          │          ◂
  accepted      │          ▸▸
  cardon        │          ▸▸
  t1            │           ▸▸    ◂◂
  tok           │                 ▪═ ◂◂
  sched         │                  ▸▸▸          ◂◂◂           ◂◂
  p1            │                    ▪══
  p2            │                    ▸▪═
  p3            │                     ▪═        ═▪◂
  now           │                       ▪══
  ring          │                         ▸▸▸
  done          │                               ▸▪═           ═▪◂
  menu          │                                 ▸▸          ◂◂
  duehl         │                                  ▸
  due22         │                                  ▸▸
  phone         │                                   ▸▸▸       ◂◂
  amelie        │                                    ▸▸
  knob          │                                      ═══
  knobok        │                                        ▸
  track         │                                             ▸▪═           ◂◂
  ctok          │                                             ▸▸            ◂◂
  tfill         │                                               ▪▪▪▪▪
  tn0           │                                               ▸▸
  tn1           │                                                 ▸▸
  tn2           │                                                   ▸
  ur0           │                                                   ▸▸▸
  t2            │                                                     ▸▸    ◂◂
  cap0          │▸▸               ◂
  cap1          │                 ▸▸            ◂◂
  cap2          │                               ▸▸            ◂◂
  cap3          │                                             ▸▸            ◂
  rail (timer)  │═════════════════════════════════════════════════════════════
```
▸ enter · ◂ exit · ═ travel · ▪ scale or several at once · 0.5 s per column, real playback seconds

### Proposed timeline

Plays once, ~22 s, ends on Stage 4 with Replay. ✱ changed · + new · − removed.

```
                │0   2   4   6   8   10  12  14  16  18  20  22
────────────────┼─────────────────────────────────────────────
✱ up            │▸   ◂
✱ pill          │══◂
✱ pfill         │  ▪▪
+ TOKEN         │   ▸══       ══        ══        ══
− fly           │
✱ order         │    ▸▸       ◂
✱ quote         │      ▸      ◂
✱ ql0–3         │      ▸▸
✱ total count   │      ··
✱ ★sent→acc+card│        ▪
✱ t1            │         ▸   ◂
− tok (morph)   │
✱ sched         │              ▸▸       ◂
✱ p1–p3         │               ══
+ connectors    │                ▸▪
✱ now           │                 ═══
  ★ring         │                   ▸▸
✱ done          │                        ▸▸       ◂
✱ menu (from ⋮) │                         ▪      ◂◂
✱ due roll      │                          ═
✱ ★knob         │                           ═▪
+ comment row   │                             ▸  ◂◂
− phone         │
✱ track         │                                  ▸▸
✱ tracker segs  │                                   ═══
  ★update row   │                                      ▸
✱ t2            │                                       ▸
✱ captions      │             ◂▸▸       ◂▸▸       ◂▸▸
✱ rail          │             ══        ══        ══
+ replay btn    │                                           ▸▸
```

Proposed key times (real seconds):

| Stage | Beats | Hold | Transition out |
|---|---|---|---|
| 1 Accounts (0–5.0) | pill drops in 0.45–1.0 → progress 1.0–1.8; upload out left 2.1, token docks in the order card's file slot 2.25–2.7, order card in from right 2.3; quote in from right 3.0, lines stagger 80 ms, total counts 3.3–3.9; ★ 4.1 Sent→Accepted + Credit card together; toast rises 4.5 | 5.0–6.5 | 6.5–7.6 |
| 2 Production (7.6–10.1) | timeline + stations in 7.15; token docks as parent chip; parts grow from chip (0.6→1), 120 ms stagger 7.6–8.4; connectors draw 8.4–8.7, fade 8.7–8.9; time line sweeps 8.9–9.8; ★ Printer 1 outline 9.8 | 10.1–11.6 | 11.6–12.7 |
| 3 Operations (12.7–14.95) | indigo card in with token docked; Task actions grows from ⋮ 12.7, Due date row highlights 13.0, chip rolls 13.2–13.45; ★ knob 13.6–14.3, check fills mint, comment row drops in 14.5 | 14.95–16.45 | 16.45–17.55 |
| 4 Shipping (17.55–19.95) | tracking card in, token docks top-left; tracker 3 × (350 ms + 120 ms pause) 17.55–18.96; ★ update row 19.0, toast rises 19.5 | 19.95–21.95, then Replay | — |

Every transition runs the same 1.1 s: satellites out 0–180 ms (exit ease) → primary out left 150–400 ms (250 ms, exit ease; token detaches and stays opaque) → token travels 300–850 ms (550 ms travel ease) with the rail moving in the same window and ease → next card in from the right 650–1100 ms (450 ms enter ease, 16 px).

## 5. Contact sheets

All frames are 50 ms of real playback apart. Labels show real ms and timeline seconds.

### Transitions

T1 · Accounts → Production — order morphs into the token over the semi-transparent quote and toast; the token arrives oversized and settles.
![T1](before/sheets/T1-accounts-to-production.png)

T2 · Production → Operations — part -03 balloons to 1.9× while the indigo card grows from 0.5× underneath it.
![T2](before/sheets/T2-production-to-operations.png)

T3 · Operations → Shipping — tracking card, indigo card, compact token and both captions overlap semi-transparently for ~300 ms.
![T3](before/sheets/T3-operations-to-shipping.png)

T4 · Loop reset — everything fades and the rail runs back, then the upload card rises again.
![T4](before/sheets/T4-loop-reset.png)

### Accent moments

S1 · Upload (first 3.8 s; never seen on first play — P1-1). Pill clipped at the stage edge 450–1150 ms; upload/order double exposure 2850–3050 ms.
![S1](before/sheets/S1-upload.png)

A1 · Accept and pay.
![A1](before/sheets/A1-accept-and-pay.png)

A2a · Split — three parts scale out of the token at up to 1.69×, overlapping each other semi-transparently.
![A2a](before/sheets/A2a-split.png)

A2b · Time sweep and Printer 1 outline.
![A2b](before/sheets/A2b-sweep-and-ring.png)

A3 · Mark done.
![A3](before/sheets/A3-mark-done.png)

A4 · Ship — tracker fill, checks, update row, toast.
![A4](before/sheets/A4-ship.png)

### Final frames

| | Stage 1 | Stage 2 | Stage 3 | Stage 4 |
|---|---|---|---|---|
| Desktop 1440 (sticky) | ![](before/finals/desktop-stage1.png) | ![](before/finals/desktop-stage2.png) | ![](before/finals/desktop-stage3.png) | ![](before/finals/desktop-stage4.png) |
| Tablet 1024 (inline) | ![](before/finals/tablet-stage1.png) | ![](before/finals/tablet-stage2.png) | ![](before/finals/tablet-stage3.png) | ![](before/finals/tablet-stage4.png) |
| Mobile 375 | ![](before/finals/mobile-stage1.png) | ![](before/finals/mobile-stage2.png) | ![](before/finals/mobile-stage3.png) | ![](before/finals/mobile-stage4.png) |
| Reduced motion, desktop | ![](before/finals/reduced-desktop-stage1.png) | ![](before/finals/reduced-desktop-stage2.png) | ![](before/finals/reduced-desktop-stage3.png) | ![](before/finals/reduced-desktop-stage4.png) |
| Reduced motion, mobile | ![](before/finals/reduced-mobile-stage1.png) | ![](before/finals/reduced-mobile-stage2.png) | ![](before/finals/reduced-mobile-stage3.png) | ![](before/finals/reduced-mobile-stage4.png) |

### Story from the final frames alone

| Stage | What I'd say happened | Intended | Match |
|---|---|---|---|
| 1 | A Brownie banner order came in with an artwork file, a €580 quote was accepted by John Walters and paid by card, and it's queued. | Files, quote and payment | Yes |
| 2 | That order was split into three jobs, two printing side by side on Printers 1 and 2, then cutting on Cutter 1, among other orders; it's 11:24. | Split across stations | Yes |
| 3 | The job was marked done, due 22/04, and someone left a note in the thread. | Adjusted and finished | **Partly** — "finished" reads; "adjusted" only by reading the note |
| 4 | It was printed and shipped and is on its way, arriving 23/04. | Shipped | Yes |

## 6. Files

- `before/timeline.json` — every tween, with real and timeline times
- `before/checks.json` — all automated checks, with timestamps
- `before/perf-frame-deltas.json` — every rAF delta from the real-time run
- `before/sheets/` — contact sheets (50 ms steps)
- `before/finals/` — final frames: desktop, tablet, mobile, reduced motion
- `before/video/promo-full-realtime.mp4` — the full loop at 60 fps, real speed
- `before/video/promo-transitions-0.25x.mp4` — the three transitions and the reset at quarter speed
- Dev instrumentation: `window.__promo` (localhost only) in `tools/content/production-hub.js` — `seek(sec)`, `seekT(t)`, `tracks()`, `uAt`, `tAt`


## 7. After the fixes

Fixed on `promo-critique`, shipped to `main` as `918cf9e`. Sections 2 and 3 re-run exactly as before: same captures, same 50 ms sampling, same checks, on desktop 1440 and mobile 375. Evidence in this folder's top level; the original is under `before/`.

Changes beyond the proposal, decided during review:
- **Production is now a scheduling moment, not a three-way split.** The calendar arrives full of #98410 and #618716 jobs; the two free slots outline in dashed mint (Printer 2 09:45–10:45, then Cutter 1 10:45–11:30); the order splits into `#39208-01 Print` and `#39208-02 Cutting` that fill them; the time line sweeps to 11:24, inside the cutting slot, and Cutter 1 takes the mint outline. Caption: "Fits into the gaps". The connector lines from the proposal were tried and removed.
- **Parts pass under the station headers** on the way to their lanes, so nothing crosses header text.
- **Notifications float**: a separate unclipped layer lets both toasts overhang the stage's bottom-left corner instead of being cut by its edge.
- **Mobile**: the menu sits below the indigo card and leaves after the date changes, before the comment row drops in.

### Scorecard, before → after

| # | Check | Before | After | Evidence |
|---|---|---|---|---|
| 3.1 | Simultaneity (≤ 2 primary movers) | Fail · 5 · score 2 | **Pass** · max 2 (the two parts move as one split group) · 5 | [checks.json](checks.json) |
| 3.2 | Double exposure | Fail · 15 pairs · 1 | **Pass** · 0 pairs, captions never overlap · 5 | [T1](sheets/T1-accounts-to-production.png) [T2](sheets/T2-production-to-operations.png) [T3](sheets/T3-operations-to-shipping.png) |
| 3.3 | Distortion | Fail · 0.52–1.90 · 1 | **Pass** · no text scaled outside 0.96–1, no non-uniform scale · 5 | [checks.json](checks.json) |
| 3.4 | Continuity (≥ 0.9) | Fail · 66/603 · 2 | **Pass** · 0/432 desktop, 0/432 mobile · 5 | [checks.json](checks.json) |
| 3.5 | Direction + toasts | Fail · 1 | **Pass** · cards enter +16 px from the right, leave −16 px left; both toasts rise 12 px from one slot, exit 8 px down · 5 | [timeline.json](timeline.json) |
| 3.6 | Transform origin | Fail · 2 | **Pass** · Task actions grows from the card's ⋮ (0.3 px off desktop, 0.2 px mobile) · 5 | [A3](sheets/A3-mark-done.png) |
| 3.7 | Timing tokens | Fail · 2 | **Pass** · cards in 450 ms `(.22,1,.36,1)`, out 250 ms `(.55,0,1,.45)`, satellites 180 ms; token and parts travel 450/550 ms by distance on `(.65,0,.35,1)`; the 900 ms sweep and 700 ms knob are authored beats · 5 | [checks.json › timing](checks.json) |
| 3.8 | Holds | Pass with caveat · 3 | **Pass** · fully still, rail included: 1.70 / 1.75 / 1.65 / 2.15 s · 5 | [checks.json › holds](checks.json) |
| 3.9 | Rail sync | Fail · 2 | **Pass** · rail moves 6800/11900/16750 ms for 550/450/450 ms, identical to the token's trips · 5 | [checks.json › railSync](checks.json) |
| 3.10 | Legibility (≥ 11 px) | Fail · 5.8 px · 2 | **Pass** · smallest 11.04 px desktop, 11.75 px mobile, 0 frames under · 5 | [checks.json](checks.json) |
| 3.11a | Stage scaled with transform | Fail · 3 | **Pass** · sized with `cqw` container units, no transform · 5 | — |
| 3.11b | box-shadow animated | Fail · 4 | **Pass** · 0 transitions; lift is an opacity-faded child · 5 | — |
| 3.11c | will-change left on | Fail · 4 | **Pass** · set 100 ms before a move, cleared after; 0 at the end · 5 | — |
| 3.11d | Tabular numerals | Pass · 5 | Pass · 5 | — |
| 3.12a | Starts mid-stage | Fail · 1 | **Pass** · first frame drawn before view; plays from 0 at 50 % visible · 5 | [checks.json › playback](checks.json) |
| 3.12b | Loops | Fail · 2 | **Pass** · plays once, ends on Shipping (stage log 0 → 1 → 2 → 3) · 5 | — |
| 3.12c | Pause / Replay | Partial · 3 | **Pass** · Pause while playing, Play when paused, Replay at the end · 5 | — |
| 3.12d | Reduced motion | Pass · 4 | **Pass** · no playback, shows Shipping's final frame, no control, nodes switch instantly · 5 | [finals/reduced-desktop-initial.png](finals/reduced-desktop-initial.png) |
| 3.12e | Keyboard | Pass · 5 | **Pass** · tab order kept, Enter plays the standard transition into that stage, 2 px ring (was rounding to 1 px after the unit change) · 5 | [finals/keyboard-focus.png](finals/keyboard-focus.png) |
| 3.13 | Performance | Pass · 5 | **Pass** · 2958 frames, p99 9.3 ms, 0 over 20 ms, 0 long tasks; three more runs also 0/0 (headless Chromium) · 5 | [perf-frame-deltas.json](perf-frame-deltas.json) |
| 5.1 | Story from finals | Partial · 3 | **Pass** · 4/4 (below) · 5 | [finals](finals/) |
| 5.2 | Hierarchy | Fail · 2 | **Pass** · one card at a time; between cards only the token moves · 4 | [video](video/promo-transitions-0.25x.mp4) |
| 5.3 | Continuity by eye | Partial · 3 | **Pass** · the token is never out of sight · 5 | — |
| 5.4 | Rhythm | Partial · 3 | **Pass** · ≤ 3 beats a stage, same 1.1 s change each time, 21.6 s total · 4 | [gantt](#timeline-after) |
| 5.5 | Polish | Fail · 2 | **Pass** · no stretched text, no ghosting, toasts unclipped · 4 | [sheets](sheets/) |
| 5.6 | Restraint | Fail · 2 | **Pass** · plays once and stops; nothing moves after 21.6 s · 4 | [video](video/promo-full-realtime.mp4) |

All P0 and P1 issues pass. Remaining 4s are judgement calls, not failures: the first stage is still the busiest, and the split's parts briefly overlap each other mid-flight (both opaque).

### Story from the final frames, after

| Stage | What I'd say happened | Match |
|---|---|---|
| 1 | A banner order came in with its file; a €580 quote was accepted by John Walters and paid by card; it's queued. | Yes |
| 2 | In a full day's schedule the order took the only free print slot on Printer 2 and the cutting slot right after on Cutter 1; it's being cut now. | Yes |
| 3 | The due date moved from 23/04 to 22/04 (you see it roll), Amélie explains why, and the task is marked done. | Yes |
| 4 | Printed, shipped 22/04 with Seur, arriving 23/04. | Yes |

### Timeline, after

```
                │0   2   4   6   8   10  12  14  16  18  20  
────────────────┼────────────────────────────────────────────
  up            │    ◂                                       
  pill          │▪▪◂                                         
  pfill         │  ▪▪                                        
  token         │ ▸▸ ══       ══        ══        ══         
  order         │    ▸▸       ◂                              
  quote         │      ▸      ◂                              
  ql0           │      ▸▸                                    
  qt            │      ▸▸                                    
  sent          │        ◂                                   
  accepted      │        ▸                                   
  cardon        │        ▸                                   
  queued        │         ▸                                  
  t1            │         ▸   ◂                              
  sched         │              ▸▸       ◂                    
  p1            │                ▪═                          
  p2            │                ▪═                          
  now           │                 ▪══                        
  ring          │                   ▸▸                       
  done          │                        ▸▸       ◂          
  menu          │                         ▸      ◂◂          
  duehl         │                          ▸                 
  due22         │                          ▸                 
  knob          │                           ══               
  knobok        │                            ▸               
  cmt           │                             ▸  ◂◂          
  track         │                                  ▸▸        
  tfill         │                                   ▪▪       
  tn0           │                                  ▸▸        
  tn1           │                                   ▸▸       
  tn2           │                                    ▸▸      
  ur0           │                                     ▸▸     
  t2            │                                      ▸▸    
  cap0          │             ◂                              
  cap1          │              ▸▸       ◂                    
  cap2          │                        ▸▸       ◂          
  cap3          │                                  ▸▸        
  rail          │             ▪▪        ▪▪        ▪▪         
```

▸ enter · ◂ exit · ═ travel · ▪ scale or several at once · 0.5 s per column, real seconds. [timeline.json](timeline.json) holds all 82 tweens.

### Before / after contact sheets

Each pair is the same moment, 50 ms per frame.

**Accounts → Production.** Before: the order card scales into the token over the fading quote. After: the toast and quote leave, the order card exits left, the token travels alone while the rail moves, the calendar enters from the right.

| Before | After |
|---|---|
| ![](before/sheets/T1-accounts-to-production.png) | ![](sheets/T1-accounts-to-production.png) |

**The Production moment.** Before: three parts balloon out of the token, semi-transparent and overlapping. After: the two free slots outline, then the print and the cut drop into them under the station headers.

| Before | After |
|---|---|
| ![](before/sheets/A2a-split.png) | ![](sheets/A2a-search-and-split.png) |

**Production → Operations.** Before: part -03 grows to 1.9× over a shrunken indigo card. After: the calendar exits left, the token travels to the indigo card's slot as it enters.

| Before | After |
|---|---|
| ![](before/sheets/T2-production-to-operations.png) | ![](sheets/T2-production-to-operations.png) |

**Operations → Shipping.** Before: tracking, indigo card, token and both captions overlap semi-transparently. After: menu and comment out, card out left, token docks on the tracking card's edge.

| Before | After |
|---|---|
| ![](before/sheets/T3-operations-to-shipping.png) | ![](sheets/T3-operations-to-shipping.png) |

**Upload.** Before: never seen on first play, pill clipped at the edge, upload/order double exposure. After: first frame drawn before play, pill glides in from inside the stage, token appears in the file row.

| Before | After |
|---|---|
| ![](before/sheets/S1-upload.png) | ![](sheets/S1-upload.png) |

Accent moments, after: [accept and pay](sheets/A1-accept-and-pay.png) · [sweep and ring](sheets/A2b-sweep-and-ring.png) · [mark done](sheets/A3-mark-done.png) · [ship](sheets/A4-ship.png)

### Final frames, after

| | Stage 1 | Stage 2 | Stage 3 | Stage 4 |
|---|---|---|---|---|
| Desktop 1440 (sticky) | ![](finals/desktop-stage1.png) | ![](finals/desktop-stage2.png) | ![](finals/desktop-stage3.png) | ![](finals/desktop-stage4.png) |
| Tablet 1024 (inline) | ![](finals/tablet-stage1.png) | ![](finals/tablet-stage2.png) | ![](finals/tablet-stage3.png) | ![](finals/tablet-stage4.png) |
| Mobile 375 | ![](finals/mobile-stage1.png) | ![](finals/mobile-stage2.png) | ![](finals/mobile-stage3.png) | ![](finals/mobile-stage4.png) |
| Reduced motion, desktop | ![](finals/reduced-desktop-stage1.png) | ![](finals/reduced-desktop-stage2.png) | ![](finals/reduced-desktop-stage3.png) | ![](finals/reduced-desktop-stage4.png) |
| Reduced motion, mobile | ![](finals/reduced-mobile-stage1.png) | ![](finals/reduced-mobile-stage2.png) | ![](finals/reduced-mobile-stage3.png) | ![](finals/reduced-mobile-stage4.png) |

Videos, after: [full sequence, real speed](video/promo-full-realtime.mp4) · [transitions, 0.25×](video/promo-transitions-0.25x.mp4). Before: [full](before/video/promo-full-realtime.mp4) · [transitions](before/video/promo-transitions-0.25x.mp4).
