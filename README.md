# ScaleBench — Public Benchmark for Auto-Renormalization in Weather Modeling

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22933596.svg)](https://doi.org/10.5281/zenodo.22933596)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)

**Public page (rolling, bilingual)**: https://dolo-borigjin.github.io/The-everything/ （[中文镜像](https://dolo-borigjin.github.io/The-everything/index-zh.html)）

ScaleBench measures **auto-renormalization**: given a microscopic simulator,
can a machine discover macroscopic effective laws and honestly report their
validity boundaries and error bars? Nine tasks in six families
(Known-Recovery / Cross-Resolution / Long-Horizon / Discovery /
Extrapolation / Chaos-Audit), deterministic leaderboard v1.3
(overall **0.9556**), executable arbitration via `reproduce.py`, and a live
AIFS comparison harness with sha256-provenance rolling archive and measured
baselines (12–120 h, lagged-ensemble CRPS, first valid precipitation ETS
landed 2026-09-24).

- Leaderboard (single source of truth): [leaderboard.json](./leaderboard.json)
- Public rules & reproduction guide: [README-PUBLIC.md](./README-PUBLIC.md)
- Preprint (draft): *Adjudication First: A Reproducible Scoreboard for
  Auto-Renormalization in Weather Modeling* — see Zenodo record for the
  archived release.

## Citation

```bibtex
@software{lswm_scalebench_2026,
  author    = {LSWM Project Team},
  title     = {ScaleBench: A Reproducible Scoreboard for
               Auto-Renormalization in Weather Modeling},
  year      = {2026},
  publisher = {Zenodo},
  version   = {v1.3},
  doi       = {10.5281/zenodo.22933596},
  url       = {https://doi.org/10.5281/zenodo.22933596}
}
```

*ScaleBench is the adjudication component of the LSWM world model. Governance:
benchmark and theory-card formats are open — openness in exchange for standard
status.*
