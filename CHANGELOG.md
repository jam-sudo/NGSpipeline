# jam-sudo/ngspipeline: Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## v1.0.4 - [2026-11-02]

Repository-hygiene patch and the first release archived on Zenodo; no change to pipeline logic, parameters or outputs.

### `Added`

- `CITATION.cff` with the citation metadata used for the Zenodo record.

### `Changed`

- Comments, docstrings, test names and docs no longer reference internal task ids or documents that are not in the repository; the slurm and awsbatch error messages and the `--help` text of the cluster options are plain instructions.
- `assets/test_data/README.md` shows the regeneration command with placeholder paths.

### `Removed`

- `.github/ISSUE_TEMPLATE/config.yml`, which linked a chat channel that does not exist.

## v1.0.3 - [2026-10-28]

Documentation patch; no change to pipeline logic, parameters or outputs.

### `Changed`

- README: main-parameter table, validation results (concordance with the authors' guide assignments, cross-engine reproducibility, `-resume`), scope and limitations, links to the usage and output docs.
- `docs/usage.md` and `assets/samplesheet.csv` describe the real 6-column samplesheet, the guide library, the reference options and guide assignment instead of the nf-core template text.
- `docs/output.md` covers `counts/`, `guides/`, `alive/` and `reference/`.
- `CITATIONS.md` and the MultiQC methods description cite kallisto, bustools, kb-python, anndata and (for `mixture`) Replogle et al. 2022.
- `-profile test_full` holds the settings of the full-size validation run (inputs supplied by the user) instead of unrelated template data that could not run.

### `Fixed`

- `--help` descriptions no longer reference documents that are not in the repository; the dual-guide row of the ALIVE schema table renders correctly.
- Conda environments of the local modules pin `anndata=0.9.2`, the version in the kb-python container; conda could otherwise resolve 0.8.0 and write different h5ad bytes.

## v1.0.2 - [2026-10-07]

Documentation patch; no change to pipeline code, parameters or outputs.

### `Changed`

- README, CHANGELOG and the `conf/slurm.config` header describe the SLURM test host generically instead of naming the cluster and partition; the CHANGELOG v1.0.0 entry no longer links to an untracked runbook.

## v1.0.1 - [2026-10-06]

Documentation and repository-hygiene patch; no change to pipeline code, parameters or outputs.

### `Changed`

- README revised for users; it points to the latest release tag instead of `master`, whose manifest had read `1.1.0dev` since a direct dev → master merge.
- `.gitignore` keeps internal planning notes and local tool configuration out of the repository.

### `Fixed`

- `master` manifest and `.nf-core.yml` version are a release version again (`nf-core pipelines lint --release` passes).

## v1.0.0 - [2026-09-17]

First release. Perturb-seq raw reads → GEX and sgRNA count matrices → per-cell guide assignment → ALIVE-ready h5ad.

### `Added`

- GEX quantification with kallisto|bustools (`kb count --filter bustools`; in-pipeline `kb ref` or a prebuilt `--reference_index`).
- sgRNA counting with the kb kite workflow (`kite:10xFB` feature-barcode translation for 10x 3' v3).
- Per-cell guide assignment (`threshold`: top-1 UMI ≥ `min_umi` and top-1/top-2 ≥ `min_ratio`; `mixture`: per-vector Poisson/Gaussian EM) with defaults from the lane_4 ambient statistics.
- ALIVE-ready `alive/<sample>.h5ad` (single-assignment cells, `obs['gene']`, raw counts), schema in `docs/alive_schema.md`; `--pool_alive` writes `alive/pooled.h5ad` across GEM groups with obs index `<barcode>-<sample_id>`.
- MultiQC report with FastQC, kb quantification and guide-assignment sections.
- 18 MB deterministic test data subsampled from Replogle 2022 lane_4 (`-profile test` < 2 min); nf-test module and pipeline tests.
- Profiles `local`, `slurm` (tested on a SLURM cluster), `awsbatch` (configuration only, not run on AWS); Docker and Singularity/Apptainer.
- Validation against the authors' guide identities: 96.0 % per-cell concordance on lane_4 and 95.3–96.3 % on 12 GEM groups without threshold tuning; `-resume` check; byte-identical h5ad across engines and schedulers; ALIVE `prepare` + `fit` on the pooled h5ad with zero loader changes (README, Validation).

### `Fixed`

### `Dependencies`

kb-python 0.28.2 (kallisto 0.50.1, bustools), FastQC 0.12.1, MultiQC (nf-core module), anndata; Nextflow ≥ 25.10.4, nf-core template 4.1.0.

### `Deprecated`
