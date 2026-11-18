# qgctgan

<!-- badges: start -->
<!-- badges: end -->

**Quality-aware cluster-conditioned oversampling with intra-cluster filtering.**

`qgctgan` is a generator-agnostic R implementation of the Q-G-CTGAN method for
imbalanced tabular classification (Yang, 2026, *Applied Soft Computing*). The
minority class is partitioned into sub-clusters with a Gaussian mixture model, a
generator is fitted per sub-cluster, and synthetic candidates pass a two-stage
quality filter before being added to the training set:

- **Stage 1** — a composite quality score
  `Q(x) = ⅓·Ŝ_density + ⅓·Ŝ_sep + ⅓·Ŝ_lof` combining intra-cluster density
  fidelity, class-boundary separability and local-outlier behaviour, with an
  **adaptive per-cluster rejection quantile** `α_c` calibrated to cluster
  stability.
- **Stage 2** — a greedy **maximum mean discrepancy (MMD)** selection that
  aligns the retained synthetic set with the real minority distribution.

The generator is **pluggable**. A dependency-free Gaussian generator is the
default, so the whole pipeline runs without Python; an optional adapter calls the
Python `ctgan` package through **reticulate** to reproduce the GAN-based
generation of the reference study.

## Installation

From the built source tarball (no compiler required; pure-R package):

```r
install.packages(c("mclust", "dbscan"))             # dependencies
install.packages("qgctgan_0.1.0.tar.gz", repos = NULL, type = "source")
```

Once published to a Git host you can also use
`remotes::install_github("<owner>/qgctgan")`.

## Usage

```r
library(qgctgan)

## x: numeric feature matrix (ideally standardised); y: binary labels
res <- qgctgan(x, y, seed = 1)

res$x          # rebalanced features
res$y          # rebalanced labels
res$synthetic  # the synthetic rows that were added
res$info       # best_k, alpha_vec, MMD before/after, per-cluster quality info
```

Isolate the filter's contribution with the unfiltered baseline:

```r
base <- gctgan(x, y, seed = 1)   # same generative base, no filter
```

Use CTGAN for high-fidelity generation (needs `reticulate` + Python `ctgan`):

```r
res <- qgctgan(x, y, generator = generator_ctgan(epochs = 100), seed = 1)
```

Plug in any generator:

```r
gen <- qgctgan_generator(
  fit    = function(X) list(mu = colMeans(X),
                            L  = chol(cov(X) + diag(1e-6, ncol(X)))),
  sample = function(m, n)
    sweep(matrix(rnorm(n * length(m$mu)), n) %*% m$L, 2, m$mu, "+"),
  name   = "mvnorm")
res <- qgctgan(x, y, generator = gen)
```

## Scope

The quality filter operates in numeric feature space (as in the reference study,
where features are standardised before oversampling). Discrete/categorical
handling is delegated to the generator (e.g. CTGAN).

## Citation

```bibtex
@article{yang2026qgctgan,
  author  = {Yang, Munil},
  title   = {Quality-Aware Cluster-Conditioned Oversampling via
             Intra-Cluster Synthetic Sample Filtering},
  journal = {Applied Soft Computing},
  year    = {2026}
}
```

## License

MIT © Munil Yang
