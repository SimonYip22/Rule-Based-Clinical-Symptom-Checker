# Rule-Based Clinical Symptom Checker

***A Deterministic Python Clinical Inference System for Symptom-to-Differential Ranking***

---

## Executive Summary

**Tech Stack:** Python · FastAPI · Uvicorn · Pydantic · Pytest · GitHub Actions (CI/CD)

This project is a Python-based clinical decision-support engine that interprets patient-reported symptoms and produces a structured, ranked differential diagnosis. The system accepts symptom input through either a command-line interface (CLI) or a deployed FastAPI JSON API and applies explicitly encoded clinical reasoning to map reported features to candidate conditions. A formula-weighted scoring mechanism evaluates symptom alignment and prioritisation logic to generate the three most likely conditions, alongside matched symptoms and structured management guidance for the highest-ranked result.

The system implements a deterministic rule-based architecture rather than a trained machine learning model. Clinical knowledge is encoded deterministically through condition–symptom mappings and transparent weighting rules, ensuring outputs remain interpretable, reproducible, and auditable. A preprocessing layer performs input normalisation and synonym mapping to standardise user-reported terminology prior to evaluation. Results are returned in structured JSON format to support downstream integration and programmatic consumption.

Version 2 extends the original CLI implementation into a deployable backend service with documented API endpoints, enabling web-based interaction and external system integration. Automated testing and continuous integration workflows validate both inference logic and API functionality to ensure reliability and structural integrity. The modular codebase supports future expansion, including potential integration of probabilistic reasoning frameworks or machine learning components, while preserving a deterministic reasoning core.

Overall, the system demonstrates structured knowledge modelling, deterministic inference design, API deployment, automated testing, and clinically grounded rule encoding within a production-oriented Python project.

