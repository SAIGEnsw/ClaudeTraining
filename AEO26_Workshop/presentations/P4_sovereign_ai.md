# P4. Sovereign AI: waratah_mcp on the DGX Spark / HP Z6, and how to replicate it

> ## ⚠️ NOT CLEARED: section 2 draws on the private waratah_mcp repo; needs presenter approval before public showing
>
> Section 2 was written on 5 Nov 2026 from a read-only review of the private waratah_mcp repository (no code was run). Until the presenter clears it (B14), do not copy this file into the staging or production workshop repos, and do not show or link the repo. Never put on screen the repo URL, file paths, usernames, the model's system prompt, service logs, internal machine or network names, or example-chat share links (see "Do not show" in section 2). The public companion, `docs/local_ai/`, describes waratah_mcp only in general terms.

**Slot:** 11:35–12:00, Mon 9 Nov 2026 (Presentations 2, second talk) · **Length:** 25 min · **Presenter:** guest speaker (likely Adam Roff; TBC) · **Status:** demo script drafted (section 2); **needs presenter clearance** and a timed rehearsal
**Format:** outline and speaker notes only (no slide deck). Live demo by Teams screen-share from the DGX Spark or HP Z6; screenshot walkthrough in `docs/local_ai/` (to be captured).
**Timing note:** B8 allots 12 + 8 + 5 min. Sections 1–2 are the 12-min demo block: the 2-min sovereignty framing (section 1) is spoken over the live landing page, then 10 min of live tasks. Sections 3–6 take 8 min, and sections 7–8 take 5 min (4.5 + 0.5).

---

## 1. What "sovereign" means: four layers (2 min; opens the demo block, spoken over the live landing page)

**On screen**
- **Data:** where it goes, how long it is kept
- **Model:** who controls the weights and their updates
- **Compute:** where it runs, under which jurisdiction
- **Governance:** whose rules apply: the Commonwealth *Policy for the responsible use of AI in government* v2.0; the NSW AI Assessment Framework; **CARE** alongside **FAIR** (Carroll et al., 2020; Wilkinson et al., 2016)

**Speaker notes**
For the last two hours your agents have run on Anthropic's computers, using a model Anthropic controls, on public data. That was a deliberate choice. Sovereignty is not one property but four. Data: where your inputs and outputs travel and how long they are retained. Model: who holds the weights and can change them under you. Compute: whose hardware, in which jurisdiction. Governance: whose rules apply, from the Commonwealth and NSW AI frameworks to the CARE principles for Indigenous data governance, which complement FAIR by asking who benefits and who has authority to control the data. Check the current policy versions before presenting. A local model can satisfy the first three layers. Only people and institutions satisfy the fourth.

**References:** Carroll et al. (2020); Wilkinson et al. (2016); Digital Transformation Agency (2025); NSW Government (n.d.).

---

## 2. Live demo: waratah_mcp on the DGX Spark / HP Z6 (10 min live; with section 1, the 12-min demo block)

**What waratah_mcp is (state it exactly)**
- **The end product is AskDCCEEW:** an NSW-branded Open WebUI chat in which a **local open-weights model** answers questions by calling **`nsw-tools`**, a read-only MCP server (FastMCP, streamable HTTP, bound to `127.0.0.1:8765`). Its **28 tools** cover five **public** NSW government data services, all anonymous and read-only: BioNet species records (including the new BioNet Atlas map viewer), the SEED spatial-data catalogue, DCCEEW publications, Data.NSW and nsw.gov.au search. Answers are built from tool payloads that you can open in the chat; record exports are served as CSV or Darwin Core download links. An optional retrieval sidecar (loopback, GPU or CPU) adds full-text search inside 2,875 publications; its index is not in the repo.
- **Stack on the demo box.** *DGX Spark* (GB10, aarch64, 128 GB unified memory): AskDCCEEW is Qwen3.8-27B (NVFP4) on vLLM, with Qwen3.8-Flash-Next 125B and Nemotron-3 Super/Nano as alternatives; the code interpreter is a Jupyter kernel on the host. *HP Z6* (RTX PRO 4500 Blackwell, 32 GB VRAM): Nemotron-3-Nano, Nemotron-3.5-Lightning, Muse-Glimmer (llama.cpp) or Qwen3.8-27B (shown as AskDCCEEW); the code interpreter is Pyodide in the browser. Either box holds one model at a time. The repo states a licence only for Muse-Glimmer (Apache 2.0): **check the licence of whichever model you show.**
- **Where it sits on this talk's A/B** (say it on screen):
  - **As demoed, it is the Option B pattern** (section 5): the model, prompts, chat history and tool results stay on the box, and no AI company sees them. It is built from Open WebUI + vLLM, not Claude Code + Ollama, so section 5's "not supported by Anthropic" caveat is about a different route; the capability gap applies in full.
  - **Local, not offline:** each tool call sends its arguments (species, place, keywords, coordinates) to public NSW government hosts (BioNet logs requests upstream). Web search and, on the DGX, the host code interpreter can also reach the internet, so keep both toggles off.
  - **The same server registered in Claude Code is Option A** (section 4: `claude mcp add --transport http nsw-tools http://127.0.0.1:8765/mcp`), and so is the repo's bundled stdio `bionet/` server in Claude Desktop: **not data-sovereign**.
