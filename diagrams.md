# Project Diagrams: Ledger (AI-Based Fake News Detection System)

**Project Group 12** | College of Engineering, Cherthala  
**Base Paper:** *Advancing Fake News Detection: Hybrid Deep Learning With FastText and Explainable AI* (IEEE Access, 2024)

---

### Note on ER Diagram
* **Why an ER Diagram is NOT applicable:** Ledger is a stateless AI inference tool, not an OLTP database application. Users submit text, the model verifies it in-memory, and returns the result with LIME explanations. There are no relational tables (such as users, orders, carts, or transactions) to model.

---

## 1. High-Level System Architecture

A clean overview of the 3-tier system: React frontend, FastAPI backend, and the core AI engine.

```mermaid
flowchart TD
    User(["User / Fact-Checker"]) --> UI["Frontend UI\n(React.js)"]
    UI <-->|"REST API (JSON)"| API["Backend Server\n(FastAPI)"]
    
    subgraph AI_Engine ["Core AI Pipeline"]
        API --> PRE["Preprocessing\n(Clean & Lemmatize)"]
        PRE --> FT["FastText Embedding\n(300D Subwords)"]
        FT --> MODEL["CNN-LSTM Model\n(Feature Extraction & Sequence Learning)"]
        MODEL --> XAI["LIME Explainer\n(Word Importance Attribution)"]
    end
    
    XAI --> API
```

---

## 2. End-to-End System Pipeline Flowchart

Linear flow showing how input text travels from raw submission to the final highlighted report.

```mermaid
flowchart TD
    A["Raw News Article / Headline"] --> B["Text Preprocessing\n(Cleaning, Tokenization, Lemmatization)"]
    B --> C["FastText Embedding\n(300-dim Subword Vectors)"]
    C --> D["1D-CNN Layer\n(Extract Local n-gram Patterns)"]
    D --> E["LSTM Layer\n(Learn Sequential Context)"]
    E --> F["Softmax Classifier\n(Real vs Fake Probability)"]
    F --> G["LIME Explainer\n(Calculate Word Weights)"]
    G --> H["Output Result\n(Confidence Score & Highlighted Words)"]
```

---

## 3. Deep Learning Model Architecture (CNN-LSTM)

Layer-by-layer structure of the hybrid neural network as described in the base paper.

```mermaid
flowchart TD
    In["Input Tokens"] --> Emb["FastText Embedding Layer (300D)"]
    Emb --> Conv1["Conv1D Layer 1 (64 Filters, Kernel Size = 4)"]
    Conv1 --> Conv2["Conv1D Layer 2 (64 Filters, Kernel Size = 3)"]
    Conv2 --> Pool["1D Max Pooling Layer"]
    Pool --> Lstm1["LSTM Layer 1 (50 Units, Return Sequences)"]
    Lstm1 --> Lstm2["LSTM Layer 2 (30 Units)"]
    Lstm2 --> Dense["Dense Layer (Softmax)"]
    Dense --> Out["Output: Real / Fake (Confidence %)"]
```

---

## 4. Explainable AI (LIME) Workflow

Simple step-by-step mechanism of how LIME explains individual predictions.

```mermaid
flowchart LR
    A["Input Text"] --> B["Perturb Words\n(Randomly Mask Words)"]
    B --> C["Predict with\nCNN-LSTM"]
    C --> D["Fit Linear Surrogate\n(Weight by Proximity)"]
    D --> E["Extract Word Weights\n(Red: Fake | Green: Real)"]
```

---

## 5. Data Flow Diagram (DFD) - Level 0 (Context Diagram)

High-level boundary between external actors and the Ledger system.

```mermaid
flowchart LR
    User(["User / Fact-Checker"]) -->|"News Text"| System[["Ledger System"]]
    System -->|"Verdict, Confidence & Explanations"| User

    Admin(["ML Engineer / Admin"]) -->|"Training Datasets (WELFake)"| System
    System -->|"Accuracy & Loss Metrics"| Admin
```

---

## 6. Data Flow Diagram (DFD) - Level 1

Functional breakdown of data movement across preprocessing, embedding, model inference, and explainability.

