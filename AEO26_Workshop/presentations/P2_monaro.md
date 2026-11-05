# P2. Monaro Grasslands: an agentic research programme in practice

> ## ⚠️ NOT CLEARED: needs presenter approval; aggregate results and workflow only; no plot locations; do not link the private repo
>
> The source project (`RSGISagent/MonaroGrasslands`) is private and holds unpublished results and private-land data. Field-plot coordinates are on private land, BCT and ACT data need consent, and the ACT `gee/` code needs permission (project `docs/data_licensing.md`, as summarised in WORKSHOP_PLAN.md B8). This outline contains **only** what WORKSHOP_PLAN.md B8 states. Do not add maps, plot locations, screenshots of the repo, or the repo URL. Do not copy this file into the staging or production workshop repos.

**Slot:** 10:25–10:50, Mon 9 Nov 2026 (Presentations 1, second talk) · **Length:** 25 min · **Presenter:** Adam Roff · **Status:** needs clearance
**Format:** outline and speaker notes only (no slide deck).
**Source:** WORKSHOP_PLAN.md B8 P2, which drew on the project's `docs/research_plan.md` v0.2, `docs/results/red_team_review.md`, `docs/results/errata_2026-09.md`, `docs/reconciliation_2026-09.md` and `outputs/wave3/token_accounting.json` (read 5 Nov 2026). The project's CLAUDE.md requires numbers to be quoted **with the errata and red-team caveats**.

**Presenter checks before the talk (do not skip)**
- [ ] Confirm against `errata_2026-09.md` which of the figures in sections 4–6 are post-errata, and which are maxima over many runs. The plan quotes them as below; the red team found that headline results were maxima over 50–124 runs.
- [ ] Confirm whether the Presto vs AlphaEarth comparison ("8 of 10 folds") used the ten-fold scheme that the red team flagged as uncommitted. If so, say so on screen.
- [ ] Expand "API polygons" and "PCT" for the audience in your own words.

---

## 1. Title, and what this talk is (0.5 min)

**On screen**
- Monaro Grasslands: an agentic research programme in practice
- Unpublished work in progress: aggregate results and workflow only

**Speaker notes**
This is a real research programme run largely by Claude Code agents, presented as a case study in method, not as a results paper. The results are unpublished and the data include private land, so you will see aggregate numbers and the workflow only, no maps of plots and no repository link. I will quote every number with the caveats our own red-team process attached to it, because that process is the point of the talk.

**References:** none.

---

## 2. The problem (3 min)

**On screen**
- Goal: a **10 m map** of Temperate Montane Grassland **composition and condition** across the **Monaro IBRA subregion (12 677 km²)**
- Reference data: **1 153 new rapid plots + 637 legacy plots**, plus **142 API polygons**
- Predictors: **Sentinel-1/2**, **AlphaEarth** embeddings, **Presto** embeddings

**Speaker notes**
The task: map temperate montane grassland across the whole Monaro subregion at ten metres, both what it is made of and what condition it is in. That is about twelve and a half thousand square kilometres, and the plots sit on private land. Reference data come from just over eleven hundred new rapid plots, around six hundred legacy plots and a set of interpreted polygons. Predictors range from conventional Sentinel-1 and Sentinel-2 composites to two foundation-model embeddings: AlphaEarth and Presto, both described in preprints. In other words, the same traditional-versus-foundation-model question your agents are tackling this morning, at a larger scale and with harder labels.

**References:** Brown et al. (2025, preprint); Tseng et al. (2023, preprint); Drusch et al. (2012).

---

## 3. How the agent was organised (4 min)

**On screen**
- A **CLAUDE.md** of working rules
- A research plan written as a **numbered experiment register** with **pre-registered success criteria**
- A **fixed evaluation harness**:
  - committed, hash-verified **spatial folds**
  - **trivial baselines**
  - **paired comparisons** with **fold-bootstrap intervals**
  - an **append-only results registry** carrying the **commit hash**

**Speaker notes**
Three things made this workable. First, a CLAUDE.md file of working rules that every session reads before it does anything. Second, the research plan was written as a numbered register of experiments, each with its success criterion stated before it ran, which is pre-registration in miniature. It stops an agent, or a person, redefining success after seeing the result. Third, a fixed evaluation harness: spatial folds committed to the repo and verified by hash, trivial baselines that every model must beat, paired fold-by-fold comparisons with bootstrap intervals, and a registry that only ever grows and records the commit that produced each row. If that sounds familiar, it is because your agents are working inside a scaled-down copy of it right now.

**References:** Roberts et al. (2017).

---

## 4. Findings I: composition yes, condition grade no (2 min)

**On screen**
- **Composition is learnable:** best balanced accuracy **0.813** (exotic vs native) and **0.687** (PCT)
- **A categorical condition grade is not:** the four-grade model scored **below the majority class**
- Caveat: "best" figures are subject to the red-team correction in section 7

