# Automotive Skills Suite — Releases

Weekly snapshots of `github.com/jherrodthomas/automotive-skills-suite`. Tags are lightweight; the human clicks Publish on GitHub after reviewing this file.

---

## v2026.05.W20 — 2026-05-16

ISO week 20 (2026-05-11 → 2026-05-16). First weekly snapshot of the autonomous run cadence.

### Highlights

- Three builders moved from "shipped" to "reviewed against the polish playbook": `hara-builder`, `cs-concept-builder`, `aspice-assessment-builder`. Each now carries a dated entry in `docs/skill-polish-log/` with severity-rated findings.
- Weekly target file `docs/weekly/WEEK-2026-W20.md` opened the loop on Monday and the four GitHub issues it spawned are now mostly serviced; one (`8d-problem-solving-builder`) carries forward to W21.
- Suite still at 76 builder + 76 reviewer pairs, 100% paired (`item-definition-builder` ↔ `item-def-checklist-reviewer` and `ppap-package-builder` ↔ `ppap-checklist-reviewer` are alias pairs and now reflected correctly in STATUS).

### Changes this week

**plan**
- `16df7a5` auto(plan): W20 targets — hara, cs-concept, aspice-assessment, 8d

**polish**
- `5b6ad4e` auto(polish): regen STATUS.md and log W20 hara-builder polish findings
- `49fd9a9` auto(polish): regen STATUS.md and log W20 cs-concept-builder polish findings
- `c4fad8a` auto(polish): W20 #5 aspice-assessment-builder pass, regen STATUS.md

**docs / release** _(this snapshot commit)_
- STATUS.md refreshed (no flag changes vs. prior run)
- RELEASES.md created
- `docs/AUTONOMOUS_LOG.md` updated with RELEASE-mode entry

### Skills inventory

- Builders: 76
- Reviewers: 76
- Paired: 76/76 (100.0%)
- Domain spread: safety=12, quality=8, comms=8, program-mgmt=6, cyber=6, autosar=5, diagnostics=5, v&v=5, aspice=4, sysml=4, mbse=3, sotif=3, calibration=3, other=4

### Open issues at snapshot

- #6 W20 polish target: 8d-problem-solving-builder.skill _(carries to W21)_
- #5 W20 polish target: aspice-assessment-builder.skill _(serviced this week, awaiting close-out)_
- #4 W20 polish target: cs-concept-builder.skill _(serviced this week, awaiting close-out)_
- #3 W20 polish target: hara-builder.skill _(serviced this week, awaiting close-out)_
- #2 goodd _(needs human triage — non-conforming title, no domain signal)_

### Compare

[`3c69553...v2026.05.W20`](https://github.com/jherrodthomas/automotive-skills-suite/compare/3c69553...v2026.05.W20)

### Notes for the human

Click Publish on the v2026.05.W20 tag on GitHub once you've skimmed this section. Issue #2 (`goodd`) is the one item Sunday's TRIAGE pass should probably still escalate to you — autonomous run is intentionally not labeling or commenting it because confidence is well below 80%.

---

## v2026.05.W21 — 2026-05-23

ISO week 21 (2026-05-18 → 2026-05-23). Second weekly snapshot — W21 polish cycle closed; CHANGELOG and the first `examples/` doc stubs landed.

### Highlights

- W21 polish ran the full Tue/Wed/Thu cadence over `8d-problem-solving-builder`, `dbc-builder`, and `autosar-swc-builder`; the autosar-swc pass shipped an actual in-allowlist description fix, the other two produced severity-rated polish-log findings held for human review.
- `CHANGELOG.md` introduced and the first `examples/<skill>/README.md` stubs added — the repo now has a docs spine beyond STATUS.
- Suite steady at 76 builder + 76 reviewer pairs, 100% paired; no flag changes vs. W20.

### Changes this week

**plan**
- `3320c23` auto(plan): W21 targets — 8d carryover, dbc, autosar-swc, uds-services, classifier-freeze

**polish**
- `e8ff3b2` auto(polish): W21 #6 8d-problem-solving-builder pass, regen STATUS.md
- `76b2fff` auto(polish): W21 #7 dbc-builder pass, regen STATUS.md
- `387bbcd` auto(polish): W21 #8 autosar-swc-builder pass — description fix applied, STATUS regen

**docs**
- `1a5ca80` auto(docs): add CHANGELOG, W21 example README stubs, regen STATUS

**docs / release** _(this snapshot commit)_
- STATUS.md refreshed (no flag changes vs. prior run)
- RELEASES.md updated with the W21 section
- CHANGELOG.md `[Unreleased]` rolled into a dated `[v2026.05.W21]` section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

### Skills inventory

- Builders: **76** · Reviewers: **76** · Paired ratio: **100.0%** (76/76)
- Domain spread: safety 14, quality 8, comms 8, program-mgmt 6, cyber 6, autosar 5, diagnostics 5, v&v 5, aspice 4, sysml 4, calibration 3, mbse 3, sotif 3, other 2

### Compare

https://github.com/jherrodthomas/automotive-skills-suite/compare/v2026.05.W20...v2026.05.W21

### Open items for the human

- 10 issues open. Issue **#2** ("goodd", empty body) has stayed `needs-triage` across multiple runs — needs a human decision (likely close as spam/invalid).
- Issue **#11** ("packaging utility for Claude-compatible skill exports") is still unlabeled — Sunday TRIAGE should label it.
- W21 polish target **#9** (`uds-services-builder`) and tooling target **#10** (classifier freeze) were not serviced this week — both carry to W22.
