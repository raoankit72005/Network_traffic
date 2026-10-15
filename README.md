# Network Traffic Classification

Classify UNSW-NB15 traffic as normal or malicious using Logistic Regression, Random Forest, XGBoost and LightGBM. Compare models using malicious-class F1, with visualizations and separate validation/test evaluation.

[Open notebook in Google Colab](https://colab.research.google.com/github/raoankit72005/Network_traffic/blob/main/UNSW_NB15_Classical_ML.ipynb)

## Run
1. Open the notebook in Colab.
2. Download both Excel files from `data/`.
3. Run the cells in order and upload both datasets when prompted.
4. Download the results ZIP produced by the final cell.

The notebook includes preprocessing, exploratory plots, duplicate-aware validation, model comparisons and a saved prediction pipeline. Full training has not been run in this repository; metrics are generated when the notebook runs.

## Dataset
The two Excel workbooks are the user-supplied UNSW-NB15 prepared splits (175,341 training records and 82,332 test records). Their contents are unchanged; the training filename has been normalized.

Source and usage terms: https://research.unsw.edu.au/projects/unsw-nb15-dataset

Dataset credit: Nour Moustafa and Jill Slay, “UNSW-NB15: a comprehensive data set for network intrusion detection systems (UNSW-NB15 network data set),” MilCIS, 2015. Dataset rights remain with its authors; refer to the official page for citation and usage requirements.
