# 🎓 EduGuard — Student Burnout Early Warning System

> **Behavioural Analytics Hackathon · OrgX · Problem Statement 1**  
> *Early Detection of Student Burnout & Dropout Risk using Machine Learning*

---

## 📌 Overview

Universities typically identify academic issues **only after performance has already dropped** — by which point intervention becomes significantly harder. **EduGuard** flips this by analysing early behavioural signals (LMS activity, attendance patterns, submission delays, sentiment) to predict burnout risk *before* it becomes a crisis.

This project delivers:
- ✅ A **burnout risk classifier** (Low / Medium / High) using Random Forest
- ✅ A **continuous burnout score** (0–100) using Gradient Boosting Regression
- ✅ A **dropout probability** estimate per student
- ✅ A **fully interactive browser dashboard** — no server needed, just open the HTML
- ✅ Two **synthetic datasets** (500 students each) showing contrasting risk profiles

---

## 🖥️ Live Dashboard

The dashboard is a **single self-contained HTML file** — no installation, no dependencies.

### How to use it:

1. Download `burnout_dashboard_live.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. Either:
   - **Drag & drop** `student_burnout_data.csv` or `student_burnout_data_semester4.csv`
   - Or click **"Use built-in sample data"** to instantly load 500 students

### What the dashboard shows:

| Section | Description |
|---|---|
| **Stats Cards** | Live counts of High / Medium / Low risk students + averages |
| **Risk Distribution Bars** | Animated breakdown of cohort risk split |
| **Feature Importance** | Which behavioural signals matter most |
| **Scatter Plot** | Every student plotted: LMS logins vs Burnout Score (with regression line) |
| **Histogram** | Burnout score distribution across the cohort |
| **Student Table** | All 500 students — searchable, filterable, sortable, paginated |
| **Student Profile Panel** | Click any row → full signal breakdown + personalised interventions |

---

## 📁 Repository Structure

```
EduGuard/
│
├── burnout_dashboard_live.html        ← Main interactive dashboard (open in browser)
├── student_burnout_data.csv           ← Dataset 1: Semester 2 (lower risk cohort)
├── student_burnout_data_semester4.csv ← Dataset 2: Semester 4 Engineering (higher risk)
├── EduGuard_Model_Explanation.docx    ← Full model explanation document (5 pages)
├── generate_dataset.py                ← Script to regenerate Dataset 1
├── generate_dataset2.py               ← Script to regenerate Dataset 2
├── model_pipeline.py                  ← ML training pipeline (RF + GBM)
└── README.md                          ← You are here
```

---

## 📊 Datasets

### Dataset Type: **Synthetic**

**Why synthetic?** No real student behavioural dataset was available due to privacy regulations (FERPA, GDPR). The datasets were generated using realistic statistical distributions calibrated against published research on academic disengagement.

### Dataset 1 — `student_burnout_data.csv` (Spring Semester 2)

Represents a **general undergraduate cohort** with moderate overall risk.

| Feature | Value |
|---|---|
| Records | 500 students (STU0001–STU0500) |
| High Risk | 36 students (7.2%) |
| Medium Risk | 386 students (77.2%) |
| Low Risk | 78 students (15.6%) |
| Avg Burnout Score | 46.5 / 100 |
| Avg Attendance | 74% |

### Dataset 2 — `student_burnout_data_semester4.csv` (Semester 4 Engineering)

Represents a **mid-degree cohort under heavier academic pressure** — deliberately higher risk profile to demonstrate contrast.

| Feature | Value |
|---|---|
| Records | 500 students (STU0501–STU1000) |
| High Risk | 231 students (46.2%) |
| Medium Risk | 191 students (38.2%) |
| Low Risk | 78 students (15.6%) |
| Avg Burnout Score | 63.4 / 100 |
| Avg Attendance | 58% |

### Feature Descriptions

| Column | Description | Type |
|---|---|---|
| `student_id` | Unique student identifier | String |
| `lms_logins_per_week` | Weekly LMS access frequency | Float (0–14) |
| `attendance_rate` | Class attendance proportion | Float (0–1) |
| `avg_submission_delay_days` | Average days past deadline | Float (0+) |
| `gpa_trend` | GPA change over semester | Float (–1.5 to +1.5) |
| `sleep_irregularity_score` | Proxy from login timestamp variance | Float (0–1) |
| `social_activity_drop` | Decline in forum/peer interaction | Float (0–1) |
| `sentiment_score` | NLP-derived from feedback forms | Float (0–1) |
| `weekend_activity_ratio` | Weekend vs weekday LMS activity | Float (0–1) |
| `burnout_score` | Composite target score | Float (0–100) |
| `burnout_level` | Categorical risk label | Low / Medium / High |
| `dropout_probability` | Estimated dropout likelihood | Float (0–1) |

---

## 🤖 Model Summary

### Classification — Burnout Level

| Property | Value |
|---|---|
| Algorithm | Random Forest (150 trees) |
| Class Weighting | Balanced (handles imbalance) |
| CV F1-Score (weighted) | **0.830 ± 0.031** |
| Test Accuracy | **84%** |

### Regression — Burnout Score

| Property | Value |
|---|---|
| Algorithm | Gradient Boosting Regressor |
| R² Score | **0.978** |
| Mean Absolute Error | **1.46 points** |

### Feature Importance (from Random Forest)

| Rank | Feature | Importance |
|---|---|---|
| 1 | LMS Login Frequency | 24.5% |
| 2 | Submission Delay | 24.2% |
| 3 | Attendance Rate | 15.0% |
| 4 | Sleep Irregularity | 12.5% |
| 5 | Sentiment Score | 7.8% |
| 6 | GPA Trend | 5.7% |
| 7 | Weekend Activity Ratio | 5.2% |
| 8 | Social Activity Drop | 5.1% |

---

## 🚀 Running the Python Scripts

If you want to regenerate the datasets or retrain the models:

### Prerequisites

```bash
pip install pandas numpy scikit-learn
```

### Generate Datasets

```bash
# Dataset 1 (Semester 2 cohort)
python generate_dataset.py

