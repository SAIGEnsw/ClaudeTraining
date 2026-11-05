# P4. Sovereign AI: waratah_mcp on the DGX Spark / HP Z6, and how to replicate it

**Slot:** 11:35–12:00, Mon 9 Nov 2026 (Presentations 2, second talk) · **Length:** 25 min · **Presenter:** guest speaker (likely Adam Roff; TBC) · **Status:** needs waratah_mcp (demo section is a placeholder)
**Format:** outline and speaker notes only (no slide deck). Live demo by Teams screen-share from the DGX Spark or HP Z6; screenshot walkthrough in `docs/local_ai/` (to be captured).
**Timing note:** B8 allots 12 + 8 + 5 min. This outline takes 2 min from the demo for a short sovereignty framing (section 1), giving 10 + 8 + 4.5.

---

## 1. What "sovereign" means: four layers (2 min)

**On screen**
- **Data:** where it goes, how long it is kept
- **Model:** who controls the weights and their updates
- **Compute:** where it runs, under which jurisdiction
- **Governance:** whose rules apply: the Commonwealth *Policy for the responsible use of AI in government* v2.0; the NSW AI Assessment Framework; **CARE** alongside **FAIR** (Carroll et al., 2020; Wilkinson et al., 2016)

**Speaker notes**
For the last two hours your agents have run on Anthropic's computers, using a model Anthropic controls, on public data. That was a deliberate choice. Sovereignty is not one property but four. Data: where your inputs and outputs travel and how long they are retained. Model: who holds the weights and can change them under you. Compute: whose hardware, in which jurisdiction. Governance: whose rules apply, from the Commonwealth and NSW AI frameworks to the CARE principles for Indigenous data governance, which complement FAIR by asking who benefits and who has authority to control the data. Check the current policy versions before presenting. A local model can satisfy the first three layers. Only people and institutions satisfy the fourth.

**References:** Carroll et al. (2020); Wilkinson et al. (2016); Digital Transformation Agency (2025); NSW Government (n.d.).

---

## 2. Live demo: waratah_mcp on the DGX Spark / HP Z6 (10 min) — PLACEHOLDER

> **Placeholder.** waratah_mcp has not been shared and has not been reviewed (WORKSHOP_PLAN.md B13). Fill in this section once access is granted and the material is cleared for public showing.

**On screen (to be written)**
- What waratah_mcp does: [TBC]
- Hardware: DGX Spark or HP Z6, [GPU/memory TBC]; model: [open-weights model, licence checked: TBC]
- What stays on the box, and what (if anything) leaves it: [TBC; must be stated exactly]
- One end-to-end task, live: [TBC]

**Speaker notes (template)**
[Guest speaker to write once the demo is fixed, about 100 words.] Cover: the problem waratah_mcp solves; the stack (model server, model and licence, MCP server, client); which of the four layers it satisfies and which it does not; one task run live, narrating each tool call; one thing it cannot do that a cloud frontier model can. If the client is Claude Code talking to Anthropic's API with waratah_mcp as a local MCP server, say explicitly that this is the hybrid pattern in section 4 and is **not** data-sovereign.

**Backup plan (prepare by Sat 7 Nov; rehearse over Teams per B11)**
1. **Primary:** live Teams screen-share from the Spark/Z6, rehearsed on the venue network or hotspot.
2. **If Teams or the venue network fails:** play the recorded demo video (captured during rehearsal, stored locally on the presenter laptop, not streamed).
3. **If waratah_mcp is not ready or not cleared:** demonstrate a generic local stack instead (a local model server plus a small open-weights model answering the Module 1 prompt with the network off), then go straight to section 3. Do not show unreviewed code or private data.
4. **If all of the above fail:** extend sections 3–7 using the screenshot walkthrough and give the room 3 extra minutes on the decision matrix.

**References:** none until the demo is defined.

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

**Speaker notes**
Option A keeps Claude as the model and adds a local MCP server: a small program on your machine that gives Claude tools, such as querying a local database or reading local rasters. It feels local, because the files never leave the disk. But think about the data flow. Claude decides to call the tool, your machine runs it, and the tool's result is sent back to Claude, on Anthropic's servers, so the model can reason about it. Your prompts go too. If a tool returns plot coordinates or a species record, that record has left your machine. So the hybrid pattern is useful for convenience and capability, but it is not data-sovereign. Treat it, for governance purposes, like any cloud service.

**References:** Anthropic (n.d.-c).

---

## 5. Option B, fully local: Claude Code pointed at Ollama (2.5 min)

**On screen**
- Set `ANTHROPIC_BASE_URL` to Ollama's Anthropic-compatible endpoint; run `claude --model <local-model>`
- **Ollama ≥ 0.14's Anthropic-compatible endpoint is community-supported. It is NOT supported by Anthropic for Claude Code with non-Claude models.**
- Expect a **capability gap**: weaker planning, tool use and long-task reliability than a frontier model
- Experimental: fine for exploration, not for a production pipeline without your own testing

**Speaker notes**
Option B swaps the model. Ollama from version 0.14 serves an Anthropic-compatible messages endpoint, so you can set the base URL environment variable to point Claude Code at Ollama and name a local model. Now prompts, tool calls and results stay on your box. Two honest caveats. First, this is community-supported. Anthropic's documentation does not support routing Claude Code to non-Claude models, so if something breaks, you own it. Check the Ollama documentation for any other variables it currently requires. Second, there is a capability gap. Claude Code's agent loop was built around frontier models; small local models plan less well, misuse tools more and drift on long tasks. Ask whether the task still works, not only whether the data stayed home.

**References:** Ollama (n.d.); Anthropic (n.d.-c).

---

## 6. Prove it is offline, and isolate it (1 min)

**On screen**
- **Block egress** (firewall rule, or a container with no network), then run a task: it must still work
- **Isolate:** run the agent in a container or VM; use permission modes; give it only the folders it needs
- Link Anthropic's sandbox/isolation guidance in `docs/local_ai/` (URL to be confirmed before presenting)

**Speaker notes**
Do not take "local" on trust; test it. Block outbound network at the firewall, or run the whole stack in a container with networking disabled, and run a real task. If it fails, something was phoning home. Then isolate the agent itself. A local model with shell access can still delete files or read things it should not, so run it in a container or virtual machine, use Claude Code's permission modes, and mount only the folders the task needs. These are the same principles as this morning's cloud sandbox, moved onto your own hardware.

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
