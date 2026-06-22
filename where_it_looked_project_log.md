# Project Log — "Where It Looked" (hotel-search divergence demo)

Working title. Subdomain TBD (see Open Questions). This file is the shared memory for the build. It lives in the repo at root, beside `index.html`. Keep it there.

---

## 0. How to use this log

Two assistants, never talking directly. You relay between them.

- **Claude (chat)** — architect and reviewer. Holds the LinkedIn/Substack strategy as the governing reference. Designs the artifact, writes work orders, reviews results against objective and constraints. Cannot see GitHub.
- **Claude Code** — implementer and scribe. Has repo access. Writes and commits code. Also applies the log updates that the chat architect pre-writes into each work order. Has no strategy file and no chat memory of its own, so every work order it receives must remain self-contained — Code can read this log for context, but the work order is the binding contract.

**Why Code edits the log now:** the log lives in the repo, so Code can update it directly instead of you copy-pasting the architect's edits by hand. But Code does not author log content — it cannot make architect-level judgment calls. The chat architect writes the exact log text inside each work order, in a dedicated **Log Updates** block; Code applies that text verbatim and commits it alongside the code.

**The loop, per session:**

1. Open a chat with Claude (architect). Paste this log's current contents (still required — the architect cannot see the repo), then say **"ready for session X."**
2. Claude reads the log and produces a **Work Order** for Claude Code. The work order ends with a **Log Updates** block: exact rows/edits for Code to apply to this file.
3. You paste the Work Order into Claude Code. It implements against the repo, applies the Log Updates block to `where_it_looked_project_log.md`, and commits both code and log.
4. You paste back Claude Code's **Handback** (what changed, deviations, questions, how to preview, confirmation that the log was updated).
5. Claude reviews. If the build or the log needs a correction beyond what was pre-specified, Claude issues a short follow-up instruction; otherwise it says to advance.

**Source of truth for "where are we" is Section 7, the State Snapshot.** It is rewritten each session, not appended. Everything else below is append-only history.

---

## 1. Objective

Build a tiny, self-contained artifact that makes the argument of post #28 ("Where did it stop looking?") physical: three AI assistants, one hotel query, three confident but divergent answers. The confidence is constant; the answer is not — because each assistant has a hidden search boundary and ranking rule.

Role in the body of work: a **structural artifact** that ships beside the post on the divergencelog domain, the way `worldcup.divergencelog.com` did for #28. It *shows* the divergence rather than asserting it. It is not a product. It is proof a reader can inspect.

Success = a visitor sees, in one screen, that "neutral" search returns non-neutral answers, and can flip a toggle to see the boundary that produced each one.

---

## 2. The artifact (canonical spec)

Single HTML page. Deterministic. No backend, no API, no live model.

- **Fixed query** at top, e.g. "Lisbon, 2 nights, walkable, mid-range."
- **Three assistant cards** side by side. Each returns a confident top-three with a one-line blurb and a tone of certainty. The three lists visibly differ.
- **Headline divergence number** in divergencelog house style, e.g. *overlap across the three "best" lists: 0 of 3.*
- **"Show the boundary" toggle.** Reveals, per card, the two hidden things: (a) the slice of the market it could see, (b) the rule it used to rank (lowest price / highest rating / "recommended for you").
- **Boundary slider** (the load-bearing interaction): widen one assistant from "partner hotels only" to "whole market" and watch its top-three re-sort live, just as confident as before.
- **Aesthetic:** matches existing divergencelog cards — cream/off-white, serif display, quiet, the divergence number doing the visual work. No dashboard look, no gold accents.

---

## 3. Constraints (non-negotiable — restate in every Work Order)

These come from the governing strategy. Claude Code does not have that file, so they live here.