- **How good is it?** Author-run checks, not an independent benchmark: seven transcripts with about 40 factual claims (numbers, dates, titles, URLs) checked against the raw tool payloads (14 Aug 2026, Nemotron-3.5-Lightning on the Z6), and a 42-question regression bank whose results are kept outside the repo.

**Run sheet (the 12-min block)**

| Clock | On screen | Say / expect |
|---|---|---|
| 0:00–2:00 | AskDCCEEW landing page, "How it works" tiles | Section 1's four layers. Point at "Local by design … Nothing leaves the machine except tool calls to public NSW data services", and add that this Teams stream is leaving the box too. |
| 2:00–3:00 | **Open the chat**; model picker shows AskDCCEEW; Web Search and Code Interpreter toggles **off** | "Browser → Open WebUI → local model on vLLM ⇄ nsw-tools MCP server on loopback → public NSW APIs. No AI company in the loop." |
| 3:00–5:00 | Prompt 1 | Narrate each tool call as it appears. Expand one block: `total_records` and `attribution … as at <date>`. Every figure has a receipt. |
| 5:00–6:30 | Prompt 2 | The chart renders in the Artifacts panel, with 2026 flagged as a part-year. |
| 6:30–8:00 | Prompt 3 (only if it passed rehearsal; otherwise 4b) | The participants' AOI A, through a local model. |
| 8:00–9:30 | Prompt 4a | The model declines to invent a population. |
| 9:30–11:00 | Terminal (only if cleared), then the Option A one-liner | "This list is everything the tool server contacts. One line turns it into Option A." |
| 11:00–12:00 | Back to the chat: close (below) | Classification and limits; hand over to section 3. |

**Prompts, word for word, and expected outputs.** Bracketed numbers are from the repo's recorded runs (BioNet as at 24 Aug 2026). BioNet refreshes daily, so expect higher counts on 9 Nov; read out only what comes back, with its date.

1. *"What data is on BioNet for greater gliders since 2022? Give counts by year and by LGA only, no individual records."*
   - **Expect** `bionet_lookup_species`, then `bionet_count_by` by year and by LGA. "Greater glider" resolves to *Petauroides volans* (Southern Greater Glider), Endangered on both the NSW and EPBC registers, with zero *P. minor* records. Per year [2022: 717 · 2023: 1,988 · 2024: 2,677 · 2025: 3,588 · 2026: 1,464, part-year; total 10,434]; top LGA Armidale Regional; "records measure survey effort, not abundance"; quarantined records excluded.
   - **Why "counts only":** it keeps record-level coordinates off the stream. The recorded run also listed recent nest and roost records with precise coordinates.
2. *"Using BioNet, get the yearly counts of greater glider sighting records in NSW from 2020 to 2026, then build a visualisation of the trend."* (new chat)
   - **Expect** one or two `bionet_count_by` calls by year, a table [2020: 806 · 2021: 677 · 2022: 717 · 2023: 1,988 · 2024: 2,677 · 2025: 3,588 · 2026: 1,464, part-year; total 11,917], the survey-effort caveat and one self-contained HTML chart in the Artifacts panel (no code interpreter needed).
   - **Type the prompt** rather than using the landing page's "Run it live" link, which switches the code interpreter on (on the DGX, a kernel on the host with network access).
