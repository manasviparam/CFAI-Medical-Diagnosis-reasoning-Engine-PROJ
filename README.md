# MedAI — Python/Flask Edition

Medical diagnosis web app with a Bayesian inference engine moved from JavaScript to Python.

## Project Structure

```
project/
├── frontend/
│   ├── index.html          ← unchanged UI
│   ├── engine.js           ← MODIFIED: only BP/Cholesterol classifiers remain
│   └── app.js              ← MODIFIED: _parseFreetext & runDiagnosis use fetch()
└── backend/
    ├── app.py              ← Flask server + API routes
    ├── bayesian_engine.py  ← Bayesian inference, synonym parser (ported from JS)
    └── diseases_data.json  ← Disease knowledge base (converted from diseases_data.js)
```

## Files Changed

| File | Change |
|------|--------|
| `frontend/engine.js` | Removed: SYNONYMS, DISEASES, parseFreText, diagnose, checkEmergency, ageGaussian. Kept: classifyBP, classifyCholesterol |
| `frontend/app.js` | `_parseFreetext()` → async, calls `/api/parse_freetext`. `runDiagnosis()` → async, calls `/api/diagnose` |
| `frontend/index.html` | Removed `<script src="diseases_data.js">` tag |
| `backend/app.py` | **NEW** — Flask app serving frontend + 3 API endpoints |
| `backend/bayesian_engine.py` | **NEW** — Python port of engine.js inference logic |
| `backend/diseases_data.json` | **NEW** — JSON conversion of diseases_data.js |

## Run Guide

### 1. Install Flask

```bash
pip install flask
```

### 2. Start the server

```bash
cd backend
python app.py
```

### 3. Open the app

Navigate to **http://localhost:5000** in your browser.

## API Endpoints

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/` | Serves the frontend |
| POST | `/api/parse_freetext` | Parse free-text symptoms → matched symptoms + unknown tokens |
| POST | `/api/classify_bp` | Classify blood pressure reading → category string |
| POST | `/api/classify_cholesterol` | Classify cholesterol value → category string |
| POST | `/api/diagnose` | Run full Bayesian diagnosis → ranked disease list + emergency flag |

## What Was NOT Changed

- All HTML/CSS layout and styling
- The 4-step wizard flow (profile → BP → cholesterol → symptoms)
- Symptom toggle buttons and free-text chip display
- Disease ranking, probability display, belief trace table
- Risk assessment colours and emergency banner
- Recommendations and "see doctor if" sections
- All pre-existing condition modifiers, age Gaussian, BP/cholesterol modifiers
- Diagnosis results are numerically identical to the original JS version
