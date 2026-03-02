# QuickLoan Mobile: Ethical Data Review Report

**Prepared by:** Independent Data Governance Consultant

---

## 1. Executive Summary
This report outlines the findings of an independent data governance review for QuickLoan Mobile. The review identifies critical risks in data quality, legal compliance, and algorithmic fairness, and proposes a corrected data pipeline to align with Ghana's Data Protection Act (Act 843).

## 2. Governance Review Card

| Section | Issue/Definition | Impact | Suggested Fix/Mitigation |
| :--- | :--- | :--- | :--- |
| **1. Data Quality Risk** | **Inconsistent Data Entry**: Customer details (like phone numbers and names) are stored in varying formats or sometimes contain missing fields. | **Model Inaccuracy**: Inconsistent input data leads to incorrect loan-scoring results, potentially rejecting creditworthy applicants or approving risky ones. | **Input Validation & Standardization**: Implement strict client-side and server-side validation; use automated cleaning scripts to standardize formats (e.g., E.164 for phone numbers). |
| **2. Legal & Compliance Risk** | **Data Classification**: **Sensitive** / Confidential. <br><br> **Governance Violation**: Absence of explicit user consent and lack of data minimization (collecting entire contact lists). | **Legal Sanctions**: Breach of Ghana's DPA (Act 843) leads to heavy fines and loss of operating license. | **Consent Management**: Implement a granular "Opt-in" consent screen and enforce Data Minimization by only collecting data essential for credit scoring. |
| **3. Bias & Fairness Risk** | **Source of Bias**: Demographic Proxy - The ML model may use variables (like location or phone type) that correlate with protected groups, leading to systemic bias. | **Ethical & Social Harm**: Systematically denying loans to specific demographic groups, leading to unfair financial exclusion. | **Fairness Monitoring**: Implement "Disparate Impact" testing to compare approval rates across different demographic groups and adjust model weights if bias is detected. |

### 2.1 Ethical Reporting Metric
*   **Metric to Monitor:** **Demographic Parity Ratio (DPR)** - The ratio of the loan approval rate for a protected demographic group compared to the approval rate for the "control" or majority group.
*   **Visualization Type:** **Grouped Bar Chart** showing approval rates vs. application rates across demographics.
*   **Why It Matters:** It provides a clear, transparent snapshot of whether the automated system is serving all segments of the population equitably.

## 3. Corrected Data Flow Diagram
![Corrected Data Flow Diagram](User%20Mobile%20App%20Data-2026-03-02-134641.png)

### 3.1 Annotations & Corrections
1.  **Data Minimization at Source**: The Mobile App now limits collection to essential data only. Collecting "entire contact lists" is removed.
2.  **Consent Management Integration**: Added a dedicated Consent Management Service between the API Gateway and the Raw Data DB.
3.  **Data Classification & Retention**: The Raw Data DB now applies classification tags and automated retention policies.
4.  **Transparency & Logging Service**: A Logging & Transparency Service is introduced to capture features and logic used by the ML model.
5.  **Data Masking & Anonymization**: PII is masked or anonymized before data enters the Analytics DB to protect customer privacy.

## 4. Summary of Review Process
The review process utilized the Data Lifecycle framework to trace information from collection to disposal. At the **Collection** stage, I identified "Excessive Collection" (entire contact lists) as a violation of the **Data Minimization** principle. By applying **Data Classification**, I categorized customer PII as **Sensitive**, which necessitated the introduction of Data Masking at the **Storage and Analytics** phases.

The proposed **Demographic Parity Ratio (DPR)** ensures ethical governance by shifting accountability from technical accuracy to social impact. This proactive reporting transforms "Black Box" automated decision-making into an auditable and transparent process, ensuring that the company’s growth does not come at the cost of financial exclusion or legal non-compliance. These corrections build a foundation of trust, vital for a fintech startup scaling in a regulated environment.
