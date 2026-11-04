# Course plan: Practical and Sovereign AI for Earth Observation

### A hands-on agentic coding workshop: AEO26 Workshop 1

**Status:** Plan only (draft v0.1, 4 Nov 2026). No course content has been built yet.
**Skeleton repo:** [RSGISagent/AEO26_AgenticAI_Workshop](https://github.com/RSGISagent/AEO26_AgenticAI_Workshop). This plan fills in its `coursework/` folder.

| | |
|---|---|
| **When** | Monday 9 Nov 2026, 09:00 to 12:30. That is 180 min of teaching plus a 30 min morning tea. |
| **Where** | AEO26, Nipaluna/Hobart |
| **Presenters** | Adam Roff (DCCEEW NSW), Fang Yuan (EOA), plus a guest speaker (TBA) |
| **Audience** | Remote sensing, GIS and EO practitioners with mixed coding experience, from none to developer |
| **Participant kit** | A free Claude account, plus a browser or the mobile app. No installs. |
| **Through-line** | One Australian case study runs through every module, so each level of agency adds to the same analysis |

---

## 1. Constraints this plan is designed around (verified 4 Nov 2026)

These decide what participants can actually do, so the exercises are built around them.

| Constraint | Implication for design | Source |
|---|---|---|
| Free accounts **do** get code execution and file creation, web search and artifacts. Uploads are limited to 30 MB per file. | Modules 1–2 run entirely in claude.ai on the free tier. | [Create and edit files](https://support.claude.com/en/articles/12111783-create-and-edit-files-with-claude) |
| The free sandbox's network access covers **approved package sources** (PyPI, npm, GitHub and similar). It is unclear whether free users can open egress to arbitrary domains. | Stage all workshop data on GitHub, which is reachable, and offer upload as a fallback. Don't rely on live DEA/STAC calls from free accounts. **Test this before the day** (see §9). | same |
| **Claude Code cloud sessions need Pro or above.** | The autonomous Claude Code run in Module 3 is a **presenter demo**. Pro attendees can follow along on an optional track. Free attendees run an autonomous long-task brief in claude.ai instead. | [Claude Code on the web](https://code.claude.com/docs/en/claude-code-on-the-web) |
| Free-tier usage limits are usage-based and not published as a message count. Third-party estimates suggest roughly 15–40 messages per 5 h, but this is unverified. | Budget about 12–15 prompts per account across the morning. Work in **pairs and alternate accounts** between modules. Every exercise has a pre-generated fallback output. | measure in pre-flight |
| Claude Academy now lives at academy.claude.com and uses the same course slugs as Skilljar. | Point the pre- and post-workshop links at academy.claude.com. Check the existing Skilljar links in the README still redirect. | [academy.claude.com sitemap](https://academy.claude.com/sitemap.xml) |

---

## 2. Learning outcomes

By the end of the workshop, participants will be able to:

1. **Distinguish** the three levels of AI agency (vibe coding, agentic tool use, autonomous agents) and **choose** the right one for an EO task.
2. **Write** an outcome-focused prompt that produces a working geospatial analysis from open Australian data, and that states its assumptions (CRS, dates, filters).
3. **Direct** an agent to chain tools (search, code execution, files, artifacts) and **inspect** which tools it used and why.
4. **Audit** an autonomous agent's output with a structured checklist covering provenance, spatial and temporal correctness, numbers, packages, citations and side effects.
5. **Explain** the sandbox boundary and the data-sovereignty trade-offs between cloud frontier models and local open-weights models, using Australian governance frameworks.

These map to the **4D AI Fluency framework** taught on Claude Academy. It gives the workshop a shared vocabulary and connects directly to the pre-reading:

| 4D | Where it is practised |
|---|---|
| **Delegation**: what to hand to AI, and at what level of agency | Framing, plus the Module 4 decision matrix |
| **Description**: prompts, mission briefs, CLAUDE.md | Modules 1, 2 and 3a |
| **Discernment**: is the output right? | The check step at the end of every module, plus the Module 3b audit |
| **Diligence**: responsibility, disclosure, sovereignty | Module 4 and the wrap-up |

---

## 3. Pedagogical design

- **Follow Academy's lesson rhythm.** Academy alternates short concept lessons with "Try it out" pages and ends each course with a quiz (e.g. `ai-capabilities-and-limitations`). Mirror that in every module: **concept (≤ 6 min), then a live demo ("I do"), then hands-on in pairs ("we/you do"), then a check.**
- **Pair work in driver/navigator roles.** The driver prompts and the navigator checks. Swap roles and accounts at each module. This halves usage per account, gives mixed-skill rooms a built-in peer tutor, and makes the auditor role visible.
- **Tiered tasks.** Every hands-on has a *Starter* task (copy-paste prompt), an *Extend* task (modify it) and a *Stretch* task (open goal). Everyone succeeds, and no one is bored.
- **Answer keys for Discernment.** Every numeric result participants produce has an answer computed by a reviewed Python script in the repo. "Did you get 37 %? If not, why not?" is the core teaching moment.
- **One case study.** The data that gets mapped in Module 1 is reused in Module 2's tool chain and Module 3's autonomous run. The analysis grows, but cognitive load doesn't.
- **Make time do the teaching.** The autonomous run is **launched just before morning tea and audited just after it**. "Set a goal, walk away" happens literally.

---

## 4. Case study and data package

### Recommended through-line: Black Summer fire and a threatened species

*How much greater glider habitat burnt in 2019–20, and how badly? Is it recovering?*

This suits the presenters' expertise in vegetation and biodiversity mapping. It covers the README's themes (vegetation, biodiversity, land cover, disaster resilience) and uses well-documented open data.

**Alternative with a local hook:** the 2019 Tasmanian Wilderness fires (Gell River) and fire-sensitive endemic conifers (e.g. *Athrotaxis* spp.). The structure is the same and only the data changes. The licence and data availability of Tasmanian fire-history layers still need checking.

### Data package (`coursework/data/`). Pre-clip everything and keep each file under 5 MB.

| File | Source | Licence (verify) | Used in |
|---|---|---|---|
| `glider_occurrences.csv` | ALA download for *Petauroides volans* within the AOI bbox, 2000–2025, coordinate uncertainty ≤ 1 km. Keep the download DOI. | Per record, mostly CC BY 4.0. Cite the ALA download DOI. | M1, M3 |
| `fire_severity_2019_20.geojson` | NSW Fire Extent and Severity Mapping (FESM), clipped and simplified | Check on SEED (expected CC BY 4.0) | M1, M2 |
| `s2_pre_fire.tif`, `s2_post_fire.tif` | DEA Sentinel-2 ARD (or Earth Search L2A): B8A and B12 at 20 m, about 10 × 10 km AOI, as cloud-optimised GeoTIFF | DEA CC BY 4.0 / Copernicus licence | M2, M3 |
| `ndvi_timeseries.csv` | Monthly NDVI medians at burnt and unburnt sample points, 2017–2025, from DEA ARD | CC BY 4.0 | M3 |
| `landcover_aoi.tif` *(stretch)* | DEA Land Cover, clipped | CC BY 4.0 | M2 alt |
| `seeded_agent_report.md` | Written by the presenters, with planted errors (see §6, Module 3b) | n/a | M3b |
| `MANIFEST.csv`, `ATTRIBUTION.md` | Source URL or DOI, licence, access date, processing step and SHA-256 for each file | n/a | all (and the provenance lesson itself) |

**Data prep script:** `scripts/prepare_data.py` (Python). Workshop participants never run it, but it makes the package reproducible and generates the answer keys.

- Use `pystac-client` and `odc-stac`/`rioxarray` for the rasters, and `geopandas`/`shapely` 2 for vectors.
- Pin the environment (`environment.yml` or a `uv` lockfile). Use typed functions, an `argparse` CLI and deterministic outputs with checksums.
- Write `scripts/answer_keys.py` to compute every number the exercises ask for. Lint it with `ruff` and add a small `pytest` suite.
- Run all area calculations in an **equal-area CRS (EPSG:3577, GDA94 / Australian Albers)**. This is also a teaching point, because agents often compute areas in degrees.

---

## 5. Run sheet

The rule if the conference fixes a different morning-tea time: **Module 3a is always the last 10 minutes before the break.** Shift the other blocks around it.

| Time | Min | Block | Format |
|---|---|---|---|
| 09:00 | 15 | **0. Welcome and setup.** Acknowledgement of Country, goals, safety briefing, login check, form pairs. | Plenary + room check |
| 09:15 | 10 | **Framing: three levels of agency, and what the evidence says** | Talk (slides) |
| 09:25 | 35 | **1. Vibe coding: from prompt to map** | Demo + pairs |
| 10:00 | 35 | **2. Agentic tool use: let the model pick the tools** | Demo + pairs |
| 10:35 | 10 | **3a. Autonomous agents: write the brief, launch, walk away** | Demo + launch |
| 10:45 | 30 | *Morning tea, while the agents work* (check the run from a phone) | |
| 11:15 | 35 | **3b. Trust but verify: audit the agent** | Demo + pairs |
| 11:50 | 25 | **4. Sovereign AI: a model you can hold in your hand** | Live demo + discussion (guest slot) |
| 12:15 | 15 | **5. Wrap-up:** decision framework, take-home pack, exit poll | Plenary |

**Cut list if running late (in this order):** M2 Stretch tier, M4 discussion trimmed to 3 min, M1 debrief run during the M2 intro.

---

## 6. Module plans

### 0. Welcome and setup (15 min)

- Acknowledgement of Country, matching the README.
- **Safety briefing (5 min). This is the workshop's first lesson, not admin.**
  - Everything runs in Anthropic's sandbox, not on your laptop. That's why we said no Desktop app.
  - **Don't paste** unpublished, departmental, personal or culturally sensitive data. Use only the workshop data.
  - Check *Settings → Privacy* for whether your chats may be used to improve models.
  - Check *Settings → Capabilities* that code execution is on.
- Login check: everyone opens claude.ai and sends one test prompt from a QR-linked page. Helpers sweep the room.
- Pair up, mixing coders with non-coders where possible.

### Framing (10 min)

- **Three levels of agency** (the README table), framed as a ladder. The higher you go, the more you delegate and the more you must verify.
- **Evidence slide: why "verify" is the theme.** All figures below have been checked against the papers.
  - Developers using an AI assistant wrote significantly less secure code and were *more* confident it was secure (Perry et al., 2023).
  - In an RCT, 16 experienced open-source developers took **19 % longer** with early-2025 AI tools. They had forecast a 24 % speed-up and still believed afterwards that they had been about 20 % faster (Becker et al., 2025, *preprint*).
  - Code-generating LLMs recommend non-existent packages: on average at least 5.2 % of packages for commercial models and 21.7 % for open-source models. This creates a supply-chain risk known as "slopsquatting" (Spracklen et al., 2024/2025).
  - 55 % of GPT-3.5 and 18 % of GPT-4 citations in generated literature reviews were fabricated (Walters & Wilder, 2023).
  - Automation bias and complacency are well-documented human factors. They get worse when the human hasn't watched the work being done (Parasuraman & Manzey, 2010).
- **EO context:** LLM-driven "autonomous GIS" (Li & Ning, 2023), built on the open data cube infrastructure Australians already use (Lewis et al., 2017; Dhu et al., 2017).

### 1. Vibe coding: from prompt to map (35 min)

**Goal:** a working geospatial analysis built from a plain-language prompt, with no coding needed.

| Min | Step |
|---|---|
| 5 | **Concept.** Describe the outcome, not the steps. Iterate by saying what's wrong. The hidden risk is that you never read the code. |
| 7 | **Demo.** Upload `glider_occurrences.csv` and `fire_severity_2019_20.geojson`, then run the starter prompt live. Show Claude writing, running and fixing Python, and the map PNG it produces. |
| 18 | **Hands-on in pairs.** *Starter:* reproduce the demo. *Extend:* change the date window, add a 1 km buffer, or use a colour-blind-safe palette. *Stretch:* ask for a downloadable interactive map artifact. |
| 5 | **Check.** Compare the result with the answer key. Discuss why answers differed: CRS mismatch, duplicate records, coordinate uncertainty, or an area computed in degrees. |

**Starter prompt (draft):**
> I've uploaded species occurrence records (CSV) and a 2019–20 fire-severity layer (GeoJSON). Using Python, tell me what percentage of records since 2010 fall in each fire-severity class, and make a map. **Before you start, tell me the coordinate reference system of each file and how you'll handle any mismatch.** At the end, list every assumption you made.

**Build:** `01_vibe_coding/README.md`, `prompts.md` with copy buttons on GitHub Pages, `expected_outputs/`, `facilitator_notes.md`.
**Academy links:** *Intro to Artifacts*; *Create and edit files with Claude*; *Claude 101*.

### 2. Agentic tool use: let the model pick the tools (35 min)

**Goal:** chain web search, code execution, file creation and artifacts over open data, and inspect the tool trace.

| Min | Step |
|---|---|
| 6 | **Concept.** Tools are search, code execution, files, artifacts and connectors (MCP). The agent loop is think, act, observe, repeat. Show how to expand the tool-call blocks in claude.ai. Tools reduce hallucination but don't remove it: recall the package and citation figures from the framing. |
| 7 | **Demo.** One prompt makes Claude search for NBR/dNBR method documentation, fetch the staged Sentinel-2 clips from the repo's raw GitHub URL, compute dNBR, classify it, compare against FESM and write a one-page PDF report with references. Narrate each tool it chooses. |
| 17 | **Hands-on in pairs.** Choose a task card: **A** burn severity (dNBR), **B** land-cover change, or **C** species plus vegetation. The deliverable is a report or artifact, **plus a "tool trace"**: the tools used, the URLs accessed, and the claims that need checking. |
| 5 | **Debrief.** Put two pairs' outputs on screen. Which tools did Claude choose, and why? Where did it go wrong? Did its cited thresholds actually match the source? |

**Demo prompt (draft):**
> Find authoritative documentation for the Normalized Burn Ratio and its burn-severity thresholds. Then load the pre- and post-fire Sentinel-2 clips from `<raw GitHub URL>`, compute dNBR, classify it using the thresholds you found (cite them), and compare class areas, in an equal-area projection, with the FESM layer I uploaded. Produce a two-panel map PNG and a one-page PDF with a methods paragraph and references. **Finish with a table of every tool you used and every URL you accessed.**

**Method references for the answer key:** NBR/dNBR and RdNBR (Miller & Thode, 2007, *Remote Sensing of Environment*).
**Optional presenter-only extra:** a connector (MCP) demo, e.g. a literature connector, to show that tools can be added. Free-tier connector availability is unverified, so don't make it hands-on.
**Academy links:** *Getting started with connectors*; *Introduction to Model Context Protocol*; *Can you trust what AI tells you?*

### 3a. Autonomous agents: brief, launch, walk away (10 min)

**Goal:** write a mission brief good enough that the agent can work without you.

- **Concept (3 min).** A good brief has: *Goal → Inputs → Constraints → Definition of done → Self-check*. CLAUDE.md is a persistent brief for coding agents.
- **Presenter run (Claude Code on the web, Pro), shown live (4 min).**
  1. Open a template repo with a `CLAUDE.md` containing rules like: never fabricate data; record every data URL in `provenance.csv`; compute areas in EPSG:3577; write tests; don't push to `main`.
  2. Show the cloud environment's **network access setting**, the default allowlist plus the specific STAC/COG domains. This is the sandbox boundary made visible.
  3. Use **plan mode** first, approve the plan, then switch to an autonomous permission mode.
  4. Task: build a small `burn_severity` Python package (STAC query → cloud mask → dNBR/RdNBR → classify → validate against FESM with a confusion matrix → `pytest` → README with provenance → open a PR).
  5. Close the laptop. Keep it open on a phone over morning tea.
- **Participants launch their own (3 min).** Free accounts paste a **mission brief** into claude.ai: *"Has vegetation recovered in burnt vs unburnt glider habitat, 2020–2025?"* using `ndvi_timeseries.csv`. The deliverable is a Python script, figures, a results table, a provenance table, an ASSUMPTIONS section and a self-audit. The brief ends with *"Don't ask me questions; make reasonable assumptions and log them."* Pro attendees may run the same task in Claude Code on the web from a fork of the template.

**Academy links:** *Claude Code 101: The Explore → Plan → Code → Commit workflow* and *The CLAUDE.md file*; *Claude Code in Action: A CLAUDE.md that follows*, *Permission modes*, *Steering long sessions*; tutorial *Cloud session or Remote Control?*

### 3b. Trust but verify: audit the agent (35 min)

**Goal:** verify work you didn't watch happen.

| Min | Step |
|---|---|
| 5 | **Concept.** Automation bias, and why unwatched work needs a systematic audit rather than a vibe check. Introduce the 10-point checklist below. |
| 10 | **Presenter audit of the Claude Code run.** Walk through the plan, diff, test results, `provenance.csv` and PR. Recompute one number independently. Show what it got right *and* wrong. |
| 15 | **Pairs, in two parts.** (a) **"Find the planted errors"** in `seeded_agent_report.md`. It's the same for everyone, so it's fair and comparable. (b) Apply the checklist to your own run from 3a. |
| 5 | **Debrief.** How many planted errors did each pair find? Which checklist items caught them? |

**Planted errors (draft):**
1. Areas computed in EPSG:4326 degrees.
2. A post-fire image dated *before* the fire.
3. A plausible but non-existent reference.
4. A non-existent Python package in `requirements`. Use an obviously fictional name, and don't instruct anyone to install it.
5. *(Optional)* A severity class silently dropped.

**Audit checklist (draft; build as `03_autonomous_agents/audit_checklist.md`):**

| # | Check | Question |
|---|---|---|
| 1 | Scope | Did it do what was asked, and only that? |
| 2 | Provenance | Does every dataset have a URL or DOI, a licence and an access date? |
| 3 | Spatial | Is the CRS stated and consistent? Were areas computed in an equal-area projection? Do the extents match? |
| 4 | Temporal | Are the dates and windows correct, and is pre-fire really pre-fire? |
| 5 | Numbers | Recompute one key number independently, or sanity-check it against the answer key or a known value. |
| 6 | Code | Does it run end-to-end from a clean state? Are there hard-coded fudges? |
| 7 | Packages | Do all the packages exist, and are they reputable? |
| 8 | Citations | Does each DOI resolve, and does the source actually support the claim? |
| 9 | Side effects | What did the agent touch (files, network, commits)? Did it stay inside the sandbox? |
| 10 | Uncertainty | Are limitations stated? Would you put your name to it? |

**Academy links:** *Claude Code in Action: Trust it? Verifying unsupervised runs*, *Verification skills*; tutorials *Discernment toolkit*, *Why do AI models hallucinate?*, *What is sycophancy in AI models?*

### 4. Sovereign AI: a model you can hold in your hand (25 min)

**Goal:** know when cloud AI is appropriate and when local open-weights models are.

| Min | Step |
|---|---|
| 5 | **Concept: the layers of sovereignty.** *Data* (where it goes, how long it's kept), *model* (who controls the weights and updates), *compute* (where it runs, under which jurisdiction) and *governance* (whose rules apply). Australian anchors: the Commonwealth *Policy for the responsible use of AI in government* v2.0 (effective 15 Dec 2025), the NSW AI Assessment Framework, and the **CARE Principles for Indigenous Data Governance** (Carroll et al., 2020) alongside FAIR (Wilkinson et al., 2016). |
| 12 | **Live demo with Wi-Fi off.** A small open-weights model (3–8 B parameters, quantised) runs on a laptop or mini-PC via Ollama, LM Studio or llama.cpp, and on a phone via an offline LLM app. Run the **same Module 1 prompt** and compare it side by side with the cloud result for code quality, correctness, speed and what it *can't* do (no tool execution unless it's wired into an agent). Optionally, a local vision-language model describes a satellite image chip. |
| 8 | **Discussion: the decision matrix.** Data sensitivity (public / internal / sensitive / culturally restricted) against task complexity. Each cell maps to an option: frontier cloud in a sandbox, an enterprise tenancy with data controls, local open-weights, or no AI. The guest speaker can take this slot. |

**Pre-checks:** check the licences of the open-weights models (some restrict use), confirm the demo hardware works fully offline, and record a backup video.

### 5. Wrap-up (15 min)

- **One-slide synthesis:** the agency ladder mapped onto the 4Ds.
- **Research Diligence:** journals' rules on disclosing LLM use (e.g. *Nature*'s 2023 ground rules: LLMs can't be authors, and their use must be documented). Point to Academy's *Writing an AI diligence statement*.
- **Take-home pack:** the prompts, mission-brief template, audit checklist, decision matrix and Python notebooks for redoing the analysis yourself (e.g. on DEA Sandbox or Colab).
- **Next steps:** the Academy tracks already listed in the README. Beginners: *AI Fluency* → *Claude 101*. Advanced: *Claude Code 101* → *Claude Code in Action*.
- **Exit poll (2 min, QR):** confidence on the 5 outcomes, plus "one thing I'll try on Monday".

---

## 7. Facilitation and logistics

- **Roles:** a lead presenter, a co-presenter who drives the demos and watches the clock, and **one roving helper per ~15 participants**. Recruit volunteers. One helper should be Pro-track savvy.
- **Participant materials:** everything copy-pasteable from a mobile-friendly GitHub Pages site (the existing `index.html` and `.nojekyll`), with QR codes on slides. Use large fonts and colour-blind-safe figures, and have Claude use viridis/cividis, which is itself a teaching point.
- **Room:** projector plus a second screen for "agent running"; power boards; a mobile hotspot as backup to venue Wi-Fi.
- **Mobile users:** prefer exercises with the GitHub-URL data route, because uploading files on mobile is clunkier.

## 8. Risks and contingencies

| Risk | Mitigation |
|---|---|
| Free usage limits hit mid-session | Pairs alternate accounts. Single, well-packed prompts. Pre-generated outputs in `expected_outputs/` let anyone continue the audit and discussion. |
| Sandbox can't reach the raw GitHub data URL | Upload route (download from Pages, then upload). Test both in pre-flight. |
| Venue Wi-Fi is overloaded with ~40 concurrent users | Hotspot. Stagger the demo. Recorded backup videos of every demo. |
| The Claude Code demo run fails or overruns | Start a duplicate run earlier that morning as a backup, and have a recorded run ready. Audit whichever finished. |
| Agent output differs wildly between pairs | That's the point: the answer keys plus the *shared* seeded report keep the audit comparable. |
| Local model demo fails | Backup video, plus a phone as a second device. |
| Participants paste sensitive data | Safety briefing, a reminder on every exercise card, and workshop-only datasets. |

## 9. Pre-flight test checklist (must pass by Saturday)

- [ ] Run **every exercise on a fresh free account** on web, iOS and Android. Record the **number of messages used** and whether usage limits were hit.
- [ ] From the free sandbox, test reachability of `raw.githubusercontent.com`, DEA (`explorer.dea.ga.gov.au`, `data.dea.ga.gov.au`), Earth Search, Copernicus Data Space and the ALA API. Record the results in `facilitator/egress_test.md`.
- [ ] Check which Python geo libraries are preinstalled in the sandbox (`geopandas`, `rasterio`, `rioxarray`) and whether `pip install` works for them.
- [ ] Check whether artifact maps render basemap tiles. If not, use a static PNG or a vector-only map.
- [ ] Run the Claude Code on the web demo end-to-end with the exact `CLAUDE.md` and prompt, check its duration, and **record a backup video**.
- [ ] Run the local-model demo fully offline and record a backup video.
- [ ] Check that answer keys match the solutions, and that each seeded error is detectable with the checklist.
- [ ] Check data licences and attribution, and that README links resolve (the Skilljar → Academy move).

## 10. What to build (repo structure for `coursework/`)

```
coursework/
├── README.md                    # module index, run sheet, links
├── 00_setup/                    # login check, safety briefing card, pairing guide
├── 01_vibe_coding/              # README, prompts.md, expected_outputs/, facilitator_notes.md
├── 02_agentic_tools/            # task cards A/B/C, tool-trace template, expected_outputs/
├── 03_autonomous_agents/        # mission_brief_template.md, CLAUDE.md example,
│                                #   audit_checklist.md, seeded_agent_report.md (+ answer sheet, facilitator-only)
├── 04_sovereign_ai/             # sovereignty layers, decision matrix, local-model setup notes
├── 05_wrap_up/                  # take-home pack, further learning, exit-poll link
├── data/                        # pre-clipped files + MANIFEST.csv + ATTRIBUTION.md
├── notebooks/                   # take-home Python notebooks (STAC → dNBR → validation)
└── scripts/                     # prepare_data.py, answer_keys.py, tests/
slides/                          # one deck, with module dividers
facilitator/                     # run_sheet.md, preflight_checklist.md, egress_test.md, contingency.md
```

Also update `index.html` and `README.md` so the "Coursework" section links to these modules.

## 11. Build timeline (about 4 days to delivery)

| Day | Work |
|---|---|
| **Wed 4 / Thu 5** | Lock the case study and AOI. Write `prepare_data.py`, stage the data, write the manifest and answer keys. Run the egress tests. |
| **Thu 5** | Write the Module 1–2 prompts, task cards and expected outputs, and test them on free accounts (web and mobile). |
| **Fri 6** | Module 3: template repo, `CLAUDE.md`, mission brief, seeded report, checklist. Rehearse the Claude Code run. Set up the Module 4 hardware. |
| **Sat 7** | Slides, facilitator notes, GitHub Pages. **Record the backup videos. Content freeze.** |
| **Sun 8** | Full timed dry run with both presenters, then fixes. Send a reminder to attendees via the organisers: "create your account, and here's the Pages link". |
| **Mon 9** | Early backup Claude Code run, room check, deliver. |

## 12. After the workshop

- Publish the solutions, the facilitator-only answer sheet, demo recordings and the exit-poll summary.
- If you plan to **publish on the workshop's outcomes** (e.g. a teaching note), get human-research ethics approval *before* collecting poll data, and add a consent line to the poll.

## 13. Decisions needed from the presenters

1. Case study: **Black Summer / greater glider (recommended)** or the Tasmanian alternative.
2. The exact morning-tea time in the AEO26 program, which shifts the run sheet around Module 3a.
3. Expected numbers, which set how many helpers are needed.
4. The guest speaker's slot and topic (Module 4 is suggested).
5. Module 4 hardware: which device(s) and which open-weights model, after a licence check.

---

## References (all checked with Scite on 4 Nov 2026; none retracted)

**Evidence on AI-assisted coding and verification**
- Becker, J., Rush, N., Barnes, E., & Rein, D. (2025). *Measuring the impact of early-2025 AI on experienced open-source developer productivity* [Preprint]. arXiv. https://doi.org/10.48550/arXiv.2507.09089
- Parasuraman, R., & Manzey, D. H. (2010). Complacency and bias in human use of automation: An attentional integration. *Human Factors, 52*(3), 381–410. https://doi.org/10.1177/0018720810376055
- Perry, N., Srivastava, M., Kumar, D., & Boneh, D. (2023). Do users write more insecure code with AI assistants? In *Proceedings of the 2023 ACM SIGSAC Conference on Computer and Communications Security* (pp. 2785–2799). https://doi.org/10.1145/3576915.3623157
- Spracklen, J., Wijewickrama, R., Sakib, A. H. M. N., Maiti, A., Viswanath, B., & Jadliwala, M. (2024). *We have a package for you! A comprehensive analysis of package hallucinations by code generating LLMs* [Preprint; to appear in USENIX Security 2025, so check the proceedings citation]. https://doi.org/10.48550/arXiv.2406.10279
- Walters, W. H., & Wilder, E. I. (2023). Fabrication and errors in the bibliographic citations generated by ChatGPT. *Scientific Reports, 13*, 14045. https://doi.org/10.1038/s41598-023-41032-5
- Ji, Z., Lee, N., Frieske, R., Yu, T., Su, D., Xu, Y., Ishii, E., Bang, Y. J., Madotto, A., & Fung, P. (2023). Survey of hallucination in natural language generation. *ACM Computing Surveys, 55*(12), 1–38. https://doi.org/10.1145/3571730

**EO, GIS and AI**
- Dhu, T., Dunn, B., Lewis, B., Lymburner, L., Mueller, N., Telfer, E., Lewis, A., McIntyre, A., Minchin, S., & Phillips, C. (2017). Digital Earth Australia – unlocking new value from earth observation data. *Big Earth Data, 1*(1–2), 64–74. https://doi.org/10.1080/20964471.2017.1402490
- Lewis, A., Oliver, S., Lymburner, L., et al. (2017). The Australian Geoscience Data Cube — Foundations and lessons learned. *Remote Sensing of Environment, 202*, 276–292. https://doi.org/10.1016/j.rse.2017.03.015
- Li, Z., & Ning, H. (2023). Autonomous GIS: The next-generation AI-powered GIS. *International Journal of Digital Earth, 16*(2), 4668–4686. https://doi.org/10.1080/17538947.2023.2278895
- Miller, J. D., & Thode, A. E. (2007). Quantifying burn severity in a heterogeneous landscape with a relative version of the delta Normalized Burn Ratio (dNBR). *Remote Sensing of Environment, 109*(1), 66–80. https://doi.org/10.1016/j.rse.2006.12.006
- Reichstein, M., Camps-Valls, G., Stevens, B., Jung, M., Denzler, J., Carvalhais, N., & Prabhat. (2019). Deep learning and process understanding for data-driven Earth system science. *Nature, 566*(7743), 195–204. https://doi.org/10.1038/s41586-019-0912-1

**Data governance, reproducibility and disclosure**
- Carroll, S. R., Garba, I., Figueroa-Rodríguez, O. L., et al. (2020). The CARE principles for Indigenous data governance. *Data Science Journal, 19*, 43. https://doi.org/10.5334/dsj-2020-043
- Wilkinson, M. D., Dumontier, M., Aalbersberg, I. J., et al. (2016). The FAIR Guiding Principles for scientific data management and stewardship. *Scientific Data, 3*, 160018. https://doi.org/10.1038/sdata.2016.18
- Nature. (2023). Tools such as ChatGPT threaten transparent science; here are our ground rules for their use [Editorial]. *Nature, 613*, 612. https://doi.org/10.1038/d41586-023-00191-1
- Peng, R. D. (2011). Reproducible research in computational science. *Science, 334*(6060), 1226–1227. https://doi.org/10.1126/science.1213847
- Wilson, G., Bryan, J., Cranston, K., Kitzes, J., Nederbragt, L., & Teal, T. K. (2017). Good enough practices in scientific computing. *PLOS Computational Biology, 13*(6), e1005510. https://doi.org/10.1371/journal.pcbi.1005510

**Policy (grey literature; check the current version before presenting)**
- Digital Transformation Agency. *Policy for the responsible use of AI in government*, v2.0 (effective 15 Dec 2025). https://www.digital.gov.au/ai/ai-in-government-policy
- NSW Government. *NSW Artificial Intelligence Assessment Framework*. https://www.digital.nsw.gov.au/nsw-artificial-intelligence-assurance-framework

> Notes on the reference list:
> - Existence and retraction status were checked with Scite. Volume, issue, pages and author lists were checked against Crossref on 4 Nov 2026.
> - Two items are preprints. They are cited because no peer-reviewed equivalent with the same evidence exists yet; label them as preprints on the slides.
> - Author lists marked "et al." are abbreviated here only; use the full lists in any published version.
