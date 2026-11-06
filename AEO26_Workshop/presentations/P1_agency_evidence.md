# P1. Three levels of agency, and what the evidence says

**Slot:** 10:10–10:25, Mon 9 Nov 2026 (Presentations 1, first talk) · **Length:** 15 min · **Presenter:** TBC · **Status:** draft
**Format:** outline and speaker notes only (no slide deck). "On screen" bullets can be shown from this file or the repo's GitHub Pages site.
**Context:** participants' cloud agents were launched by 10:10 and are working through their research cards while this talk runs.

---

## 1. Your agent is working right now (1 min)

**On screen**
- ~40 agents launched in the last 15 minutes, all on one dataset
- Each is planning, coding, fitting models and pushing to your fork
- Question for this talk: why should you believe what comes back at 12:00?

**Speaker notes**
Welcome back. Before you sat down, each of you briefed an agent and walked away. Right now about forty of them are cloning the repo, reading CLAUDE.md, fitting baselines and pushing commits to your forks. Some will finish with a clean pull request. Some will produce a beautiful map with a subtle error in it. At midday you will review that work, and you did not watch it being done. This talk sets up that audit. First, where autonomous agents sit on a ladder of agency. Then what controlled studies say about humans working with AI-generated code and text. Finally, why the repo your agent is working in has a fixed evaluation harness.

**References:** none.

---

## 2. Three levels of agency: a ladder (2.5 min)

**On screen**
- **Vibe coding:** you describe the outcome; the model writes code; you iterate on what's wrong
- **Agentic tool use:** the model chooses tools (search, code execution, files, connectors) in a think–act–observe loop
- **Autonomous agents:** you write a brief, set boundaries, and walk away
- The higher you climb, the more you delegate, and the more you must verify

**Speaker notes**
The repo README describes three levels of agency. Treat them as a ladder. At the bottom, vibe coding: you describe the result, the model writes and runs code, you say what is wrong. You see every step, though you may not read the code. In the middle, agentic tool use: the model decides which tools to call and in what order. You can still inspect the tool trace. At the top, which is where your agents are now, you write a brief, set boundaries and leave. The work you check at midday is work you never saw. Each rung buys productivity with oversight. The rest of this talk is about what that trade costs, and how to pay for it deliberately.

**References:** none (README framing).

---

## 3. Evidence: confidence is not correctness (3 min)

**On screen**
- With an AI assistant, developers wrote **significantly less secure code** and were **more confident** it was secure (Perry et al., 2023)
- RCT, 16 experienced open-source developers: early-2025 AI tools made them **19 % slower**; they forecast a 24 % speed-up and still believed afterwards they had been ~20 % faster (Becker et al., 2025, *preprint*)

**Speaker notes**
Two studies set the tone. Perry and colleagues ran a user study at a top security conference: participants with an AI assistant wrote significantly less secure code than those without, and were more likely to believe their code was secure. Becker and colleagues ran a randomised controlled trial with sixteen experienced open-source developers on their own repositories. With early-2025 AI tools they took nineteen per cent longer. Before starting they forecast a twenty-four per cent speed-up, and afterwards they still believed they had been about twenty per cent faster. That is a preprint, and sixteen developers is a small sample, so treat it as a warning, not a law. The shared lesson: our sense of how well AI-assisted work went is a poor measurement instrument.

**References:** Perry et al. (2023); Becker et al. (2025, preprint).

---

## 4. Evidence: things that do not exist (2.5 min)

**On screen**
- Code LLMs recommend **non-existent packages**: on average at least **5.2 %** for commercial models, **21.7 %** for open-source models; attackers can register those names ("slopsquatting") (Spracklen et al., 2024, *preprint*)
- **55 %** of GPT-3.5 and **18 %** of GPT-4 citations in generated literature reviews were **fabricated** (Walters & Wilder, 2023)
- Your agent's `requirements` and reference list are both audit targets

