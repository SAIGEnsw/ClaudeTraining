# P3. Traditional vs foundation models for forest mapping: evidence and pitfalls

**Slot:** 11:20–11:35, Mon 9 Nov 2026 (Presentations 2, first talk) · **Length:** 15 min · **Presenter:** TBC · **Status:** draft
**Format:** outline and speaker notes only (no slide deck).
**Context:** agents have been running for about 70 minutes; results and audit start at 12:00. Preprints are labelled *preprint* on screen, as the plan requires.

---

## 1. Why forest/non-forest, and why here (2 min)

**On screen**
- Black Summer 2019–20 burnt an **unprecedented** area of Australian forest (Boer et al., 2020)
- Overlaying the fire extent on mapped habitat suggested large impacts on native fauna habitat (Ward et al., 2020)
- Later field work nuances this: 20 months on, fire severity had **limited effects on the abundance of most species** in one south-eastern study system (Linley et al., 2025)
- So: is burnt forest still forest, and how would we know from 3 bands?

**Speaker notes**
Your agents are mapping forest and change in greater glider habitat across 2019, 2020 and recovery years. The context matters. Boer and colleagues showed the 2019–20 fires burnt an unprecedented share of Australia's forests. Ward and colleagues overlaid the fire extent on mapped habitat and estimated how much of each native species' habitat burnt. Field studies since have added nuance. Linley and colleagues, in Oikos, used wildlife cameras across about 180 000 hectares of eucalypt forest twenty months after the fires. Severity changed abundance in eight of twenty-nine species, and only four responded negatively. They note these were mostly common species, and threatened species may differ. Habitat exposure and population response are different questions; keep them separate.

**References:** Boer et al. (2020); Ward et al. (2020); Linley et al. (2025) [newly verified 5 Nov 2026].

---

## 2. The traditional baseline is strong (2.5 min)

**On screen**
- **Random forest** is a mature, well-understood workhorse in remote sensing (Belgiu & Drăguţ, 2016)
- **Sentinel-2** was designed for exactly this: 10–20 m land bands, frequent revisit (Drusch et al., 2012)
- NSW's own fire-severity mapping uses **Sentinel-2 + random forest** (Gibson et al., 2020)
- Global forest change mapping from Landsat set the reference for scale (Hansen et al., 2013)

**Speaker notes**
Before we talk about foundation models, give the traditional approach its due. Belgiu and Drăguţ reviewed random forest in remote sensing: it is robust, handles mixed predictors, and has well-understood failure modes. Sentinel-2 was designed for land monitoring at ten to twenty metres, which is why three bands, red, near-infrared and shortwave infrared, already carry most of the forest signal. Closer to home, Gibson and colleagues built New South Wales fire-severity mapping on Sentinel-2 and random forest, and that is the lineage of the severity layer in your repo. Hansen and colleagues showed what global, consistent forest-change mapping looks like. That is the bar a foundation model has to clear.

**References:** Belgiu & Drăguţ (2016); Drusch et al. (2012); Gibson et al. (2020); Hansen et al. (2013).

---

## 3. Foundation-model claims (2.5 min)

**On screen**
- **AlphaEarth Foundations:** 64-d annual embeddings; claims accurate mapping from **sparse labels** (Brown et al., 2025, *preprint*)
- **TESSERA:** 128-d temporal embeddings of surface spectra (Feng et al., 2025, *preprint*)
- **Presto:** lightweight pre-trained transformer for pixel time series (Tseng et al., 2023, *preprint*)
- **Prithvi-EO-2.0:** multi-temporal image foundation model (Szwarcman et al., 2024, *preprint*)
- All four are **preprints**: claims, not settled evidence

**Speaker notes**
The foundation-model arm of your research cards uses two of these. AlphaEarth gives a sixty-four-dimensional embedding per ten-metre pixel per year, and its authors claim accurate mapping from sparse labels. TESSERA gives a hundred-and-twenty-eight-dimensional embedding built from the time series of surface spectra. Presto is a small transformer for pixel time series, which Adam's Monaro work probed. Prithvi-EO-2.0 is a multi-temporal image model, a stretch option. The attraction is clear: someone else has done the expensive pre-training, so a linear probe on a few hundred labels might match a model trained from scratch. But note the labels on screen. All four are preprints. Treat their headline numbers as claims to test, which is what your agent is doing.

**References:** Brown et al. (2025, preprint); Feng et al. (2025, preprint); Tseng et al. (2023, preprint); Szwarcman et al. (2024, preprint).

---

## 4. Counter-evidence and local results (2.5 min)

**On screen**
- Independent benchmark **PANGAEA**: geospatial foundation models **do not consistently outperform supervised baselines** (Marsocci et al., 2024, *preprint*)
- **GEO-Bench**: a shared benchmark for testing Earth-monitoring foundation models under common protocols (Lacoste et al., 2023, *preprint*)
- Local NSW woody-vegetation mapping results (Backman et al., 2026a, 2026b)
- Monaro (P2): logistic regression beat RF; no single representation won everywhere

