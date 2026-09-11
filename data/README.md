# Data directory

This directory holds downloaded or cached inputs created while running the notebooks. Its generated contents are intentionally ignored by Git.

Expected local files include:

```text
data/
├── corrdiff_grid/
│   ├── corrdiff_output_lat.npy
│   └── corrdiff_output_lon.npy
└── smokehouse_creek_firms_viirs.csv
```

## Rules

- Do not commit API keys, authentication files, or request URLs containing keys.
- Do not commit large GEFS caches or NGC model-package files.
- Treat FIRMS CSV files as reproducible downloads: record the source, product, geographic bounds, and date range in the notebook.
- If a fixed data snapshot becomes necessary for a paper, archive it in an approved research-data repository and add a citation or DOI here instead of pushing a large file to Git.
- Review the source terms and licenses before redistributing downloaded data.

The notebook will create missing subdirectories automatically when it is started from the repository root.
