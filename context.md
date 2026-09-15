# Project Handover Context: Ledger Phase I (Presentation & Report)

This document provides complete, self-contained context regarding the codebase, project background, formatting standards, faculty feedback history and architectural requirements. Any AI agent resuming this project should read this document carefully before proposing or making edits.

---

## 1. Project Overview & Institutional Metadata

- **Project Title:** Ledger: AI-Based Fake News Detection System Using Explainable AI
- **Course & Stage:** CSD 415 Project Phase I (Seventh Semester B.Tech, September 2026)
- **Institution:** College of Engineering, Pallippuram P O, Cherthala, Alappuzha PIN: 688541 (College of Engineering Cherthala - CEC)
- **Department:** Department of Computer Science and Engineering
- **Affiliated University:** APJ Abdul Kalam Technological University (KTU)
- **Principal:** Dr. Jaya V.L
- **Head of Department (HOD):** Dr. Preetha Theresa Joy (Professor)
- **Project Co-ordinator:** Mrs. Rakhi R (Assistant Professor)
- **Project Guide:** Mrs. Rajeswary R (Assistant Professor) *(Strict: ensure name is always written as Mrs. Rajeswary R)*
- **Student Group (Group 12):**
  - ANANDHAKRISHNAN S (Reg. No. CEC23CS033)
  - ARAVIND A KAMATH (Reg. No. CEC23CS041)
  - ARJUN MANOJ (Reg. No. CEC23CS043)
  - SANGEETH KRISHNAN S (Reg. No. CEC23CS105)

---

## 2. Core Technical Architecture & Base Paper

- **Selected Base Paper:** 
  E. Hashmi, S. Y. Yayilgan, M. M. Yamin, S. Ali and M. Abomhara, "Advancing Fake News Detection: Hybrid Deep Learning With FastText and Explainable AI," *IEEE Access*, vol. 12, pp. 44463-44481, 2024. DOI: 10.1109/ACCESS.2024.3381038.
- **Core Pipeline:**
  1. **Preprocessing & Embedding:** Regex cleaning, lemmatization and tokenization followed by **FastText** subword n-gram embeddings (handles out-of-vocabulary terms, internet slang, typos and noisy text).
  2. **Feature Extraction:** **1D Convolutional Neural Network (CNN)** extracts localized n-gram features and deceptive syntactic patterns.
  3. **Contextual Modeling:** **Long Short-Term Memory (LSTM)** recurrent network captures long-range sequential contextual dependencies across article bodies.
  4. **Classification:** Dense Softmax output layer yields binary authenticity verdicts (Real or Fake) with calibrated confidence percentages.
  5. **Explainability (XAI):** **LIME** (Local Interpretable Model-agnostic Explanations) generates word-level attribution weights to visually highlight influential positive and negative terms, overcoming black-box opacity.
  6. **Application Stack:** High-throughput Python **FastAPI** backend for sub-second REST inference and an interactive **React.js** web dashboard.
- **Key Datasets Benchmarked in Literature:** WELFake, FakeNewsNet and LIAR.

---

## 3. Repository Structure

```text
Presentation and Report GitHub Repo/
├── context.md                                  # This handover document
├── Sample_Project_Report.pdf                   # Department sample reference 1
├── Sample_Project_Report_1.pdf                 # Department sample reference 2
├── report_code/
│   ├── main.tex                                # Primary LaTeX report source
│   ├── CEC_Logo.jpeg                           # College emblem for title/certificate
│   ├── ArchitectureDiagram.drawio.png          # System Architecture diagram
│   ├── DFD_0.drawio.png                        # Data Flow Diagram Level 0
│   ├── DFD_1.drawio.png                        # Data Flow Diagram Level 1
│   └── uml_sequence_diagram.png                # UML Sequence diagram
├── presentation_code/
│   ├── main.tex                                # LaTeX Beamer presentation slides
│   ├── CEC_Logo.jpeg                           # College emblem
│   └── diagrams/                               # Diagram assets for slides
├── diagrams/                                   # Source .drawio files and exported PNGs
├── backups/                                    # Timestamped checkpoints of PDFs & ZIPs
└── context/                                    # Base paper PDF and abstract documentation
```

---

## 4. Strict Behavioral Rules & Stylistic Constraints

1. **Always Edit Files on Disk:**
   - The user often modifies files directly in Overleaf or on disk. Never rely purely on conversational memory. Always check file contents on disk with `view_file` before making modifications.
