

# IDS-AI: Network Intrusion Detection System

An AI-driven network intrusion detection system (NIDS) that exposes a REST API for predictions and ships with a lightweight frontend for manual exploration. A pre-trained logistic regression model (serialized with `joblib`) powers the inference backend, allowing you to classify network traffic as benign or malicious using 20 engineered features.

## Repository structure

| Path | Description |
| ---- | ----------- |
| `app.py` | Flask application that loads the trained model and serves `/predict` for intrusion inference. |
| `frontend.html` | Minimal web UI that lets users supply feature values and view prediction results. |
| `test_api.py` | Simple script that submits a sample payload to the API for smoke testing. |
| `snort_log_to_csv.py` | Utility that converts Snort IDS alerts into a CSV file for downstream analysis. |
| `*.pkl` | Serialized machine-learning assets, including logistic regression and XGBoost models. |
| `*.xlsx`, `*.xls`, `*.csv` | Example datasets and exported insights for experimentation. |

## Requirements

  - Python 3.10+
  - Recommended packages:
    ```bash
    pip install flask flask-cors joblib numpy requests
    ```
  - (Optional) Jupyter Notebook if you plan to explore the bundled `NIDS (1).ipynb` notebook.

## Getting started

1.  **Clone the repository and enter the project directory**
    ```bash
    git clone <repo-url>
    cd IDS-AI
    ```
2.  **Create and activate a virtual environment (optional but recommended)**
    ```bash
    python -m venv .venv
    source .venv/bin/activate  # Linux / macOS
    .venv\Scripts\activate     # Windows
    ```
3.  **Install dependencies**
    ```bash
    pip install flask flask-cors joblib numpy requests
    ```

## Running the API backend

1.  Ensure the serialized model (`nids_logistic_regression.pkl`) is located in the project root (it is included in the repo).
2.  Start the Flask server:
    ```bash
    python app.py
    ```
3.  The API listens on `http://127.0.0.1:5000` by default. You should see `NIDS API is running!` when visiting the root endpoint in a browser.

### Predicting intrusions via API

Send a `POST` request to `/predict` with a JSON body containing a `features` array of 20 numeric values:

```bash
curl -X POST http://127.0.0.1:5000/predict \
  -H "Content-Type: application/json" \
  -d '{"features": [0.12, -1.5, ... 20 values total ...]}'
```

The API responds with the predicted class (`0` for normal, `1` for intrusion) and the class probabilities.

To sanity-check the service you can also run:

```bash
python test_api.py
```

which submits a pre-canned example payload.

## Using the frontend

Open `frontend.html` in a browser while the Flask server is running. The page renders 20 numeric inputs corresponding to the model's features and displays the prediction returned by the API.

> **Tip:** If you host the backend on a different machine or port, edit the `fetch` URL defined in `frontend.html` accordingly.

## Converting Snort logs to CSV

The `snort_log_to_csv.py` utility parses a Snort alert log and writes a structured CSV file with attack type, protocol, and source/destination IPs. Update `log_file` and `output_csv` in the script to match your environment, then run:

```bash
python snort_log_to_csv.py
```

## Model assets

  - `nids_logistic_regression.pkl`: Primary model used by the API.
  - `nids_model_log_reg.pkl`, `intrusion_detection_model.pkl`, `xgboost_model.pkl`: Alternative or experimental models retained for future comparison.

The repository also ships `X_test.pkl` and `y_test.pkl` for offline evaluation, along with synthetic datasets (`synthetic_dataset.xlsx`, `synthetic_dataset.xls`).

## Extending the project

  - Retrain or swap the model: load your own scikit-learn or XGBoost estimator, export it with `joblib.dump`, and update `app.py` to reference the new file.
  - Integrate streaming data sources: adapt the API to ingest live traffic statistics and log predictions.
  - Harden for production: consider adding authentication, input validation, and deployment automation.

## License

Specify your project's license here (e.g., MIT, Apache 2.0). Update this section if you adopt a formal license.
