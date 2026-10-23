# 🛡️ NetSentinel

### Network Intrusion Detection Using Apache Spark MLlib

NetSentinel is an academic machine-learning project that analyzes
network traffic records and classifies them as **normal traffic** or
**potential attack traffic**. It uses the NSL-KDD benchmark dataset and
Apache Spark MLlib to train and evaluate multiple classification models.

> **Project scope:** NetSentinel currently performs dataset-based
> analysis. It does not capture live network packets or act as a
> production firewall or intrusion prevention system.

------------------------------------------------------------------------

## ✨ Features

-   **Network traffic classification** --- predicts whether a record is
    normal or an attack.
-   **Multiple ML models** --- Decision Tree, Logistic Regression,
    Linear SVM, Naive Bayes, and Random Forest.
-   **Model evaluation** --- compares Accuracy and F1-score; ROC curves
    can be used to compare discrimination performance.
-   **Feature importance** --- explores which input features the
    selected Decision Tree relies on.
-   **Attack analysis** --- examines normal-versus-attack traffic and
    frequent attack labels.
-   **Streamlit dashboard** --- provides a user interface for uploading
    supported CSV/TXT data, viewing predictions, and exporting results.
-   **Saved models** --- keeps trained Spark MLlib pipeline models in
    the `models/` directory.

## 🧰 Technology Stack

-   Python
-   Apache Spark / PySpark MLlib
-   Streamlit
-   Pandas
-   Matplotlib
-   scikit-learn (for evaluation/ROC calculations where used)
-   Jupyter Notebook

## 📁 Project Structure

``` text
NetSentinel/
├── data/
│   ├── KDDTrain+.txt
│   └── KDDTest+.txt
├── models/
│   ├── decision_tree/
│   ├── logistic_regression/
│   ├── naive_bayes/
│   ├── random_forest/
│   └── svm/
├── notebooks/
│   └── NetSentinel.ipynb
├── app.py
├── README.md
└── requirements.txt
```

The model directories contain saved Spark MLlib pipeline artifacts. Keep
each model directory intact; do not treat it as a single model file.

## 📊 Dataset

NetSentinel uses the **NSL-KDD** dataset, a benchmark dataset created
for research into network intrusion detection. The project folder
includes:

-   `KDDTrain+.txt` --- training records
-   `KDDTest+.txt` --- test records

The original files are comma-separated and typically contain 43 columns:
41 traffic features, an attack label, and a difficulty field. In the
binary-classification workflow, `normal` is mapped to label `0` and
attack labels are mapped to label `1`.

Dataset source and background: - University of New Brunswick, Canadian
Institute for Cybersecurity: https://www.unb.ca/cic/datasets/nsl.html -
NSL-KDD dataset mirror: https://github.com/jmnwong/NSL-KDD-Dataset

Please follow the dataset's terms and cite the dataset source in
academic submissions.

## ⚙️ Setup

### 1. Prerequisites

Install a compatible Python, Java, and PySpark combination. This project
was developed in a local Windows environment using Spark. **Check your
installed PySpark version against your Python and Java versions before
troubleshooting runtime errors.** PySpark 3.5.x is not a supported
combination with Python 3.13; Python 3.11 is a safer option for PySpark
3.5.3.

### 2. Create and activate a virtual environment (Windows PowerShell)

From the project root:

``` powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

If you need to keep a different Python version for other projects, this
virtual environment does not change your system-wide Python
installation.

### 3. Install dependencies

``` powershell
pip install -r requirements.txt
```

If `requirements.txt` does not yet contain all packages used by the
notebook and dashboard, install the required packages:

``` powershell
pip install pyspark==3.5.3 streamlit pandas matplotlib scikit-learn
```

Use package versions compatible with the environment in which the models
were trained and saved.

## ▶️ Run the Notebook

Open the project folder in VS Code, select the project's virtual
environment as the Jupyter kernel, and open:

``` text
notebooks/NetSentinel.ipynb
```

Run the notebook cells in order. The notebook covers data preparation,
feature encoding, model training, evaluation, model comparison, feature
importance, and traffic analysis.

The notebook saves model artifacts under `models/`. If you retrain a
model, ensure that the saved pipeline matches the preprocessing and
feature order expected by the dashboard.

## 🚀 Run the Streamlit Dashboard

From the **project root** (the folder containing `app.py`), activate the
correct virtual environment and run:

``` powershell
streamlit run app.py
```

Streamlit normally opens the application at:

``` text
http://localhost:8501
```

The dashboard includes navigation for: - **Overview** --- project and
model information - **Detect Intrusions** --- upload supported traffic
records and run predictions - **Prediction Results** --- review
predicted classes, traffic counts, and export results - **About
Project** --- summary of the project workflow and technology

### Input file expectations

The dashboard expects the 41 NSL-KDD input features in the same order
and representation used during training. Depending on the
implementation, it may support headerless 41-, 42-, or 43-column input
files, or CSV files with named feature columns. Check the input
validation in `app.py` before uploading a custom dataset.

For the original NSL-KDD text files, records are comma-separated. Use
the file format expected by the dashboard; do not assume every arbitrary
CSV will work without preprocessing.

## 🧪 Models and Evaluation

The project trains and compares these classifiers:

  Model                 Purpose
  --------------------- -------------------------------------------------
  Decision Tree         Learns interpretable decision rules
  Logistic Regression   Estimates the likelihood of the positive class
  Linear SVM            Learns a linear decision boundary
  Naive Bayes           Applies a probabilistic classification approach
  Random Forest         Combines multiple decision trees

Metrics used in the notebook include: - **Accuracy** --- fraction of
predictions that are correct. - **F1-score** --- combines precision and
recall. - **ROC-AUC** --- measures ranking/discrimination across
classification thresholds when valid model scores are available.

Use the values produced by your actual notebook in the final report. Do
not assume a model is best based on its name alone; report the measured
metrics and explain the selection criteria. For intrusion detection,
recall and false-positive behaviour can also matter because missed
attacks and false alarms have different costs.

## 🔍 Feature Importance

The notebook examines feature importance from the trained Decision Tree.
Feature importance indicates how much the model used a feature when
making decisions; it does **not** prove that a feature causes an attack.
If categorical variables are one-hot encoded, importance may refer to
encoded feature vectors rather than each original category individually.

## 🛠️ Troubleshooting

### Spark Python worker crashes

If the dashboard reports `Python worker exited unexpectedly` or
`PythonRDD.runJob`, check:

1.  The terminal is using the intended virtual environment.
2.  `python --version` and `pip show pyspark` show the expected
    versions.
3.  The Python and Java versions are compatible with the installed
    PySpark release.
4.  The saved model can be loaded and used in the notebook environment.
5.  The model folder `models/decision_tree/` exists and contains the
    full saved pipeline.

For PySpark 3.5.3, use a compatible Python version such as Python 3.11
rather than Python 3.13. Keep the environment used to load the model
consistent with the environment used to create it.

### Model folder not found

Run Streamlit from the project root and confirm that this directory
exists:

``` text
models/decision_tree/
```

The folder contains Spark model artifacts, not a single `.pkl` file.

### Predictions fail on an uploaded file

Confirm that the input has all required features, the correct column
order, valid numeric values, and categorical values handled by the saved
preprocessing pipeline.

## 🔐 Limitations

-   The project evaluates benchmark dataset records; it does not inspect
    live network traffic.
-   NSL-KDD is an older benchmark and does not represent every modern
    network environment or current attack type.
-   Model performance on this dataset may not transfer directly to real
    networks.
-   Predicted attacks are model estimates and should not be treated as
    confirmed incidents.
-   Results should be reported from the actual experiment outputs and
    interpreted alongside suitable metrics.

## 🔮 Possible Future Improvements

-   Add per-class evaluation and recall to better assess missed attacks.
-   Add a detailed attack-category analysis view.
-   Improve validation for uploaded files and provide clear input-format
    guidance.
-   Add model versioning and a reproducible experiment configuration.
-   Evaluate on newer intrusion-detection datasets and, where
    appropriate, validated live-traffic data.
-   Add authentication and deployment security before making the
    dashboard publicly accessible.

## 👩‍💻 Academic Project

**Project name:** NetSentinel --- Network Intrusion Detection Using
Apache Spark MLlib

This repository is intended for academic learning and demonstration. Add
your name, institution, supervisor, and project year here if required by
your submission format.

## 📚 References

1.  University of New Brunswick, Canadian Institute for Cybersecurity.
    *NSL-KDD dataset information*.
    https://www.unb.ca/cic/datasets/nsl.html
2.  Tavallaee, M., Bagheri, E., Lu, W., & Ghorbani, A. A. *A Detailed
    Analysis of the KDD CUP 99 Data Set*. 2009 IEEE Symposium on
    Computational Intelligence for Security and Defense Applications
    (CISDA). https://doi.org/10.1109/CISDA.2009.5356528

------------------------------------------------------------------------

**NetSentinel --- Turning network traffic records into actionable model
predictions.**
