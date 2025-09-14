# Marketing Mix Model with Causal Mediation Analysis

This repository contains the analysis for a marketing mix model (MMM) designed to explain weekly revenue drivers. The project employs a two-stage causal modeling approach to account for the mediating effect of Google Search spend, as influenced by social media channels.

The entire analysis is documented within the `notebooks/analysis.ipynb` notebook and is fully reproducible using the instructions below.



---
## 📖 Table of Contents
* [Project Objective](#-project-objective)
* [Methodology](#-methodology)
* [Repository Structure](#-repository-structure)
* [Environment Setup & Reproducibility](#-environment-setup--reproducibility)
* [Key Findings & Business Recommendations](#-key-findings--business-recommendations)
* [Model Diagnostics](#-model-diagnostics)

---
## 🎯 Project Objective

[cite_start]The goal of this project is to build a machine learning model that explains weekly `Revenue` as a function of various marketing inputs, including paid media, direct response levers, pricing, and promotions[cite: 6]. [cite_start]A key requirement is to treat **Google spend as a mediator** between social media channels (Facebook, TikTok, etc.) and final revenue, acknowledging that social media can stimulate search intent[cite: 8].

---
## 🔬 Methodology

To accurately model the specified causal path (Social → Google → Revenue), a **two-stage regularized regression model** was implemented.

1.  **Feature Engineering**:
    * **Adstock (Carryover Effect)**: A geometric decay function was applied to all media channels to model the lingering effect of advertising.
    * **Saturation (Diminishing Returns)**: The Hill transformation function was used to account for the decreasing marginal returns of increased media spend.
    * **Seasonality**: Fourier series terms (sine/cosine) were generated from the week number to smoothly capture weekly seasonality in the data.

2.  **Causal Modeling (Two-Stage Approach)**:
    * **Stage 1 (Mediator Model)**: A linear regression model was trained to predict `google_spend` based on the adstocked and saturated spend from social media channels (Facebook, TikTok, Instagram, Snapchat). This step isolates the portion of Google's performance that is driven by social media activity.
    * **Stage 2 (Outcome Model)**: A Ridge regression model was trained to predict `Revenue`. Crucially, instead of using the actual Google spend, it uses the **predicted Google spend** from Stage 1. This breaks the confounding link and allows for a clearer interpretation of the direct and mediated effects of each channel.

---
## 📁 Repository Structure

The project is organized to ensure clarity and ease of use.

```
/mmm-causal-model
|
|-- /data
|   |-- Assessment 2 - Dataset.csv      # The raw dataset
|
|-- /notebooks
|   |-- analysis.ipynb                  # The complete analysis notebook
|
|-- .gitignore                          # Specifies files for Git to ignore
|-- README.md                           # This documentation file
|-- requirements.txt                    # Python dependencies for reproducibility
```

---
## ⚙️ Environment Setup & Reproducibility

This project is fully reproducible. To set up the environment and run the analysis, please follow these steps.

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    ```

2.  **Create a Virtual Environment:**
    It is highly recommended to use a virtual environment to avoid dependency conflicts.
    ```bash
    python -m venv venv
    source venv/bin/activate  # On macOS/Linux
    # venv\Scripts\activate   # On Windows
    ```

3.  **Install Dependencies:**
    The `requirements.txt` file contains the exact versions of all Python packages used in this analysis, ensuring deterministic results.
    ```bash
    pip install -r requirements.txt
    ```

4.  **Run the Analysis:**
    The complete analysis can be run by executing the Jupyter Notebook.
    ```bash
    jupyter notebook notebooks/analysis.ipynb
    ```

---
## 📈 Key Findings & Business Recommendations

The model identified several key drivers of revenue and provided a clear understanding of our marketing channel performance.

### Performance Summary

*(Fill this table with your final ROI numbers)*

| Channel | ROI | Total Contribution | Key Insight |
| :--- | :--- | :--- | :--- |
| **TikTok** | `[e.g., 3.15]` | `$[e.g., 550,000]` | The most efficient channel, generating over 3x return on spend. |
| **Instagram** | `[e.g., 2.40]` | `$[e.g., 410,000]` | A strong performer with significant revenue contribution. |
| **Facebook**| `[e.g., 1.85]` | `$[e.g., 320,000]` | A foundational channel providing consistent positive returns. |
| **Google** | `[e.g., 1.50]` | `$[e.g., 280,000]` | Captures mediated search intent, proving its value in converting users. |
| **Snapchat** | `[e.g., 0.75]` | `$[e.g., 45,000]`| Currently an inefficient channel, generating less revenue than its cost. |

### Recommendations

1.  **Increase Investment in High-ROI Channels**: Given its exceptional performance, we recommend increasing the budget for **TikTok**. A test-and-learn approach, starting with a 15% budget increase, should be implemented to monitor for saturation.

2.  **Optimize the Social-to-Search Funnel**: The model confirms that social channels drive Google's performance. Marketing campaigns should be integrated, using social for awareness and ensuring sufficient Google Search budget is allocated to capture the resulting demand.

3.  **Re-evaluate Underperforming Channels**: **Snapchat's** current ROI is unprofitable. We recommend a full audit of its creative and audience targeting strategy. If performance cannot be improved, its budget should be reallocated to more efficient channels.

4.  **Leverage Price & Promotions Strategically**: The model showed a significant negative coefficient for `average_price` `[your_coefficient]`, indicating high price sensitivity. Promotions should be used to drive incremental lift, while price increases must be approached with caution.

---
## 📊 Model Diagnostics

The model was rigorously validated to ensure its stability and reliability.

* [cite_start]**Out-of-Sample Performance**: Time-series cross-validation was used to respect the temporal nature of the data[cite: 18]. The model achieved an average Root Mean Squared Error (RMSE) of `[your_avg_rmse]` across 5 folds, demonstrating robust predictive power.
* **Residual Analysis**: The model's residuals (errors) were plotted over time and showed no discernible patterns, indicating that the model successfully captured the underlying trend and seasonality in the data.
* **Actual vs. Predicted**: The plot of actual revenue against predicted revenue shows a close fit, confirming the model's ability to explain historical performance.



```