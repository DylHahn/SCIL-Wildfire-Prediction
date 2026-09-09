# SCIL Wildfire Prediction

Research notebooks for exploring current wildfire activity across the contiguous United States and examining the relationship between CorrDiff-predicted winds and NASA FIRMS detections during the 2024 Smokehouse Creek Fire.

> **Research use only.** These notebooks are not an operational fire-warning system. Satellite heat detections are not fire perimeters, and generated maps should not be used for evacuation or emergency decisions.

<img width="2530" height="1628" alt="Smokehouse Creek CorrDiff wind and FIRMS detections" src="https://github.com/user-attachments/assets/01e8e2fb-1891-4c8f-b6a9-ba4b24bdcef7" />

## Project workflows

| Notebook                                                                       | Purpose                                                                                                                                                                          | Main output                                 |
| ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| [`conus_wildfire_activity.ipynb`](notebooks/conus_wildfire_activity.ipynb)     | Downloads recent VIIRS/MODIS heat detections and current NIFC/WFIGS wildfire perimeters, then builds an interactive CONUS map.                                                   | `outputs/conus/conus_wildfire_map.html`     |
| [`corrdiff_smokehouse_creek.ipynb`](notebooks/corrdiff_smokehouse_creek.ipynb) | Runs a historical CorrDiff timeline, compares ensemble-mean 10 m wind at time \(T\) with FIRMS detections from \(T\) through \(T+6\) hours, and creates panels and an animation. | `outputs/corrdiff/panels/*.png` and `*.gif` |

The CONUS notebook is the easier starting point. The Smokehouse Creek notebook additionally requires a running NVIDIA CorrDiff NIM and an Earth2Studio environment.

## Repository organization

```text
SCIL-Wildfire-Prediction/
├── notebooks/
│   ├── conus_wildfire_activity.ipynb
│   ├── corrdiff_smokehouse_creek.ipynb
│   └── README.md
├── data/
│   └── README.md
├── outputs/
│   └── README.md
├── docs/
│   ├── OUTPUT_MANAGEMENT.md
│   └── assets/
│       └── smokehouse_creek_preview.png
├── requirements-conus.txt
├── requirements-corrdiff.txt
├── SECURITY.md
└── README.md
```

Generated data and routine outputs are ignored by Git. Keep only small, selected figures for the project page under `docs/assets/`.

## Quick start: CONUS activity map

1. Clone the repository and move into it.

   ```bash
   git clone https://github.com/DylHahn/SCIL-Wildfire-Prediction.git
   cd SCIL-Wildfire-Prediction
   ```

2. Create an environment and install the lightweight mapping dependencies.

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   pip install -r requirements-conus.txt
   ```

   On Windows PowerShell, activate the environment with:

   ```powershell
   .venv\Scripts\Activate.ps1
   ```

3. Request a free [NASA FIRMS MAP_KEY](https://firms.modaps.eosdis.nasa.gov/api/area/), then start Jupyter from the repository root.

   ```bash
   jupyter lab
   ```

4. Open `notebooks/conus_wildfire_activity.ipynb` and run the cells from top to bottom. Enter the FIRMS key when prompted. The CARTO key is optional; leave it blank to use OpenStreetMap.

The generated HTML is a time-stamped snapshot. Set `AUTO_REFRESH = True` only when you intentionally want the kernel to keep running and periodically replace the file.

## API keys and service configuration

The notebooks use the following credentials and service settings:

* **NASA FIRMS MAP_KEY — required:** Used to download VIIRS and MODIS active-fire detections. Request a free key from the [NASA FIRMS Area API](https://firms.modaps.eosdis.nasa.gov/api/area/).
* **CARTO API key — optional:** Used to display the CARTO Voyager basemap in the CONUS wildfire map. If no CARTO key is provided, the notebook automatically uses OpenStreetMap.
* **NVIDIA NGC API key — conditionally required:** Needed to download the official CorrDiff latitude and longitude grid files when they are not already available locally.
* **CorrDiff NIM URL — required for CorrDiff inference:** The Smokehouse Creek notebook must be able to reach a running NVIDIA CorrDiff NIM service. The default URL is `http://localhost:8000`.

Credentials can be entered through the notebook prompts or provided using the following environment variables:

```text
FIRMS_MAP_KEY
CARTO_API_KEY
NGC_API_KEY
CORRDIFF_NIM_URL
```

API keys should never be written directly into the notebooks, uploaded to GitHub, or included in committed configuration files.

> **CARTO security note:** The CARTO key is inserted into the tile URL of the generated HTML map. Do not publicly share an HTML map created with a private CARTO key. Leave the CARTO key blank and use OpenStreetMap when generating a map for public distribution.

## Quick start: Smokehouse Creek CorrDiff study

This workflow assumes that:

* an NVIDIA Earth-2 CorrDiff NIM is already deployed and reachable;
* the notebook environment can import `earth2studio`;
* GEFS data can be downloaded;
* you have a NASA FIRMS MAP_KEY; and
* you have an NVIDIA NGC key if the CorrDiff latitude and longitude arrays are not already available.

Install the notebook-side dependencies in a dedicated environment:

```bash
pip install -r requirements-corrdiff.txt
jupyter lab
```

Before running inference, configure `CORRDIFF_NIM_URL` or edit `NIM_BASE_URL` in the settings cell. The default is `http://localhost:8000`.

Run the notebook once with `RUN_CORRDIFF = False` to review the settings and check the NIM connection. After the NIM reports that it is ready, set `RUN_CORRDIFF = True` and run the notebook from the top. When the raw `.tar` files already exist, switch it back to `False` to avoid submitting the inference jobs again.

See [NVIDIA's CorrDiff NIM quickstart](https://docs.nvidia.com/nim/earth-2/corrdiff/latest/quickstart-guide.html) for NIM deployment, GEFS input preparation, inference, and output-format instructions.

## Data sources

* [NASA FIRMS Area API](https://firms.modaps.eosdis.nasa.gov/api/area/) — VIIRS and MODIS active-fire and thermal-anomaly detections.
* [NIFC wildland-fire maps and open data](https://www.nifc.gov/fire-information/maps) — current wildfire perimeter context used by the CONUS map.
* [NVIDIA CorrDiff NIM](https://docs.nvidia.com/nim/earth-2/corrdiff/latest/) — probabilistic correction and downscaling of GEFS inputs over CONUS.
* [NVIDIA Earth2Studio](https://nvidia.github.io/earth2studio/) — weather-data access used to prepare GEFS inputs.

## Output and Git workflow

Read [Output management](docs/OUTPUT_MANAGEMENT.md) before committing results. In short:

* Do not commit raw CorrDiff arrays, tar archives, downloaded grid files, cached data, or routine HTML snapshots.
* Clear notebook outputs before committing.
* Copy only publication-ready figures into `docs/assets/`.
* Never commit API keys or generated HTML that embeds a private tile-service key.

Detailed notebook instructions are available in [`notebooks/README.md`](notebooks/README.md).

## Interpretation limits

* FIRMS points indicate detected thermal anomalies, not exact burned area, acreage, or confirmed incident boundaries.
* A single fire can produce repeated detections across satellites and overpasses.
* Current WFIGS perimeters can lag active conditions and do not provide a complete historical record.
* The Smokehouse Creek comparison is a spatiotemporal case study. Visual alignment between wind and later detections does not by itself establish causation or predictive skill.
* The CorrDiff panels use the ensemble-mean 10 m wind field. They do not display the full probabilistic distribution of CorrDiff samples.

## Project

Developed by Dylan Hahn through the SDSU Climate Informatics Lab (SCIL).
