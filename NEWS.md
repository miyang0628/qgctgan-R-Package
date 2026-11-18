# qgctgan 0.1.0

* Initial release.
* `qgctgan()`: quality-aware cluster-conditioned oversampling with the two-stage
  intra-cluster quality filter (composite quality score with adaptive per-cluster
  rejection quantile, followed by greedy MMD selection).
* `gctgan()`: unfiltered baseline sharing the same generative base, to isolate
  the contribution of the filter.
* Pluggable generators via `qgctgan_generator()`, with a dependency-free
  `generator_gaussian()` default and an optional `generator_ctgan()` backend
  (via reticulate / the Python 'ctgan' package).
