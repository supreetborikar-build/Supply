# College Placement Prediction API

This repository contains a machine learning pipeline that trains a random forest classification model on a college placements dataset (`College_placement.csv`) and serves predictions via a Flask web API.

---

## Project Structure

```text
Nirmaaan 2/
├── College_placement.csv   # The placement dataset
├── requirements.txt         # Project dependencies
├── model_building.py       # Preprocesses data, trains classifier, and exports model.pkl
├── model.pkl               # Serialized trained model (generated after training)
├── app.py                  # Flask application serving the API
└── test_client.py          # Python integration test script
```

---

## Quick Start Guide

### 1. Set Up the Python Virtual Environment

Create and activate a virtual environment to isolate project dependencies:

* **Windows (Command Prompt / PowerShell)**:
  ```bash
  python -m venv venv
  .\venv\Scripts\activate
  ```
* **macOS / Linux**:
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```

### 2. Install Dependencies

Install the required packages listed in `requirements.txt`:
```bash
pip install -r requirements.txt
```

### 3. Build & Train the Model

Run the model building script to train the model and generate `model.pkl`:
```bash
python model_building.py
```
This script splits the data, trains a `RandomForestClassifier`, prints validation performance, and serializes the model.

### 4. Start the Flask Server

Launch the prediction API:
```bash
python app.py
```
The server will start locally at `http://127.0.0.1:5000`.

---

## How to Query the API

### Option A: Using PowerShell (Windows)
Run the following cmdlet to send a prediction payload:
```powershell
Invoke-RestMethod -Uri http://127.0.0.1:5000/predict -Method Post -ContentType "application/json" -Body '{"IQ": 120, "Prev_Sem_Result": 7.97, "CGPA": 7.76, "Academic_Performance": 1, "Extra_Curricular_Score": 9, "Communication_Skills": 9, "Projects_Completed": 2, "Internship_Experience": 1.0}'
```

### Option B: Using curl (macOS / Linux)
```bash
curl -X POST http://127.0.0.1:5000/predict \
     -H "Content-Type: application/json" \
     -d '{"IQ": 120, "Prev_Sem_Result": 7.97, "CGPA": 7.76, "Academic_Performance": 1, "Extra_Curricular_Score": 9, "Communication_Skills": 9, "Projects_Completed": 2, "Internship_Experience": 1.0}'
```

### Option C: Using Python `requests`
Execute the pre-configured test client script:
```bash
python test_client.py
```

---

## API Request / Response Schema

### Single Prediction Request

* **Endpoint**: `POST /predict`
* **Headers**: `Content-Type: application/json`
* **Request Payload**:
```json
{
  "IQ": 120,
  "Prev_Sem_Result": 7.97,
  "CGPA": 7.76,
  "Academic_Performance": 1,
  "Extra_Curricular_Score": 9,
  "Communication_Skills": 9,
  "Projects_Completed": 2,
  "Internship_Experience": 1.0
}
```

* **Response (200 OK)**:
```json
{
  "results": {
    "placement_prediction": 1,
    "placement_probability": 0.97,
    "status": "Placed"
  }
}
```

### Batch Prediction Request
You can pass a JSON array to perform bulk predictions:
```json
[
  { "IQ": 120, "Prev_Sem_Result": 7.97, "CGPA": 7.76, "Academic_Performance": 1, "Extra_Curricular_Score": 9, "Communication_Skills": 9, "Projects_Completed": 2, "Internship_Experience": 1.0 },
  { "IQ": 101, "Prev_Sem_Result": 8.34, "CGPA": 7.87, "Academic_Performance": 9, "Extra_Curricular_Score": 7, "Communication_Skills": 9, "Projects_Completed": 3, "Internship_Experience": 0.0 }
]
```