**Speaker notes**
The second family of failures is plausible things that do not exist. Spracklen and colleagues measured package hallucination across many code-generating models: on average at least five point two per cent of recommended packages did not exist for commercial models, and twenty-one point seven per cent for open-source ones. Anyone can publish a package under a hallucinated name, which turns a hallucination into a supply-chain attack. That paper is a preprint accepted to USENIX Security 2025. Walters and Wilder, in Scientific Reports, found that fifty-five per cent of GPT-3.5's citations and eighteen per cent of GPT-4's were fabricated. Models have improved since, and tools help, but at midday check that every package exists and every DOI resolves to a paper that supports the claim.

**References:** Spracklen et al. (2024, preprint); Walters & Wilder (2023).

---

## 5. The human factor: automation bias (1.5 min)

**On screen**
- Automation bias and complacency are well-documented human factors (Parasuraman & Manzey, 2010)
- They get worse when you did not watch the work being done
- An unwatched run needs a systematic audit, not a vibe check

**Speaker notes**
Why do capable people miss these errors? Parasuraman and Manzey reviewed decades of human-factors research on automation. Two effects recur: complacency, where we monitor automated systems less carefully than we should, and automation bias, where we accept automated outputs without enough scrutiny. Both are worse when we were not involved in producing the output. That describes exactly what happens at midday: you will open a pull request full of confident prose, tidy figures and green ticks, produced while you were drinking coffee. The answer is not to distrust everything; it is to check systematically. That is why the 12:00 session uses the ten-point audit checklist, not a general impression.

**References:** Parasuraman & Manzey (2010).

---

## 6. EO context: autonomous GIS on open data cubes (1 min)

**On screen**
- LLM-driven "autonomous GIS" is an active research direction (Li & Ning, 2023)
- It builds on open, analysis-ready infrastructure Australians already use: the Australian Geoscience Data Cube and Digital Earth Australia (Lewis et al., 2017; Dhu et al., 2017)

**Speaker notes**
None of this is abstract for our field. Li and Ning set out a vision of autonomous GIS, where a language model plans and executes spatial analyses. The foundations it would stand on are already here: the Australian Geoscience Data Cube and Digital Earth Australia give us open, analysis-ready Sentinel and Landsat archives. Your agents are using that kind of data now. Agentic EO is therefore not hypothetical; the question is whether its outputs are trustworthy enough to inform management of threatened-species habitat. That depends far less on the model than on the scaffolding around it.

**References:** Li & Ning (2023); Lewis et al. (2017); Dhu et al. (2017).

---

## 7. Why this repo has a harness (3 min)

**On screen**
- **Committed, hashed spatial folds** (`data/folds/folds_v1.csv`, SHA-256 checked by `aeo26.harness`): your agent cannot quietly pick easier folds, or leak neighbouring pixels across train and test
- **Baselines first:** a trivial baseline and a reference random forest, so "0.9 accuracy" has something to beat
- **Paired fold-bootstrap 95 % intervals:** differences are reported with uncertainty, fold by fold
- **Append-only registry** (`results/registry.csv`, with commit hash): every configuration tried stays on record, so a "best" result sits beside its distribution
- **Subagents, then an auditor:** your agent hands the traditional and AlphaEarth arms to two subagents running in parallel, a map-maker draws the figures, and an independent **auditor** subagent (fresh context; it can only write its report) checks the run before the PR. Agents reviewing agents

**Speaker notes**
Here is the practical answer. Your agent is working inside a harness it cannot easily game. The spatial folds are committed and hash-checked; nearby points are spatially autocorrelated, so random folds flatter models, and an agent optimising a score will drift towards them if allowed. It must run a trivial baseline and a reference random forest before anything fancy. Comparisons are paired by fold, with bootstrap intervals, so a gain that sits inside the noise is visible as such. And every configuration is appended to the registry with its commit hash, never deleted. If your agent tried forty models, you will see forty rows, not just the winner. And your agent is not working alone: it is orchestrating subagents. Two run the method arms in parallel, each with a complete brief, because a subagent sees nothing else. A map-maker draws the figures. Then an auditor subagent, with a fresh context and permission to write only its report, checks the run against the same rules before the pull request. These are the same guard-rails, learned the hard way, that Adam will describe in the next talk, including agents reviewing agents.