**Speaker notes**
What did we learn? Composition can be mapped. Separating exotic from native grassland reached a best balanced accuracy of 0.813, and plant community type reached 0.687. Note the word "best": I will come back to what "best" means once you have run many configurations. The negative result is just as important. A four-grade categorical condition model scored below simply predicting the majority class. Condition, as currently graded, is not learnable from these predictors. Pre-registration matters here: because the success criterion was written down in advance, that result went into the register as a clear failure rather than being quietly reframed.

**References:** none (project documents, not for public citation).

---

## 5. Findings II: what spatial validation costs (2 min)

**On screen**
- **Spatial validation cost 0.12–0.18 balanced accuracy**
- The same model: **0.595** random folds → **0.477** 2 km groups → **0.413** 5 km groups
- Random folds measure interpolation; spatial folds approximate mapping new places

**Speaker notes**
This is the slide I would most like you to remember at midday. One model, three validation schemes. With random folds it scored 0.595 balanced accuracy. Group the folds into two-kilometre blocks and it drops to 0.477. Five-kilometre blocks: 0.413. That is a loss of twelve to eighteen points from nothing but an honest choice of folds. Random folds put near neighbours in both training and test, so they measure interpolation. A wall-to-wall map is mostly extrapolation to places with no plots. This is the pattern Ploton and colleagues and Roberts and colleagues warned about, and it is why your repo's folds are spatial and committed.

**References:** Ploton et al. (2020); Roberts et al. (2017).

---

## 6. Findings III: models and embeddings (2 min)

**On screen**
- Regularised **logistic regression beat RF** by **0.02–0.07**
- **AlphaEarth** came within **0.03–0.04** of stacks **four times its size**
- Frozen **Presto linear probe 0.842 vs AlphaEarth 0.769** (exotic vs native; **8 of 10 folds, p = 0.064**), but it **lost to Sentinel-2 composites** for PCT and cover
- Caveat: after the red-team correction, the Presto win is **about 1 SE** (section 7)

**Speaker notes**
Three model comparisons. Regularised logistic regression beat random forest by two to seven points: with few labels, a simpler, better-regularised model generalised better. AlphaEarth's sixty-four-dimensional embedding came within three to four points of predictor stacks four times its size, which is impressive compactness, not superiority. And a frozen Presto linear probe scored 0.842 against AlphaEarth's 0.769 for exotic versus native, winning on eight of ten folds, but with p equal to 0.064, and it lost to plain Sentinel-2 composites for community type and cover. So no single representation won everywhere. Hold on to that p-value; the red team had more to say about it.

**References:** Brown et al. (2025, preprint); Tseng et al. (2023, preprint); Belgiu & Drăguţ (2016).

---

## 7. Agents reviewing agents (4 min)

**On screen**
- **Three parallel sessions** on separate branches; **two adversarial red-team reviews**, reconciled and merged
- Tier-0 **plumbing** defects found:
  - imputation fitted across folds (**a train-on-test leak**)
  - an **uncommitted ten-fold scheme**
  - `balanced_accuracy` called **without `labels=`**
  - **undeclared dependencies**
- Headline results were **maxima over 50–124 runs**; corrected, the **Presto win is about 1 SE**
- Corrections went into an **errata file, never by editing published results**

**Speaker notes**
We then pointed agents at the agents. Three sessions worked in parallel on separate branches, and two were briefed as adversarial red teams to find what was wrong. Their reviews were reconciled and merged. They found plumbing defects, the unglamorous kind that silently inflate scores: imputation fitted on all folds at once, which leaks test information into training; a ten-fold scheme that was never committed; a balanced-accuracy call missing its explicit labels argument; and undeclared dependencies. More importantly, they found that headline numbers were maxima over fifty to a hundred and twenty-four runs. Corrected for that, the Presto advantage is about one standard error. Every correction went into an errata file. Published numbers were never edited in place.

**References:** none (project documents, not for public citation).

---

## 8. What it cost (2 min)

**On screen**
- Token accounting: **36 tasks**, **1 452 assistant messages**
- **~2.5 M output tokens**, **~630 M cache-read tokens**
- A single delegation of hours of autonomous work absorbs all of that

**Speaker notes**
What did this cost? Across thirty-six tasks the agents produced one thousand four hundred and fifty-two assistant messages, roughly two and a half million output tokens, and about six hundred and thirty million cache-read tokens. Cache reads dominate because each long session re-reads its accumulated context many times. The practical point: one delegation of "go and do hours of work" absorbs all of that without you seeing it happen. Your own agents this morning are smaller, but the same accounting applies, which is why the workshop asks them to push every twenty minutes and finish by a fixed time.

**References:** none (project token-accounting file, not for public citation).

---

## 9. What still needed a human (2 min)

**On screen**
- **Earth Engine authentication**
- **Field data** collection
- **Data permissions** (private land, consent-bound datasets)
- **Domain judgement**