**Speaker notes**
Independent benchmarks are more sobering. Marsocci and colleagues' PANGAEA benchmark found that geospatial foundation models do not consistently beat supervised baselines. GEO-Bench, from Lacoste and colleagues, matters for a different reason: it puts models on common tasks and protocols, which is what makes such comparisons possible at all. Both are preprints. Locally, Backman, Wood and Roff have two peer-reviewed 2026 papers on woody-vegetation mapping in New South Wales, one on loss alignment for zero-shot regrowth and woody segmentation, and one on prediction fusion for data efficiency. [Adam: one sentence on the key finding of each.] And from the last talk: in Monaro, simple regularised models were competitive and no embedding won everywhere.

**References:** Marsocci et al. (2024, preprint); Lacoste et al. (2023, preprint); Backman et al. (2026a, 2026b).

---

## 5. How to compare fairly (2 min)

**On screen**
- **Spatial validation:** random cross-validation overstates map accuracy when data are spatially autocorrelated (Ploton et al., 2020; Roberts et al., 2017)
- **Same folds, same labels, paired differences** for every arm
- **Area and accuracy:** stratified estimation with error-adjusted area and confidence intervals, in an equal-area CRS (EPSG:3577) (Olofsson et al., 2014)

**Speaker notes**
Most published disagreements between these camps come down to evaluation. Ploton and colleagues showed in Nature Communications that large-scale ecological maps that look excellent under random cross-validation can have poor predictive performance under spatial validation. Roberts and colleagues set out why: structured data need structured folds. So every arm, traditional or foundation, must use the same spatial folds and labels, and be compared fold by fold. For change and area, Olofsson and colleagues' good-practice guidance applies: report error-adjusted areas with confidence intervals, not pixel counts, and compute them in an equal-area projection. Your repo's CLAUDE.md asks your agent for all of this. Whether it complied is what you check at noon.

**References:** Ploton et al. (2020); Roberts et al. (2017); Olofsson et al. (2014).

---

## 6. How to read your agent's results at 12:00 (3 min)

**On screen**
1. **Folds:** did it use `folds_v1.csv` via the harness (hash verified)? Any random-fold number is **not** a headline (audit checks 5, 6)
2. **Baselines:** does the method beat the trivial baseline and the reference RF, on the same folds? (checks 5, 10)
3. **Maxima:** how many rows did it add to `results/registry.csv`? Is "best" shown beside the distribution? (checks 1, 10)
4. **Intervals:** does the paired fold-bootstrap 95 % interval for the difference **include 0**? Then there is no evidence of a difference (check 10)
5. **Areas:** EPSG:3577 and error-adjusted intervals? (check 3)

**Speaker notes**
In twenty-five minutes you open your agent's pull request. Use these five questions alongside the ten-point audit checklist. First, folds: confirm the harness loaded the committed, hash-checked spatial folds; random-fold numbers may appear but must never be the headline. Second, baselines: a foundation model that does not beat the reference random forest on the same folds has not won. Third, maxima: count the registry rows; if your agent tried thirty configurations and reports the best, that is the Monaro red-team finding again. Fourth, intervals: if the paired interval for the difference includes zero, say "no evidence of a difference". With five folds, intervals are wide. Fifth, areas: equal-area projection, error-adjusted intervals. Then compare yours with the leaderboard distribution.

**References:** Ploton et al. (2020); Olofsson et al. (2014); Botvinik-Nezer et al. (2020).

---

## 7. Hand-over (0.5 min)

**On screen**
- Next: P4, Sovereign AI, where the model and the data live

**Speaker notes**
Everything so far assumed your data are public and your agent runs on someone else's computer. That is fine for open Sentinel-2 composites. It is not fine for private-land plots, threatened-species locations or Indigenous data. Our next speaker shows what running the model yourself looks like, and where its limits are.

**References:** none.

---

## References

Backman, K., Wood, J., & Roff, A. (2026a). Learning woody clearing with loss alignment for zero-shot regrowth and woody segmentation. *IEEE Transactions on Geoscience and Remote Sensing, 64*, 1–18. https://doi.org/10.1109/TGRS.2026.3676347

Backman, K., Wood, J., & Roff, A. (2026b). Mapping woody vegetation from multisource imagery and prediction fusion for enhanced data efficiency and accuracy. *IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing, 19*, 9856–9867. https://doi.org/10.1109/JSTARS.2026.3672149

Belgiu, M., & Drăguţ, L. (2016). Random forest in remote sensing: A review of applications and future directions. *ISPRS Journal of Photogrammetry and Remote Sensing, 114*, 24–31. https://doi.org/10.1016/j.isprsjprs.2016.01.011

Boer, M. M., Resco de Dios, V., & Bradstock, R. A. (2020). Unprecedented burn area of Australian mega forest fires. *Nature Climate Change, 10*(3), 171–172. https://doi.org/10.1038/s41558-020-0716-1

Botvinik-Nezer, R., Holzmeister, F., Camerer, C. F., Dreber, A., Huber, J., Johannesson, M., Kirchler, M., et al. (2020). Variability in the analysis of a single neuroimaging dataset by many teams. *Nature, 582*(7810), 84–88. https://doi.org/10.1038/s41586-020-2314-9