3. *"How many greater glider records does BioNet have within 10 km of -35.4846, 149.6252, by year since 2015?"* (the centre of the workshop's AOI A, Tallaganda, from the workshop repo's `data/README.md`)
   - **Expect** `bionet_count_by` with `lat`, `lon` and `radius_km`, by year. The answer should say the counts cover the **bounding box** around the circle, not the circle (system-prompt hard rule 9).
   - **Talking point:** compare the fire years with later years, then repeat "survey effort, not abundance". Cross-check: the workshop data package counts 551 ALA glider records in the 20 × 20 km AOI A square (311 since 2020; as of 5 Nov, before any ALA licence filtering). ALA aggregates BioNet and other sources, and the boxes differ, so expect the same order of magnitude, not the same number.
   - **The repo has no recorded run of this prompt: rehearse it or skip it.**
4. Honesty and guardrails (4a; add 4b if time, or use it instead of 3):
   - **4a** *"How many koalas are there in NSW?"* (a landing-page chip). **Expect** no population figure. The recorded run made five tool calls and said BioNet holds [293,795] koala sighting records, "a count of records, not of individuals"; Endangered under both the NSW and Commonwealth acts, each from its own register. It pointed to the population estimates in *Bioregional assessment of koala populations in New South Wales* (2017) and the *NSW Koala Strategy 2021–26*, and said full-text search inside reports is unavailable if the sidecar is not installed. (The 14 Aug transcripts quoted 243,629.)
   - **4b** *"Can you download all koala records for me?"*, then *"Just the records from 2026 then."* **Expect** a refusal at the 50,000-row export cap, with suggested slices, then a CSV download link [1,263 rows] carrying a CC BY 4.0 citation header. Do not expand the export tool block: its payload shows the server's local file path.
