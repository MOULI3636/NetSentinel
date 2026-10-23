# 🛡️ NetSentinel

## Network Intrusion Detection Using Apache Spark MLlib

NetSentinel is a machine-learning project that analyzes network traffic
records and classifies them as **normal traffic** or **potential attack
traffic**. It uses the NSL-KDD dataset and Apache Spark MLlib to build,
evaluate, and compare classification models.

The project includes a Jupyter Notebook for data preparation and
experiments, saved model pipelines, and a Streamlit dashboard for
interacting with the trained Decision Tree model.

## Project Features

-   **Network traffic classification:** Predicts whether a network
    record is normal or potentially malicious.
-   **Data preparation:** Loads the NSL-KDD training and testing files,
    names the dataset columns, and prepares a binary target label.
-   **Feature processing:** Converts categorical network fields into
    numerical features and combines the input columns into a feature
    vector for Spark MLlib.
-   **Model training and comparison:** Trains and evaluates Decision
    Tree, Logistic Regression, Linear SVM, Naive Bayes, and Random
    Forest classifiers.
-   **Performance evaluation:** Uses metrics such as Accuracy and
    F1-score, with ROC curve analysis where included in the notebook.
-   **Feature importance:** Examines the features used by the selected
    Decision Tree model.
-   **Traffic analysis:** Counts normal and attack records and explores
    frequent attack labels.
-   **Interactive dashboard:** Provides an Overview page, CSV/TXT upload
    for intrusion prediction, prediction results, and project
    information.
-   **Exportable results:** Allows prediction results to be reviewed and
    downloaded from the dashboard.

## Technology Stack

-   **Python** --- project programming language
-   **Apache Spark / PySpark MLlib** --- data processing and
    machine-learning pipelines
-   **Streamlit** --- interactive web dashboard
-   **Pandas** --- tabular data handling
-   **Matplotlib** --- charts and visualizations
-   **Jupyter Notebook** --- experimentation and model evaluation

## Project Structure

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

-   `data/` contains the training and testing dataset files.
-   `models/` contains saved Spark MLlib pipeline models.
-   `notebooks/NetSentinel.ipynb` contains the data preparation,
    training, evaluation, comparison, and analysis workflow.
-   `app.py` contains the Streamlit dashboard.
-   `requirements.txt` lists the project's Python dependencies.

## How the Project Works

### 1. Data Loading and Preparation

The notebook loads the training and test files and assigns names to the
dataset columns. The attack label is converted into a binary target:
normal traffic is represented by `0`, while attack traffic is
represented by `1`. The original attack and difficulty columns are
excluded from the model input so that the model learns from the network
traffic features.

### 2. Feature Processing

The dataset contains numeric and categorical fields. The categorical
fields `protocol_type`, `service`, and `flag` are converted into indexed
values and encoded into feature vectors. The numeric and encoded columns
are then combined into a single `features` vector for model training.

### 3. Model Training

The project trains five classifiers using Spark MLlib:

  -----------------------------------------------------------------------
  Model                               Role in the project
  ----------------------------------- -----------------------------------
  Decision Tree                       Learns decision rules and provides
                                      feature-importance information

  Logistic Regression                 Provides a linear classification
                                      approach

  Linear SVM                          Separates classes using a linear
                                      decision boundary

  Naive Bayes                         Uses a probabilistic classification
                                      approach

  Random Forest                       Combines multiple decision trees
  -----------------------------------------------------------------------

The models are evaluated on the prepared test data. The model with the
strongest measured results under the project's chosen metrics can be
selected for further analysis and dashboard predictions. The actual
scores should be taken from the notebook's output rather than assumed in
advance.

### 4. Model Evaluation and Analysis

Accuracy measures the proportion of correctly classified records.
F1-score balances precision and recall. ROC curve analysis can be used
to compare how well models distinguish the two classes across different
decision thresholds, provided suitable prediction scores are available.

The notebook also visualizes feature importance for the selected
Decision Tree. This helps explain which inputs the model relies on, but
feature importance does not prove that a feature causes an attack.

### 5. Traffic Analysis

The notebook summarizes normal versus attack traffic and identifies
frequent attack labels in the dataset. These views help describe the
dataset's class distribution and provide context for interpreting the
model's predictions.

### 6. Streamlit Dashboard

The dashboard provides four main pages:

-   **Overview:** Summarizes the project, model, and analysis
    capabilities.
-   **Detect Intrusions:** Accepts a supported CSV/TXT file and runs the
    saved Decision Tree pipeline on its records.
-   **Prediction Results:** Displays the number of records classified as
    normal or attack, a traffic distribution chart, record-level
    predictions, and a downloadable results file.
-   **About Project:** Describes the objective, dataset, workflow, and
    technology stack.

The dashboard expects the required 41 input features in the correct
order and format. Input support depends on the validation implemented in
`app.py`. Predictions are estimates from the trained model, not
confirmed security incidents.

## Setup and Usage

Use Python, Java, and PySpark versions that are compatible with one
another. In particular, PySpark 3.5.3 is a better fit for Python 3.11
than Python 3.13.

From the project root on Windows PowerShell, create and activate an
environment if needed:

``` powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

If dependencies are missing from `requirements.txt`, install the
packages used by the project:

``` powershell
pip install pyspark==3.5.3 streamlit pandas matplotlib scikit-learn
```

Open `notebooks/NetSentinel.ipynb` in VS Code, select the intended
environment as the notebook kernel, and run the cells in order. The
notebook saves trained model artifacts under `models/`.

To launch the dashboard from the folder containing `app.py`, run:

``` powershell
streamlit run app.py
```

## 🚀 Live Demo

**Live URL:** https://netsentinel-36.streamlit.app/

Explore the NetSentinel interface for network intrusion detection
and attack analysis.

## Limitations

-   NetSentinel analyzes stored dataset records; it does not capture
    live network packets.
-   NSL-KDD is a benchmark dataset and may not represent current network
    traffic or every modern attack.
-   Model performance on the dataset may not transfer directly to a real
    network.
-   The model's output should be treated as a prediction that requires
    further investigation, not proof of malicious activity.
-   The saved model and dashboard must use compatible Python, Java, and
    PySpark environments.

## Project Summary

NetSentinel combines network traffic preparation, Spark MLlib
classification, model evaluation, feature-importance analysis, traffic
summaries, and a Streamlit interface in one project. It demonstrates an
end-to-end workflow for dataset-based network intrusion detection and
makes model predictions easier to inspect through an interactive
dashboard.
