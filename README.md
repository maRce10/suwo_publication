Obtaining nature media with the R package suwo: data and code repository
================
Marcelo Araya-Salas
2026-10-07

<!-- README.md is generated from README.Rmd. Please edit that file -->

## Obtaining nature media with the R package suwo

<!-- raw block so pandoc doesn't escape the GitHub alert syntax -->

> [!NOTE]
> Analysis code and data for the manuscript introducing [`suwo`](https://github.com/ropensci/suwo), an R package for querying, standardizing, and downloading biodiversity media (audio, images, video) from GBIF, iNaturalist, Macaulay Library, WikiAves, and Xeno-Canto.

- **Repository**: <https://github.com/maRce10/suwo_publication>
- **Website**: <https://marce10.github.io/suwo_publication/>

## Repository structure

    .
    ├── .github/workflows/
    │   └── publish.yml                      # builds & deploys the site to GitHub Pages on push
    ├── scripts/
    │   ├── _quarto.yml                      # website project config (navbar, theme, 300-dpi figures)
    │   ├── index.qmd                        # site landing page
    │   ├── p_averano_bock_analysis.qmd      # case study 1 notebook
    │   ├── mink_detection_analysis.qmd      # case study 2 notebook
    │   ├── mink_detection_data.yaml         # YOLO dataset config for case study 2
    │   ├── qmd.css                          # shared styling for the site
    │   ├── fig_download.html                # adds a "Download" button under each figure
    │   ├── images/                          # static images embedded in the notebooks
    │   └── _freeze/                         # cached chunk outputs for the website build
    ├── data/
    │   ├── raw/
    │   │   ├── p_averano_annotations/       # Raven selection tables (case study 1)
    │   │   └── mink_detection_logs/         # example image-download log (case study 2)
    │   └── processed/
    │       ├── p_averano_recordings_metadata.csv          # case study 1
    │       ├── p_averano_bock_extended_selection_table.rds
    │       ├── pca_bock_broken_stick.rds
    │       ├── dist_long_bock.rds
    │       ├── results_bock.rds
    │       ├── fits/                                      # saved brms model fits
    │       └── mink_detection_queries/                    # case study 2: suwo query metadata, one CSV per species
    └── output/
        ├── catalogs/                                      # case study 1: per-population + per-recording annotation catalogs
        ├── fig_results_bock.png
        ├── fig_twelve_spectrograms.png
        ├── bearded_bellbird_map_locations.png
        ├── mink_detection_species_barplot.png             # case study 2: data availability
        ├── mink_detection_species_barplot_lontra_spp.png
        └── mink_detection_records_map.png

Case study 1’s raw audio recordings and case study 2’s images and
training labels are not stored in the repository (see each case study
below for why and how to reproduce them).
`data/processed/p_averano_extended_selection_table.rds` (the unfiltered,
all-song-types selection table, ~100 MB) is also excluded via
`.gitignore`; only the “bock”-song-filtered table actually used in the
analysis is tracked.

## Case studies

### 1. Geographic variation in Bearded Bellbird (*Procnias averano*) songs

Recordings and metadata for the species were assembled across five
repositories with `suwo`, then annotated, bandpass-filtered,
de-duplicated, and summarized acoustically (MFCCs + PCA) before fitting
a Bayesian multi-membership model (`brms`) relating pairwise acoustic
dissimilarity to geographic distance, temporal separation, population
identity, and recording quality.

- [`scripts/p_averano_bock_analysis.qmd`](scripts/p_averano_bock_analysis.qmd)
  — the full pipeline, start to finish: metadata retrieval and media
  download via `suwo`, annotation curation, acoustic and statistical
  analysis, annotation catalogs (`output/catalogs/`), and the
  spectrogram comparison figure (`output/fig_twelve_spectrograms.png`).

Raw audio recordings are **not** stored in this repository. To replicate
the analysis, either:

1.  Use `data/processed/p_averano_bock_extended_selection_table.rds`
    ([download](https://github.com/maRce10/suwo_publication/raw/main/data/processed/p_averano_bock_extended_selection_table.rds),
    ~25 MB) directly — this extended selection table already embeds the
    annotated “bock” song clips, so the full acoustic/statistical
    pipeline runs without downloading anything (this is the default
    entry point in `p_averano_bock_analysis.qmd`), or
2.  Re-run the download/annotation chunks at the top of
    `p_averano_bock_analysis.qmd` (set `eval: true`) to re-query and
    re-download the dataset from the five repositories via `suwo`, then
    rebuild the selection table yourself.

### 2. Invasive species management: detecting mammals (American Mink) in Patagonia

Case study demonstrating `suwo`-enabled image retrieval to train a YOLO
detection/classification model for the American Mink (*Neogale vison*),
Black Rat (*Rattus rattus*), and *Lontra* otters (*L. provocax*, *L.
annectens*, *L. longicaudis*, *L. canadensis*, *L. felina*).

- [`scripts/mink_detection_analysis.qmd`](scripts/mink_detection_analysis.qmd)
  — the full pipeline: image query/download via `suwo`, a
  pretrained-YOLO detection preview, auto-generated training labels,
  train/validation split, and the YOLO training command.
- [`scripts/mink_detection_data.yaml`](scripts/mink_detection_data.yaml)
  — YOLO dataset config (classes + train/validation paths) used by the
  training command.

The `suwo` query metadata (one CSV per species, 74,490 records) is
stored in `data/processed/mink_detection_queries/` and is used to render
the data-availability barplots and the records map. Images and training
labels are **not** stored in this repository (tens of thousands of
files, all reproducible via the chunks in `mink_detection_analysis.qmd`,
which default to `eval: false` since every step is either a slow bulk
download or requires a GPU). The one artifact kept for reference is the
log of an actual download run:
`data/raw/mink_detection_logs/download_images.log`.

## Manuscript

The manuscript itself is under review; this repository holds the
supporting code and data only.