2. **Zero Em-Dashes:**
   - Never use em-dashes (`---` or `—`) in generated text. Use standard hyphens or rephrase sentences naturally.
3. **Zero Oxford Commas:**
   - Never use Oxford commas before 'and'. Always format serial lists as `A, B and C` (not `A, B, and C`).
4. **Preserve User Disk Edits:**
   - Never overwrite or revert guide name (`Mrs. Rajeswary R`), certificate vertical spacing (`0.2cm`), single-space title blocks or single-paragraph abstract structure.
5. **No Vertical Alignment of Student Names:**
   - On Cover and Inner Title pages, student names must use natural line spacing (`\\[0.1cm]`), not horizontal quad tabs or vertical columnar alignment.
6. **Abstract Must Be a Single Paragraph:**
   - Keep the abstract concise, dense and formatted as a single unified paragraph without hard line breaks.

---

## 5. LaTeX Report Layout & Formatting Specifications (`report_code/main.tex`)

### A. Document Class & Preamble Geometry
- `\documentclass[12pt,a4paper,oneside]{report}`
- Preamble geometry (used for Front Matter to avoid title/certificate overflow):
  ```latex
  \usepackage[a4paper, top=1in, bottom=1in, left=1.25in, right=1in, headheight=15pt, headsep=15pt, footskip=25pt]{geometry}
  ```

### B. Main Matter Geometry & Breathing Room
- At the start of the Main Matter (`\pagenumbering{arabic}`, line ~280), geometry is adjusted to guarantee a comfortable **~15.3 mm (~0.6 inches)** vertical gap between the lowest text content and the horizontal footer rule across all body pages:
  ```latex
  \clearpage
  \newgeometry{top=1in, bottom=1.4in, left=1.25in, right=1in, headheight=15pt, headsep=20pt, footskip=48pt}
  \raggedbottom
  \pagenumbering{arabic}
  \setcounter{page}{1}
  ```

### C. Running Headers, Running Footers & Page Styles (`fancyhdr`)

| Section / Page Type | Page Style | Running Header | Header Rule | Running Footer | Footer Rule | Page Numbering |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Cover Page** (p. 1) | `empty` | None | 0 pt | None | 0 pt | Unnumbered |
| **Inner Title Page** (p. 2) | `empty` | None | 0 pt | None | 0 pt | Unnumbered |
| **Certificate** (p. 3) | `empty` | None | 0 pt | None | 0 pt | Unnumbered |
| **Acknowledgement** (p. 4) | `frontmatter` | None | 0 pt | None | 0 pt | Roman `i` (centered) |
| **Abstract** (p. 5) | `frontmatter` | None | 0 pt | None | 0 pt | Roman `ii` (centered) |
| **Table of Contents** | `frontmatter` | None | 0 pt | None | 0 pt | Roman `iii`+ (centered) |
| **List of Figures** | `frontmatter` | None | 0 pt | None | 0 pt | Roman `v` (centered) |
| **Chapter Opening Pages** | `plain` | None | 0 pt | None | 0 pt | Arabic `\thepage` (centered) |
| **Main Matter Body Pages** | `fancy` | Left: `College of Engineering, Cherthala`<br>Right: `\nouppercase{\leftmark}` | 0.4 pt | Left: `Department of Computer Science and Engineering`<br>Right: `\thepage` | 0.4 pt | Arabic `\thepage` (right) |

### D. Implementation Details for Header/Footer Rules
- **Chapter Marks:** `\renewcommand{\chaptermark}[1]{\markboth{#1}{}}` ensures `\leftmark` contains only the clean uppercase chapter title (e.g. `INTRODUCTION`, `PROBLEM STATEMENT`, `LITERATURE SURVEY`, `SYSTEM DESIGN`, `REFERENCES`).
- **No Manual `\thispagestyle{fancy}` on Chapters:** Do not add `\thispagestyle{fancy}` under `\chapter{...}` or `\chapter*{REFERENCES}`. LaTeX's internal call to `\thispagestyle{plain}` must remain in effect so that chapter opening pages carry only the centered page number without headers or footers.
- **Main Matter `plain` Definition:**
  ```latex
  \fancypagestyle{plain}{
    \fancyhf{}
    \fancyfoot[C]{\small\thepage}
    \renewcommand{\headrulewidth}{0pt}
    \renewcommand{\footrulewidth}{0pt}
  }
  ```
