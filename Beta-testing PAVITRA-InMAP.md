# Beta-testing PAVITRA-InMAP

PAVITRA-InMAP is a fork of the open-source [InMAP](https://github.com/spatialmodel/inmap) air pollution model, being adapted by IIT Bombay together with UC Berkeley, the University of Washington, and CSTEP to support air quality modeling for India. 

This document records an initial beta test of the model engine itself, to confirm the build-and-run pipeline works correctly ahead of region-specific data being added.

## Running the Model

1. [Go](https://go.dev/dl/) (1.11+) and [Git](https://git-scm.com/) were installed first as per the model requirement.
2. The repo was cloned and binary was built:
   ```bash
   git clone https://github.com/INMAP-PAVITRA/InMAP-PAVITRA.git
   cd InMAP-PAVITRA
   GO111MODULE=on go build ./cmd/inmap
   ```
3. Evaluation data(`evaldata_v1.6.1.zip`) was downloaded, which is a full dataset publicly avaialble on [Zenodo](https://zenodo.org/records/3403934). For a much lighter test, use the `la_test/` subset bundled inside that same zip.
4. The `.toml` config file template provided on the repo was edited to the paths point to our downloaded evaluation data, and so the `[VarGrid]` settings (origin, cell size, nesting) match the metadata of the `.ncf` file we're using.
5. (`toml`) config file was run:
   ```bash
   ./inmap run steady --config=yourconfig.toml
   ```
6. The generated output shapefile was ready to view on GIS applications. We loaded it directly inside Python with `geopandas` and `matplotlib` to plot it.

Running locally needs a lot of RAM for full-scale data. [Google Colab](https://colab.research.google.com/) is a more reliable place to test, since it gives a clean 12GB+ environment with nothing else competing for memory.

## What We Did

- We tried running the full-scale evaluation dataset, both on a local machine and on Colab's free tier; it consistently ran out of memory, either while loading the large national emissions shapefiles or while initializing the model grid.
- Switched to the `la_test` subset included in the same evaluation data: a small, pre-built Los Angeles-area dataset meant for quick functional tests.
- Fixed a series of config mismatches along the way: wrong data filenames, mismatched shapefile field names (`AllCause` vs `allcause`), and a grid origin/extent that didn't match the actual `.ncf` file's geography.
- Got a complete steady-state simulation to run end-to-end on Colab, converging within a few minutes.

## Result

![PM2.5 concentration heatmap](./inmao.png)
The map shows the estimated total PM2.5 concentration across the LA test grid. There's a clear pollution hotspot near the single test emission source, fading outward with distance, an expected pattern for a point source, and a good sign that the model and pipeline are producing physically sensible output. The emissions input here is a placeholder test file, not real-world data, so the absolute numbers don't represent actual air quality.

## Limitations & Challenges

- **No India-specific data yet**: this repo currently only contains the original upstream evaluation dataset; the README itself still has unresolved Git merge markers from the upstream project, confirming India-specific data and config haven't been merged in yet.
- **Memory**: a full national-scale run needs significantly more RAM (likely 32–64GB+) than a typical laptop or Colab's free tier (12–16GB) provides; this was the main blocker throughout.