# Dataset 2 (Semester 4 Engineering cohort)
python generate_dataset2.py
```

### Train Models

```bash
python model_pipeline.py
```

This will output:
- CV F1-Score and classification report
- Burnout Score R² and MAE
- Feature importances
- `model_results.json` with summary metrics

---

## 🧠 Intervention Strategy

| Risk Level | Score Range | Actions |
|---|---|---|
| 🔴 **High** | 67–100 | Personal counsellor within 48h · Academic load review · Mental health referral · Peer mentor pairing |
| 🟡 **Medium** | 34–66 | Fortnightly advisor check-ins · LMS nudge alerts · Study group matching · Progress report |
| 🟢 **Low** | 0–33 | Milestone celebration · Peer mentoring role · Stretch challenges · Periodic monitoring |

---

## 📈 Key Behavioural Insights

1. **LMS logins + submission delays together account for ~49% of predictive power** — digital disengagement is the earliest signal
2. **Attendance drops lag behind digital disengagement** by ~2 weeks — students disengage online before skipping class
3. **Sleep irregularity (12.5% importance)** is a stronger predictor than GPA trend — behavioural patterns matter more than outcomes
4. **Students showing both low LMS activity AND high submission delays are 3.2× more likely to be High risk** than those showing only one signal

---

## 🏆 Hackathon Details

| | |
|---|---|
| **Event** | Behavioural Analytics Hackathon |
| **Organiser** | OrgX |
| **Problem Statement** | PS1 — Early Detection of Student Burnout & Dropout Risk |
| **Start** | Saturday, 28 February 2025 — 9:00 AM |
| **Deadline** | Sunday, 1 March 2025 — 11:59 PM |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Data Generation | Python · NumPy · Pandas |
| ML Models | Scikit-learn (Random Forest, Gradient Boosting) |
| Dashboard | Vanilla HTML · CSS · JavaScript (zero dependencies) |
| CSV Parsing | Browser FileReader API + custom parser |
| Charts | HTML5 Canvas (hand-written, no Chart.js) |

---

## 📄 License

This project was created for the OrgX Behavioural Analytics Hackathon. Dataset is synthetic and does not contain any real student data.

---

*Built with ❤️ for EduGuard · OrgX Hackathon 2025*
