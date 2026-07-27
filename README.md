# OONI Network Censorship Analysis: Applying ML to Global Network Telemetry

**Student:** Haley Archer | **Course:** SDC486L: Advanced Data Analytics and AI/ML | **ECPI University** | **July 2026**

---

## Project Overview

This project applies machine learning to the Open Observatory of Network Interference (OONI) dataset; a free, globally distributed measurement network that captures DNS manipulation, HTTP blocking, and TCP interference events across 240+ countries since 2022.

The dataset was selected because it speaks the same language as real network operations: ASN-level data, DNS resolution failures, HTTP header anomalies, TCP latency. The same primitives used here to detect censorship are the same ones used in AIOps and network observability platforms to detect infrastructure anomalies.

---

## Interactive Dashboard

View the full interactive dashboard (charts, filters, scenario analysis):

[Open Dashboard](https://thepacketpoet-school.github.io/sdc486l/OONI_Dashboard.html](https://thepacketpoet-school.github.io/sdc486l/Archer_SDC486L_Final_OONI_Dashboard.html)

---

## Research Questions

1. **Classification:** Can a machine learning model reliably distinguish confirmed censorship events from measurement noise and clean results?
2. **Clustering:** Do countries with similar network behavior group together without using political labels as input?
3. **Trend Prediction:** Are there leading indicators in the data that predict censorship escalation 30 days in advance?

---

## Key Findings

| Task | Baseline | Neural Network | Key Finding |
|---|---|---|---|
| Task 1: Classification | LR Accuracy: 84.8% | MLP Accuracy: 86.5% | dns_mismatch_score (r=0.61) is the dominant censorship signal |
| Task 2: Clustering | K-Means Silhouette: 0.40 | Autoencoder+KMeans: 0.56 | Clusters matched Freedom House tiers without political labels |
| Task 3: Trend Prediction | ARIMA MAE: 0.0277 | LSTM MAE: 0.0287 | 30-month series too short for LSTM to outperform ARIMA |

### Scenario Analysis Results

| Scenario | Finding | Operational Implication |
|---|---|---|
| S1: Censorship Fingerprint | 89.5% combined anomaly+confirmed detection rate | Model is production-viable for alert generation |
| S2: Data Throttling | 24.3pp confirmed recall drop when telemetry is pre-aggregated | Platform fidelity directly impacts detection capability |
| S3: ASN Diversity Shift | 0/20 countries changed cluster | Model is robust to infrastructure topology changes |

---

## Repository Structure

```
ooni-capstone-sdc486l/
├── Archer_SDC486L_W2_OONI_EDA_Part2.ipynb              # Part 2: Data preparation and EDA
├── Archer_SDC486L_W3_OONI_ML_Part3.ipynb               # Part 3: Baseline models and neural networks
├── Archer_SDC486L_W5_OONI_Part5_ScenarioAnalysis.ipynb # Part 5: Scenario analysis
├── Archer_SDC486L_Final_OONI_Capstone_Dashboard.twbx  # Tableau dashboard
├── Archer_SDC486L_W5_OONI_Final_Report.docx            # Comprehensive final report
├── ooni_sample.csv                                     # 5,000-record structured dataset sample used by all three notebooks; preprocessed from the OONI AWS Open Data source (s3://ooni-data-eu-fra)
├── README.md                         # This file
├── tableau_data/
│   ├── ooni_sample_tableau.xlsx
│   ├── ooni_country_summary.xlsx
│   ├── ooni_monthly_trend.xlsx
│   ├── ooni_model_comparison_full.xlsx
│   ├── ooni_feature_importance.xlsx
│   ├── ooni_task1_metrics.xlsx
│   └── ooni_scenario_results.xlsx
└── presentation/
    └── Archer_SDC486L_OONI_Capstone_Presentation.pdf
```

---

## Dependencies

```
python >= 3.10
pandas >= 2.0
numpy >= 1.24
scikit-learn >= 1.3
tensorflow >= 2.13
matplotlib >= 3.7
seaborn >= 0.12
statsmodels >= 0.14
openpyxl >= 3.1
```

Install all dependencies:
```bash
pip install pandas numpy scikit-learn tensorflow matplotlib seaborn statsmodels openpyxl
```

---

## Data Source

**Open Observatory of Network Interference (OONI)**
- AWS S3: `s3://ooni-data-eu-fra` (no account required)
- Web: https://ooni.org/data/
- License: Creative Commons Attribution-NonCommercial-ShareAlike 4.0

This project uses a 5,000-record structured sample covering January 2022 through June 2024 across 20 countries. The sample is included in the `data/` folder as Excel files pre-processed for Tableau compatibility.

---

## Running the Notebooks

1. Clone the repository
2. Install dependencies (see above)
3. Place data files in the same directory as the notebooks, or update file paths
4. Run notebooks in order: Part 2 → Part 3 → Part 5

Each notebook is self-contained and re-runs the full preprocessing pipeline from the raw sample CSV.

---

## Dashboard

The Tableau dashboard (`OONI_Capstone_Dashboard_HArcher.twbx`) requires Tableau Desktop or Tableau Public to open. It contains 7 interactive sheets plus a scenario analysis sheet, assembled into a single dashboard with Freedom Tier and Cluster filters.

---

## References

- Azzabi, S., Alfughi, Z., & Ouda, A. (2024). Data lakes: A survey of concepts and architectures. *Computers, 13*(7), 183. https://doi.org/10.3390/computers13070183
- Brown, A., & Schreiber, T. (2025). Join me if you can: ClickHouse vs. Databricks vs. Snowflake. ClickHouse Engineering Blog.
- Freedom House. (2023). Freedom on the Net 2023. https://freedomhouse.org/report/freedom-net
- OONI. (2024). OONI Data. https://ooni.org/data/
