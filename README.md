# Forest fire area prediction  

In this project, we aim to predict the burned area of forest fires in the northeast region of Portugal, using meteorological and soil moisture data. Forest fires are major environmental concerns with the potential of endangering human lives (Cortez and Morais, 2007). Particularly, here in BC, millions of acres of forests are burned annually, damaging the environment and posing significant financial challenges. Being able to predict the size of burned area of future forest fires may help fire monitoring and fire mitigation efforts. 

We will be recreating a study performed in Cortez and Morais (2007), which looked at forest fire burn sizes in Montesinho natural park in northeast Portugal. This study provides us data that consists of meteorological observations (such as temperature, wind, relative humidity etc.), soil moisture indices, and locational data. The dataset contains 517 observations, with each row representing one fire monitoring instance, with the column area as our target (showing the burned area), and 12 other measurements and indexes as features (including `month`, `day`, `RH`, `rain`, `DC`, `ISI` etc.). 

To predict the size of wildfires, we will build a predictive regression model. First, we split our data set into train and test splits by 20:80 ratio. Our EDA analysis includes some preliminary information of the data set (such as type of features, value counts etc.) and plotting the dependent variables (area) as a function of predictors (features). We found that our target variable, `fire area` is highly skewed to small values. We observe that one of the features (`day`) is not significantly impacting the output (the burned area is almost equally distributed across different days of the week), so we drop that feature but keep the rest. We also note that the `month` feature is unbalanced with fewer observations for some specific months. In order to avoid overfitting on this unbalanced feature, we define a new variable `season`, and plot the burned are versus this new variable. Surprisingly and contrary to our expectations, the summer fires are not significantly larger than other seasons. We also plot the size of the burned area as a function of spatial locations of the forest, where it shows that two particular locations with (X,Y)=(6,5) and (X,Y)=(8,6) have the largest burnt area. The pairplot between numerical variables shows that there are some outliers in the data that we have to be aware of when building the regression model. Finally, the correlation table (or heatmap) of all features shows that some features such as (`ISI` and `FFMC`), (`temp` and `FMC`), (`DC` and `DMC`) are quite associated with high correlation coefficients. On the other hand, some features such as (`DC` and `rain`), (`RH` and `DMC`) has almost zero correlation. 

## Report
The final report can be found [here](https://github.com/UBC-MDS/forest-fire-area-prediction-group-2/blob/dev/reports/Final_report.md).

## Usage
To replicate our analysis install the dependencies that are listed below and run the following commands in order in your terminal from the root directory of the project:

```
# Clean and Split Data
python src/clean_n_split.py --file_path=data/raw/forestfires.csv --test_data_file=test_data --train_data_file=train_data

# Exploratory Data Analysis
python src/EDA.py --file_path=data/processed/train_data.csv --out_folder=results

# Preprocess, Cross-validate, and Tune Model
python src/preprocess_n_tune.py --train_data=data/processed/train_data.csv --results_path=results/

# Evaluate model
python src/evaluate.py --test_data=data/processed/test_data.csv --results_path=results/

# Render Final Report
Rscript -e "rmarkdown::render('reports/Final_report.Rmd', output_format = 'github_document')"

```

**Dependencies**   
python :
```
channels:
  - conda-forge
  - defaults
dependencies:
  - altair>=4.1.0
  - altair_data_server>=0.4.1
  - altair_saver>=0.5.0
  - pandas>=1.3.4
  - docopt>=0.6.2
  - pip
  - matplotlib[version='>=3.2.2']
  - scikit-learn[version='>=1.0']
  - requests[version='>=2.24.0']
  - dataframe-image
```
R:
```
  - knitr==1.26
  - tidyverse==1.2.1
  - caret==6.0-84
  - ggthemes==4.2.0
```

## References

P. Cortez and A. Morais. A Data Mining Approach to Predict Forest Fires using Meteorological Data. In J. Neves, M. F. Santos and J. Machado Eds., New Trends in Artificial Intelligence, Proceedings of the 13th EPIA 2007 - Portuguese Conference on Artificial Intelligence.

Data is publically avaliable at https://archive.ics.uci.edu/ml/datasets/Forest+Fires.