**Speaker notes**
Agents did not replace the people. Someone had to authenticate to Earth Engine, which an agent should never do with your credentials. Someone had to collect the field data on private land. Someone had to negotiate data permissions and consent, which is exactly why I cannot show you the plots today. And someone had to exercise domain judgement: whether a condition grade is even a meaningful mapping target, and whether a one-standard-error difference matters for management. Agents compress the analysis loop dramatically. They do not compress fieldwork, relationships, permissions or ecological expertise.

**References:** none.

---

## 10. Lessons, mapped onto your repo (3 min)

**On screen**
| Monaro lesson | In your AEO26 fork |
|---|---|
| Random folds flatter models | `folds_v1.csv`, hash-checked by the harness |
| Leaks hide in plumbing | Baselines first; the reference model; the 12:00 audit |
| "Best" is a maximum over many runs | Append-only `results/registry.csv`; show the distribution |
| Small wins can be noise | Paired fold-bootstrap 95 % intervals |
| Rules must outlive one session | `CLAUDE.md` |
| Fix forward, never rewrite | Errata, not edits; never delete registry rows |

**Speaker notes**
Each guard-rail in the workshop repo exists because of something in this table. Committed spatial folds, because random folds flattered us by up to eighteen points. Baselines and a reference model first, because leaks hide in plumbing and a suspiciously good result needs a yardstick. An append-only registry, because our "best" numbers were maxima over dozens of runs. Paired intervals, because a win of one standard error is not a finding. CLAUDE.md, because rules have to outlive any single session. And errata rather than edits, because the record of what you claimed matters. At midday, audit your agent's pull request as if it were our red team.

**References:** Ploton et al. (2020); Botvinik-Nezer et al. (2020).

---

## 11. Questions (0.5 min)

**On screen**
- Questions (aggregate and method only)

**Speaker notes**
Take one or two questions. If asked about sites, data access or the repository, say that the data include private-land and consent-bound records and the results are unpublished, so those details cannot be shared. Then hand over to morning tea, reminding everyone to check their agent from their phone.

**References:** none.

---

## References

Belgiu, M., & Drăguţ, L. (2016). Random forest in remote sensing: A review of applications and future directions. *ISPRS Journal of Photogrammetry and Remote Sensing, 114*, 24–31. https://doi.org/10.1016/j.isprsjprs.2016.01.011

Botvinik-Nezer, R., Holzmeister, F., Camerer, C. F., Dreber, A., Huber, J., Johannesson, M., Kirchler, M., et al. (2020). Variability in the analysis of a single neuroimaging dataset by many teams. *Nature, 582*(7810), 84–88. https://doi.org/10.1038/s41586-020-2314-9

Brown, C. F., Kazmierski, M. R., Pasquarella, V. J., Rucklidge, W. J., et al. (2025). *AlphaEarth Foundations: An embedding field model for accurate and efficient global mapping from sparse label data* [Preprint]. arXiv. https://doi.org/10.48550/arXiv.2507.22291

Drusch, M., Del Bello, U., Carlier, S., Colin, O., Fernandez, V., Gascon, F., Hoersch, B., et al. (2012). Sentinel-2: ESA's optical high-resolution mission for GMES operational services. *Remote Sensing of Environment, 120*, 25–36. https://doi.org/10.1016/j.rse.2011.11.026

Ploton, P., Mortier, F., Réjou-Méchain, M., Barbier, N., Picard, N., Rossi, V., Dormann, C., et al. (2020). Spatial validation reveals poor predictive performance of large-scale ecological mapping models. *Nature Communications, 11*, 4540. https://doi.org/10.1038/s41467-020-18321-y

Roberts, D. R., Bahn, V., Ciuti, S., Boyce, M. S., Elith, J., Guillera-Arroita, G., Hauenstein, S., et al. (2017). Cross-validation strategies for data with temporal, spatial, hierarchical, or phylogenetic structure. *Ecography, 40*(8), 913–929. https://doi.org/10.1111/ecog.02881

Tseng, G., Cartuyvels, R., Zvonkov, I., Purohit, M., Rolnick, D., & Kerner, H. (2023). *Lightweight, pre-trained transformers for remote sensing timeseries* [Preprint]. arXiv. https://doi.org/10.48550/arXiv.2304.14065

**Internal sources (presenter only; do not display or link):** Monaro Grasslands project `docs/research_plan.md` v0.2, `docs/results/red_team_review.md`, `docs/results/errata_2026-09.md`, `docs/reconciliation_2026-09.md`, `outputs/wave3/token_accounting.json`, `docs/session_setup.md`, `docs/data_licensing.md` (as summarised in WORKSHOP_PLAN.md B8, read 5 Nov 2026).

*All published references are from WORKSHOP_PLAN.md (Part A and Part B lists, checked with Scite on 4–5 Nov 2026). Author lists shortened with "et al." follow the plan.*
