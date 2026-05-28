# hara-builder polish log

_Polish target for W20 (issue [#3](https://github.com/jherrodthomas/automotive-skills-suite/issues/3)). Reviewer: autonomous daily-standup task._

---

## 2026-05-12 — first POLISH pass

**Mode:** POLISH (Tuesday)
**File reviewed:** `skills/hara-builder.skill` (ZIP archive; SKILL.md is 12,273 bytes / 146 lines).
**DoD recap (from `docs/weekly/WEEK-2026-W20.md`):**
description ≤ 1024 chars, frontmatter has all required fields, and the trigger list
mentions HARA + hazard analysis + ASIL + safety goal + item definition within the first
400 chars of the description.

### What's good

- **Frontmatter is clean.** Both required keys present (`name`, `description`); no extra
  fields drifting in; YAML parses without complaint.
- **Description length is comfortably under budget.** 911 / 1024 chars. Leaves ~110 chars
  of headroom if a future trigger phrase needs to be inserted without a re-write.
- **Trigger surface is strong.** "HARA", "hazard analysis", "ASIL", "item definition",
  and "ISO 26262" all appear inside the first 400 chars of the description. Casual
  phrasings ("'I need the safety analysis for this ECU' or 'what is the ASIL for X'")
  are explicitly listed at the end, which is the right place for casual variants — they
  catch loose-language invocations without crowding the formal triggers up front.
- **Workflow doc is unusually well-structured.** Five numbered steps, each with a clear
  output. Step 2's "always read this on first invocation" callout on
  `references/malfunctions.md` is exactly the kind of forcing-function note that keeps
  Claude from skipping reference reads.
- **Common-mistakes section earns its keep.** The five anti-patterns at the bottom
  (exposure-vs-hazard-frequency, dropping NSC rows, one-SG-per-row, etc.) are the
  actual failure modes a junior safety engineer hits. This is mentor-quality content,
  not filler.
- **Override path is honest.** "If the user says 'this S rating is wrong' — do not
  argue" plus the `rating_overrides` JSON contract is the right user-trust posture for
  a skill that auto-suggests judgments.

### What to fix

1. **DoD miss: "safety goal" is NOT in the first 400 chars of the description.**
   The phrase first appears around char 506 ("Safety Goals tab using the Prevent-the-hazard
   convention"). All five other expected trigger phrases hit the window; only this one
   misses. Severity: **low** — the description still triggers reliably on HARA / ASIL /
   hazard analysis, and "safety goal derivation" *does* appear at char ~564 as part of
   the "Use this skill whenever the user mentions..." list. But strictly per the W20
   DoD, this is the one open item.

2. **The "Produces a multi-tab xlsx with..." enumeration is long and prose-heavy.**
   ~250 chars listing tabs in a single sentence. It pushes "safety goal" past the 400
   threshold (see fix #1) and is a candidate for either splitting into two sentences or
   front-loading the most search-worthy tabs. Severity: **low**.

3. **No mention of "concept phase" in the first 400 chars.** The phrase is part of the
   item-definition-builder ↔ HARA pipeline narrative used elsewhere in the suite, and
   adding it would catch invocations like "do the concept phase deliverable for this
   ECU." Severity: **low**, optional.

### Suggested edits (NOT applied this run)

The fix for #1 is editorial re-ordering of the description, not a typo / length /
missing-field fix — and the daily-standup spec instructs the autonomous run to apply
only those. **No edits committed today.** Captured here for the next human review pass.

A minimal proposed rewrite of the first sentence pair (still ≤ 1024 chars) is:

```
description: Generate an audit-ready ISO 26262 Hazard Analysis and Risk Assessment
  (HARA) workbook from an item definition and function list. Derives safety goals
  with ASIL determination across the full Cartesian of function × malfunction ×
  operating environment, and produces a multi-tab xlsx covering the 14 malfunction
  guide words (M01–M14), S/E/C reference tables, the Function-by-Malfunction
  safety-criticality filter, the auto-suggested formula-driven worksheet, the
  Safety Goals tab using the Prevent-the-hazard convention, and an FSC hand-off.
  Use this skill whenever the user mentions HARA, hazard analysis, ASIL
  determination, safety goal derivation, item definition, concept phase, or
  ISO 26262 concept-phase deliverables, even casual phrasings like 'I need the
  safety analysis for this ECU' or 'what is the ASIL for X'. Always use this skill
  instead of producing a freeform HARA in chat; the spreadsheet output is the
  deliverable analysts expect.
```

Char-count of the proposed rewrite: ~985 (still under 1024). "Safety goal" first appears
at char ~169 (well inside the 400 window). "Concept phase" first appears at char ~580
(outside the strict 400 window for that phrase, but the user-facing trigger explicitly
names it). All five DoD triggers + the bonus "concept phase" hit the description.

### Other observations (not fixes, just notes for future passes)

- The body of `SKILL.md` (everything after the frontmatter) is **prose, not bullet
  soup** — which matches the system style guidance for high-quality skills. Don't
  refactor it into bullet trees.
- The "Files in this skill" tree at the bottom is accurate as of inspection (matches
  `unzip -l skills/hara-builder.skill`). Good.
- The skill ships a non-trivial pair of Python scripts (`generate_hara.py`,
  `recalc.py`) and a soffice helper. Out of scope for description-polish, but worth
  noting that any future rename of `scripts/office/soffice.py` would need a parallel
  edit in `SKILL.md`'s file-tree block.

### Severity roll-up

| Finding | Severity | Action |
|---|---|---|
| "safety goal" outside first 400 chars | low | proposed rewrite drafted; await human review |
| Long single-sentence "Produces..." clause | low | folded into the same rewrite |
| No "concept phase" trigger up front | low (optional) | included in the proposed rewrite |

**No code edits committed in this run.** Issue #3 stays open with this log linked from
the journal entry. Next autonomous touch on this skill: probably W21 if it stays in the
LRT bucket, otherwise close on human approval of the rewrite.

---

## 2026-05-28 — W22 POLISH pass (carryover from W20)

**Mode:** POLISH (Thursday)
**File reviewed:** `skills/hara-builder.skill` (ZIP archive; SKILL.md is 12,273 bytes / 146 lines).
**Tracking issue:** [#3](https://github.com/jherrodthomas/automotive-skills-suite/issues/3) (open).
**Plan reference:** target #2 in `docs/weekly/WEEK-2026-W22.md` (carryover from W20).

### File-state check vs. previous pass

The archive has not changed since the W20 pass (`2026-04-28 07:08` mtime on every
entry inside the `.skill`). The SKILL.md byte size, line count, and frontmatter all
match what the 2026-05-12 entry above recorded. So everything that was true then is
still true now — no regressions, no drift.

Re-ran the trigger-coverage check fresh against the current file rather than trusting
the prior log:

| DoD check (W20) | This pass |
|---|---|
| Description ≤ 1024 chars | **PASS** — 911 chars (unchanged) |
| Frontmatter has required fields (`name`, `description`) | **PASS** |
| `HARA` in first 400 chars | **PASS** — char 71 |
| `hazard analysis` in first 400 chars | **PASS** — char 34 |
| `ASIL` in first 400 chars | **PASS** — char 285 |
| `item definition` in first 400 chars | **PASS** — char 94 |
| `safety goal` in first 400 chars | **FAIL** — char 454 (same gap the W20 pass found) |
| `ISO 26262` in first 400 chars | **PASS** — char 24 |
| `concept phase` (bonus, not strict DoD) | **FAIL** — char 672 |

Also did a fresh typo scan (`teh recieve seperate occured definately accomodate
begining wether`) — no hits. Step headers all use em-dashes consistently (Step 1
through Step 5). Archive file tree matches the "Files in this skill" block at the
bottom of SKILL.md exactly (cross-checked against `unzip -l`).

### Autonomous-edit allowlist decision

The autonomous-edit allowlist is narrow: **typo / description > 1024 chars / missing
required frontmatter field**. None of the three conditions hold here. The only open
DoD gap — `safety goal` reordered into the first 400 chars — is an editorial sentence
re-ordering, not a surgical fix. Per the standing rule ("NEVER do large refactors —
small and shipped beats big and broken"), I am leaving the .skill file untouched.

**No edits committed in this run.** The proposed rewrite from the 2026-05-12 entry
above remains the recommended human action.

### Why not just apply the W20-proposed rewrite this run?

Two reasons. (a) The proposed rewrite touches ~600 chars of the 911-char description
— that crosses the line from "small obvious fix" into "editorial restructure," which
the spec reserves for human review. (b) The rewrite was drafted two POLISH cycles
ago and the suite-wide trigger-coverage pattern (hara, cs-concept, aspice — three
in a row, see notes at the bottom of the W20 entries) has been flagged for a
suite-wide DoD audit in W21/W22 PLANs. A point fix here would short-circuit that
audit before it lands.

### Severity roll-up (unchanged from W20)

| Finding | Severity | Action |
|---|---|---|
| `safety goal` outside first 400 chars | low | proposed rewrite drafted in W20 entry; await human review |
| Long "Produces..." enumeration clause | low | folded into the same proposed rewrite |
| No `concept phase` trigger up front | low (optional) | folded into the same proposed rewrite |

### Follow-ups for the human

1. **Approve or modify the W20 proposed rewrite of the description** (block quote near
   the end of the 2026-05-12 entry). If approved, it can land as a one-shot edit and
   issue [#3](https://github.com/jherrodthomas/automotive-skills-suite/issues/3)
   closes the same day.
2. **Decide whether the suite-wide trigger-coverage DoD audit is W23 PLAN-eligible.**
   Three consecutive POLISH passes (hara → cs-concept → aspice) have shipped the
   same shape; one focused audit pass is cheaper than chasing individual skills.
3. **Optional:** consider adding `concept phase` to the canonical trigger phrase
   list applied during the audit, since the same gap is likely present on
   `fsc-builder` and `tsc-builder` too.

This is the second POLISH visit to `hara-builder` and the file is unchanged between
visits. If the human review of the W20 rewrite does not land before next month,
the LRT rotation will surface this skill a third time — recommend either closing
issue #3 with a "won't fix (within autonomous scope)" note, or merging the W20
rewrite, to break the loop.
