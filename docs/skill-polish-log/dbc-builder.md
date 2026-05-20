# dbc-builder polish log

_Polish target for W21 (issue [#7](https://github.com/jherrodthomas/automotive-skills-suite/issues/7)). Reviewer: autonomous daily-standup task._

---

## 2026-05-20 — first POLISH pass (W21 Wednesday)

**Mode:** POLISH (Wednesday)
**File reviewed:** `skills/dbc-builder.skill` (ZIP archive; `SKILL.md` is 6,878 bytes / 115 lines).
**DoD recap (from `docs/weekly/WEEK-2026-W21.md`, target #2):**
description trigger list includes "CAN database," "signal definition," "DBC file,"
"Vector CANdb," and at least one casual phrasing ("define CAN messages");
description ≤ 1024 chars; frontmatter complete.

### DoD verdict

| DoD check | Result | Evidence |
|---|---|---|
| Trigger phrase "CAN database" | **PASS** | verbatim twice — "from a CAN database definition" and in the "mentions DBC, CAN database, Vector DBC..." trigger list |
| Trigger phrase "signal definition" | **MARGINAL PASS** | present only as a substring of "signal definitions with bit positions" in the descriptive sentence; **not** in the explicit "mentions ..." trigger list (which carries "signal catalog" instead) |
| Trigger phrase "DBC file" | **FAIL** | description carries "DBC" and "Vector DBC" but never the bigram "DBC file"; "DBC file" appears only in the SKILL.md body |
| Trigger phrase "Vector CANdb" | **FAIL** | description says "Vector DBC"; Vector's authoring tool / format family is CANdb (CANdb++). The DoD-specified phrase "Vector CANdb" is absent |
| At least one casual phrasing | **PASS** | two present — "document our CAN signals" and "audit the message catalog" (the DoD's "define CAN messages" is one example of the class, not a literal requirement) |
| Description ≤ 1024 chars | **PASS** | 674 chars — generous headroom |
| Frontmatter complete | **PASS** | `name` + `description` both present; YAML parses; no drift fields |

**Headline:** the W20 pattern (one missing formal trigger phrase per polish target)
**resumes and doubles** — dbc-builder is missing two DoD trigger phrases, not one.
Predicted in yesterday's 8d log ("comms cluster hasn't been touched yet so
trigger-coverage drift is plausible"); confirmed. Separately, this pass turned up a
**doc-vs-archive accuracy defect** that is more impactful than the trigger gaps (see
"What to fix" #3).

### What's good

- **11-tab claim is exact and matches the generator.** Cross-checked SKILL.md's
  "11-tab xlsx" + the Output structure table against `scripts/generate_dbc.py`:
  the generator emits `00_Title_Page` · `01_Document_Control` · `02_ECU_Inventory` ·
  `03_Message_Catalog` · `04_Signal_Catalog` · `05_Signal-to-Message_Mapping` ·
  `06_Value_Tables` · `07_Signal_Group_Definitions` · `08_Network_Topology` ·
  `09_Validation_Rules` · `10_References` — 11 `create_sheet` calls, 00–10. Count,
  names, and ordering all match the doc. No drift between description, the Output
  structure table, and code.
- **Description char count is generous.** 674 / 1024 — ~350 chars of headroom, so the
  proposed rewrite (adds three trigger phrases + one casual phrasing) lands well under
  the cap.
- **Frontmatter is clean.** Both required keys present; YAML parses; no stray fields.
- **Body is well-structured.** Five-step workflow (Capture → Read refs → Build JSON →
  Generate → Present/iterate), an explicit "When to use" list, a "Common pitfalls"
  list, and a "Files in this skill" tree. Among the more complete SKILL.md bodies in
  the suite — the comms cluster is in good shape structurally.
- **Standards anchoring is correct.** CAN 2.0A/2.0B, ISO 11898-1:2015, AUTOSAR
  Communication module all cited accurately; generator's References tab matches.
- **JSON-is-source-of-truth guidance is explicit.** Step 5 tells the model to edit the
  input JSON and regenerate rather than hand-editing the xlsx — good operational
  hygiene, consistent with peer builders.

### What to fix

1. **"DBC file" absent from the description trigger list.** DoD-required phrase.
   The description has "DBC" and "Vector DBC"; a user who types "generate a DBC file"
   or "we need a DBC file for this bus" gets only a partial-token match on "DBC".
   Adding the literal bigram "DBC file" to the "mentions ..." list closes it.
   Severity: **medium** (DoD gap, common user phrasing).

2. **"Vector CANdb" absent; description says "Vector DBC" instead.** DoD-required
   phrase. "Vector DBC" and "Vector CANdb" are both real — CANdb / CANdb++ is Vector's
   DBC-authoring product line — but the DoD specifies "Vector CANdb". Best fix keeps
   *both*: a user may search either. Severity: **medium** (DoD gap).

3. **`SKILL.md` "Files in this skill" tree claims `examples/sample_input_can.json`,
   but the archive ships NO `examples/` directory.** Extracted the `.skill` ZIP and
   enumerated all 7 files: `SKILL.md`, `references/dbc_conventions.md`,
   `references/methodology.md`, `scripts/generate_dbc.py`, `scripts/recalc.py`,
   `scripts/office/__init__.py`, `scripts/office/soffice.py`. There is no
   `examples/` folder. Yet the SKILL.md tree block lists
   `examples/sample_input_can.json   # full example to clone for new projects`, and
   Step 3 of the workflow leans on cloning a worked example. This is a real accuracy
   defect: a model/engineer following SKILL.md will look for a file that was never
   packaged. Note the contrast with `8d-problem-solving-builder` (yesterday's target),
   whose archive *does* ship a populated `examples/sample_8d_input.json`. Two
   candidate remedies: (a) author and bundle `examples/sample_input_can.json` — the
   generator docstring already documents the schema, so a worked CAN example is
   straightforward to produce; or (b) drop the `examples/` line from the tree and
   soften Step 3. Remedy (a) is preferred — it brings dbc-builder to parity with the
   suite norm. Severity: **medium** (doc references a non-existent shipped artifact).

4. **"signal definition" only reaches the description as a substring.** It rides
   inside "signal definitions with bit positions" in the descriptive sentence; the
   explicit trigger list carries "signal catalog" but not "signal definition". A
   substring match is weaker than a list entry. Folding "signal definition" into the
   "mentions ..." list makes the DoD check unambiguous. Severity: **low**.

5. **Casual-phrasing coverage is adequate but misses the canonical verb "define".**
   The two casual phrasings present ("document our CAN signals", "audit the message
   catalog") are document/audit framed. A very common authoring-intent phrasing —
   "define CAN messages" / "define the CAN signals" — has no match. The DoD's
   parenthetical names exactly this phrase. Adding it is cheap and closes a realistic
   invocation path. Severity: **low**.

### Suggested edits (NOT applied this run)

The autonomous-edit allowlist for the daily-standup task is narrow — typo,
over-length description, missing required frontmatter field. None of findings #1–#5
match that list: #1, #2, #4, #5 are editorial trigger-coverage rewrites and #3 is
either a content-authoring task (bundle a new example file) or a doc accuracy rewrite.
Per the hard rule "stop short of any change you'd need to think hard about," and
consistent with the 8d-problem-solving-builder pass on 2026-05-19, **no edits were
committed to the `.skill` archive this run.** Findings captured here for the next
human review pass. Issue #7 stays open with this log linked from the journal entry.

Minimal proposed rewrite of the description — adds "DBC file", "Vector CANdb"
(keeping "Vector DBC"), "signal definition", and the casual phrasing "define CAN
messages" to the trigger surface; all existing phrases preserved; 741 chars, under
the 1024 cap:

```
description: Generate an audit-ready DBC specification workbook from a CAN database
  definition. Produces an 11-tab xlsx capturing ECU inventory, message catalog with
  IDs and DLC, signal definitions with bit positions/length/byte order/scaling/units/
  value tables, message-signal allocation, network topology, and validation rules per
  CAN 2.0A/2.0B and ISO 11898. Use this skill whenever the user mentions DBC, DBC
  file, CAN database, Vector CANdb, Vector DBC, communication matrix for CAN, message
  catalog, signal catalog, signal definition, or CAN message specification, even in
  casual phrasings like "document our CAN signals", "define CAN messages", or "audit
  the message catalog". Always use this skill instead of producing freeform DBC notes
  in chat.
```

| Check | Result |
|---|---|
| Char count | 741 (under 1024 cap) |
| "CAN database" in trigger list | yes |
| "DBC file" in trigger list | yes (added) |
| "Vector CANdb" in trigger list | yes (added) |
| "signal definition" in trigger list | yes (added) |
| Casual phrasing "define CAN messages" | yes (added) |
| Existing phrases preserved | yes — DBC, Vector DBC, communication matrix for CAN, message catalog, signal catalog, CAN message specification all retained |

Finding #3 (the missing `examples/` file) is **not** addressed by the description
rewrite — it needs either a new bundled example JSON or a tree-block edit, and is
left for human review.

### Other observations (not fixes, just notes for future passes)

- The comms cluster (8 builders) all key off this skill's vocabulary —
  communication-matrix, bus-load, gateway-routing, arxml-system. The
  `examples/` gap (finding #3) is worth a spot-check across the rest of the comms
  cluster on a future pass: if dbc-builder shipped without its example, sibling
  comms builders may have the same packaging miss.
- `references/dbc_conventions.md` mentions J1939 awareness; the description does not.
  J1939 is a CAN higher-layer protocol with its own large user base. Not a DoD item
  and arguably out of scope for a generic DBC builder, but a "J1939" trigger token is
  a plausible future addition if the suite ever wants J1939 reach. Flagged, no action.
- CAN FD: Step 1 of the workflow mentions "CAN FD awareness" in the interview prompts,
  but neither the description nor the Output structure surfaces CAN FD. If the
  generator does not actually model CAN FD (64-byte payloads, BRS), the workflow
  prompt slightly over-promises. Worth a generator spot-check next pass — out of DoD
  scope today.
- `recalc.py` and `scripts/office/soffice.py` are present — the shared helper pair
  every builder in the suite carries. Consistent.

### Severity roll-up

| Finding | Severity | Action |
|---|---|---|
| "DBC file" absent from trigger list | medium | proposed rewrite drafted; await human review |
| "Vector CANdb" absent (has "Vector DBC") | medium | folded into the same rewrite |
| `examples/sample_input_can.json` claimed but not shipped | medium | needs bundled example or tree edit; left for human |
| "signal definition" only a substring, not a list entry | low | folded into the rewrite |
| Casual phrasing "define CAN messages" missing | low | folded into the rewrite |

**No code edits committed in this run.** Issue #7 stays open with this log linked.
The W20 trigger-coverage-drift pattern resumed and intensified (two missing DoD
phrases vs. one). The standout finding is non-DoD: the SKILL.md "Files in this skill"
tree advertises an `examples/` file the archive never shipped. Recommendation for the
next POLISH (W21 #8 autosar-swc-builder, Thursday): when extracting the archive,
explicitly diff the SKILL.md "Files in this skill" tree against the actual ZIP
contents — the dbc-builder `examples/` miss suggests this drift may be cluster-wide.