```mermaid
flowchart LR
    User(["User"]) -->|"News Text"| P1["1.0 Preprocess Text"]
    P1 -->|"Tokens"| P2["2.0 Generate Embeddings"]
    DB1[("FastText Vectors")] --> P2
    P2 -->|"Vectors"| P3["3.0 Classify News"]
    DB2[("Model Weights")] --> P3
    P3 -->|"Prediction"| P4["4.0 Generate Explanation"]
    P4 -->|"Verdict & Highlighted Words"| User
```

---

## 7. UML Use Case Diagram

Primary use cases for the regular user and administrator.

```mermaid
flowchart LR
    subgraph UserCases ["User Actions"]
        UC1(["Enter News Text"])
        UC2(["Verify News Authenticity"])
        UC3(["View Confidence Score"])
        UC4(["View Highlighted Explanation"])
    end

    subgraph AdminCases ["Admin Actions"]
        UC5(["Train / Update Model"])
        UC6(["View Model Metrics"])
    end

    User(["User / Fact-Checker"]) --- UC1
    User --- UC2
    User --- UC3
    User --- UC4

    Admin(["ML Engineer / Admin"]) --- UC5
    Admin --- UC6
```

---

## 8. UML Sequence Diagram

Chronological request-response lifecycle from user interaction to backend inference and result display.

```mermaid
sequenceDiagram
    autonumber
    actor User as User / Fact-Checker
    participant Client as React.js Frontend
    participant API as FastAPI Backend
    participant NLP as Text Preprocessor & FastText
    participant Model as CNN-LSTM Model
    participant LIME as LIME Explainer

    User ->> Client: Enter news text & click "Verify"
    Client ->> API: POST /api/v1/verify
    API ->> NLP: Preprocess & extract embeddings
    NLP -->> API: Return embedding tensor
    API ->> Model: Predict label & confidence
    Model -->> API: Return prediction (Real / Fake)
    API ->> LIME: Generate word explanations
    LIME ->> Model: Predict on perturbed samples
    Model -->> LIME: Return sample predictions
    LIME -->> API: Return word attribution weights
    API -->> Client: Return JSON (Verdict, Score, Highlights)
    Client -->> User: Display verdict & highlighted text
```

---

## 9. UML Activity Diagram

Control flow showing input validation, model execution, and decision paths.

```mermaid
flowchart TD
    Start([Start]) --> Enter["User enters news text"]
    Enter --> Valid{"Is text valid?"}
    Valid -->|"No"| Err["Show error message"]
    Err --> Enter
    Valid -->|"Yes"| Prep["Preprocess & clean text"]
    Prep --> Embed["Generate FastText embeddings"]
    Embed --> Classify["Predict with CNN-LSTM"]
    Classify --> Explain["Generate LIME word highlights"]
    Explain --> Display["Display verdict & explanations"]
    Display --> Done([End])
```

---

## 10. UML Component Diagram

Modular software components and their dependencies.

```mermaid
flowchart TD
    UI["Frontend Component\n(React.js)"] -->|"REST API"| API["API Controller\n(FastAPI)"]
    API --> PRE["NLP Preprocessor\n(NLTK)"]
    API --> FT["FastText Vectorizer"]
    API --> DL["CNN-LSTM Classifier\n(PyTorch / Keras)"]
    API --> XAI["LIME Explainer"]
    
    FT -.-> VEC[("cc.en.300.bin")]
    DL -.-> WTS[("Model Weights (.pt)")]
```

---

## 11. UML Deployment Diagram

Hardware and runtime mapping across client and server environments.

```mermaid
flowchart LR
    subgraph ClientDevice ["Client Device"]
        Browser["Web Browser\n(React.js App)"]
    end

    subgraph ServerDevice ["Cloud / Local Server"]
        API["FastAPI / Uvicorn Server"]
        ML["PyTorch & FastText Engine"]
        Storage[("Model Weights & Vectors")]
        
        API --> ML
        ML --> Storage
    end

    Browser -->|"HTTPS / JSON"| API
```

---

## 12. State Machine Diagram

Lifecycle states of an article verification request.

```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> Validating : Submit text
    Validating --> Idle : Invalid input
    Validating --> Processing : Valid input
    Processing --> Classifying : Preprocessing & Embedding done
    Classifying --> Explaining : Prediction ready
    Explaining --> ResultDisplayed : LIME weights calculated
    ResultDisplayed --> Idle : New verification
```
