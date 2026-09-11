# Notebook instructions

Start Jupyter from the repository root so that the relative `data/` and `outputs/` paths are created in the intended locations:

```bash
cd SCIL-Wildfire-Prediction
jupyter lab
```

Run each notebook from top to bottom after restarting its kernel. Do not run several copies of the automatic-refresh loop at the same time.

## CONUS wildfire activity

File: [`conus_wildfire_activity.ipynb`](conus_wildfire_activity.ipynb)

### Inputs

- NASA FIRMS MAP_KEY
- Optional CARTO API key
- Recent FIRMS data from `VIIRS_NOAA21_NRT`, `VIIRS_NOAA20_NRT`, `VIIRS_SNPP_NRT`, and `MODIS_NRT`
- Current WFIGS interagency wildfire perimeters

### Important settings

| Setting | Default | Meaning |
| --- | --- | --- |
| `CONUS_BBOX` | lower 48 bounding box | Download and display region |
| `DAYS` | `1` | Number of recent FIRMS days; the API accepts 1–5 |
| `OUTPUT_HTML` | `outputs/conus/conus_wildfire_map.html` | Saved standalone map |
| `AUTO_REFRESH` | `False` | Whether to rerun downloads continuously |
| `REFRESH_MINUTES` | `10` | Delay between refreshes |

The automatic refresh works only while the kernel remains active. Stop it with **Kernel → Interrupt**. For unattended updates, convert the workflow to a scheduled script or job rather than leaving a personal notebook kernel running indefinitely.

### Reading the map

- Colored dots are satellite heat detections, not fire-size symbols.
- Dark red polygons are current operational perimeters.
- Use the upper-right layer control to compare satellites.
- The saved HTML contains the data available at the time it was generated.

## Smokehouse Creek CorrDiff/FIRMS study

File: [`corrdiff_smokehouse_creek.ipynb`](corrdiff_smokehouse_creek.ipynb)

### Inputs

- GEFS forecast fields downloaded through Earth2Studio
- CorrDiff NIM inference service
- CorrDiff latitude and longitude arrays from the NGC model package
- Historical standard-processing FIRMS VIIRS detections

### Run sequence

1. Activate the Earth2Studio/CorrDiff notebook environment.
2. Confirm that `CORRDIFF_NIM_URL` points to the correct service.
3. Leave `RUN_CORRDIFF = False` and run through the health check.
4. Review `VALID_TIMES`, map bounds, `SAMPLES`, `STEPS`, and the FIRMS date range.
5. Once the NIM is ready, set `RUN_CORRDIFF = True` and submit the timeline.
6. Confirm that all expected `.tar` files exist in `outputs/corrdiff/raw/`.
7. Continue through grid loading, FIRMS download, window preparation, panel creation, and GIF creation.
8. Return `RUN_CORRDIFF` to `False` when rerunning plots from existing outputs.

### Main settings

| Setting | Default | Meaning |
| --- | --- | --- |
| `VALID_TIMES` | 6-hourly, Feb. 26–29, 2024 | CorrDiff valid times |
| `SAMPLES` | `8` | CorrDiff ensemble samples per request |
| `STEPS` | `10` | Diffusion steps |
| `OVERWRITE` | `False` | Whether existing raw outputs may be replaced |
| `FUTURE_HOURS` | `6` | Forward FIRMS validation window |
| `PREVIOUS_HISTORY_HOURS` | `24` | Context shown before each valid time |
| `PLOT_ONLY_WINDOWS_WITH_FIRMS` | `True` | Skip frames with no following detections |

### Rerun scenarios

| Goal | What to rerun |
| --- | --- |
| Change plot style only | Settings, loading, FIRMS preparation, and plotting cells; keep `RUN_CORRDIFF = False` |
| Change the FIRMS window | Settings plus FIRMS/window/plotting cells; raw CorrDiff files can be reused |
| Add valid times | Update `VALID_TIMES`, set `RUN_CORRDIFF = True`, and run inference for missing times |
| Change `SAMPLES`, `STEPS`, or model output | Use a new output directory or deliberately set `OVERWRITE = True` |
| Reuse downloaded FIRMS data | Point `FIRMS_CSV_PATH` to `data/smokehouse_creek_firms_viirs.csv` |

## Before committing

Clear notebook outputs to prevent embedded maps and figures from making the repository unnecessarily large:

```bash
jupyter nbconvert \
  --ClearOutputPreprocessor.enabled=True \
  --inplace notebooks/*.ipynb
```

Then check the staged file sizes and names:

```bash
git status --short
git diff --stat
```
