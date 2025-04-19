## Pipeline

- First download `Crimes_-_2001_to_Present.csv` from [Chicago Data Portal](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8tq).
- Then unzip the file and place it in the `data` folder.
  the directory structure should look like this:

```
.
├── data
│   └── Crimes_-_2001_to_Present.csv
└── src
    ├── config.py
    ├── data_preprocessing_and_EDA.ipynb
    └── train_eval_model.ipynb
```

- Run `data_preprocessing_and_EDA.ipynb` to preprocess the data and create two new files: `chicago_clean_final_arrest.csv` and `chicago_clean_final_not_arrest.csv`.
- The dir structure should be like this now:

```
.
├── data
│   ├── chicago_clean_final_arrest.csv
│   ├── chicago_clean_final_not_arrest.csv
│   └── Crimes_-_2001_to_Present.csv
└── src
    ├── config.py
    ├── data_preprocessing_and_EDA.ipynb
    └── train_eval_model.ipynb
```

- Now, run `train_eval_model.ipynb` to train and evaluate the model.
- The final directory structure should look like this:
  - we use 4 models: `Logistic Regression`, `Random Forest`, `XGBoost`, and `LightGBM`.
