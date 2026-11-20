# Quantifying the Economic-Environmental Trade-offs of Retrofitting Legacy Buildings with AI-IoT Overlays

Course project, **AI for Sustainability** (B.Tech AI, 7th semester), SVNIT Surat.

## Team

| Name | Roll number |
|---|---|
| Dhruv Jindal | U23AI083 |
| Mohit Agrawal | U23AI117 |
| Devansh Upadhyay | U23AI104 |

## GitHub repository

https://github.com/JindalDhruv05/ai-iot-building-retrofit-payback-carbon

## What this project does

Older commercial buildings cannot be rebuilt, so retrofitting them with sensors and AI-based control (an AI-IoT overlay) is the practical way to cut energy use and emissions. This project estimates, for real buildings, **how long a retrofit takes to pay back** and **how much carbon it avoids**.

No hardware is deployed. The overlay is simulated:

1. A per-building regression model (XGBoost) predicts business-as-usual (BAU) hourly electricity use from weather and calendar features.
2. Savings percentages taken from published studies (smart HVAC control, occupancy-based lighting, fault detection) are applied to the BAU.
3. Saved energy is converted to payback, NPV, tCO2 avoided and cost per tonne abated.
4. Buildings are ranked for retrofit priority, and the results are tested with Monte Carlo simulation, tornado sensitivity, a legacy-vs-newer statistical test and a regional (India vs US) comparison.

**Key results (62 buildings, cited savings, assumed costs):** median payback 3.2 years, 82% of buildings pay back within 5 years, about 10,856 tCO2 avoided per year; energy use per m2 and size explain 99% of payback variation, and building age shows no clear effect. Capital cost is the largest uncertainty. Costs, the older-saves-more split and some end-use shares are the team's own assumptions and are labelled as such in the notebook.

## Repository contents

| File | Description |
|---|---|
| `AI_IoT_Retrofit.ipynb` | Full analysis notebook (run top to bottom) |
| `requirements.txt` | Python dependencies |
| `bdg2_data/` | Dataset cache, created automatically on first run |

## Dataset resources

The project uses the **Building Data Genome Project 2 (BDG2)**: hourly meter data from about 1,600 non-residential buildings with weather and metadata (2016 and 2017).

- GitHub repository: https://github.com/buds-lab/building-data-genome-project-2
- Paper: C. Miller et al., "The Building Data Genome Project 2, energy meter data from the ASHRAE Great Energy Predictor III competition," *Scientific Data*, vol. 7, art. 368, 2020. https://doi.org/10.1038/s41597-020-00712-x
- Archived copy on Zenodo: https://doi.org/10.5281/zenodo.3887306
- Related competition: ASHRAE Great Energy Predictor III on Kaggle, https://www.kaggle.com/c/ashrae-energy-prediction

Files used by the notebook (downloaded automatically on the first run and cached locally):

- `data/meters/cleaned/electricity_cleaned.csv` (about 175 MB)
- `data/metadata/metadata.csv`
- `data/weather/weather.csv`

The BDG2 files are stored with Git LFS, so the notebook downloads them from `media.githubusercontent.com`.

Other sources used for assumptions: lighting-controls meta-analysis (Williams et al., 2012), building commissioning study (Mills, 2011), HVAC field demonstrations review (Khabbazi et al., 2025), CEA CO2 Baseline Database (India), US EPA and EIA data for the US comparison, and ASHRAE Guideline 14.

## Environment setup

### Option A: Google Colab (easiest)

1. Open https://colab.research.google.com and upload `AI_IoT_Retrofit.ipynb`.
2. Runtime > Run all. All required libraries are preinstalled on Colab.
3. The first run downloads the dataset (a minute or two). To keep the cache across Colab sessions, set `USE_DRIVE = True` in the "Load data" cell; this mounts Google Drive.

### Option B: Local machine

Requirements: Python 3.9 or newer, about 1 GB of free disk space, an internet connection for the first run.

```bash
git clone https://github.com/JindalDhruv05/ai-iot-building-retrofit-payback-carbon.git
cd ai-iot-building-retrofit-payback-carbon

python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

jupyter notebook AI_IoT_Retrofit.ipynb
```

Then run all cells from top to bottom.

### Runtime notes

- The baseline step trains two XGBoost models for each of 80 buildings; expect a few minutes on a standard laptop or Colab CPU.
- Random seeds are fixed (42) so results are reproducible.
- Key settings are at the top of the notebook and in the assumptions cells: `TYPES`, `N_PER_GROUP`, `LEGACY_BEFORE`, `AGE_DEPENDENT`, `TARIFF`, `EF`, and the cost dictionaries.

## Limitations

Savings are literature-based assumptions, not measured results; capex and opex are assumptions; the dataset is mostly outside India while the tariff and emission factor are Indian.