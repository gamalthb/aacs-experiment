# AI-Augmented Cognitive Scaffolding for Phishing Detection
### Experiment Materials, Data, and Replication Package

This repository contains the complete research materials for the paper:

> **"Evaluating AI-Augmented Cognitive Scaffolding for Phishing Detection 
> and Transfer Learning"**  
> 2026

---

## Overview

This study presents a controlled between-subjects experiment (n=60) 
comparing AI-augmented cognitive scaffold warnings against generic 
warnings for phishing detection accuracy and transfer learning among 
Indonesian non-technical office workers. Participants assessed 16 
organizational email stimuli across two phases: a warned acquisition 
phase and an unwarned transfer phase.

---

## Repository Structure

AACS-anti-phishing-experiment/ /n
├── components/                      # Email display, warning box, response form  
├── data/ /n  
│   ├── emails.json                  # 16 email stimuli (8 Phase 1, 8 Phase 2)  
│   └── survey.json                  # 8 post-experiment survey items  
├── prompts/  
│   ├── stage1_scaffold_analysis.txt # LLM Stage 1 analysis for scaffold generation prompt  
│   ├── stage1_stimuli_analysis.txt  # LLM Stage 1 prompt (social engineering analysis)  
│   ├── stage2_scaffold_gen.txt      # Three-layer scaffold generation prompt  
│   └── stage2_stimuli_rewrite.txt   # LLM Stage 2 prompt (AI-augmented localization)  
├── results/  
│   └── anonymized_responses_policy.txt     # Anonymized participant data (n=60)  
├── screens/                         # Consent, demographics, email, survey, debrief  
├── utils/                           # Session management, scoring, data export  
├── app.py                           # Main Streamlit entry point  
├── config.py                        # Constants and mode definitions  
├── README.md  
└── requirements.txt                 # Python dependencies  
---

## Stimulus Design

The 16 email stimuli were constructed across three phishing categories:

| Category | IDs | Description |
|---|---|---|
| Legacy (machine-translated) | P1, P2, P6 | English source passed through Google Translate without post-editing |
| AI-augmented | P3, P4, P7, P8 | Two-stage LLM localization pipeline producing fluent Bahasa Indonesia |
| Authentic Indonesian | P5 | Reconstructed from BPJS Ketenagakerjaan phishing patterns |
| Legitimate | L1–L8 | Reconstructed from verified official platform templates |

Phishing sources: W3LL Microsoft 365 credential-theft campaign (Group-IB, 2023; FBI & Polri, 2026), DocuSign APAC impersonation (Check Point Research, 2025), and BEC patterns (FBI IC3, 2024).

---

## Warning Conditions

**Group A — Generic Warning:** A blunt label appended to all Phase 1 
emails ("This email has been flagged as potentially suspicious" / 
"This email appears to come from a legitimate sender"), representing 
the output of commercial email security gateways.

**Group B — AI Cognitive Scaffold:** A three-layer structured 
explanation generated through a two-stage LLM prompting architecture 
(Cau et al., 2025), covering:
1. Named observable technical cue (NIST Phish Scale Technical Indicator)
2. Psychological manipulation mechanism (Vishwanath et al., 2011 IIPM)
3. Exploited work context and realistic consequence (NIST Premise Alignment)

Scaffold prompts and all generated outputs are provided in `/prompts/`.

---

## Running the Experiment Locally

### Requirements
- Python 3.9+
- A Google Sheets service account (for data export)

### Setup

```bash
git clone [repository_url]
cd [folder_name]
pip install -r requirements.txt
```

Create `.streamlit/secrets.toml` with your Google Sheets credentials 
(see template in `/.streamlit/secrets_template.toml`).

```bash
streamlit run app.py
```

Access modes via URL parameters:
- `?mode=pilot` — Calibration mode (no warnings shown)
- `?mode=a` — Group A (generic warning condition)
- `?mode=b` — Group B (AI scaffold condition)

---

## Data

`results/anonymized_responses.csv` contains per-participant records 
for all n=60 participants. Columns include:

- `participant_id` — Anonymous 8-character ID
- `mode` — Condition assignment (a or b)
- `[demographics]` — Age range, gender, education, job function, 
  experience, email frequency, prior training, device
- `[P1–L8]_answer`, `_correct`, `_confidence`, `_cues`, `_time` — 
  Per-email responses
- `Q1–Q8` — Post-experiment survey responses
- `phase1_accuracy`, `phase2_accuracy` — Computed detection accuracy
- `phase1_avg_confidence`, `phase2_avg_confidence` — Mean confidence

No personally identifiable information was collected. Participation 
was voluntary and all responses were anonymous.

---