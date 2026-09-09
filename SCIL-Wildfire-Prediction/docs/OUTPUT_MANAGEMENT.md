# Output management

The notebooks produce three different classes of files. Managing them separately keeps the repository small and makes results easier to reproduce.

| Output | Default location | Commit to Git? | Recommended handling |
| --- | --- | --- | --- |
| CorrDiff input arrays | `outputs/corrdiff/raw/input_*.npy` | No | Keep locally only if reruns are likely |
| CorrDiff sample archives | `outputs/corrdiff/raw/*.tar` | No | Preserve on lab storage; these can be expensive to regenerate |
| CorrDiff coordinate arrays | `data/corrdiff_grid/*.npy` | No | Keep locally or redownload from the model package |
| FIRMS historical CSV | `data/smokehouse_creek_firms_viirs.csv` | Usually no | Redownload or archive with documented provenance |
| CorrDiff/FIRMS panels | `outputs/corrdiff/panels/*.png` | Selected files only | Copy final figures to `docs/assets/` |
| CorrDiff/FIRMS animation | `outputs/corrdiff/panels/*.gif` | Selected files only | Compress and copy only if small enough |
| Current CONUS HTML map | `outputs/conus/*.html` | Usually no | Treat as a dated snapshot; regenerate from current feeds |
| Notebook cell output | Inside `.ipynb` | No | Clear before committing |

## Naming final results

Use names that identify the case, variable, and valid period without relying on notebook execution order. For example:

```text
docs/assets/
├── smokehouse_creek_wind_firms_20240227_18z.png
└── smokehouse_creek_wind_firms_20240226_18z_to_20240229_00z.gif
```

Avoid names such as `figure1_final_v2_new.png`.

## Recommended run record

For an experiment that may appear in a thesis or paper, record:

- run date;
- Git commit hash;
- CorrDiff model/container version;
- GEFS initialization and lead times;
- CorrDiff sample count, diffusion steps, and seed;
- FIRMS products and requested date range;
- map bounds and time zone;
- whether the figure uses ensemble mean or individual members; and
- any missing or failed time steps.

This information can be stored in a small Markdown or JSON manifest beside an archived run, outside the normal Git repository.

## Publishing the interactive map

The CONUS HTML is a static snapshot, even though it is interactive in a browser. If you later publish one through GitHub Pages:

1. Run the notebook with the optional CARTO key left blank so a private key is not embedded in the HTML.
2. Confirm the timestamp and data-source labels.
3. Copy the reviewed snapshot to `docs/index.html`.
4. In GitHub repository settings, configure Pages to deploy from the `main` branch `/docs` folder.

Do not describe the snapshot as live unless an external scheduled workflow actually regenerates and republishes it.

## Cleaning notebooks

Before each commit:

```bash
jupyter nbconvert \
  --ClearOutputPreprocessor.enabled=True \
  --inplace notebooks/*.ipynb
```

Then confirm that large generated files remain ignored:

```bash
git status --short
git check-ignore -v outputs/corrdiff/raw/example.tar
```

If a large file was accidentally committed, simply adding it to `.gitignore` will not remove it from earlier Git history.
