# Ledger: Evaluation Screenshot Guide

**Course:** CSD415 Project Phase I (Seventh Semester B.Tech, September 2026)  
**Institution:** College of Engineering Cherthala (CEC), APJ Abdul Kalam Technological University (KTU)  
**Department:** Department of Computer Science and Engineering  
**Project Guide:** Mrs. Rajeswary R, Assistant Professor  
**Student Team (Group 12):**  
- Anandhakrishnan S (Reg. No. CEC23CS033)  
- Aravind A Kamath (Reg. No. CEC23CS041)  
- Arjun Manoj (Reg. No. CEC23CS043)  
- Sangeeth Krishnan S (Reg. No. CEC23CS105)  

---

## 1. Quick Launch Instructions

To launch the Ledger interactive frontend for evaluation and screenshot capture:

```powershell
cd C:\Users\aravindanirudh\Downloads\ledger\frontend
npm run dev
```

Open your browser at:
```
http://localhost:5173
```

---

## 2. Mandatory Submission Screenshots Checklist

The following 7 screenshots should be captured using Windows Snipping Tool (`Win + Shift + S`) and saved into `docs/screenshots/`:

### Screenshot 1: Blank Input State
- **View:** Full dashboard view prior to running verification.
- **Key Elements to Show:**
  - Navigation bar with Ledger branding, and active model badge.
  - Hero section with the three key performance metrics (99.0% Accuracy, &lt; 800 ms Latency and 100% Explainability).
  - Empty textarea with preset loader buttons and character/token counters.
- **File Name:** `docs/screenshots/01_blank_dashboard.png`

### Screenshot 2: Real News Classification
- **Action:** Select preset button **Sample 1: Verified Real News** ("Dr. Ben Carson speech...").
- **Key Elements to Show:**
  - Emerald green **AUTHENTIC NEWS** verdict badge.
  - Model confidence bar showing **98.6%** with probability distribution breakdown.
- **File Name:** `docs/screenshots/02_real_news_verdict.png`

### Screenshot 3: Fake News Classification
- **Action:** Select preset button **Sample 2: Fabricated Fake News** ("Secret government memo leaks...").
- **Key Elements to Show:**
  - Crimson red **DECEPTIVE / FAKE NEWS** verdict badge.
  - Model confidence bar showing **99.2%**.
  - Red-highlighted deceptive terms such as `dismantling`, `penalties`, `secret` and `leaks`.
- **File Name:** `docs/screenshots/03_fake_news_verdict.png`

### Screenshot 5: Interactive Word Attribution Tooltip and Top 10 Feature Importance Bar Chart
- **Action:** Hover cursor over words like `dismantling` and also show bilateral feature ranking chart below the heatmap.
- **Key Elements to Show:**
  - Floating tooltip displaying exact numerical LIME score (e.g. `-0.584`).
  - Directional indicator text ("Flags deceptive pattern" or "Shifts verdict to Authentic").
  - Centered zero-axis.
  - Green bars extending right for authentic terms and red bars extending left for deceptive terms.
  - Numerical weights displayed next to each bar.
- **File Name:** `docs/screenshots/05_feature_importance_chart.png`

### Screenshot 6: Model Architecture & Metrics Modal
- **Action:** Click **Model Benchmarks** in the navigation bar.
- **Key Elements to Show:**
  - Comparative benchmark evaluation table (WELFake, FakeNewsNet and LIAR metrics).
  - Neural pipeline tier specifications (FastText 300-d -> 1D-CNN -> LSTM -> LIME). 
- **File Name:** `docs/screenshots/06_model_architecture_modal.png`

### Screenshot 7: Mobile Responsiveness View
- **Action:** Press `F12` in Chrome/Edge, click the device toggle icon (`Ctrl + Shift + M`), select **iPhone 14** or **Pixel 7** and reload.
- **Key Elements to Show:**
  - Mobile layout adapting cleanly with responsive stacking, full-width cards and touch-friendly controls.
- **File Name:** `docs/screenshots/07_mobile_responsive_view.png`

---

## 3. Presentation Slides & Interim Report Integration

After saving the images into `docs/screenshots/`:
1. Use them directly in your interim report.
2. Link or embed the images in your seminar and project evaluation slide decks.
