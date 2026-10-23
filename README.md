# Fish Classification — STA2453

An R-based course project exploring fish species classification from acoustic target-strength measurements across frequencies and repeated pings. The analyses compare lake trout, lake whitefish, and smallmouth bass using exploratory plots and several statistical and machine-learning models.

## Analysis workflow

- Explore fish characteristics and frequency-response patterns.
- Prepare acoustic features, remove unavailable frequency columns and the fish `LT008` with missing high-frequency measurements, and standardize predictors.
- Split observations by fish ID so that pings from one fish stay in the same training or test set.
- Compare PCA with multinomial logistic regression, random forest, a deep neural network, and an LSTM for ping sequences.
- Evaluate predictions with accuracy and confusion matrices; ping-level model predictions are also aggregated to fish-level classifications where implemented.

The preprocessing notebook uses `set.seed(2453)` and `p = 0.8` for its stratified training split. Some narrative comments describe a different split; consult the executable code when reproducing the analysis.

## Repository structure

```text
Fish Classification.Rproj         RStudio project
Notebooks/
  01_Exploration/
    01_Characterstics.Rmd         Fish characteristics
    01_Frequency.Rmd             Frequency-response exploration
  02_Models/
    02_Data_Process.Rmd          Feature preparation and train/test split
    02_Multinomial.Rmd           PCA and multinomial classification
    02_Random_Forest.Rmd         Random forest
    02_Deep_NN.Rmd               Deep neural network
    02_LSTM.Rmd                  Sequence classification
    02_Models.Rmd                Combined modeling notebook
Output/
  HTML/                          Previously rendered analysis reports
  PDF/                           Previously rendered exploration reports
Presentation/
  Fish_Classification_Pre.pdf    Project presentation
```

Start with the [project presentation](Presentation/Fish_Classification_Pre.pdf), then browse the notebooks or download the HTML reports in [Output/HTML](Output/HTML) and open them in a browser.

## Data and model artifacts

**Data files are not included in this repository.** Local datasets, saved R workspaces, and fitted-model artifacts are excluded through `.gitignore`.

To rerun the notebooks, supply the original preprocessed data locally at:

```text
Data/ProcessedData/processed_AnalysisData.Rdata
```

The notebooks expect `load()` to create a tibble named `processed_data`, with the original column order, fish identifiers, species labels, and frequency measurements. A CSV is not automatically interchangeable with this R data file.

The model-specific notebooks also load local artifacts from:

```text
Output/Data/02_Data_Process.RData
Output/Data/02_RF_Tuned_Models.rds
```

Despite its `.rds` extension, the existing random-forest notebook reads the second file with `load()`; provide an artifact compatible with that call.

## Running the project

1. Clone the repository and open `Fish Classification.Rproj` in RStudio.
2. Install the R packages used by the notebooks:

   ```r
   install.packages(c(
     "rmarkdown", "knitr", "here", "rprojroot",
     "dplyr", "purrr", "ggplot2", "reshape2", "ggridges",
     "gridExtra", "patchwork", "CCA", "FactoMineR",
     "caret", "recipes", "tidymodels", "nnet", "ranger",
     "keras", "tensorflow", "abind"
   ))
   ```

3. Configure a compatible Keras/TensorFlow backend for the neural-network notebooks. This repository does not pin package or Python environment versions, so reproducing the original environment may require adjustments.
4. Place the preprocessed data at the path above. Create `Output/Data/` locally if needed.
5. Run the exploration notebooks and `02_Data_Process.Rmd`. Its workspace-saving line is commented out; enable it or save the resulting workspace to `Output/Data/02_Data_Process.RData` before running the separate model notebooks.
6. Run the desired model notebook. The random-forest tuning block is commented out and the notebook loads a saved tuning artifact; supply that artifact or enable and run the tuning block first.

Use RStudio's **Knit** button to render a notebook. The notebook configuration directs HTML output to `Output/HTML/`. Rendering PDF output additionally requires a LaTeX installation.

## Reproducibility notes

The committed HTML/PDF files are existing analysis outputs. They can be reviewed without the local datasets, but the full pipeline cannot be rerun from this repository alone. The combined `02_Models.Rmd` notebook includes experimental and repeated sections; the separate model notebooks provide a more focused entry point. No new model training or numerical-result verification is implied by the repository upload.
