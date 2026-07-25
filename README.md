# Sector Template — How to Replicate

This is the template for **one sector** (Energy). To add another sector (e.g. Agriculture):

1. Copy `sectors/energy.md` → `sectors/agriculture.md`, edit content.
2. Copy the whole `sectors/energy/` folder → `sectors/agriculture/`, rename nothing (keep `calculations.md`, `sources.md`, `literature.md`, `methodology.md`).
3. Update the "Back to [Sector]" link at the top of each sub-page.
4. Update file paths under `data/` to match the new sector's Excel files.
5. Add the new sector to your homepage (`index.md`) sector list.

## Suggested full repo structure

```
/
  index.md              → homepage, links to all sectors
  methodology.md        → cross-sector methodology (QA/QC, tiers, uncertainty)
  references.md         → master bibliography across all sectors
  /sectors
    energy.md
    agriculture.md
    ippu.md
    lulucf.md
    waste.md
    /energy
      calculations.md
      sources.md
      literature.md
      methodology.md
    /agriculture
      ... (same 4 files)
    ...
  /data
    energy_1A1_2023.xlsx
    agriculture_*.xlsx
    ...
  /references            (optional — PDFs you have rights to host)
```

## Status emoji legend (used in sector status tables)
- 🟢 Complete
- 🟡 In progress
- 🔴 Not started
