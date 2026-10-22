# Product Requirements Document (PRD): Ledger - AI-Based Fake News Detection System Using Explainable AI

**Project Title:** Ledger: AI-Based Fake News Detection System Using Explainable AI  
**Course Code:** CSD415 Project Phase I (Seventh Semester B.Tech, September 2026)  
**Academic Institution:** College of Engineering Cherthala (CEC), APJ Abdul Kalam Technological University (KTU)  
**Department:** Department of Computer Science and Engineering  
**Project Guide:** Mrs. Rajeswary R, Assistant Professor  
**Student Team (Group 12):**  
- Anandhakrishnan S (Reg. No. CEC23CS033)  
- Aravind A Kamath (Reg. No. CEC23CS041)  
- Arjun Manoj (Reg. No. CEC23CS043)  
- Sangeeth Krishnan S (Reg. No. CEC23CS105)  

**Primary Base Paper:**  
E. Hashmi, S. Y. Yayilgan, M. M. Yamin, S. Ali and M. Abomhara, "Advancing Fake News Detection: Hybrid Deep Learning With FastText and Explainable AI," *IEEE Access*, vol. 12, pp. 44463-44481, 2024. DOI: 10.1109/ACCESS.2024.3381038.

**Primary Benchmark Dataset:** WELFake Corpus (72,134 verified news articles: 35,028 fake and 37,106 real news items).  
**Secondary Datasets:** FakeNewsNet (23,196 records) and LIAR benchmark.

---

## 1. Executive Summary & Purpose of this PRD

### 1.1 Document Objective
This Product Requirements Document (PRD) serves as an authoritative, machine-readable and developer-friendly technical roadmap. It is explicitly crafted for execution by an **AI CLI Agent** (such as Claude Code, Cursor, Aider, Antigravity CLI or Gemini CLI) to build the complete **Ledger** software system phase by phase, while simultaneously detailing **manual steps** that require human student execution.

### 1.2 Urgent Academic Priority: Early Frontend Delivery
Under KTU CSD415 interim milestone requirements, the project team must submit graphical user interface (UI) pictures and design progress to faculty this week. Consequently, **Phase 2 of this PRD delivers a fully functional, high-fidelity React.js frontend with an autonomous Mock Verification Engine**. This enables the student team to launch the frontend immediately, trigger realistic visual veracity classifications with color-coded LIME word-attribution heatmaps and capture high-resolution screenshots for academic submission prior to heavy model training.

### 1.3 System Overview
Ledger is an Explainable AI (XAI) web application for real-time fake news verification. Unlike black-box models or transformer networks that demand costly GPU hardware and incur high latency, Ledger pairs lightweight **FastText** subword embeddings with a **Hybrid 1D-CNN and LSTM** neural network, interpreted through **LIME** (Local Interpretable Model-agnostic Explanations). The system operates on standard CPU hardware with sub-second inference, providing everyday citizens, journalists and fact-checkers with transparent word-level justifications for every classification.

```
+-----------------------------------------------------------------------------------+
|                                 LEDGER PIPELINE                                   |
|                                                                                   |
|  [User News Text]                                                                 |
|         |                                                                         |
|         v                                                                         |
|  [1. Regex Cleaning & Tokenization]                                               |
|         |                                                                         |
|         v                                                                         |
|  [2. FastText Subword Embeddings (300-d, OOV & Slang Resilient)]                  |
|         |                                                                         |
|         v                                                                         |
|  [3. 1D-CNN Feature Extraction (Kernels 4 & 3, Salient Local N-Grams)]            |
|         |                                                                         |
|         v                                                                         |
|  [4. LSTM Sequence Learning (50 & 30 units, Temporal Context Dependencies)]       |
|         |                                                                         |
|         +-------------------------------------+                                   |
|         |                                     |                                   |
|         v                                     v                                   |
|  [5. Dense Softmax Classification]    [6. LIME Post-Hoc Local Explainer]          |
|    - Label: Real (1) / Fake (0)         - Perturbation sampling                   |
|    - Calibrated Confidence Score        - Word-level attribution weights          |
|         |                                     |                                   |
|         +------------------+------------------+                                   |
|                            |                                                      |
|                            v                                                      |
|  [7. React.js Interactive Dashboard]                                              |
|    - Verdict Badge (Real vs Fake)                                                 |
|    - Confidence Gauge Bar                                                         |
|    - Dynamic Color-Coded Word Heatmap (Green = Credible, Red = Deceptive)         |
+-----------------------------------------------------------------------------------+
```

---

## 2. System Architecture & Formal Design Specifications

The software architecture strictly adheres to Chapter 4 (System Design) and Chapter 5 (Software and Hardware Requirements) of the approved academic report.

### 2.1 Multi-Tier Architecture Mapping
1. **Presentation Tier (Frontend):**
   - Framework: React.js (v18+) with Vite, Tailwind CSS and Lucide React icons.
   - Purpose: Single-page application (SPA) providing news text input, preset sample loading, asynchronous API dispatch, responsive verdict rendering and interactive word-attribution heatmaps.
   - Built-in Mock Adapter: Enables offline demonstration and snapshot capture without backend dependencies.

2. **Application Tier (Backend API):**
   - Framework: Python FastAPI with Uvicorn ASGI server and Pydantic v2 schemas.
   - Purpose: Exposes stateless REST endpoints (`/api/v1/predict`, `/api/v1/explain`, `/api/v1/sample-news`, `/health`). Manages asynchronous request queuing, text preprocessing orchestration, inference caching and error handling.

