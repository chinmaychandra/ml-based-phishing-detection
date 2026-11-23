# ML-Based Phishing Detection System

A machine-learning-based web application that analyzes URLs and predicts
whether they are legitimate or potentially phishing. The system combines
URL-based feature extraction with trained classification models and
provides additional SSL certificate information during analysis.

## Features

-   **URL analysis:** Extracts features from a submitted URL to assess
    phishing risk.
-   **Machine learning models:** Supports Random Forest, XGBoost,
    LightGBM, and Support Vector Machine (SVM).
-   **SSL inspection:** Retrieves SSL-related information for a URL.
-   **Certificate verification:** Includes certificate-chain
    verification functionality.
-   **Web interface:** Provides a Flask-based interface for submitting
    URLs.
-   **API endpoint:** Supports URL analysis through a JSON API.
-   **Model training pipeline:** Loads a dataset, prepares it, extracts
    features, and trains/evaluates models.

## Technology Stack

-   **Language:** Python
-   **Web framework:** Flask
-   **Machine learning:** scikit-learn, XGBoost, LightGBM
-   **Data processing:** pandas, NumPy
-   **URL and web analysis:** requests, BeautifulSoup, tldextract,
    python-whois
-   **SSL and certificates:** pyOpenSSL
-   **Model persistence:** joblib

## Project Structure

``` text
ml-based-phishing-detection/
├── report/
├── src/
│   ├── content_analyzer.py
│   ├── crypto_verifier.py
│   ├── dataset_loader.py
│   ├── feature_extractor.py
│   ├── model_trainer.py
│   ├── predictor.py
│   └── url_analyzer.py
├── templates/
│   └── index.html
├── .gitignore
├── app.py
├── config.py
├── requirements.txt
├── security_test.py
└── train.py
```

The `data/`, `models/`, and generated output directories may be created
locally during dataset preparation and model training; they are not
necessarily included in the repository.

## Getting Started

### 1. Clone the repository

``` bash
git clone https://github.com/chinmaychandra/ml-based-phishing-detection.git
cd ml-based-phishing-detection
```

### 2. Create a virtual environment

**Windows (PowerShell):**

``` powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**Linux/macOS:**

``` bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

``` bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## Dataset and Model Training

The training script runs the dataset-loading, preprocessing,
feature-extraction, model-training, and model-comparison pipeline.

Before training, check the dataset path and expected CSV format in
`config.py` and `src/dataset_loader.py`. Make sure the required dataset
is available at the configured location.

Run:

``` bash
python train.py
```

The training pipeline is configured to evaluate the supported models and
save trained model artifacts in the configured models directory.

## Run the Web Application

After the required model artifacts are available, start the Flask
application:

``` bash
python app.py
```

Open the following address in your browser:

``` text
http://127.0.0.1:5000
```

Submit a URL through the web interface to receive its analysis.

## API Usage

### `POST /api/check`

This endpoint accepts a JSON object containing the URL to analyze.

**Request:**

``` http
POST /api/check
Content-Type: application/json
```

``` json
{
  "url": "https://example.com"
}
```

**Example using Python:**

``` python
import requests

response = requests.post(
    "http://127.0.0.1:5000/api/check",
    json={"url": "https://example.com"}
)

print(response.status_code)
print(response.json())
```

### `POST /analyze`

The `/analyze` endpoint accepts a URL through form data or JSON. In
addition to the prediction, the application includes SSL-related
information and certificate-chain verification information in its
response.

Example JSON body:

``` json
{
  "url": "https://example.com"
}
```

A missing URL is reported as a client error. An unexpected processing
exception is returned as a server error.

## Configuration

Project-level settings are maintained in `config.py`, including:

-   Dataset and output directory paths
-   Random seed and test-set size
-   Model hyperparameters
-   SSL timeout
-   HTTP request timeout

Review these settings before running the training pipeline.

## Security Notes

-   A model prediction is an assessment, not a guarantee that a URL is
    safe or malicious.
-   Do not open suspicious URLs in a normal browser just to test them.
-   Network-based URL and certificate checks may fail because of DNS,
    connectivity, server, or certificate issues.
-   Avoid exposing the development Flask server publicly. The
    application currently starts with Flask debug mode enabled; disable
    debug mode and use a production-ready WSGI server before deployment.

## Limitations

-   Training depends on the availability and quality of the dataset.
-   Prediction quality depends on the training data, extracted features,
    and model performance.
-   Some URLs may not provide accessible website content or SSL
    information.
-   The repository's exact dataset source, dataset download
    instructions, and model artifacts should be documented once
    finalized.
