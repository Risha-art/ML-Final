# 30-Minute Wind Speed Forecasting at the MIT Sailing Pavilion

Final project for MIT 6.C01 — ML for Sustainability. Predicts wind speed at the MIT Sailing Pavilion 30 minutes ahead using weather observations from four surrounding stations (Boston College, Northeastern, Swampscott, NOAA Buoy 44013).

**Authors:** Ritisha Das, Nat Edmonds

## Repository contents

| File | Description |
| ML Data Files | Zipped folder of the following data files |
| `44013Pressure2025.txt.gz` | NOAA Buoy 44013 raw data (2025) |
| `BC2025.csv` | Boston College weather station data (2025) |
| `NEU2025.csv` | Northeastern University weather station data (2025) |
| `Swamp2025.csv` | Swampscott Technical School weather station data (2025) |
| `MIT2025.csv` | MIT Sailing Pavilion target data (2025) |
| `Final_ML_Project_Code.ipynb` | End-to-end notebook: data loading, preprocessing, EDA, hyperparameter tuning, model training, evaluation, uncertainty quantification, SHAP analysis, and station ablation |
| `Final_Report.pdf` | Full writeup |
| `README.md` | This file |

## How to run

The notebook is configured to run in Google Colab with data files loaded from Google Drive.

1. **Download** all data files and the notebook from this repository.
2. **Upload** the five data files to the root of your Google Drive (`MyDrive/`):
   - `44013Pressure2025.txt.gz`
   - `BC2025.csv`
   - `NEU2025.csv`
   - `Swamp2025.csv`
   - `MIT2025.csv`
3. **Open** `Final_ML_Project_Code.ipynb` in Google Colab.
4. **Run all cells** top to bottom (`Runtime > Run all`). The notebook will:
   - Mount your Google Drive (you'll be prompted to authorize)
   - Load and merge all five data sources
   - Run preprocessing, EDA, and modeling end-to-end
   - Generate every figure and table reported in the writeup

Expected total runtime: **20–30 minutes**, dominated by hyperparameter tuning (~2 minutes) and the station ablation study (~5 minutes, 750 model fits).

## Dependencies

All libraries are pre-installed in Colab. If running locally, the notebook requires:

- `xgboost` 2.x (for `reg:quantileerror`)
- `shap` 0.44+
- `scikit-learn` 1.x
- `pandas`, `numpy`, `matplotlib`, `seaborn`

## Reproducibility

All results in the writeup are reproducible by running the cells in sequential order, from top to bottom. 

## Method summary

- **Model:** XGBoost regression with hyperparameters selected via 5-fold cross-validated grid search
- **Uncertainty:** Residual-based and quantile-regression prediction intervals (90% nominal coverage)
- **Interpretation:** Gain-based feature importance and SHAP values
- **Ablation:** 15-model station-subset study averaged over 50 random splits

## Key results

- **Test R²:** 0.565 (single split); 0.594 ± 0.032 across 50 splits
- **RMSE:** 0.870 m/s
- **Top feature:** Gust speed at Boston College (~25% gain-based importance)
- **Station ablation:** All-stations model best, but BC-only retains ~90% of full-model performance

## Notes

- The notebook reads data from `/content/drive/MyDrive/` (Colab's mount point). If running locally, update the file paths in the data-loading cells.
- The notebook's cell ordering is significant: the grid search cell must run before the main training cell, since the latter pulls hyperparameters from the former.