- **Synthetic data, clearly labeled as a model.** Invented hotels. No real OTA results, no scraping, no naming a real assistant. A visible line states the data is illustrative.
- **Structural claim, never empirical.** The point is that a hidden ranking rule exists and changes the answer — not that any named platform cheats. No accusation against any real company.
- **No live AI in the mechanism.** Ranking is deterministic and inspectable. Keeping the model out is itself the punchline (mirrors #28 keeping the model out of the forecast).
- **Inspectable, not magical.** Anyone reading the source sees plain data and plain rules. The tool is the middleman that shows its boundary.
- **Tiny and self-contained.** One HTML file, embedded dataset, runs from a static host. No keys, no build step unless trivial.
- **Lucid, not alarmist.** Copy describes; it does not warn.

---

## 4. Sessions

Plan for two; hold a third.

**Session 1 — working skeleton.** *Done when:* page renders end-to-end — fixed query, three cards, three diverging top-threes, headline overlap number, "show the boundary" toggle functional. Data is in and divergence is real but rough. Styling minimal. Goal: you can see and react to the thing. **Status: complete, accepted (2026-06-19).**

**Session 2 — the slider + polish.** *Done when:* boundary slider re-sorts one card live; divergencelog styling applied; card copy and "illustrative data" labeling finalized; assistants renamed off archetypes (D5); per-assistant blurbs; readable on mobile. Goal: publishable.

**Session 3 — buffer (reserved, likely partial).** Absorbs dataset rework, divergence tuning, deploy to subdomain, first-comment link prep. Skip if 1 and 2 land clean.

Build is not blocked: Session 1 ran in a fresh local repo with a placeholder name. Subdomain and repo home are only needed before deploy.

---

## 5. Work Order format (Claude → Claude Code)

Each work order is self-contained and contains:

- **Context** — one paragraph: what the project is, that Claude Code has repo access, that Section 3 constraints are binding.
- **This session's goal + definition of done.**
- **Tasks**, ordered.
- **Out of scope** — so it does not over-build.
- **Handback request** — files changed, key decisions and deviations, how to preview, any questions for the architect.
- **Log Updates** — exact text and edits for Code to apply to `where_it_looked_project_log.md`: which Decision Log rows to add, the full replacement text for Section 7, any Open-Question status changes, and the Session Log block to append. Code applies these verbatim and commits them with the code.

---

## 6. Decision Log (append-only)

| # | Date | Decision | Rationale |
|---|------|----------|-----------|
| D1 | 2026-06-17 | Two-assistant workflow (chat architect + Code implementer); this log is shared memory | Chat Claude cannot see GitHub; Code can. Log carries state across blind sessions. |
| D2 | 2026-06-17 | Synthetic, labeled data; deterministic ranking; no live model | Anti-spectacle stance; structural-not-empirical claim; mirrors #28 keeping the model out of the mechanism. |
| D3 | 2026-06-17 | Two planned sessions plus one buffer | Code volume is small; the dataset, the ranking rules, and the aesthetic are the judgment-heavy risk and need iteration. |
| D4 | 2026-06-19 | Session 1 skeleton accepted: single self-contained index.html, overlap computed 0/3, top-threes H1-H2-H3 / H8-H9-H10 / H6-H7-H11, boundary toggle live. | Met definition of done; deviations minor and in-family. |
| D5 | 2026-06-19 | Assistant names must NOT telegraph their ranking rule. Move off Concierge/Scout/Critic to neutral assistant-style names (e.g. Atlas/Vera/Pilot). | Archetype names leak the hidden boundary and spoil the toggle reveal, undercutting the #28 argument. A/B/C rejected as too sterile. |
| D6 | 2026-06-19 | Log now lives in the repo and is maintained by Claude Code, applying exact Log-Updates text pre-written by the chat architect in each work order. | Removes manual paste step. Architect still authors all log content; Code is scribe only, preserving the architect/implementer split. |
| D7 | 2026-06-22 | Final assistant names locked: Atlas / Vera / Pilot, mapped by boundary label (Atlas = partner-only, Vera = full-index, Pilot = editorial selection). Closes Q7. | Neutral, non-telegraphing per D5; mapped to the stable boundary label, not the old placeholder names. |
| D8 | 2026-06-22 | Boundary slider attaches to the partner-only card (Atlas). Widening admits non-partner hotels in ascending id order from 5 up to the full market of 12; Atlas's top-three and the headline overlap recompute live while its blurb/confidence stay constant. | Makes #28's "where did it stop looking" physical: the slice is one lever, the rule another; the slider isolates the slice. |
| D9 | 2026-06-22 | Display face exposed as a single swappable CSS variable `--display-face` at index.html:9, used by h1, .divergence .num, .card h2, .res-rank. | Makes face-lock a one-line change. |
| D10 | 2026-06-22 | Q6 closed by delegation to the architect. Display face locked to a self-contained literary serif stack: "Iowan Old Style", "Palatino Linotype", Palatino, "Book Antiqua", Georgia, serif. Applied in Session 3 (one-line swap at index.html:9). | No webfont dependency, preserves file:// self-containment; warmer/editorial vs bare Georgia, Georgia as guaranteed fallback. Re-swappable if a real divergencelog house face is later supplied. |
| D11 | 2026-06-22 | Atlas's slider lives inside the "show the boundary" reveal (not always-visible), same arc as Vera/Pilot. Accepted. | Keeps the default screen to three confident answers + the divergence number; mechanism revealed on inspection, in-family with #28's anti-spectacle stance. |
| D12 | 2026-06-22 | Slider re-sorts Atlas only at full market (slider=12); positions 5–11 stay H1,H2,H3 because H12 is the only non-partner whose recScore cracks the top three. Accepted as honest/correct for Session 2; intermediate-shift tuning is the Session 3 lead task. | The interaction must feel alive across the drag, not only at the extreme; the fix is dataset tuning, reserved for the buffer session. |

---

## 7. State Snapshot (rewrite each session — source of truth)

- **Phase:** Session 2 built and accepted as a faithful, correct implementation. Publishable pending one Session-3 nudge (the slider only re-sorts at full market — see Next). Slider + polish are in on top of the accepted Session 1 skeleton.
- **Repo:** Local only, `git init` in `d:\WinProjects\WIL`. Single `index.html` at root plus this log. Session 1 commit `922dfec`; Session 2 build commit `180a77e`; this log-close commit `1333172`. No GitHub remote yet (deferred to deploy).
- **Last session:** Session 2 — boundary slider on the partner-slice card (Atlas) re-sorts its top-three and recomputes headline overlap live; assistants renamed Atlas / Vera / Pilot (D7); per-assistant blurbs; illustrative-model line locked; cream/Georgia polish with the display face exposed as swappable `--display-face` (D9); mobile pass to ~360px. Verified Atlas = H1,H2,H3 at slider min and H1,H12,H2 at full market; overlap stays 0 of 3; four console.asserts pass. Build commit `180a77e`.
- **Next:** Session 3 (buffer) — (1) **dataset shift-timing fix**: raise a mid-range non-partner's recScore so an intermediate slider position also flips Atlas's top-three (so dragging feels alive, not dead until the last stop), ideally so the headline overlap also ticks 0→1 at some width for a numeric payoff; (2) apply the locked display face (D10) — one-line swap at index.html:9; (3) consider restoring the `walk` field as the user-stated preference no ranker honors; (4) deploy to subdomain (Q1), repo home (Q2), first-comment link prep.
- **Blocked on:** nothing blocks Session 3. Before deploy: subdomain (Q1) and repo home (Q2).

---

## 8. Open Questions

| # | Question | Status | Owner |
|---|----------|--------|-------|
| Q1 | Subdomain name: `hotels.divergencelog.com` vs `whereitlooked.divergencelog.com` (ties to post title) vs other | Open | You |
| Q2 | Repo: new repo, or subfolder of an existing divergencelog repo? | Open | You |
| Q3 | Fixed query — "Lisbon, 2 nights, walkable, mid-range" placeholder; keep or change? | Tentative | You |
| Q4 | Three assistant cards confirmed (matches the rhetorical "three options")? | Confirmed: 3; renamed Atlas / Vera / Pilot per D7 | You |
| Q5 | Does the post #28 opener reflect a real booking moment? Post and demo ship together. | Open | You |
| Q6 | What headline/display face do existing divergencelog cards use? | Closed — architect-picked self-contained literary serif stack (D10); re-swappable if a real house face is supplied | You |
| Q7 | Final assistant names — three neutral, non-telegraphing names (D5). | Closed — Atlas / Vera / Pilot (D7) | You |

---

## 9. Session Log (append one block per session)

### Session 1 — working skeleton (2026-06-19)

**Built:** Single self-contained `index.html` (HTML+CSS+JS, no build/keys/deps). Fixed Lisbon query, three assistant cards with computed top-threes (name / neighborhood / price / rating / confident blurb), headline overlap number, "show the boundary" toggle with live market-slice counts, illustrative-data line. Ranking written as pure functions (`visibleSet()` → `.sort(rank)` → `.slice(0,3)`); lists computed, not embedded.

**Verified:** Overlap renders 0/3 (`computeOverlap`, not hardcoded). Top-threes match expected ids exactly — H1,H2,H3 | H8,H9,H10 | H6,H7,H11 — confirmed by in-page `console.assert` and headless run. Boundary labels show live counts: "partner hotels only (5 of 12 listed)", "full index (12 of 12 listed)", "editorial selection (6 of 12 listed)". Styling in-family: #f6f1e7 cream, Georgia serif headline + divergence number, dark ink, thin rules, no gold, no dashboard look. Commit `922dfec`.

**Deviations:** (1) Two `console.assert`s instead of one — second pins the exact id sequence so silent data drift trips it; kept. (2) Placeholder blurbs reused by position across all three cards — Session 2 makes them per-assistant. (3) Minimal mobile stacking breakpoint (`@media max-width:760px`) added though mobile is Session 2 scope — harmless, kept. (4) Commit authored as Claude / noreply@anthropic.com (no repo identity configured) — reset author if you want your name on history once the repo goes remote.

**Architect decisions this session:** Rename assistants off archetypes (D5). Hold Georgia until house face confirmed (Q6). Keep working headline copy ("Three assistants. One question. Three different 'best' answers."). Log moves into the repo, maintained by Code going forward (D6). Advance to Session 2.

**Open for You before / during Session 2:** Q6 (house display face) and Q7 (final assistant names). Neither blocks the build starting.

### Session 2 — slider + polish (2026-06-22)

**Built:** Boundary slider on the partner-slice card (Atlas), range 5→12 (8 stops); the 5 partner hotels (H1–H5) always visible, non-partners admitted in ascending id order (H6→H12). On change, Atlas's visible set → `rank` → top-three and the headline overlap recompute live; Atlas's blurb/confidence held constant. Slider lives inside the "show the boundary" reveal, same arc as Vera/Pilot (D11). Assistants renamed Atlas / Vera / Pilot by boundary label (D7). Three distinct per-assistant blurbs replacing the position-reused placeholders; `.tone` renamed `.blurb`, archetype character-descriptions removed, dead CSS (`counter-reset`, `.res-blurb`) cleaned. Illustrative-model line and headline copy locked. Polish kept in the cream `#f6f1e7` system; display face exposed as `--display-face` at index.html:9, used by h1, .divergence .num, .card h2, .res-rank. Mobile: single-column at 760px, 28px slider thumb on touch, type scaled and legible to ~360px, no horizontal scroll.

**Verified:** Atlas at slider min (5) = H1,H2,H3 (Session-1 baseline). Atlas at full market (12) = H1,H12,H2 (H12, recScore 94, enters and outranks H2 at 92; H3 drops to 4th). Overlap via `computeOverlap` = 0 of 3 at every slider position (H1/H12/H2 never intersect Vera {H8,H9,H10} or Pilot {H6,H7,H11}); never hardcoded. Four console.asserts pass: Atlas@5 → "H1,H2,H3"; Atlas@12 → "H1,H12,H2"; Vera|Pilot → "H8,H9,H10 | H6,H7,H11"; initial overlap === 0. Build commit `180a77e`.

**Known issue → Session 3:** slider changes Atlas's top-three only at the final stop (12); positions 5–11 yield H1,H2,H3 because H12 is the only non-partner whose recScore cracks the top three. Honest/correct as built; the fix (raise a mid-range non-partner's recScore for an intermediate shift, ideally also nudging overlap 0→1) is dataset tuning, Session 3 lead task.

**Deviations (architect-reviewed):** (1) Slider placed inside the boundary reveal rather than always-visible — accepted (D11). (2) `.tone`→`.blurb` rename, archetype character-descriptions removed, dead CSS cleaned — accepted, aligns with D5. (3) `walk` field removed from hotel data as unused — noted; reconsider restoring in the Session 3 dataset pass as the user-stated "walkable" preference no ranker honors.

**Architect decisions this session:** Q6 closed by delegation — display face locked to a self-contained literary serif stack (D10), applied in Session 3. Q7 closed — Atlas / Vera / Pilot (D7). Session 2 accepted; publishable gated only on the Session 3 shift-timing nudge.

**Open for You:** deploy questions Q1 (subdomain) and Q2 (repo home).