3. **Core AI & Machine Learning Engine Tier:**
   - NLP Preprocessing: Python RegEx, NLTK and SpaCy for normalization, lowercasing, ASCII cleaning and lemmatization.
   - Embeddings: FastText (Meta AI) 300-dimensional subword character n-gram vectors (range 3 to 6 characters).
   - Deep Learning Classifier: PyTorch (or TensorFlow Keras) implementing two Conv1D layers (64 filters, kernel sizes 4 and 3, ReLU, L2 regularization lambda = 0.01), MaxPooling1D, two LSTM layers (50 and 30 memory units, L2 regularization lambda = 0.01), Dropout (0.3), Dense(16) and Dense Softmax(2).
   - Explainability Engine: LIME (`LimeTextExplainer`) generating localized perturbation surrogate models to output signed attribution weights for constituent words.

### 2.2 System Design Diagrams Reference
- **System Architecture Diagram:** Mapped to Figure 4.1 in report (`ArchitectureDiagram.drawio.png`).
- **Data Flow Diagram Level 0 (Context Diagram):** Mapped to Figure 4.2 in report (`DFD_0.drawio.png`). External entities: End User / Fact-Checker and Machine Learning Engineer.
- **Data Flow Diagram Level 1:** Mapped to Figure 4.3 in report (`DFD_1.drawio.png`). Processes: 1.0 Preprocess Text, 2.0 Generate Embeddings, 3.0 Classify News and 4.0 Generate Explanation.
- **UML Sequence Diagram:** Mapped to Figure 4.4 in report (`uml_sequence_diagram.png`). Six lifelines: User, React Frontend, FastAPI Backend, Preprocessor & FastText Vectorizer, CNN-LSTM Classifier and LIME Explainer.

---

## 3. Master Repository Structure

The AI CLI Agent must create and maintain the following directory layout:

```
Ledger/
├── .env.example
├── .gitignore
├── README.md
├── PRD.md
├── docker-compose.yml
├── frontend/
│   ├── package.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   ├── index.html
│   ├── public/
│   │   └── favicon.ico
│   └── src/
│       ├── main.jsx
│       ├── App.jsx
│       ├── index.css
│       ├── components/
│       │   ├── Navbar.jsx
│       │   ├── HeroSection.jsx
│       │   ├── VerificationForm.jsx
│       │   ├── VerdictCard.jsx
│       │   ├── LimeHeatmap.jsx
│       │   ├── FeatureImportanceChart.jsx
│       │   ├── ModelMetricsModal.jsx
│       │   └── Footer.jsx
│       ├── services/
│       │   ├── api.js
│       │   └── mockData.js
│       └── utils/
│           └── textHighlighter.js
├── backend/
│   ├── requirements.txt
│   ├── Dockerfile
│   ├── main.py
│   ├── app/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   ├── routes.py
│   │   │   └── schemas.py
│   │   ├── core/
│   │   │   ├── __init__.py
│   │   │   ├── preprocessor.py
│   │   │   ├── vectorizer.py
│   │   │   ├── classifier.py
│   │   │   └── explainer.py
│   │   └── data/
│   │       └── sample_articles.json
│   └── models/
│       ├── .gitkeep
│       └── weights_metadata.json
├── model_pipeline/
│   ├── requirements-training.txt
│   ├── 01_download_data.py
│   ├── 02_preprocess_corpus.py
│   ├── 03_fasttext_embeddings.py
│   ├── 04_train_cnn_lstm.py
│   ├── 05_evaluate_benchmarks.py
│   ├── 06_export_model.py
│   └── notebooks/
│       └── Ledger_Colab_Training_WELFake.ipynb
└── docs/
    ├── KTU_Project_Diary_Template.md
    └── Screenshot_Guide_For_Evaluation.md
```

---

## 4. Phase-by-Phase Implementation Roadmap

```
+------------------------------------------------------------------------------------+
|                               PHASE EXECUTION ORDER                                |
|                                                                                    |
|  [Phase 1: Environment Scaffolding & Config]                                       |
|         |                                                                          |
|         v                                                                          |
|  [Phase 2: High-Fidelity React Frontend + Mock Engine]  <-- CRITICAL FOR PICTURES  |
|         |                                                                          |
|         v                                                                          |
|  [Phase 3: FastAPI Backend & API Contracts]                                        |
|         |                                                                          |
|         v                                                                          |
|  [Phase 4: NLP Preprocessing & FastText Subword Pipeline]                          |
|         |                                                                          |
|         v                                                                          |
|  [Phase 5: Hybrid CNN-LSTM Architecture & Model Training]                          |
|         |                                                                          |
|         v                                                                          |
|  [Phase 6: LIME Post-Hoc Explainability Engine]                                    |
|         |                                                                          |
|         v                                                                          |
|  [Phase 7: End-to-End System Integration & Live Verification]                       |
|         |                                                                          |
|         v                                                                          |
|  [Phase 8: Evaluation, Benchmarking & KTU Phase 1 Compliance]                      |
+------------------------------------------------------------------------------------+
```

---

### Phase 1: Environment Scaffolding, Repository Setup and Configuration

#### 1. Goal
Initialize the clean repository architecture, establish Git version control ignores, set up environment configuration files and configure package definitions for both backend and frontend.

#### 2. AI CLI Agent Actions
1. Initialize repository root directory with `.gitignore`:
   - Exclude `node_modules/`, `dist/`, `build/`, `__pycache__/`, `.venv/`, `*.pyc`, `*.h5`, `*.pt`, `*.bin`, `.env`.
2. Generate root `.env.example`:
   ```env
   # Backend Environment
   PORT=8000
   HOST=0.0.0.0
   ENVIRONMENT=development
   FASTTEXT_MODEL_PATH=models/fasttext_dim300.bin
   CLASSIFIER_MODEL_PATH=models/cnn_lstm_welfake.pt
   USE_MOCK_FALLBACK=true

   # Frontend Environment
   VITE_API_BASE_URL=http://localhost:8000/api/v1
   VITE_ENABLE_MOCK_MODE=true
   ```
