# tara-builder polish log

_Polish target for W23 (issue [#16](https://github.com/jherrodthomas/automotive-skills-suite/issues/16)). Reviewer: autonomous daily-standup task._

---

## 2026-06-03 — first POLISH pass

**Mode:** POLISH (Wednesday)
**File reviewed:** `skills/tara-builder.skill` (ZIP archive; SKILL.md is 12,279 bytes / 150 lines).
**DoD recap (from `docs/weekly/WEEK-2026-W23.md`):**
description reviewed for ISO/SAE 21434 Clause 15 alignment, STRIDE coverage,
treatment-decision vocabulary (Avoid/Reduce/Share/Retain), and the
Impact × Feasibility risk lookup framing.

### What's good

- **Frontmatter is clean.** Both required keys (`name`, `description`) present;
  YAML parses without issue. Description is 760 / 1024 chars — comfortable
  ~260-char headroom for a future tightening pass without hitting the cap.
- **Clause anchor is explicit and correct.** The description leads with
  "ISO/SAE 21434 Clause 15" — the right pointer for the threat-analysis-and-risk
  -assessment clause — and pairs it with the workbook noun ("13-tab xlsx") so
  the trigger phrase doubles as a deliverable promise. Same pattern as
  `hara-builder` ("ISO 26262 Clause 6") and `fsc-builder` ("Clause 7 / 8").
- **STRIDE vocabulary is canonically named.** Section 2 references the six
  STRIDE pillars (Spoofing, Tampering, Repudiation, Info Disclosure, Denial
  of Service, Elevation of Privilege) inside the threat-taxonomy bullet, and
  the reference doc (`stride_taxonomy.md`) is wired into the workflow at
  Step 2. Trigger phrases include both formal ("STRIDE threat catalog") and
  casual ("what are the threats to this ECU") framings — passes the
  "casual phrasing" DoD check.
- **Treatment vocabulary is consistent.** "Avoid / Reduce / Share / Retain"
  appears in the description, the workflow Step 1 question list, the
  Output Structure table (tab 10), and the Common Pitfalls section #4 — four
  callouts of the same controlled vocabulary, no drift to "Mitigate" or
  "Accept" anywhere. Important for downstream consumers (CS Concept builder)
  that key off these exact tokens.
- **CAL framing is right.** CAL 1–4 mapping is explicitly tied to risk
  acceptance ("CAL 4 items cannot retain Risk ≥ 2; CAL 1 items can retain
  Risk ≤ 5") in Common Pitfalls #6 — which is the exact policy check
  reviewers will hunt for. Saves the analyst from defending the rule in the
  audit room.
- **Pipeline hand-off is explicit.** Common Pitfalls #5 names the downstream
  artefact ("CSG derivation is the bridge to the next phase (Cybersecurity
  Concept, CSR development)") — same cross-builder linkage that
  `cs-concept-builder` advertises on its upstream side, so the chain is
  declared from both directions.
- **Output Structure table is complete and well-formed.** 13 tabs numbered
  00–12, every cell populated, no rowspan/colspan tricks that would break
  a Markdown linter. Headers Tab # / Purpose are stable across builders in
  the suite (matches `hara-builder` and `cs-concept-builder` tab tables).

### What to fix

- **Severity: medium — internal contradiction in "Auto-rating Heuristics".**
  The section opens with: _"TARA auto-rating is limited. The skill requires
  user-supplied Impact and Feasibility ratings in the JSON; it does NOT
  auto-suggest."_ Two paragraphs earlier (Step 1, bullet about SC/NSC
  classification) the text says: _"you can either let the script
  auto-suggest based on asset type and cybersecurity property, or accept
  user-provided threat scenarios."_ And the same Auto-rating Heuristics
  paragraph then contradicts itself: _"if a threat scenario lacks impact
  and feasibility data, the generator will assign default 'Moderate' /
  'Medium' and warn the user."_ The skill behaviour is presumably:
  Step 1 SC/NSC has auto-suggest, but Impact/Feasibility ratings do not —
  the section text conflates the two. Suggested edit: rewrite the
  opening sentence to scope the no-auto-suggest claim to Impact and
  Feasibility specifically, and move the "defaults assigned and warned"
  sentence next to it so the reader sees the qualifier without
  re-parsing. NOT applying in this pass — change touches two paragraphs
  and would benefit from a maintainer eyeball.
- **Severity: low — Step 4 shell block is shell-only.** The `python
  scripts/generate_tara.py <input.json> <output.xlsx>` block has no
  Windows-equivalent note. Other builders in the suite (cs-concept,
  dfmea) include a `# Windows: python.exe ...` parenthetical for
  cross-platform users. Add one for consistency.
- **Severity: low — Step 5 review checklist conflates "high-Risk" vs
  "Risk ≥ 3".** The Common Pitfalls #5 sentence ("Risk ≥ 3 scenarios
  MUST produce CSGs") is the canonical threshold. Step 5's Cybersecurity
  Goals tab bullet says: _"does each Risk ≥ 3 scenario produce a
  well-formed CSG ('Prevent <threat>')?"_ — consistent. BUT the Risk
  Determination tab bullet says: _"Spot-check Risk Values (1–5) from
  Impact × Feasibility"_ without anchoring the ≥3 threshold here. Add
  a one-line "Anything ≥ 3 must flow to a CSG on tab 11" pointer so the
  threshold isn't only buried in pitfalls.
- **Severity: low — references list duplicates a fact.**
  `references/cal_levels.md`, `references/risk_determination_matrix.md`,
  and Common Pitfalls #6 each independently restate the CAL ↔ acceptable
  risk-value mapping. Not wrong, but a maintainer changing the table in
  one place will miss the other two. Add a comment in `cal_levels.md`
  noting it's the canonical source so future edits are routed there.

### Suggested edits (not applied this pass)

1. Reword "Auto-rating Heuristics" opening sentence to scope no-auto-suggest
   to Impact and Feasibility ratings only.
2. Add Windows shell variant under Step 4.
3. Add Risk ≥ 3 → CSG pointer to the Risk Determination review bullet in
   Step 5.
4. Mark `references/cal_levels.md` as canonical source for the CAL ↔ risk
   acceptance mapping.

### Applied this pass

None — all four suggested edits touch substantive prose, not typo / frontmatter
territory. Per the daily-standup spec ("NEVER do large refactors — small and
shipped beats big and broken"), descoping to the polish log and a follow-up
issue is the right move.

### Overall verdict

`tara-builder.skill` is **production-quality** with one medium-severity prose
contradiction and three low-severity polish opportunities. Description fits
the 1024-char budget with headroom. Trigger phrases cover formal and casual
phrasings. Pipeline linkage to CS Concept is declared. Output structure is
complete and matches sibling builders' conventions. Ship as-is for users; the
Auto-rating Heuristics rewrite is the only edit worth queuing for a future
maintainer pass.

**Severity:** low overall, with one medium item flagged for follow-up.

**Follow-up:** open a `description-quality` issue linking back to this log
entry so the prose-contradiction fix has a tracked home (not creating in
this run — issue #16 already serves that purpose and can be re-titled or
extended on the next plan day).