- **Main Matter `fancy` Definition:**
  ```latex
  \pagestyle{fancy}
  \fancyhf{}
  \fancyhead[L]{\small College of Engineering, Cherthala}
  \fancyhead[R]{\small\nouppercase{\leftmark}}
  \fancyfoot[L]{\small Department of Computer Science and Engineering}
  \fancyfoot[R]{\small\thepage}
  \renewcommand{\headrulewidth}{0.4pt}
  \renewcommand{\footrulewidth}{0.4pt}
  ```

---

## 6. Chapter 4 Diagrams Configuration (`report_code/main.tex`)

All four diagrams reside in `report_code/main.tex` within `figure` environments using `[H]` from the `float` package:

1. **System Architecture Diagram (Figure 4.1):**
   - File: `ArchitectureDiagram.drawio.png` (Aspect ratio: 0.42, tall and narrow).
   - Scaling: `\includegraphics[height=0.62\textheight,keepaspectratio]{ArchitectureDiagram.drawio.png}`.
   - Sits on report page 13 beneath the final 3 lines of Section 4.1.
2. **Data Flow Diagram: Level 0 (Figure 4.2):**
   - File: `DFD_0.drawio.png` (Aspect ratio: 7.43, wide context diagram).
   - Scaling: `\includegraphics[width=\textwidth,keepaspectratio]{DFD_0.drawio.png}`.
3. **Data Flow Diagram: Level 1 (Figure 4.3):**
   - File: `DFD_1.drawio.png` (Aspect ratio: 4.24, 4 sub-processes).
   - Scaling: `\includegraphics[width=\textwidth,keepaspectratio]{DFD_1.drawio.png}`.
4. **UML Sequence Diagram (Figure 4.4):**
   - File: `uml_sequence_diagram.png` (Aspect ratio: 2.29, 6 lifelines).
   - Standard Scaling: `\includegraphics[width=\textwidth,keepaspectratio]{uml_sequence_diagram.png}`.
   - **Optional Portrait Margin Bleed (Manual adjustment requested by user):** If the user wants this diagram even larger in portrait mode without rotating to landscape, wrap the graphic in a centered makebox:
     ```latex
     \makebox[\textwidth][c]{%
       \includegraphics[width=1.15\textwidth]{uml_sequence_diagram.png}%
     }
     ```
     This expands the image symmetrically by 15 percent into both the left and right margins while keeping it centered on the page.

---

## 7. Chronological Summary of Recent Faculty Mandates & Fixes

1. **Guide Name & Title Overflow:** Corrected guide name to `Mrs. Rajeswary R`. Compacted vertical skips on Cover and Inner Title pages so institutional address and website do not overflow to an extra page.
2. **Abstract Simplification:** Reduced multi-paragraph abstract into a single coherent paragraph.
3. **Roman Numerals in Front Matter:** Certificate set to unnumbered (`empty`), Acknowledgement set to page `i`, Abstract to page `ii`, Contents to page `iii`.
4. **Literature Survey Sorting:** Base paper (Hashmi et al., 2024) placed first, followed by ten complementary papers ordered chronologically with newest first.
5. **Running Header and Running Footer Fixes:**
   - Faculty mandated a horizontal line above the running footer (`\footrulewidth{0.4pt}`), matching the line below the header.
   - Faculty mandated both the college name (`College of Engineering, Cherthala`) on the left and the chapter title (`\nouppercase{\leftmark}`) on the right of the header.
   - Faculty mandated that chapter starting pages must NOT have running headers or footers, showing only the centered page number at the bottom.
   - Resolved the previous bug where body pages lacked headers/footers due to `frontmatter` pagestyle persistence.
6. **Consistent Content-to-Footer Gap:**
   - User noted that page 17 had a visible gap above the footer rule, whereas other body pages ran text directly against the rule. Applied `\newgeometry` with `bottom=1.4in` and `footskip=48pt` to establish a uniform ~15.3 mm clearance on all full body pages.
7. **Diagram Scaling:**
   - Scaled Architecture Diagram from `0.48\textheight` to `0.62\textheight` and scaled DFD 0, DFD 1 and UML Sequence diagrams from `0.92\textwidth` / `0.95\textwidth` to the full `\textwidth`.
