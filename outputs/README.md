# Output directory

Notebook-generated results are written here but ignored by Git unless deliberately copied elsewhere.

```text
outputs/
├── conus/
│   └── conus_wildfire_map.html
└── corrdiff/
    ├── raw/
    │   ├── input_YYYYMMDD_HHZ.npy
    │   └── smokehouse_creek_YYYYMMDD_HHZ.tar
    └── panels/
        ├── smokehouse_creek_YYYY-MM-DD_HHZ.png
        └── smokehouse_creek_corrdiff_firms_animation.gif
```

## What each folder is for

- `conus/`: replaceable snapshots of the current interactive map.
- `corrdiff/raw/`: large, reusable inference inputs and outputs. Preserve locally when they would be expensive to regenerate, but do not commit them to ordinary Git history.
- `corrdiff/panels/`: reproducible figures and GIFs created from the raw outputs.

To feature a result on the GitHub repository page, review it, copy the selected file to `docs/assets/`, give it a descriptive stable name, and link it from the root README. Do not move the entire run directory into Git.

See [`docs/OUTPUT_MANAGEMENT.md`](../docs/OUTPUT_MANAGEMENT.md) for cleanup, archiving, and publication guidance.
