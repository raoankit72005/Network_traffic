# Network Traffic Classification

This project uses machine learning to identify potentially malicious network traffic in the **UNSW-NB15** dataset. Each row represents a network flow, described by features such as duration, protocol and byte counts. The target is binary: **0 = normal**, **1 = malicious**. This is flow classification, not packet capture or a live intrusion detection service.

The project compares **Logistic Regression, Random Forest, XGBoost and LightGBM**. Models learn patterns from labelled training data, and malicious-class F1 is used to select a model on a separate validation portion. F1 balances precision (how many flagged flows are attacks) and recall (how many attacks are detected).

## Start here: beginner notebook

[Open the beginner notebook in Google Colab](https://colab.research.google.com/github/raoankit72005/Network_traffic/blob/main/UNSW_NB15_Beginner_Colab.ipynb)

`UNSW_NB15_Beginner_Colab.ipynb` explains every stage in Markdown and uses small, sequential code cells with meaningful variable names. It includes:

- CSV or Excel loading from your computer or Google Drive.
- A class-balance plot and simple data checks.
- Explanations of features, labels, missing values, scaling and one-hot encoding.
- Separate training cells for each of the four models.
- A validation comparison table and graph.
- Final testing of the selected model, a confusion matrix and an overlap check.
- A downloadable ZIP containing the fitted model, metrics, predictions and graphs.

### Run on Colab

1. Open the beginner notebook using the link above. A CPU runtime is sufficient.
2. Obtain the prepared UNSW-NB15 training and testing files. **Dataset files are not included in this repository; there is no `data/` folder to download.**
3. Run cells from top to bottom, including the installation cell.
4. In section 2, use `DATA_SOURCE = "upload"` and select both files together, with `training` and `testing` in their respective filenames. Alternatively, use `DATA_SOURCE = "drive"` and edit `TRAIN_PATH` and `TEST_PATH` to point to files in your Google Drive.
5. Continue through training, validation comparison and final evaluation.
6. Download `beginner_results.zip` before the Colab session ends.

Prefer CSV if available: it is usually quicker to parse than Excel. Keeping files in Drive avoids repeated browser uploads. Pandas reads the table after transfer; switching away from pandas does not itself speed up the upload.

Both files must have matching columns and a binary `label` column. Use the prepared feature-table splits, not raw packet files. The commonly distributed prepared splits contain 175,341 training rows and 82,332 testing rows; the notebook prints your actual dimensions.

### Understanding training settings

These are classical models, so there is no neural-network `epochs` setting. Edit `n_estimators` in the tree model cells to change the number of trees, `max_depth` or `num_leaves` for complexity, and `max_iter` for Logistic Regression's optimization limit. The beginner notebook uses smaller tree counts than the original for a practical starting point on Colab CPU. Runtime depends on the data and allocated resources.

## Evaluation and leakage prevention

`label`, `attack_cat` and `id` are excluded from model inputs. `attack_cat` reveals the answer and would leak target information. The beginner notebook uses a stratified 80/20 train_test_split of the supplied training file, with no K-fold. Duplicate inputs can appear on both sides and may inflate validation scores. Preprocessing is explicit: fill missing categories with Unknown, use pandas.get_dummies, align columns, and learn median replacements from training only. Only original numerical columns are scaled for Logistic Regression; tree models use unscaled inputs. No Pipeline or ColumnTransformer is used. The saved model bundle includes the imputer, optional scaler and feature layout; the notebook shows each reload/prediction step.

The winner is chosen by validation F1, refitted on all supplied training records, and evaluated on the separate test set. Exact predictor overlap between training and test is reported, with a non-overlapping test F1 when both classes are available. Do not repeatedly tune on test scores. Offline benchmark results do not establish reliable detection on live networks or unseen attack families.

## Original detailed notebook

[Open the original notebook in Colab](https://colab.research.google.com/github/raoankit72005/Network_traffic/blob/main/UNSW_NB15_Classical_ML.ipynb)

`UNSW_NB15_Classical_ML.ipynb` remains available for broader exploratory plots, data audits, and test comparisons of all four refitted models. Start with the beginner version, then use the original for deeper analysis.

## Results and verification

Scores and plots are generated when you run the notebooks. No full-dataset benchmark scores are claimed. The beginner notebook was syntax-checked and its scikit-learn preprocessing, splitting, training, evaluation and model reload steps were smoke-tested on synthetic data. Actual UNSW-NB15 training and the Colab upload/Drive interface still require a run with your dataset.

## Dataset credit

Source and usage terms: https://research.unsw.edu.au/projects/unsw-nb15-dataset

Nour Moustafa and Jill Slay, “UNSW-NB15: a comprehensive data set for network intrusion detection systems (UNSW-NB15 network data set),” MilCIS, 2015. Dataset rights remain with its authors; consult the official page for citation and usage requirements.