5. Terminal, pre-opened in the deployed clone with a neutral prompt (`PS1='$ '`), only if cleared:
   - Run `grep -rhoE "https?://[a-zA-Z0-9.-]+" nsw_agent_kit/mcp_server.py nsw_agent_kit/tools/*.py | sort -u`. **Expect** seven public NSW government hosts (six are queried; the SEED website appears only in links) plus `http://127.0.0.1` for the sidecar. (The repo's 12 Aug security brief lists four; the code has grown.)
   - Then show, without running it: `claude mcp add --transport http nsw-tools http://127.0.0.1:8765/mcp`.

**Close (11:00–12:00)**
- **Scorecard against section 1.** Data ✅: prompts and chats stay; tool queries reach public NSW APIs. Model ✅: open weights on the box (licence to confirm). Compute ✅: this desk. Governance: partly. There is a written security brief, a sandboxed systemd service, and web search and the host code interpreter are never on by default; people decide the rest.
- **What a frontier model does better.** The system prompt carries 13 "hard rules", each written after a real wrong answer from a local model: an invented denominator, a Commonwealth (EPBC) listing that the record did not show, a bounding box reported as a radius, "the last decade" anchored to the training cut-off, a search spiral that ended with no answer. The author's own summary: "the numbers are reliable; treat unnumbered garnish with normal scepticism".

**Do not show (on screen or in the backup video)**
- The model's system prompt or Admin → Models (on the DGX it holds a home-directory path; on both boxes, an internal firewall note), `webui/profiles.toml` (personal paths) or `nsw_agent_kit/.env`.
- vLLM or service logs: vLLM prints its API key in its start-up log.
- An expanded `bionet_export_csv` block (local file path), or a shell prompt showing the username and hostname.
- Example-chat share links (`/s/…`), the repo URL, internal machine, repo and tracker names, or `docs/seed-team-email.md` (a draft email).
- Record-level coordinates of threatened species (den, nest or roost sites), even where BioNet publishes them.
- The BioNet Atlas map-viewer calls presented as a published API: they are reverse-engineered from the web app.
- `bionet/docs/08-replicate-this.md`: its "Option A/B" are Claude Code on the web vs Claude Code locally (both send data to Anthropic), not this talk's A/B.

**Speaker notes**
[Guest speaker to adapt; about 170 words.] This is AskDCCEEW, built from waratah_mcp. The model is an open-weights model served on this box. The chat runs in Open WebUI on this box. The tools are a small MCP server, also on this box, that queries public NSW data services: BioNet, SEED and DCCEEW's publications. Watch the tool calls: each answer is assembled from what the services returned, and you can open the receipt for every figure. Three things to notice. First, where it sits: this is the fully local pattern, Option B, with no AI company in the loop. But it is not offline, because the tool queries go out to public government APIs. Second, the same server added to Claude Code is the hybrid pattern, and then my prompts and every tool result go to Anthropic: fine for public data like this, but not data-sovereign. Third, the capability gap is real. We wrote thirteen rules into the system prompt, each one after a wrong answer from a local model.

**Backup plan (prepare by Sat 7 Nov; rehearse over Teams per B11)**
1. **Primary:** live Teams screen-share from the Spark/Z6, rehearsed on the venue network or hotspot. Share only the browser window (plus the terminal, if cleared), with notifications off. On the morning, load the model (one fits at a time; the landing page names the Start icon), check that BioNet answers, and run each prompt once. Keep those chats open in tabs.
2. **If a live answer stalls (over about 60 s) or an upstream API is down:** switch to that morning's tab for the same prompt and say when it was run, or open the matching installed example chat (recorded 24 Aug–9 Sep 2026) and say it was recorded. Tools fail with structured errors rather than crashing, so a failed call is itself a teaching moment.
3. **If Teams or the venue network fails:** play the recorded demo video (captured during rehearsal, stored locally on the presenter laptop, not streamed). Check it against "Do not show" first.
4. **If waratah_mcp is not ready or not cleared:** demonstrate a generic local stack instead (a local model server plus a small open-weights model answering the Module 1 prompt with the network off), then go straight to section 3. Do not show unreviewed code or private data.
5. **If all of the above fail:** extend sections 3–7 using the screenshot walkthrough and give the room 3 extra minutes on the decision matrix.
- Don't block egress live: it would cut the Teams stream as well as the live tools. Section 6 covers the egress test with screenshots.

**Sources** (private repo, read only on 5 Nov 2026; for the presenter, not for citation or display): `README.md`, `INSTALL.md`, `docs/SECURITY.md`, `docs/EXAMPLES.md`, `docs/ADVICE-openwebui-system-prompt.md`, `webui/README.md`, `webui/profiles.toml`, `webui/configure-models.py`, `webui/branding/landing.html.in`, `webui/branding/example-chats/{gliders,viz,koalas,export}.json`, `eval/eval-questions.json`, `nsw_agent_kit/mcp_server.py`, `nsw_agent_kit/test_mcp_server.py`, `systemd/*.service`.

**References:** none (no academic citations).

---

## 3. Replicate it on Linux: hardware, model server, CLI (2.5 min)

**On screen** (screenshots from `docs/local_ai/`, to be captured on a clean Linux machine)
1. Check hardware and drivers: `nvidia-smi`
2. Install a local model server: **Ollama ≥ 0.14** (or vLLM); pull an open-weights model **after checking its licence** (some restrict use)
3. Install the **Claude Code CLI** (official instructions)

**Speaker notes**
Here is how to build something similar yourself, in two variants. Start with the boring checks. Confirm the GPU and driver with nvidia-smi; model size is bounded by GPU memory, and quantised models trade some quality for fitting. Install a model server: Ollama is the simplest, vLLM suits heavier serving. Read the licence of any open-weights model before you pull it; "open weights" does not mean unrestricted use, and some licences limit commercial or government use. Then install the Claude Code command-line tool from the official instructions. From here there are two quite different architectures, and the difference is the most important point of this talk.

**References:** Ollama (n.d.).

---

## 4. Option A, hybrid: Claude + a local MCP server (2 min)

**On screen**
- `claude mcp add …` registers a **local MCP server** (e.g. one that reads local rasters or a local database)
- The data files stay on your machine, **but**
- **Prompts and tool results go to Anthropic.** Anything the tool returns is sent to the model
- **Hybrid Claude + local MCP is NOT data-sovereign**
- The demo's own MCP server becomes this pattern the moment it is registered in Claude Code (the one-liner from section 2)

**Speaker notes**
Option A keeps Claude as the model and adds a local MCP server: a small program on your machine that gives Claude tools, such as querying a local database or reading local rasters. It feels local, because the files never leave the disk. But think about the data flow. Claude decides to call the tool, your machine runs it, and the tool's result is sent back to Claude, on Anthropic's servers, so the model can reason about it. Your prompts go too. If a tool returns plot coordinates or a species record, that record has left your machine. So the hybrid pattern is useful for convenience and capability, but it is not data-sovereign. Treat it, for governance purposes, like any cloud service. The demo's tool server is the example: registered in Claude Code it works the same way, but then my prompts and every BioNet result go to Anthropic. For public data the first row of the matrix allows that; it is still not data-sovereign.

**References:** Anthropic (n.d.-c).

---

## 5. Option B, fully local: Claude Code pointed at Ollama (2.5 min)

**On screen**
- Set `ANTHROPIC_BASE_URL` to Ollama's Anthropic-compatible endpoint; run `claude --model <local-model>`
- **Ollama ≥ 0.14's Anthropic-compatible endpoint is community-supported. It is NOT supported by Anthropic for Claude Code with non-Claude models.**
- Expect a **capability gap**: weaker planning, tool use and long-task reliability than a frontier model
- Experimental: fine for exploration, not for a production pipeline without your own testing
- **The demo was this pattern**, built from Open WebUI + vLLM instead of Claude Code + Ollama, so no Anthropic software is in its loop. Its tool queries still reach public APIs: local, not offline

**Speaker notes**
Option B swaps the model. Ollama from version 0.14 serves an Anthropic-compatible messages endpoint, so you can set the base URL environment variable to point Claude Code at Ollama and name a local model. Now prompts, tool calls and results stay on your box. Two honest caveats. First, this is community-supported. Anthropic's documentation does not support routing Claude Code to non-Claude models, so if something breaks, you own it. Check the Ollama documentation for any other variables it currently requires. Second, there is a capability gap. Claude Code's agent loop was built around frontier models; small local models plan less well, misuse tools more and drift on long tasks. Ask whether the task still works, not only whether the data stayed home. The demo you saw is this pattern built from different parts: Open WebUI and vLLM instead of Claude Code and Ollama, so the support caveat does not arise. The capability gap does, which is why its system prompt carries thirteen hard rules.

**References:** Ollama (n.d.); Anthropic (n.d.-c).

---

## 6. Prove it is offline, and isolate it (1 min)

**On screen**
- **Block egress** (firewall rule, or a container with no network), then run a task: it must still work
- **Isolate:** run the agent in a container or VM; use permission modes; give it only the folders it needs
- Link Anthropic's sandbox/isolation guidance in `docs/local_ai/` (URL to be confirmed before presenting)

**Speaker notes**
Do not take "local" on trust; test it. Block outbound network at the firewall, or run the whole stack in a container with networking disabled, and run a real task. If it fails, something was phoning home. Then isolate the agent itself. A local model with shell access can still delete files or read things it should not, so run it in a container or virtual machine, use Claude Code's permission modes, and mount only the folders the task needs. These are the same principles as this morning's cloud sandbox, moved onto your own hardware. For a tool server like the demo's, which has to reach public APIs, prove it with an allow-list instead of a full block: the model server gets no route out, and the tool server reaches only the named government hosts.

**References:** none (Anthropic isolation guide URL to be added once confirmed).

---

## 7. Decision matrix: which option for which data? (4.5 min)

**On screen** (adapted from Part A Module 4: data sensitivity × option; task complexity noted in the last column)

| Data sensitivity | Cloud session (Claude Code on the web, today) | Option A: Claude + local MCP | Option B: fully local open-weights | No AI |
|---|---|---|---|---|
| **Public / open** (today's Sentinel-2, WorldCover, FESM) | ✅ Best capability | ✅ Same exposure as cloud | ✅ Works; capability gap on complex agentic tasks | Optional |
| **Internal / unpublished** (draft results, internal reports) | ⚠️ Only via an approved enterprise tenancy with data controls, per agency policy | ⚠️ Same as cloud: prompts and tool results leave | ✅ Preferred if the task is simple enough | ✅ If no approved route |
| **Sensitive** (personal information, private-land plot locations, sensitive-species locations, commercial-in-confidence) | ❌ Unless explicitly approved after an AI assessment | ❌ **Not data-sovereign** | ⚠️ Yes, if isolated, egress-blocked and approved | ✅ Default |
| **Culturally restricted / CARE-governed Indigenous data** | ❌ | ❌ | ⚠️ Only with the community's consent and under its governance (CARE: Authority to control) | ✅ Default unless the community decides otherwise |

- **Complex task + sensitive data?** Develop the code with a cloud agent on public or synthetic stand-in data; run it locally, without AI, on the real data.
- **Where the demo sits:** AskDCCEEW is the Option B column on public data (first row). The same MCP server in Claude Code is the Option A column, which the first row also allows. For sensitive data, Option A is out, and Option B needs isolation, a proven egress block and approval.

**Speaker notes**
Here is the decision matrix. Read across the sensitivity rows. Public data, like today's, can go anywhere, and the cloud gives the best capability. Internal data need an approved route; Option A gives no protection here, because its tool results still leave. Sensitive data, such as private-land plots or threatened-species locations, default to no AI, or a fully local, isolated, approved setup. For culturally restricted and CARE-governed data, a technical solution is not sufficient. CARE gives Indigenous peoples authority to control their data, so the community decides whether AI is used at all, local or not. The last line is the practical trick for hard tasks: build with a cloud agent on stand-in data, then run the code yourself on the real data.

**References:** Carroll et al. (2020); Wilkinson et al. (2016); Digital Transformation Agency (2025); NSW Government (n.d.); Anthropic (n.d.-a, n.d.-b).

---

## 8. Hand-over to results (0.5 min)

**On screen**
- 12:00: open your agent's pull request
- Audit checklist + P3's five questions + the leaderboard

**Speaker notes**
Your agents should be wrapping up now. Open your pull request, keep the audit checklist beside it, and use the five questions from the previous talk. Remember that the data your agent used were public by design; the decision matrix is what you take back to your own work on Monday.

**References:** none.

---

## References

Anthropic. (n.d.-a). *Claude Code on the web*. Retrieved 5 November 2026, from https://code.claude.com/docs/en/claude-code-on-the-web

Anthropic. (n.d.-b). *Cloud environments*. Retrieved 5 November 2026, from https://code.claude.com/docs/en/cloud-environments

Anthropic. (n.d.-c). *LLM gateway configuration*. Retrieved 5 November 2026, from https://code.claude.com/docs/en/llm-gateway

Carroll, S. R., Garba, I., Figueroa-Rodríguez, O. L., et al. (2020). The CARE principles for Indigenous data governance. *Data Science Journal, 19*, 43. https://doi.org/10.5334/dsj-2020-043

Digital Transformation Agency. (2025). *Policy for the responsible use of AI in government* (Version 2.0, effective 15 December 2025). Australian Government. https://www.digital.gov.au/ai/ai-in-government-policy

NSW Government. (n.d.). *NSW Artificial Intelligence Assessment Framework*. https://www.digital.nsw.gov.au/nsw-artificial-intelligence-assurance-framework

Ollama. (n.d.). *Anthropic compatibility*. Retrieved 5 November 2026, from https://docs.ollama.com/api/anthropic-compatibility

Wilkinson, M. D., Dumontier, M., Aalbersberg, I. J., et al. (2016). The FAIR Guiding Principles for scientific data management and stewardship. *Scientific Data, 3*, 160018. https://doi.org/10.1038/sdata.2016.18

*All references are from WORKSHOP_PLAN.md (Part A references and Part B platform documentation). Policy documents are grey literature: check the current versions before presenting. Anthropic (n.d.-a, n.d.-b) underpin the "cloud session" column of the matrix (sandboxed VM, network access levels). The Anthropic sandbox/isolation guide referred to in B8 is not in the plan's reference list and has been left out until its URL is confirmed.*
