# Answers (open only after you've tried)

## `answer_key.csv`

Reference numbers for Exercises 1–3, computed by [`../make_extracts.py`](../make_extracts.py)
from the same CSVs you uploaded. Columns: `aoi, quantity, value, note`; the `note` says exactly
how each number was computed.

- Shares and areas use the design `weight`. The `forest_share_unweighted` rows are there to show
  the **wrong** answer you get by counting points.
- `sampled_area_ha` is a little under 40,000 ha because permanent water and no-data pixels were
  not eligible for sampling.
- Balanced accuracies are means over the five test folds. Random-forest numbers depend on library
  version and settings, so expect differences of a point or so. Threshold numbers should match
  closely. The `*_random_cv` rows are a **contrast only**: never report them as the result.
- The two `*_emb_sample` rows use only the 300 points in `alphaearth_A_sample.csv`. With five
  folds of 60 points, the difference between them is not strong evidence either way.

## Planted errors in `03_seeded_agent_report.md`

| # | Line in the report | What's wrong | Checklist item | How to confirm |
|---|---|---|---|---|
| 1 | Forest area **40,077 ha**; method 3 | Area computed from a lon/lat bounding box in **square degrees**, ignoring the shrinking of longitude with latitude and treating the box as all forest. It's bigger than the whole 20 × 20 km (40,000 ha) AOI | 3 Spatial, 5 Numbers | `forest_area_ha` for A in the key: about 26,600 ha, from `sum(weight * forest) * 0.01` |
| 2 | Data table: post-fire image **March–May 2019**; pre-fire **2018** | Both dates are wrong. In this package, autumn 2019 is **pre-fire** and autumn 2020 is post-fire; there is no 2018 composite. (The methods section then uses 2019 and 2020 correctly, so the report contradicts itself) | 4 Temporal, 2 Provenance | `data/README.md` and the column names `*_2019`, `*_2020` |
| 3 | **Harrington & Okafor (2021)** | A fabricated reference. The DOI doesn't resolve | 8 Citations | Click the DOI link |
| 4 | `forestmask-au` in requirements | A non-existent package (not on PyPI when checked on 5 Nov 2026). If someone later registers that name, installing it could run anyone's code: this is "slopsquatting" | 7 Packages | Search https://pypi.org; **don't** `pip install` it |
| 5 | Balanced accuracy **0.912**, "ready for operational use" | It's the **random** cross-validation score (`KFold(shuffle=True)`), headlined as the result. The spatial-fold score is lower (`ba_rf_bands2019_spatial_cv`, about 0.90). The overclaim ("operational") isn't supported by either | 5 Numbers, 10 Uncertainty | Rerun with the `fold` column |
| 6 | **61.8 %** of forest burnt; method 2 | The **low** severity class (2) was silently dropped. With classes 2–5 it's about 71 % (`burnt_share_of_forest`) | 1 Scope, 5 Numbers | Recompute with `fesm_severity` in {2, 3, 4, 5} |
| 7 | **74.7 %** of AOI B is forest; method 1 | AOI B's share was computed **without weights**; the weighted estimate is about 80 % (`forest_share_weighted`). The method switches between the two AOIs | 5 Numbers, 6 Code | Compare `forest_share_unweighted` and `forest_share_weighted` for B |
| (8) | Limitations: "None identified" | Not a numeric error, but a red flag: no mention of the stratified design, label definitions, FESM class 0 or spatial folds | 10 Uncertainty | |

Correct statements kept as decoys: AOI A forest share **66.6 %** and the NDVI change table (they
match the key).
