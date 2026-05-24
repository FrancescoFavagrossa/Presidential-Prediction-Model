# US Presidential Election Model

This project builds a state-by-state model to forecast the 2024 United States Presidential Election using economic and demographic indicators. The approach follows the accompanying report, `US_Presidential_Election_model.pdf`, and implements an old-fashioned probit regression with PyTorch.

The model estimates the probability that each state votes Republican, where `0` represents a Democratic result and `1` represents a Republican result. Predicted state probabilities are then combined with Electoral College votes to estimate the national winner.

## Project Motivation

Many election forecasting models use national-level indicators. This project instead takes a micro approach by modeling each state separately. The goal is to capture state-level differences in economic conditions and demographics that may be hidden in aggregate national data.

The explanatory variables used in the model are:

- Annual GDP growth
- Year-over-year inflation
- Average house price level
- Share of the population with a college degree
- Per-capita personal income

## Model

The project uses a probit regression model:

```text
P(Y = 1 | X) = Phi(beta_0 + beta_1 X_1 + beta_2 X_2 + beta_3 X_3 + beta_4 X_4 + beta_5 X_5)
```

Where:

- `Y` is the binary election outcome.
- `Y = 0` represents Democrats.
- `Y = 1` represents Republicans.
- `Phi` is the cumulative distribution function of the standard normal distribution.
- `X_1` through `X_5` are the selected economic and demographic variables.

The implementation trains the model using binary cross-entropy loss and stochastic gradient descent.

## Methodology

The workflow is:

1. Load historical state-level election and economic data from Excel sheets.
2. Select the model features: GDP growth, inflation, house prices, college graduates, and personal income.
3. Standardize the features with `StandardScaler`.
4. Train a probit regression model for each state.
5. Predict the 2024 state-level Republican vote probability.
6. Combine predictions with Electoral College votes.
7. Plot the state probabilities on a U.S. map using shapefiles.

## Repository Structure

```text
.
├── Data
│   ├── Excel
│   │   ├── Data_2023.xlsx
│   │   ├── Electoral_Colleges.xlsx
│   │   └── Training_Data.xlsx
│   └── ShapeFile
│       ├── cb_2018_us_state_500k.dbf
│       ├── cb_2018_us_state_500k.prj
│       ├── cb_2018_us_state_500k.shp
│       └── cb_2018_us_state_500k.shx
├── US_Presidential_Election.ipynb
├── US_Presidential_Election_model.pdf
└── README.md
```

## Data Sources

The report lists the following data sources:

- GDP growth: FRED
- Year-over-year inflation: FRED
- House prices: FRED
- Electoral results: U.S. historical election archives
- Share of college graduates: FRED
- Personal income: BEA
- U.S. state shapefile: United States Census Bureau

## Requirements

The notebook uses the following main Python packages:

- `pandas`
- `numpy`
- `geopandas`
- `matplotlib`
- `openpyxl`
- `torch`
- `scikit-learn`
- `shapely`

Install them with:

```bash
pip install pandas numpy geopandas matplotlib openpyxl torch scikit-learn shapely
```

## How to Run

Open and run:

```text
US_Presidential_Election.ipynb
```

The notebook was originally written for Google Colab and uses Google Drive paths. If running locally, update the file paths near the top of the notebook to match the repository files:

```python
train_file_path = "Data/Excel/Training_Data.xlsx"
predict_file_path = "Data/Excel/Data_2023.xlsx"
Electoral_college_path = "Data/Excel/Electoral_Colleges.xlsx"
shape_path = "Data/ShapeFile/cb_2018_us_state_500k.shp"
```

## Current Notebook Output

The saved notebook output reports:

```text
Democrat: 276
Republican: 262

Democrat wins
```

These results should be treated as provisional. The report notes that the model is intended to be re-run as new monthly data becomes available.

## Notes

This project is an experimental forecasting model, not a final election prediction. Its accuracy depends on the quality and timeliness of the input data, the chosen explanatory variables, and the assumptions of the probit specification.
