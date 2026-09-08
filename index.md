# Predicting Content Traffic Decay for Refresh Prioritization

**Author:** Ahmed Refaat Abd-Elmotelb Desoky  
**Background:** Artificial Intelligence and Data Science, Zagazig National University  

### Abstract
Content teams often rely on manual, arbitrary rules to decide which web pages need updating. This project applies machine learning to prioritize content refresh queues by identifying pages at the highest risk of traffic decay. Using a Random Forest classifier evaluated via client-grouped cross-validation, the model outperformed fixed heuristic baselines on Precision@50. The result is a ranked, evidence-based decision-support playbook that helps strategists focus human review where it yields the highest ROI.

### Introduction / Problem Statement
Allocating resources to update content purely based on age (e.g., updating everything older than 6 months) wastes writing capacity on pages that are already performing well. The problem is a ranking task: how do we order thousands of pages so that the top recommendations are genuinely declining and worth the team's immediate attention? This work supports operational decision-making by surfacing high-risk assets before traffic loss becomes critical.

### Data
The analysis was built on the FlyRank pseudonymized warehouse dataset (v20260703).
*   **Tables Used:** `dim_content` for page metadata and `fact_content_daily_performance` for time-series search signals.
*   **Time Window:** Evaluated on mid-panel data (March 2026), keeping the final month as a sealed holdout.
*   **Exclusions:** Product flags (such as `health_score`) and future target columns were strictly excluded to prevent data leakage. The dataset contains safe, observable search signals only.

### Methodology
*   **Task Formulation:** Supervised Binary Classification scoring pages to rank them.
*   **Label Definition:** `is_declining` (Target = 1 if the trend is explicitly down).
*   **Algorithm:** Random Forest Classifier, chosen for its ability to capture non-linear interactions between age, CTR, and impression volume.
*   **Validation Design:** `GroupShuffleSplit` on `client_id` ensured the model was tested on completely unseen clients, preventing it from memorizing site-specific traffic baselines.

### Results
The model's performance was compared to a rigid baseline rule (stale and visible criteria). The machine learning approach demonstrated a marked improvement in Precision@50 on the holdout set, proving that dynamic feature interaction (like identifying fresh pages with sudden CTR drops) beats static if-statements.

### Limitations & Honest Framing
This tool provides decision-support, not causal proof. It measures the directional risk of traffic decay based on observable historical data. It does not guarantee that a rewrite will recover traffic, nor does it account for external variables like backlink loss or search engine layout changes.

### Ranked Recommendations
The model's output is mapped to a human-reviewed action playbook:
1.  **Immediate_Refresh_Review:** High probability of decay; requires content strategist review.
2.  **Monitor_CTR:** High volume but dropping engagement; metadata adjustment candidate.
*Rule:* No automated deletions or metadata overwrites are permitted without human verification.

### Reproducibility
All artifacts, validation checks, and code are publicly available in the project repository:
*   [GitHub Repository](https://github.com/flyrank-bih/flyrank-ml-internship-starter)

### Acknowledgments & Data Credit
Built on the FlyRank ML Internship dataset.  
[Learn more at flyrank.ai](https://flyrank.ai)