**References:** Roberts et al. (2017); Ploton et al. (2020); Botvinik-Nezer et al. (2020).

---

## 8. Over morning tea (0.5 min)

**On screen**
- Open the Claude app → **Code** tab → your session
- Is it pushing? Did it ask a question? Answer briefly, then leave it alone
- At 12:00: the audit checklist and the leaderboard

**Speaker notes**
At morning tea, check your agent from your phone. If it has stopped to ask something, answer briefly and let it continue. Look at whether it has pushed yet and whether the registry is growing. Resist the urge to steer it towards a better number; that is exactly the behaviour the harness exists to make visible. Next, Adam on what a much larger agentic research programme looked like in practice.

**References:** none.

---

## References

Becker, J., Rush, N., Barnes, E., & Rein, D. (2025). *Measuring the impact of early-2025 AI on experienced open-source developer productivity* [Preprint]. arXiv. https://doi.org/10.48550/arXiv.2507.09089

Botvinik-Nezer, R., Holzmeister, F., Camerer, C. F., Dreber, A., Huber, J., Johannesson, M., Kirchler, M., et al. (2020). Variability in the analysis of a single neuroimaging dataset by many teams. *Nature, 582*(7810), 84–88. https://doi.org/10.1038/s41586-020-2314-9

Dhu, T., Dunn, B., Lewis, B., Lymburner, L., Mueller, N., Telfer, E., Lewis, A., McIntyre, A., Minchin, S., & Phillips, C. (2017). Digital Earth Australia – unlocking new value from earth observation data. *Big Earth Data, 1*(1–2), 64–74. https://doi.org/10.1080/20964471.2017.1402490

Lewis, A., Oliver, S., Lymburner, L., et al. (2017). The Australian Geoscience Data Cube — Foundations and lessons learned. *Remote Sensing of Environment, 202*, 276–292. https://doi.org/10.1016/j.rse.2017.03.015

Li, Z., & Ning, H. (2023). Autonomous GIS: The next-generation AI-powered GIS. *International Journal of Digital Earth, 16*(2), 4668–4686. https://doi.org/10.1080/17538947.2023.2278895

Parasuraman, R., & Manzey, D. H. (2010). Complacency and bias in human use of automation: An attentional integration. *Human Factors, 52*(3), 381–410. https://doi.org/10.1177/0018720810376055

Perry, N., Srivastava, M., Kumar, D., & Boneh, D. (2023). Do users write more insecure code with AI assistants? In *Proceedings of the 2023 ACM SIGSAC Conference on Computer and Communications Security* (pp. 2785–2799). https://doi.org/10.1145/3576915.3623157

Ploton, P., Mortier, F., Réjou-Méchain, M., Barbier, N., Picard, N., Rossi, V., Dormann, C., et al. (2020). Spatial validation reveals poor predictive performance of large-scale ecological mapping models. *Nature Communications, 11*, 4540. https://doi.org/10.1038/s41467-020-18321-y

Roberts, D. R., Bahn, V., Ciuti, S., Boyce, M. S., Elith, J., Guillera-Arroita, G., Hauenstein, S., et al. (2017). Cross-validation strategies for data with temporal, spatial, hierarchical, or phylogenetic structure. *Ecography, 40*(8), 913–929. https://doi.org/10.1111/ecog.02881

Spracklen, J., Wijewickrama, R., Sakib, A. H. M. N., Maiti, A., Viswanath, B., & Jadliwala, M. (2024). *We have a package for you! A comprehensive analysis of package hallucinations by code generating LLMs* [Preprint; to appear in USENIX Security 2025]. arXiv. https://doi.org/10.48550/arXiv.2406.10279

Walters, W. H., & Wilder, E. I. (2023). Fabrication and errors in the bibliographic citations generated by ChatGPT. *Scientific Reports, 13*, 14045. https://doi.org/10.1038/s41598-023-41032-5

*All references are from WORKSHOP_PLAN.md (Part A and Part B lists, checked with Scite on 4–5 Nov 2026). Author lists shortened with "et al." follow the plan; use full lists in any published version.*