The deployed API (v2) is live on Render and can be accessed [here](https://ai-symptom-checker-5rfb.onrender.com/docs) for interactive testing and demonstration.

![Symptom Checker Flowchart](symptom-checker-flowchart.png)
*Figure 1: Flowchart illustrating symptom input, normalisation, scoring, ranking, and structured output generation.*

---

## Clinical & Technical Highlights

| Feature | Description |
|----------|-------------|
| **Rule-Based Weighted Scoring** | Each symptom is assigned a condition-specific weight. Aggregated weighted scores simulate clinical prioritisation and differential reasoning. |
| **Input Normalisation & Alias Mapping** | Accepts lay terminology (e.g., “high temperature” → “fever”) through synonym mapping to standardised clinical features. |
| **Top 3 Condition Ranking** | Produces a ranked differential using a deterministic formula-based scoring system to approximate probabilistic-style output. |
| **Educational Disclaimer** | Includes explicit usage limitations and safety framing to reinforce that outputs are informational, not diagnostic. |
| **Advice Dictionary** | Provides structured, condition-specific management guidance for the highest-ranked diagnosis. |
| **Modular CLI Architecture** | Separates input collection, normalisation, scoring logic, and output rendering to support maintainability and extensibility. |
| **Future AI/ML Extension Potential** | Architected to allow integration of machine learning classifiers, NLP-based symptom extraction, or probabilistic reasoning frameworks. |

---

## CLI vs FastAPI: Skills Showcase

| Feature | CLI (v1) | FastAPI (v2 - Live on Render) |
|---------|-----|--------------------------|
| **User Interaction** | Command-line symptom entry; interactive prompts | JSON API requests (`POST /predict`, `GET /health`); accessible via Swagger UI |
| **Data Entry** | Manual typing via terminal | Scriptable input via HTTP requests or programmatic integration |
| **Input Handling** | Normalises lay terms via aliases; prompts user interactively | Accepts JSON array of symptoms; validates via Pydantic |
| **Output** | Ranked conditions with weighted scores, matched symptoms, likely diagnosis and advice; immediate terminal feedback | JSON responses with condition scores, matched symptoms, likely diagnosis, and advice; usable in dashboards, ML pipelines, or automated workflows |
| **Deployment** | Local only | Live on Render; recruiter-accessible demo |
| **Modularity** | CLI logic in single script | CLI and API code modularly separated for maintainability |
| **Testing & CI/CD** | Pytest manual testing of scoring logic | Automated Pytest + httpx validating live API endpoints; GitHub Actions ensures continuous validation and production-ready deployment |
| **Clinical Insight** | Symptom-condition mappings encoded via clinical reasoning | Enables integration into web apps, dashboards, and future AI/ML models |
| **Explainability** | Shows contribution of each symptom to condition score in console | Structured JSON output retains transparency; API facilitates analytics or logging |

---

## How It Works

1. **User Input**  
   - Collects symptoms one at a time until “done” is entered.  
   - Normalizes input using aliases and canonical symptom mapping.  

2. **Scoring Engine**  
   - Iterates over conditions and computes weighted scores for matched symptoms.  
   - Adjusts scores relative to total possible symptoms per condition as well as total possible score.  
   - Returns top 3 conditions ranked by normalized score.  

3. **Result Display**  
   - Prints matched symptoms and percentage likelihood for top conditions.  
   - Provides management advice for the most likely condition.  
   - CLI output includes clear formatting and disclaimers.


---


## CLI Technical Highlights

- **Rule-based inference engine**: Weighted scoring allows quick adaptation for new conditions. 
-	**Normalization and alias mapping**: Handles human input variability.
-	**Modular function design**: Each step (input, scoring, display) is separated, enabling future AI integration.
-	**Scoring normalisation**: Developed a scoring algorithm that accounts for total symptom scores and relative condition length: 
`adjusted_score = (total_score / total_possible) * (num_symptoms / max_symptoms)`
-	**CLI-based UX**: Lightweight, fast, and easy to extend.

---


## Architecture & Implementation

**Key Components**
1.	**Conditions Dictionary**
	- Maps each condition to its symptoms and weighted scores.
	- **Example**: "Urinary Tract Infection": {"urinary frequency": 3, "blood in urine": 2, ...}
2.	**Symptom Aliases**
	-	Handles layman terms for symptoms.
	-	**Example**: "wee a lot" → "urinary frequency"
3.	**Input Normalisation (normalise_choice_input)**
	-	Cleans text inputs (lowercase, remove punctuation).
	-	Maps aliases to canonical symptom names.
	-	Returns None for unrecognised symptoms.
4.	**User Input Collection (user_symptoms_list)**
	-	Prompts the user to enter symptoms until "done".
	-	Normalizes input using normalise_choice_input.
	-	Returns validated symptom list.
5.	**Scoring Function (score_conditions)**
	-	Iterates through all conditions.
	-	Computes weighted scores for matched symptoms.
	-	Adjusts scores for condition length vs max symptoms.
	-	Returns a dictionary of normalized condition scores.
6.	**Display Results (display_results)**
	-	Sorts conditions by score.
	-	Displays top 3 conditions with percentage scores and matched symptoms.
	-	Prints likely diagnosis with advice.
	-	Includes educational disclaimer.
7.	**Main CLI Loop (__main__)**
	-	Continuously prompts user for input or "exit".
	-	Runs display_results() for each session.

---


## Future Improvements

**AI/ML Integration**:
  - Train classifiers on clinical datasets for probabilistic condition ranking.
  - NLP for symptom extraction from free text.
  -	Bayesian inference for probabilistic reasoning.

**UI/UX Enhancements**:
  -	Web or GUI front-end.
  -	Real-time suggestions and auto-completion.

**External Data Integration**:
  -	Pull real-time disease prevalence data.
  -	Connect with EHRs for personalised risk assessment.

---


## CLI Quick Start

**Clone repo and run**:

```bash
git clone https://github.com/SimonYip22/AI-Symptom-Checker.git
cd AI-Symptom-Checker
python ai_symptom_checker.py 
```

- Press Enter to start
- Enter symptoms one at a time
- Type "done" when finished
- Type "exit" to quit

---


## CLI Example session

```text
Press Enter to start symptom checker, or type 'exit' to quit: 

Enter a symptom: diarrhoea
Enter a symptom: vomiting
Enter a symptom: stomach pain
Enter a symptom: done
You have entered these symptoms:  ['diarrhoea', 'vomiting', 'abdominal pain']

Top 3 possible conditions:
Gastroenteritis (stomach bug): 32.3% | Matched symptoms: ['diarrhoea', 'vomiting', 'abdominal pain']
Influenza (flu): 9.1% | Matched symptoms: ['vomiting']
Migraine: 7.3% | Matched symptoms: ['vomiting']

Likely diagnosis is:  Gastroenteritis (stomach bug)
Advice: Stay at home and get plenty of rest, drink plenty of water to stay hydrated (small sips if you feel sick),
 Try and eat when you feel able to, paracetamol +/- ibuprofen if in pain.
Important: This tool is for educational purposes only and does not replace medical advice. If you are worried about your symptoms, please contact NHS 111.
 Call 999 or go to A&E immediately if your symptoms are severe, sudden, or life-threatening.
Press Enter to start symptom checker, or type 'exit' to quit: 
```


---


## v2 — API Deployment with FastAPI (LIVE)

**Python | FastAPI | Pydantic | JSON Output | Render**

### Live Demo

- Deployed API on Render: [https://ai-symptom-checker-5rfb.onrender.com/docs](https://ai-symptom-checker-5rfb.onrender.com/docs)
- API endpoints:
  - `GET /health` → {"status":"ok"}
  - `GET /` → {"message":"Clinically-Informed AI Symptom Checker v2 API is running"}
  - `POST /predict` → JSON input list of symptoms; returns top conditions

### Overview

- CLI logic wrapped into a deployable API
- JSON output includes:
  - `user_symptoms` (normalised)
  - `top_conditions` (top 3, percentage, matched symptoms)
  - `likely_diagnosis`
  - `advice`
- CLI and API are modularly separate for maintainability and scalability

### Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | API health check |
| `/` | GET | Root message confirming API is running |
| `/predict` | POST | Input: list of symptoms; Output: top conditions JSON |


### API Screenshots

![Swagger UI Home](v2_api/swagger_home.png)  
*Figure 2: FastAPI Swagger UI home page showing available endpoints.*

![Swagger Health Check](v2_api/swagger_health_check.png)  
*Figure 3: /health endpoint response confirming API is running.*

![Swagger /predict Example](v2_api/swagger_predict_example.png)  
*Figure 4: /predict endpoint example request and JSON response.*

### Example POST `/predict` Request

```json
{
  "symptoms": ["fever", "cough"]
}
```

### Example JSON response

```json
{
  "user_symptoms": [
    "fever",
    "cough"
  ],
  "top_conditions": [
    {
      "condition": "Influenza (flu)",
      "score": "27.3%",
      "matched_symptoms": [
        "fever",
        "cough"
      ]
    },
    {
      "condition": "Pneumonia (chest infection)",
      "score": "15.6%",
      "matched_symptoms": [
        "fever",
        "cough"
      ]
    },
    {
      "condition": "Otitis Media (inner ear infection)",
      "score": "9.1%",
      "matched_symptoms": [
        "fever"
      ]
    }
  ],
  "likely_diagnosis": "Influenza (flu)",
  "advice": "Rest and sleep, keep warm, paracetamol +/- ibuprofen for fever and pain,\n Drink plenty of water to stay hydrated, stay at home and avoid contact with others to prevent spreading,\n Cover mouth and nose with tissue when cough or sneezing, dispose of immediately,\n Wash hands regularly with soap and water."
}
```

### Root Endpoint

```Python
@app.get("/")
def root():
    return {"message": "Clinically-Informed AI Symptom Checker v2 API is running"}
```

### API Testing / CI

Automated endpoint tests are run using GitHub Actions to ensure the live deployment works correctly.
- **Workflow file**: .github/workflows/api-tests.yml
- Uses pytest and httpx to check endpoints against the live Render deployment.
- **Tests include**:
  - GET /health returns 200 and {"status":"ok"}
	- GET / returns 200 and {"message":"Clinically-Informed AI Symptom Checker v2 API is running"}
	- POST /predict validates correct JSON structure for sample symptoms

**Example test snippet**:

```Python
import httpx

API_URL = "https://ai-symptom-checker-5rfb.onrender.com"

def test_health_endpoint():
    response = httpx.get(f"{API_URL}/health")
    assert response.status_code == 200
    assert response.json() == {"status": "ok"}

def test_predict_endpoint_valid():
    payload = {"symptoms": ["fever","cough"]}
    response = httpx.post(f"{API_URL}/predict", json=payload)
    assert response.status_code == 200
    data = response.json()
    assert "user_symptoms" in data
    assert "top_conditions" in data
```
- Workflow triggers:
  - Push to main
  - Scheduled weekly checks

This demonstrates a production-ready backend with continuous validation.


### How to run locally

```bash
cd v2_api
uvicorn app:app --reload
```

- Visit http://127.0.0.1:8000/docs for interactive Swagger UI
- Test endpoints in-browser
- View JSON responses for example inputs


### Notes / Future Work
- Demonstrates production-ready backend for a clinician-technologist portfolio
- JSON outputs are easy to integrate into a frontend later if desired
- Future improvements:
	- Expand conditions & specialties
	- Integrate ML/NLP for symptom extraction
	- Deploy live on Render / Railway / Heroku
	- Add persistent logging/analytics
- No frontend required; API + CLI + deployment is enough for portfolio.


---



## Running Tests

**Test the scoring logic and alias handling**:

```bash
pytest -v
```
**Tests include:**
- Weighted scoring correctness
- Symptom alias normalisation
- Handling unrecognized inputs
- Top-3 ranking validation


---


## Project Structure

```text
AI-Symptom-Checker/
├── .github/
│   └── workflows/
│       ├── api-tests.yml 
│       └── python-tests.yml
├── v2_api/
│   ├── ai_symptom_checker_v2.py
│   ├── app.py
│   ├── requirements.txt
│   ├── swagger_health_check.png
│   ├── swagger_home.png
│   ├── swagger_predict_example.png
│   └── test_api_endpoints.py
├── ai_symptom_checker.py
├── notes.md
├── README.md
├── reflection.md
├── sample_run.txt
├── symptom-checker-flowchart.png
└── test_ai_symptom_checker.py
```

**Explanations**:
- **.github/workflows/**
  - **api-tests.yml** - CI workflow that runs pytest against live Render deployment to verify /health, /, and /predict endpoints.
  - **python-tests.yml** - CI for core Python logic and CLI tests
- **v2_api/** — Contains the FastAPI version of the symptom checker
	- **ai_symptom_checker_v2.py** — Core logic for scoring, normalisation, and advice, adapted for API usage
	- **app.py** — FastAPI app wrapping the core logic into /predict and /health endpoints
	- **requirements.txt** — Python dependencies for v2 API (FastAPI, Pydantic, Uvicorn)
	- **swagger_health_check.png** — Screenshot showing /health endpoint response
	- **swagger_home.png** — Screenshot of the Swagger UI home page
	- **swagger_predict_example.png** — Screenshot showing /predict example request and JSON response
  - **test_api_endpoints.py** - Test script for the API endpoints (run both locally and via GitHub Actions)
- **ai_symptom_checker.py** — Original CLI program
- **notes.md** — Daily notes and reflections
- **README.md** — Project documentation
- **reflection.md** — Final project reflection
- **sample_run.txt** — Example runs
- **symptom-checker-flowchart.png** - Flowchart explaining logic
- **test_ai_symptom_checker.py** — Automated tests


---


## Disclaimer

- Educational use only. 
- Not a substitute for professional medical advice.
- For severe, sudden, or life-threatening symptoms, contact 999/A&E immediately.
- For non-emergency concerns, contact NHS 111.
