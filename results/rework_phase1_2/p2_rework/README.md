# TD metric recomputation scripts

Scripts in this directory recompute Technical Density (TD) statistics under the unified
47-term vocabulary and matching rule defined in the paper (Appendix A.1-A.3), and reproduce
the numbers reported in Section 4.3/4.5/4.7, Table 2/5, and Appendix A.4.

All scripts import the canonical vocabulary and matching logic from `../td_metric.py`
(`CANONICAL_TERMS`), so there is no duplicated vocabulary definition in this folder.

## Files

- `recompute_td_significance_n50.py` — recomputes TD means and the three pairwise
  Mann-Whitney U / p / r values (beginner-intermediate, beginner-expert, intermediate-expert)
  for five models (ctrl_baseline, specialist_beginner, specialist_intermediate,
  specialist_expert, multilevel), using true independent n=50 samples per level.
  - `ctrl_baseline` requires de-duplication first: its raw export contains each of the 50
    unique predictions repeated 3x byte-for-byte (the `exp` field carries no seed marker),
    so de-duplication keeps the first occurrence per code rather than averaging across seeds.
  - The other four models (`specialist_*`, `multilevel`) have genuine independent training
    seeds (s42/s123/s456) in the `exp` field, so their TD per code is the mean across the
    three seeds.
- `recompute_target_td_train750.py` — computes the target TD directly from the 750 training
  references (`data/train_750.json`, 250 per level) under the canonical 47-term vocabulary,
  used for the Appendix A.4 target-TD achievement table.
- `language_degradation_analysis.py` — reproduces the Section 4.7 language-degradation
  analysis and Table 5. Computes the zero-Chinese-character output rate across 9 models x 3
  levels (27 cells), and for the three-seed models (`specialist_*`, `multilevel`) performs a
  per-code breakdown (all-3-seeds-degraded / none-degraded / some-but-not-all) plus the overlap
  between zero_shot's degraded-code set and specialist_expert's all-3-degraded-code set at the
  intermediate level.