3. Create root `README.md` providing system description, college metadata and quick-start instructions.

#### 3. Step-by-Step Manual Actions for Student
1. Open PowerShell or Terminal in the root folder: `cd C:\Users\aravindanirudh\Downloads\ProjectPhase1`.
2. Verify Python version is 3.10 or higher:
   ```powershell
   python --version
   ```
3. Verify Node.js version is 18 or higher:
   ```powershell
   node --version
   npm --version
   ```
4. If missing, install Python from [python.org](https://www.python.org/) and Node.js LTS from [nodejs.org](https://nodejs.org/).

#### 4. Acceptance Criteria
- Repository folder hierarchy strictly matches the master layout in Section 3.
- `.gitignore` and `.env.example` exist in root.

---

### Phase 2: High-Fidelity React Frontend & Interactive Dashboard (EARLY SUBMISSION PHASE)

> [!IMPORTANT]
> **Priority Phase for Immediate Academic Evaluation:**  
> The student team must submit frontend user interface pictures to faculty this week. This phase constructs the complete, production-grade React web dashboard with an embedded **Mock Verification Engine** (`mockData.js`). It allows full interactive operation, instant verdict switching and realistic LIME heatmap rendering without needing the backend or trained weights running.

#### 1. Goal
Construct a modern, responsive web application for Ledger featuring text input, preset article loading, animated evaluation feedback, verdict summary cards with confidence meters, interactive word-level attribution heatmaps and model benchmark modals.

#### 2. Tech Stack for Frontend
- **Bundler:** Vite
- **Framework:** React 18
- **Styling:** Tailwind CSS with `@tailwindcss/typography`
- **Icons:** Lucide React (`lucide-react`)
- **Visuals:** Clean academic modern aesthetic (Slate, Indigo, Emerald, Rose color palette)

#### 3. AI CLI Agent Actions

##### A. Scaffold Vite React Project
In `frontend/`:
1. Initialize `package.json` with dependencies:
   - `react`, `react-dom`, `lucide-react`, `clsx`, `tailwind-merge`
   - Dev dependencies: `vite`, `@vitejs/plugin-react`, `tailwindcss`, `postcss`, `autoprefixer`
2. Configure `tailwind.config.js` with custom color tokens:
   - `verdict-real`: `#059669` (Emerald 600)
   - `verdict-fake`: `#e11d48` (Rose 600)
   - `highlight-green`: `rgba(16, 185, 129, 0.25)`
   - `highlight-red`: `rgba(244, 63, 94, 0.25)`
3. Set up `vite.config.js` with local server proxy to `http://localhost:8000`.

##### B. Implement Mock Service (`frontend/src/services/mockData.js`)
Build realistic mock responses based directly on the base paper examples:
1. **Sample 1: Verified Real News (Political Reporting)**
   - Text: *"Dr. Ben Carson tells the story of what happened when he spoke out against Obama administration healthcare policies during the National Prayer Breakfast."*
   - Verdict: `Real`
   - Confidence: `98.6%`
   - Attribution: Words like `spoke`, `breakfast`, `reporting`, `prayer` receive positive weights (supporting authenticity).
2. **Sample 2: Fabricated Fake News (Conspiracy & Sensationalist)**
   - Text: *"Secret government memo leaks confirming administration rules dismantling healthcare reform and pushing massive tax penalties early."*
   - Verdict: `Fake`
   - Confidence: `99.2%`
   - Attribution: Words like `secret`, `dismantling`, `penalties`, `leaks` receive strong negative weights (indicating fake).
3. **Sample 3: Sensational Social Media Claim (Clickbait / Medical Myth)**
   - Text: *"Doctors discover miracle cure for all viral diseases using household spice combination that pharmaceutical giants tried to ban."*
   - Verdict: `Fake`
   - Confidence: `97.8%`
   - Attribution: `miracle`, `cure`, `giants`, `ban`, `secret` highlighted in deep red.

##### C. Implement Modular Frontend Components
1. **`Navbar.jsx`:**
   - Display brand: **Ledger** with a verified shield logo.
   - Subtitle: *"AI-Powered Fake News Detection using Explainable AI (XAI)"*.
   - Academic badge: *"KTU S7 Project - Group 12 | CEC"*.
   - Status badge indicating active model: *"FastText + CNN-LSTM + LIME (WELFake 99% Acc)"*.
   - Mock Mode Indicator toggle switch (Live API vs Mock Mode).

2. **`HeroSection.jsx`:**
   - Highlighting project purpose: *"Transparent, real-time news authenticity verification powered by subword embeddings and Explainable AI."*
   - Three quick statistic cards:
     - **99.0%** Detection Accuracy on WELFake Corpus
     - **< 800 ms** Sub-second CPU Inference Latency
     - **100%** Explainable with Token-Level Attribution

3. **`VerificationForm.jsx`:**
   - Textarea for full article body or headline (placeholder with realistic news passage).
   - Character count and token counter.
   - Preset loader dropdown: *"Load Example: Real News Passage"*, *"Load Example: Fabricated Article"*, *"Load Example: Health Misinformation"*.
   - Action buttons: "Verify Authenticity" (with spinning state) and "Clear Text".

4. **`VerdictCard.jsx`:**
   - Conditional color theme (Emerald for Real, Rose/Crimson for Fake).
   - Large verdict badge: `AUTHENTIC NEWS` or `DECEPTIVE / FAKE NEWS`.
   - Visual confidence gauge bar with numerical percentage.
   - Explanation summary box stating key findings.
   - Metadata line: Processing time (e.g. `142 ms`), Word count, Model architecture.

5. **`LimeHeatmap.jsx`:**
   - Interactive paragraph view where each token is rendered with a background highlight matching its LIME weight:
     - Green tones for words confirming credibility (`weight > 0`).
     - Red tones for words triggering deception flags (`weight < 0`).
     - Intensity scales with absolute attribution value.
   - Interactive hover tooltip on each word showing exact attribution score (e.g. `+0.342` or `-0.518`).
   - Legend explaining: Green = Evidence of Authenticity, Red = Evidence of Misinformation.

6. **`FeatureImportanceChart.jsx`:**
   - Horizontal bar chart rendering the Top 10 most influential words sorted by absolute LIME attribution score.
   - Green bars extending right for authentic terms, red bars extending left for fake indicators.

7. **`ModelMetricsModal.jsx`:**
   - Modal popup showing the comparative evaluation table from the base paper (WELFake, FakeNewsNet, LIAR performance metrics) and the neural architecture summary (FastText 300d -> 1D-CNN -> LSTM -> LIME).

8. **`Footer.jsx`:**
   - Academic citation to Hashmi et al. (IEEE Access, 2024), Department of CSE, College of Engineering Cherthala, Project Guide: Mrs. Rajeswary R.

#### 4. Step-by-Step Manual Actions for Student (HOW TO RUN & TAKE SCREENSHOTS THIS WEEK)
1. Open PowerShell and navigate to the frontend folder:
   ```powershell
   cd C:\Users\aravindanirudh\Downloads\ProjectPhase1\frontend
   ```
2. Install dependencies:
   ```powershell
   npm install
   ```
3. Start the Vite development server:
   ```powershell
   npm run dev
   ```
4. Open your browser and navigate to:
   ```
   http://localhost:5173
   ```
5. **Capturing Mandatory Submission Screenshots (Checklist):**
   - **Screenshot 1: Blank Input State**
     - Capture full dashboard view with empty textarea, preset buttons and header metadata badges.
   - **Screenshot 2: Real News Classification**
     - Select preset *"Sample 1: Verified Real News"*.
     - Click *"Verify Authenticity"*.
     - Capture the Green "AUTHENTIC NEWS" verdict badge, 98.6% confidence bar and the green-tinted LIME word heatmap.
   - **Screenshot 3: Fake News Classification**
     - Select preset *"Sample 2: Fabricated Fake News"*.
     - Click *"Verify Authenticity"*.
     - Capture the Red "DECEPTIVE / FAKE NEWS" badge, 99.2% confidence bar and red-highlighted deceptive keywords (`dismantling`, `penalties`, `secret`).
   - **Screenshot 4: Interactive Word Attribution Tooltip**
     - Hover the mouse cursor over a highlighted word (e.g. `dismantling`) to reveal the tooltip displaying the numerical LIME score. Take screenshot using Windows Snipping Tool (`Win + Shift + S`).
   - **Screenshot 5: Top 10 Feature Importance Bar Chart**
     - Scroll to the horizontal bar chart showing positive and negative word attribution rankings.
   - **Screenshot 6: Model Architecture & Metrics Modal**
     - Click *"View Model Architecture & Benchmark Metrics"* in the navigation bar and capture the comparison table modal.
   - **Screenshot 7: Mobile Responsiveness View**
     - Press `F12` in Chrome, toggle device emulation (`Ctrl + Shift + M`), select iPhone 14 / Pixel 7 and take a mobile screenshot.
6. Save these screenshots into `docs/screenshots/` and insert them into your presentation slide deck (`presentation_code/`) and Phase 1 interim report.

#### 5. Acceptance Criteria
- Running `npm run dev` in `frontend/` launches with zero errors.
- Switching between sample presets instantly updates the verification card, heatmap and bar chart.
- The interface is aesthetically polished and fully responsive on desktop and mobile.

---

### Phase 3: Backend API Architecture & REST Contracts (FastAPI)

#### 1. Goal
Construct a robust, asynchronous Python FastAPI server implementing strict Pydantic request and response schemas, CORS middleware, input sanitization, health checks and seamless integration with the Phase 2 frontend.

#### 2. Tech Stack for Backend
- **Framework:** FastAPI (v0.100+)
- **Server:** Uvicorn (ASGI)
- **Validation:** Pydantic v2
- **Utilities:** Python-dotenv, PyYAML

#### 3. AI CLI Agent Actions
1. In `backend/`, create `requirements.txt`:
   ```txt
   fastapi>=0.104.0
   uvicorn[standard]>=0.24.0
   pydantic>=2.4.0
   python-dotenv>=1.0.0
   numpy>=1.24.0
   requests>=2.31.0
   ```
2. Implement `app/api/schemas.py`:
   ```python
   from pydantic import BaseModel, Field
   from typing import List, Dict, Optional

   class VerificationRequest(BaseModel):
       text: str = Field(..., min_length=10, max_length=15000, description="News text or headline to verify")
       title: Optional[str] = Field(None, max_length=500, description="Optional article title")
       num_features: Optional[int] = Field(10, ge=5, le=30, description="Top influential LIME words to return")

   class WordAttribution(BaseModel):
       word: str
       weight: float
       is_deceptive: bool

   class VerificationResponse(BaseModel):
       verdict: str  # "Real" or "Fake"
       label: int    # 1 for Real, 0 for Fake
       confidence: float
       probabilities: Dict[str, float]
       explanation: List[WordAttribution]
       raw_tokens: List[str]
       execution_time_ms: float
       model_version: str

   class SampleNewsResponse(BaseModel):
       id: str
       title: str
       text: str
       expected_verdict: str
   ```
3. Implement `app/api/routes.py`:
   - `GET /health`: Returns service health, uptime, active model version and device (CPU/CUDA).
   - `GET /api/v1/samples`: Returns curated benchmark sample articles.
   - `POST /api/v1/predict`: Executes classification and returns verdict with confidence.
   - `POST /api/v1/explain`: Executes classification and runs LIME to return word attributions.
   - Built-in Fallback Handler: If model weights are not yet present in `backend/models/`, routes return deterministic heuristic analysis to allow continuous frontend-backend development.
4. Implement `app/config.py` for environment variable loading.
5. Implement `main.py` configuring CORS middleware allowing requests from `http://localhost:5173` and `http://127.0.0.1:5173`.

#### 4. Step-by-Step Manual Actions for Student
1. Open PowerShell and navigate to the backend folder:
   ```powershell
   cd C:\Users\aravindanirudh\Downloads\ProjectPhase1\backend
   ```
2. Create a Python virtual environment:
   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```
3. Install backend dependencies:
   ```powershell
   pip install -r requirements.txt
   ```
4. Launch the FastAPI server:
   ```powershell
   uvicorn main:app --reload --port 8000
   ```
5. Open browser at `http://localhost:8000/docs` to test interactive Swagger API documentation.

#### 5. Acceptance Criteria
- Server starts cleanly at `http://localhost:8000`.
- Swagger UI at `/docs` correctly validates incoming requests and returns structured JSON responses matching `VerificationResponse`.
- Frontend connects to `http://localhost:8000/api/v1` when `VITE_ENABLE_MOCK_MODE=false`.

---

### Phase 4: NLP Preprocessing & FastText Subword Embedding Pipeline

#### 1. Goal
Implement the exact text cleaning and subword representation algorithms defined in Section III-B and III-C of the base paper, handling out-of-vocabulary terms and noisy internet text through character n-grams.

#### 2. Theoretical Specifications from Base Paper
- **Preprocessing Routine:**
  1. Lowercase conversion.
  2. Removal of non-essential characters (ASCII symbols, punctuation, brackets).
  3. Python RegEx filtering for numbers, email addresses, URLs and phone patterns.
  4. Deduplication of records.
  5. Lemmatization using NLTK WordNetLemmatizer to reduce inflections to root forms.
  6. Whitespace normalization.
- **FastText Subword Embeddings:**
  - FastText represents words as bags of character n-grams from length 3 to 6 characters:
    $$\vec{u}_w + \frac{1}{|N|} \sum_{n \in N} \vec{x}_n$$
  - Subwords allow robust embedding generation for spelling mistakes, out-of-vocabulary words and digital slang without zero-vector mapping.
  - Dimension: 300 dimensions per token.
  - Pre-trained model: `cc.en.300.bin` (Common Crawl 2 million words, 600 billion tokens).

#### 3. AI CLI Agent Actions
1. In `backend/app/core/preprocessor.py`, implement class `NewsPreprocessor`:
   - Method `clean_text(text: str) -> str`: Executes regex filtering (stripping HTML, links, digits, punctuation, non-ASCII).
   - Method `lemmatize_text(text: str) -> str`: Splits into words, applies lemmatizer, recombines.
   - Method `preprocess(text: str) -> str`: Full composite pipeline.
2. In `backend/app/core/vectorizer.py`, implement class `FastTextVectorizer`:
   - Method `load_model(model_path: str)`: Loads binary FastText vector model.
   - Method `get_token_vectors(tokens: List[str]) -> np.ndarray`: Retrieves 300-d vectors for each token using subwords.
   - Method `pad_sequences(sequences: np.ndarray, max_len: int = 500) -> np.ndarray`: Pads or truncates to fixed input shape `(500, 300)`.
   - Fallback generator: Includes a lightweight 300-d FastText simulation matrix for testing before the large binary file is downloaded.
3. In `model_pipeline/02_preprocess_corpus.py`:
   - Batch script to preprocess the 72,134 records of WELFake CSV, creating `cleaned_welfake.csv`.

#### 4. Step-by-Step Manual Actions for Student
1. Install NLP requirements in virtual environment:
   ```powershell
   pip install nltk spacy fasttext-wheel
   python -c "import nltk; nltk.download('wordnet'); nltk.download('omw-1.4'); nltk.download('stopwords')"
   ```
2. **Acquiring FastText Embeddings:**
   - Pre-trained full model `cc.en.300.bin` is ~4.5 GB.
   - For fast local testing, download the compressed vector text file `crawl-300d-2M-subword.zip` or the official FastText English model from [fasttext.cc](https://fasttext.cc/docs/en/english-vectors.html).
   - Alternatively, use the helper script `model_pipeline/03_fasttext_embeddings.py --download-mini` created by the agent to download a pre-filtered vocabulary vector file (~120 MB) suited for development.
3. Place vector file in `backend/models/fasttext_dim300.bin`.

#### 5. Acceptance Criteria
- `NewsPreprocessor.preprocess("DR. CARSON'S 2026 SPEECH!!! https://news.com")` outputs clean, lemmatized lowercase text without URLs, digits or punctuation.
- Out-of-vocabulary slang words (e.g. `obamacarewacko`, `covidmisinfo`) generate non-zero 300-dimensional subword vectors.

---

### Phase 5: Hybrid CNN-LSTM Deep Learning Architecture & Training Pipeline

#### 1. Goal
Construct, train and serialize the hybrid 1D-CNN and LSTM deep learning neural network as specified in Section III-D and Figure 3 of the base paper, achieving benchmark accuracy on the WELFake dataset.

#### 2. Exact Network Architecture from Base Paper (Figure 3 & Table 7)
- **Input Layer:** Tensor of shape `(batch_size, max_sequence_length=500, embedding_dim=300)`.
- **Convolutional Segment (Local N-Gram Feature Extraction):**
  - **Conv1D Layer 1:** 64 filters, kernel size = 4, activation = ReLU, kernel L2 regularization ($\lambda = 0.01$).
  - **Conv1D Layer 2:** 64 filters, kernel size = 3, activation = ReLU, kernel L2 regularization ($\lambda = 0.01$).
  - **MaxPooling1D Layer:** Pool size = 2 (halving temporal dimension).
  - **Dropout Layer:** Rate = 0.3.
- **Recurrent Segment (Long-Range Sequential Context Learning):**
  - **LSTM Layer 1:** 50 hidden units, return sequences = True, kernel L2 regularization ($\lambda = 0.01$).
  - **LSTM Layer 2:** 30 hidden units, return sequences = False, kernel L2 regularization ($\lambda = 0.01$).
  - **Dropout Layer:** Rate = 0.3.
- **Dense Classification Segment:**
  - **Dense Layer:** 16 hidden units, activation = ReLU.
  - **Output Dense Layer:** 2 units (Class 0: Fake, Class 1: Real), activation = Softmax.
- **Training Hyperparameters:**
  - Optimizer: Adam (initial learning rate = 0.001)
  - Loss Function: Categorical Cross-Entropy
  - Batch Size: 64
  - Training Epochs: 10
  - Regularization: L2 regularization strictly favored over L1 to prevent underfitting and maintain smooth weight distributions.

#### 3. AI CLI Agent Actions
1. In `backend/app/core/classifier.py`:
   - Implement the PyTorch model definition `HybridCnnLstmLedger(nn.Module)` matching the exact layers, kernel sizes and filter dimensions.
   - Implement method `predict(tensor: torch.Tensor) -> Tuple[int, float, Dict[str, float]]`: Takes padded 300-d vector tensor and outputs predicted class, confidence and probability distribution.
2. In `model_pipeline/04_train_cnn_lstm.py`:
   - Standalone training script with train/validation/test split (70% train, 15% validation, 15% test).
   - Computes validation accuracy and categorical cross-entropy loss per epoch.
   - Saves model checkpoint with best validation loss to `models/cnn_lstm_welfake.pt`.
3. In `model_pipeline/notebooks/Ledger_Colab_Training_WELFake.ipynb`:
   - Generate a turnkey Jupyter Notebook for Google Colab with GPU support.
   - Cells: Kaggle dataset auto-download -> Preprocessing -> FastText embedding generation -> CNN-LSTM training with GPU -> Metrics plotting (accuracy and loss curves) -> Checkpoint export.

#### 4. Step-by-Step Manual Actions for Student (MODEL TRAINING WORKFLOW)
1. **Option A: Training in Google Colab (Recommended for Free T4 GPU Acceleration):**
   - Open Google Colab at [colab.research.google.com](https://colab.research.google.com/).
   - Click *Upload Notebook* and select `model_pipeline/notebooks/Ledger_Colab_Training_WELFake.ipynb`.
   - Go to *Runtime* -> *Change runtime type* -> Select **T4 GPU**.
   - Upload your `kaggle.json` API token (from your Kaggle account settings: *Create New Token*).
   - Run all cells: The notebook downloads WELFake (72,134 records), preprocesses the text, trains the CNN-LSTM for 10 epochs (taking ~5 minutes per epoch as reported in the base paper) and plots loss/accuracy curves.
   - Download the generated `cnn_lstm_welfake.pt` and place it in your local `backend/models/` folder.
2. **Option B: Local Training (If NVIDIA GPU is available):**
   ```powershell
   cd C:\Users\aravindanirudh\Downloads\ProjectPhase1\model_pipeline
   python 01_download_data.py
   python 04_train_cnn_lstm.py --epochs 10 --batch-size 64
   ```

#### 5. Acceptance Criteria
- Model successfully compiles and validates forward and backward passes.
- Verification on test split demonstrates accuracy exceeding 96% on WELFake corpus.
- Model checkpoint serialized to `backend/models/cnn_lstm_welfake.pt` (< 50 MB, highly compact for CPU hosting).

---

### Phase 6: Explainable AI Attribution Engine (LIME)

#### 1. Goal
Integrate Local Interpretable Model-agnostic Explanations (LIME) according to Section VI-B and Equation 12 of the base paper to produce word-level attribution scores that explain individual predictions.

#### 2. Theoretical Specifications from Base Paper
- LIME identifies an interpretable surrogate model $\hat{g} \in G$ that minimizes local fidelity loss $\mathcal{L}$ under proximity measure $\pi_x$:
  $$\hat{g} = \arg\min_{g \in G} \mathcal{L}(f, g, \pi_x) + \Omega(g)$$
- Text explainer generates perturbed variants of the input text by randomly masking constituent words.
- The CNN-LSTM model classifies each perturbed variant to observe probability deviations.
- A sparse linear model (Lasso) is fitted locally to assign a signed numerical weight to each word:
  - Positive weight: Word shifts prediction towards **Real** news.
  - Negative weight: Word shifts prediction towards **Fake** news.

#### 3. AI CLI Agent Actions
1. In `backend/app/core/explainer.py`, implement class `LedgerLimeExplainer`:
   - Initialize `lime.lime_text.LimeTextExplainer(class_names=['Fake', 'Real'], split_expression=r'\W+')`.
   - Implement `classifier_predict_proba(texts: List[str]) -> np.ndarray`:
     - Pipeline function that takes a batch of raw text strings from LIME, passes each through `NewsPreprocessor`, vectorizes via `FastTextVectorizer`, runs inference through `HybridCnnLstmLedger` and returns an `(N, 2)` matrix of Softmax probabilities.
   - Implement `explain_instance(text: str, num_features: int = 10) -> List[WordAttribution]`:
     - Calls `explainer.explain_instance(text, classifier_predict_proba, num_features=num_features, num_samples=100)`.
     - Extracts word-level tuples `(word, weight)` and serializes them for the API.
   - Caching & Optimization: For latency protection on standard CPU, cap `num_samples` at 100 to ensure explanation generation completes in under 1.5 seconds.

#### 4. Step-by-Step Manual Actions for Student
1. Install LIME in the backend environment:
   ```powershell
   pip install lime scikit-learn
   ```
2. Verify explainer operation using the standalone test script:
   ```powershell
   python -m app.core.explainer
   ```

#### 5. Acceptance Criteria
- For a sample text, LIME successfully returns signed attribution weights for the top words without memory leaks.
- Total explanation latency remains under 2 seconds on a 4-core CPU host.

---

### Phase 7: End-to-End System Integration & Live Verification

#### 1. Goal
Unify the React frontend, FastAPI backend, FastText embeddings, CNN-LSTM classifier and LIME explainer into a seamless, interconnected live application.

#### 2. AI CLI Agent Actions
1. In `frontend/src/services/api.js`:
   - Implement Axios client pointing to `VITE_API_BASE_URL`.
   - Add graceful fallback: If the live backend is unreachable or returns 500, log error and automatically switch to `mockData.js` so the user interface never crashes.
2. In `backend/main.py`:
   - On startup (`@app.on_event("startup")`), load FastText vectors and trained CNN-LSTM weights into resident memory.
   - Provide `/api/v1/verify` composite endpoint that runs preprocessing, neural inference and LIME explanation in a single call.
3. Create root `docker-compose.yml`:
   - Service `backend`: Python FastAPI container listening on port 8000.
   - Service `frontend`: Node/Nginx container serving React application on port 5173.

#### 3. Step-by-Step Manual Actions for Student
1. Launch both services simultaneously in separate PowerShell windows:
   - **Terminal 1 (Backend):**
     ```powershell
     cd C:\Users\aravindanirudh\Downloads\ProjectPhase1\backend
     .\.venv\Scripts\Activate.ps1
     uvicorn main:app --reload --port 8000
     ```
   - **Terminal 2 (Frontend):**
     ```powershell
     cd C:\Users\aravindanirudh\Downloads\ProjectPhase1\frontend
     npm run dev
     ```
2. In browser at `http://localhost:5173`, toggle the **Live API** switch in the navbar.
3. Paste an unverified news article from the web and click *"Verify Authenticity"*.
4. Inspect live console logs in both terminals to observe request flow.

#### 4. Acceptance Criteria
- Submitting an article through the browser triggers live inference in the backend.
- The UI displays the live verdict badge, confidence score and word attribution heatmap with sub-second response times.

---

### Phase 8: Benchmarking, Validation & KTU Phase 1 Deliverables

#### 1. Goal
Generate quantitative evaluation figures, confusion matrices, latency benchmarks and compile documentation required for KTU CSD415 Phase 1 project evaluation rubrics.

#### 2. Evaluation Metrics to Verify (Equations 6-9 from Base Paper)
- $\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}$
- $\text{Precision} = \frac{TP}{TP + FP}$
- $\text{Recall} = \frac{TP}{TP + FN}$
- $\text{F1-Score} = \frac{2 \cdot \text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}$

#### 3. AI CLI Agent Actions
1. In `model_pipeline/05_evaluate_benchmarks.py`:
   - Generates quantitative classification reports on test splits.
   - Exports high-resolution matplotlib figures matching the base paper:
     - `training_validation_loss.png` (matching Figure 4/7)
     - `training_validation_accuracy.png` (matching Figure 5/8)
     - `confusion_matrix.png` (matching Figure 6/9)
2. In `docs/KTU_Project_Diary_Template.md`:
   - Formats a 14-week daily logbook template for the team (Group 12) adhering to KTU evaluation guidelines (where Student Daily Diary carries 7 marks).
3. In `docs/Screenshot_Guide_For_Evaluation.md`:
   - Documents an annotated screenshot gallery mapping each UI view to its corresponding presentation slide.

#### 4. Step-by-Step Manual Actions for Student (KTU SUBMISSION PREPARATION)
1. Run benchmark script:
   ```powershell
   python model_pipeline/05_evaluate_benchmarks.py
   ```
2. Review generated confusion matrix and verify precision/recall metrics.
3. Copy generated figures to your presentation slide directory:
   ```powershell
   copy model_pipeline/outputs/*.png presentation_code/diagrams/
   ```
4. Print and maintain your **Student Project Diary / Daily Log**, filling in weekly entries using `docs/KTU_Project_Diary_Template.md` to secure full 7 marks during guide evaluation.

#### 5. Acceptance Criteria
- Test accuracy matches the performance profile established in the base paper (> 96% on WELFake).
- All evaluation figures exported and ready for inclusion in the final presentation and report.

---

## 5. Comprehensive Manual Actions Guide for the Student Team

This section aggregates every manual task that the student team (Anandhakrishnan S, Aravind A Kamath, Arjun Manoj, Sangeeth Krishnan S) must perform outside the automated AI CLI Agent.

```
+-----------------------------------------------------------------------------------+
|                           STUDENT MANUAL ACTION CHECKLIST                         |
|                                                                                   |
|  [ ] 1. Install Node.js 18+ and Python 3.10+ locally.                             |
|  [ ] 2. Run Phase 2 frontend (`npm run dev`) and capture screenshots THIS WEEK.   |
|  [ ] 3. Create Kaggle account & download WELFake dataset (`WELFake_Dataset.csv`). |
|  [ ] 4. Download FastText embeddings or use the lightweight development vectors.  |
|  [ ] 5. Run Google Colab training notebook if free GPU training is desired.       |
|  [ ] 6. Place trained `cnn_lstm_welfake.pt` into `backend/models/`.               |
|  [ ] 7. Fill out weekly entries in the KTU Project Daily Diary (7 marks).         |
|  [ ] 8. Review LaTeX slides in `presentation_code/main.tex` with captured photos. |
+-----------------------------------------------------------------------------------+
```

### Detailed Manual Steps

#### Step 1: Toolchain Installation
- Download and install **Python 3.10+** from python.org (ensure "Add Python to PATH" is checked).
- Download and install **Node.js LTS (v18 or v20)** from nodejs.org.
- Verify in PowerShell: `python --version` and `node --version`.

#### Step 2: Immediate Screenshot Capture for Weekly Evaluation
- Navigate to `frontend/`:
  ```powershell
  cd C:\Users\aravindanirudh\Downloads\ProjectPhase1\frontend
  npm install
  npm run dev
  ```
- Use Windows Snipping Tool (`Win + Shift + S`) to capture:
  1. Main dashboard with hero section and empty input.
  2. Real News verdict with green badge and green LIME attribution.
  3. Fake News verdict with red badge and red LIME attribution.
  4. Hover tooltip showing word score.
  5. Top 10 Feature Importance horizontal chart.
  6. Architecture & benchmark metrics popup.
- Save pictures into your project presentation folder for faculty review this week.

#### Step 3: Kaggle Account & Dataset Retrieval
- Visit [kaggle.com](https://www.kaggle.com/) and create an account if you do not have one.
- Go to Account Settings -> API -> Click *Create New API Token*. This downloads `kaggle.json`.
- Place `kaggle.json` into `~/.kaggle/kaggle.json` (on Windows: `C:\Users\<username>\.kaggle\kaggle.json`).
- Download the WELFake dataset directly via Kaggle CLI:
  ```powershell
  kaggle datasets download -d saurabhshahane/classification-of-welfake-dataset
  ```
- Extract `WELFake_Dataset.csv` into `model_pipeline/data/`.

#### Step 4: Acquiring FastText Embeddings
- For production: Download `cc.en.300.bin` from [fasttext.cc](https://fasttext.cc/docs/en/english-vectors.html).
- For quick local testing: Run the AI agent's mini-vector extractor script to build a compact 300-d vector subset from the vocabulary of the sample articles.

#### Step 5: Executing Colab Training (Free T4 GPU)
- Upload `model_pipeline/notebooks/Ledger_Colab_Training_WELFake.ipynb` to Google Colab.
- Change runtime to **T4 GPU**.
- Run all cells. Once training completes (10 epochs), download `cnn_lstm_welfake.pt`.
- Place the file in `C:\Users\aravindanirudh\Downloads\ProjectPhase1\backend\models\`.

#### Step 6: Maintaining the KTU Project Daily Diary / Activity Log
- Under the KTU CSD415 syllabus, **7 marks out of 30** in the guide evaluation are dedicated strictly to the Student Daily Diary / Activity Log.
- Open `docs/KTU_Project_Diary_Template.md` and ensure each member logs weekly entries detailing:
  - Anandhakrishnan S: Preprocessing, FastText embedding integration, data pipeline.
  - Aravind A Kamath: Hybrid CNN-LSTM neural architecture design and PyTorch modeling.
  - Arjun Manoj: LIME Explainable AI engine implementation and attribution extraction.
  - Sangeeth Krishnan S: React frontend dashboard, visual heatmap component and FastAPI integration.
- Have your Project Guide (**Mrs. Rajeswary R**) review and sign your physical or digital diary regularly.

---

## 6. Strict AI CLI Agent Operating Rules & Constraints

When an AI CLI agent executes this PRD, it must strictly obey the following behavioral rules:

1. **Zero Em-Dashes:** Never use em-dashes in generated text or code comments. Use standard hyphens or rephrase sentences cleanly.
2. **Zero Oxford Commas:** Never place a comma before 'and' in lists. Format all serial lists as `A, B and C`.
3. **Faculty & Guide Name Integrity:** Always preserve the guide name as `Mrs. Rajeswary R`. Never alter or truncate it.
4. **Early Frontend Delivery:** The frontend in Phase 2 must be completely independent of backend readiness via the built-in `mockData.js` engine. It must run immediately upon `npm run dev`.
5. **No Breaking Modifications:** Never overwrite or delete existing LaTeX code in `presentation_code/main.tex` or `report_code/main.tex` without explicit user confirmation.
6. **Stateless Backend Design:** Do not introduce heavy relational databases (PostgreSQL/MySQL) unless explicitly requested; the inference pipeline operates in-memory over incoming request payloads.
7. **Cross-Platform Compatibility:** Ensure all file paths and PowerShell scripts operate cleanly on Windows 10/11 64-bit systems.

---

## 7. Verification & Acceptance Checklist

| Phase | Milestone | Validation Command / Verification Action | Status |
| :--- | :--- | :--- | :--- |
| **Phase 1** | Repository Scaffolding & Config | Verify `.env.example`, `.gitignore` and folder hierarchy exist | Pending |
| **Phase 2** | Interactive Frontend Dashboard | Run `cd frontend && npm install && npm run dev`; verify UI & mock modes | **CRITICAL THIS WEEK** |
| **Phase 3** | FastAPI REST Endpoints | Run `cd backend && uvicorn main:app --reload`; verify Swagger at `/docs` | Pending |
| **Phase 4** | FastText & NLP Preprocessor | Run `python -m app.core.preprocessor`; verify clean tokens & OOV vectors | Pending |
| **Phase 5** | Hybrid CNN-LSTM Neural Network | Run forward pass test in PyTorch; verify tensor output shape `(batch, 2)` | Pending |
| **Phase 6** | LIME Attribution Engine | Run `python -m app.core.explainer`; verify word attribution scores | Pending |
| **Phase 7** | Full Live System Integration | Toggle Live API in frontend; verify end-to-end verdict and heatmap | Pending |
| **Phase 8** | Benchmarking & KTU Deliverables | Generate confusion matrix; update weekly Student Daily Diary (7 marks) | Pending |

---
*End of Product Requirements Document (PRD.md)*