Brown, C. F., Kazmierski, M. R., Pasquarella, V. J., Rucklidge, W. J., et al. (2025). *AlphaEarth Foundations: An embedding field model for accurate and efficient global mapping from sparse label data* [Preprint]. arXiv. https://doi.org/10.48550/arXiv.2507.22291

Drusch, M., Del Bello, U., Carlier, S., Colin, O., Fernandez, V., Gascon, F., Hoersch, B., et al. (2012). Sentinel-2: ESA's optical high-resolution mission for GMES operational services. *Remote Sensing of Environment, 120*, 25–36. https://doi.org/10.1016/j.rse.2011.11.026

Feng, Z., Atzberger, C., Jaffer, S., Knezevic, J., et al. (2025). *TESSERA: Temporal embeddings of surface spectra for Earth representation and analysis* [Preprint]. arXiv:2506.20380. https://arxiv.org/abs/2506.20380

Gibson, R., Danaher, T., Hehir, W., & Collins, L. (2020). A remote sensing approach to mapping fire severity in south-eastern Australia using Sentinel 2 and random forest. *Remote Sensing of Environment, 240*, 111702. https://doi.org/10.1016/j.rse.2020.111702

Hansen, M. C., Potapov, P. V., Moore, R., Hancher, M., Turubanova, S. A., Tyukavina, A., Thau, D., et al. (2013). High-resolution global maps of 21st-century forest cover change. *Science, 342*(6160), 850–853. https://doi.org/10.1126/science.1244693

Lacoste, A., et al. (2023). *GEO-Bench: Toward foundation models for Earth monitoring* [Preprint]. arXiv:2306.03831 (NeurIPS 2023 Datasets and Benchmarks; check the proceedings citation and full author list). https://doi.org/10.48550/arXiv.2306.03831

Linley, G. D., Jolly, C. J., Wooster, E. I. F., Cunningham, C. X., Spencer, E. E., Westaway, D. M., & Nimmo, D. G. (2025). The impact of gigafire on vegetation structure, terrestrial vertebrate abundance and diel activity. *Oikos, 2025*(10), e11022. https://doi.org/10.1002/oik.11022 [newly verified 5 Nov 2026]

Marsocci, V., Jia, Y., Le Bellier, G., Kerekes, D., et al. (2024). *PANGAEA: A global and inclusive benchmark for geospatial foundation models* [Preprint]. arXiv:2412.04204. https://doi.org/10.48550/arXiv.2412.04204

Olofsson, P., Foody, G. M., Herold, M., Stehman, S. V., Woodcock, C. E., & Wulder, M. A. (2014). Good practices for estimating area and assessing accuracy of land change. *Remote Sensing of Environment, 148*, 42–57. https://doi.org/10.1016/j.rse.2014.02.015

Ploton, P., Mortier, F., Réjou-Méchain, M., Barbier, N., Picard, N., Rossi, V., Dormann, C., et al. (2020). Spatial validation reveals poor predictive performance of large-scale ecological mapping models. *Nature Communications, 11*, 4540. https://doi.org/10.1038/s41467-020-18321-y

Roberts, D. R., Bahn, V., Ciuti, S., Boyce, M. S., Elith, J., Guillera-Arroita, G., Hauenstein, S., et al. (2017). Cross-validation strategies for data with temporal, spatial, hierarchical, or phylogenetic structure. *Ecography, 40*(8), 913–929. https://doi.org/10.1111/ecog.02881

Szwarcman, D., et al. (2024). *Prithvi-EO-2.0: A versatile multi-temporal foundation model for Earth observation applications* [Preprint]. arXiv:2412.02732 (full author list to be taken from arXiv). https://doi.org/10.48550/arXiv.2412.02732

Tseng, G., Cartuyvels, R., Zvonkov, I., Purohit, M., Rolnick, D., & Kerner, H. (2023). *Lightweight, pre-trained transformers for remote sensing timeseries* [Preprint]. arXiv. https://doi.org/10.48550/arXiv.2304.14065

Ward, M., Tulloch, A. I. T., Radford, J. Q., Williams, B. A., Reside, A. E., Macdonald, S. L., Mayfield, H. J., et al. (2020). Impact of 2019–2020 mega-fires on Australian fauna habitat. *Nature Ecology & Evolution, 4*(10), 1321–1326. https://doi.org/10.1038/s41559-020-1251-1

*Verification notes.* All references except Linley et al. (2025) are from WORKSHOP_PLAN.md (Part B list, checked with Scite on 5 Nov 2026). **Linley et al. (2025)** was verified on 5 Nov 2026: Scite `verify_papers` returned "verified" with no editorial notices (no retraction, correction or concern); Scite `search_literature` returned the abstract, which was read; authors, year, journal, volume, issue and article number were confirmed against the Crossref record for the DOI. Scite classifies its citation of Ward et al. (2020) as *contrasting*: the authors write that, contrary to predictions of catastrophic population decline, impacts on the relative abundance of the species they studied were relatively minor, while noting their detections were predominantly common, non-threatened species. Present it as nuance, not refutation: Ward et al. estimated habitat exposure; Linley et al. measured local abundance of mostly common species 20 months post-fire.
