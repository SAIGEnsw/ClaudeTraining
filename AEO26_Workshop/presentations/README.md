# AEO26 Workshop 1: presentation outlines (P1–P4)

Outlines with speaker notes for "Practical and Sovereign AI for Earth Observation: a hands-on agentic coding workshop", Mon 9 Nov 2026, Nipaluna/Hobart. **No slide decks, by the presenters' decision** (WORKSHOP_PLAN.md B13, update of 5 Nov). Talks run while participants' cloud agents do their research (launched by 10:10; audit at 12:00).

Source: WORKSHOP_PLAN.md B3 (run sheet) and B8 (outlines). Drafted 5 Nov 2026.

| Talk | File | Slot (B3) | Length | Presenter | Status |
|---|---|---|---|---|---|
| P1 Three levels of agency, and what the evidence says | [P1_agency_evidence.md](P1_agency_evidence.md) | 10:10–10:25 | 15 min | TBC (Adam Roff or Fang Yuan) | Draft |
| P2 Monaro Grasslands: an agentic research programme in practice | [P2_monaro.md](P2_monaro.md) | 10:25–10:50 | 25 min | Adam Roff | **Needs clearance** |
| *Morning tea* | | 10:50–11:20 | 30 min | | |
| P3 Traditional vs foundation models for forest mapping | [P3_traditional_vs_foundation.md](P3_traditional_vs_foundation.md) | 11:20–11:35 | 15 min | TBC (Adam Roff or Fang Yuan) | Draft |
| P4 Sovereign AI: waratah_mcp on the DGX Spark / HP Z6, and how to replicate it | [P4_sovereign_ai.md](P4_sovereign_ai.md) | 11:35–12:00 | 25 min | Guest speaker (likely Adam Roff; TBC) | **Needs waratah_mcp** (demo is a placeholder) |

Each outline is a numbered sequence of slide-equivalent sections (heading, on-screen bullets, speaker notes, timing, references), ending with an APA reference list. References come only from the plan's verified lists, plus one new reference in P3 (Linley et al., 2025, Oikos), verified with Scite and Crossref on 5 Nov 2026.

## Open dependencies

**P2 (Monaro)**
- [ ] Presenter approval to show P2 in public (B11, B13 item 4). Until then, the banner stays and the file stays out of staging and production.
- [ ] Confirm against the project's errata which quoted figures are post-errata, and which are maxima over 50–124 runs (best balanced accuracies 0.813 / 0.687; LR vs RF; AlphaEarth vs larger stacks).
- [ ] Confirm whether the Presto vs AlphaEarth comparison ("8 of 10 folds") used the uncommitted ten-fold scheme flagged by the red team; if so, say so on screen.

**P4 (sovereign AI)**
- [ ] Access to the waratah_mcp repository, then write section 2 (B13 item 2).
- [ ] Clear waratah_mcp material for public showing (B14 gate).
- [ ] Rehearse the Teams screen-share from the Spark/Z6 and record the backup video (B11).
- [ ] Capture the `docs/local_ai/` screenshots on a clean Linux machine (W8).
- [ ] Check the open-weights model's licence.
- [ ] Re-check the Ollama Anthropic-compatibility docs (minimum version, required environment variables) and confirm the URL of Anthropic's sandbox/isolation guide before linking it.
- [ ] Check the current versions of the DTA policy and the NSW AI Assessment Framework.

**P1 and P3**
- [ ] Assign presenters for P1 and P3.
- [ ] P3 section 4: Adam to add a one-sentence key finding for each of Backman et al. (2026a, 2026b).
- [ ] Before any published version: complete the "et al." author lists (Lacoste 2023, Szwarcman 2024 and others) and check the USENIX Security 2025 citation for Spracklen et al. and the NeurIPS 2023 citation for Lacoste et al.

**All talks**
- [ ] Timed read-through at the Sun 8 Nov dry run (P1 15, P2 25, P3 15, P4 25 min).
- [ ] Put the `docs/setup/` QR code on screen during each talk (B9).
