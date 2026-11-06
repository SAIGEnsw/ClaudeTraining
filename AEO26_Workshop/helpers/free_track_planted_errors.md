# Free track: answers and planted-error key (presenters and helpers only; do not publish)

Kept out of the public repo. **Updated 6 Nov 2026 for the lean dataset**: woody labels (DEA Land Cover
lifeform), years 2019/2020/2025, `fire_severity` 0–4 (0 unburnt, 1 low, 2 moderate, 3 high, 4 extreme).

## `answer_key.csv` (in the repo: `coursework/free_track/answers/answer_key.csv`)

Reference numbers for Exercises 1–3, computed by `coursework/free_track/make_extracts.py` from the same
CSVs participants upload. Columns: `aoi, quantity, value, note`; the `note` says exactly how each number
was computed.

- Shares and areas use the design `weight`. The `woody_share_unweighted` rows show the **wrong** answer
  you get by counting points (A 74.7 % unweighted vs 70.4 % weighted; B 77.8 % vs 80.8 %).
- `sampled_area_ha` is a little under 40,000 ha (A 39,962; B 39,981) because permanent water and
  no-data pixels were not eligible for sampling.
- Balanced accuracies are means over the five test folds. Random-forest numbers depend on library
  version and settings, so expect differences of a point or so; threshold numbers should match closely.
  The `*_random_cv` rows are a **contrast only**: never report them as the result.
- **Teach this honestly:** in this dataset random and spatial CV give almost the same score (RF 0.9468 vs
  0.9467; NDVI threshold 0.816 vs 0.809). The bigger effect is moving to the other fire: the threshold
  drops from 0.809 on A to 0.742 on B. Spatial CV is still the honest default; here it costs little, but
  transfer to a new area does.
- The `*_emb_sample` rows use only the 300 points in `alphaearth_A_sample.csv`; with five folds of 60
  points, differences between them are not strong evidence either way (bands 0.971 vs AlphaEarth 0.955:
  AlphaEarth wins 1 fold, loses 2, ties 2).

## Planted errors in `03_seeded_agent_report.md`

| # | Line in the report | What's wrong | Checklist item | How to confirm |
|---|---|---|---|---|
| 1 | Woody area **59,942 ha**; method 3 | Area from a lon/lat bounding box of the woody points (0.0484 **square degrees**) converted as if degrees were a flat grid, and treating the whole box as woody. It's bigger than the whole 20 × 20 km (40,000 ha) AOI | 3 Spatial, 5 Numbers | `woody_area_ha` for A in the key: about **28,136 ha**, from `sum(weight * woody_2019) * 0.01` (EPSG:3577) |
| 2 | Data table: pre-fire image **March–May 2018**; post-fire **March–May 2019** | Both dates are wrong. In this package autumn 2019 is **pre-fire** and autumn 2020 is post-fire; there is no 2018 composite. (The methods use 2019 and 2020 correctly, so the report contradicts itself) | 4 Temporal, 2 Provenance | `data/README.md` and the column names `*_2019`, `*_2020` |
| 3 | **Harrington & Okafor (2021)** | A fabricated reference. The DOI doesn't resolve | 8 Citations | Click the DOI link |
| 4 | `forestmask-au` in requirements | A non-existent package (not on PyPI when checked on 6 Nov 2026). If someone later registers that name, installing it could run anyone's code ("slopsquatting") | 7 Packages | Search https://pypi.org; **don't** `pip install` it |
| 5 | Balanced accuracy **0.947**, "ready for operational use" | It's the **random** CV score (`KFold(n_splits=5, shuffle=True, ...)`), headlined as the result. Here it happens to equal the spatial-fold score (`ba_rf_bands2019_spatial_cv` 0.9467), so recomputing won't expose it: the errors are the **method** and the **overclaim** (no transfer test, and the A→B threshold score drops to 0.742; no interval; one area) | 6 Code, 10 Uncertainty | Read the method; rerun with the `fold` column and an A→B transfer |
| 6 | **62.0 %** of woody burnt; method 2 | The **low** severity class (1) was silently dropped (`fire_severity` in {2, 3, 4}). With classes 1–4 it's **71.4 %** (`burnt_share_of_woody`) | 1 Scope, 5 Numbers | Recompute with `fire_severity` in {1, 2, 3, 4} |
| 7 | **77.8 %** of AOI B is woody; method 1 | AOI B's share was computed **without weights**; the weighted estimate is **80.8 %** (`woody_share_weighted`). The method switches between the two AOIs | 5 Numbers, 6 Code | Compare `woody_share_unweighted` and `woody_share_weighted` for B |
| (8) | Limitations: "None identified" | Not a numeric error, but a red flag: no mention of the stratified design, the woody label (DEA lifeform at 30 m), the unburnt class 0 or spatial folds | 10 Uncertainty | |

Correct statements kept as decoys: AOI A woody share **70.4 %** (weighted) and the NDVI change table
(weighted, classes 0–4); both match the key.
