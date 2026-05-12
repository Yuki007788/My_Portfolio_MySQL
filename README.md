# LGD Analysis: Strategic Defaults and the Credit Grade Paradox

This project explores the counter-intuitive relationship between Credit Grade and Loss Given Default (LGD) using Lending Club data. While credit ratings are the primary determinants of the Probability of Default (PD), this analysis investigates how borrower attributes bias the "economic rationality" of recovery once a default occurs.

## 📊 Motivation: The Correlation Between Credit Grade and LGD

In financial practice, the correlation between Grade and LGD remains a subject of ongoing debate. It is often assumed that higher-grade borrowers exhibit lower LGD due to stability; however, this study investigates a more nuanced hypothesis: **How do borrower attributes bias the recovery process?**

## 🔍 Key Focus Areas

*   **Information Asymmetry:** Do higher-grade borrowers utilize strategic options to manage default?
*   **Legal Resource Disparities:** How does access to legal/financial advice affect the final recovery rate?
*   **Systemic Bias:** Does the "Grade" influence the intensity or efficiency of the collection process?

---

## 🛠️ Methodology & Data Filtering

### Initial Anomalies
Initial analysis of the raw dataset revealed two major deviations from conventional theory:
1.  **Abnormally High LGD:** Consistently around 90% across all grades.
2.  **Inverse Correlation:** Grade A and B loans exhibited higher LGD than lower-grade loans.

### Maturity Analysis via ROI Trends
I hypothesized that these anomalies were driven by "noise" from Active Loans. To eliminate this bias, I analyzed ROI Trends to identify when the portfolio reached maturity.
*   **Transition Point:** February 2016 was identified as the cutoff where ROI consistently remains below the 100% threshold.
*   **Refinement:** The dataset was restricted to records issued prior to February 2016 using the following SQL logic:

```sql
WHERE DATE_FORMAT(issue_d, '%Y-%m') < '2016-02'
```

## 📈 Key Findings: The EAD Paradox

The refined data confirmed a significant contrast in repayment behavior prior to default (Exposure at Default - EAD):

*   **Grade A:** Outstanding principal at default is approximately **51.6%**.
*   **Grade G:** Outstanding principal at default is approximately **81.6%**.

**The Gap:** A 30 percentage point discrepancy exists, confirming that higher-grade borrowers repay a much larger portion of their principal before defaulting.

> **Note:** Despite this "head start," high-grade loans still result in higher LGD, reinforcing that the post-default recovery process—not the remaining balance—is the primary driver of loss for top-tier borrowers.

---

## 💡 The "Debtor's Recovery Bias" Hypothesis

The analysis reveals that the seemingly uniform LGD across grades is actually a result of two opposing dynamics:

### 1. High-Attribute Borrowers: Strategic Default
*   **Mechanism:** High-grade borrowers possess superior financial literacy and legal resources. They may intuitively grasp the "break-even point" where the cost of legal action for a creditor exceeds the potential recovery.
*   **Action:** They may opt for **Strategic Default**, utilizing legal means like personal bankruptcy to completely sever debt obligations.
*   **Result:** A higher Non-Recovery Rate (Total Loss) despite a lower initial EAD.

### 2. Low-Attribute Borrowers: Passive Default
*   **Mechanism:** Borrowers with fewer legal resources tend to be more passive toward collection efforts or wage garnishments.
*   **Action:** Recovery consists of small, fragmented payments over a longer duration.
*   **Result:** A lower Non-Recovery Rate; the persistence of the process effectively averts a total loss scenario.

---

## 🏁 Final Synthesis

Standard credit scoring models often underestimate the binary nature of recovery in high-grade segments. This project suggests that for high-attribute portfolios, LGD estimation requires a model that accounts for:
*   Strategic behavior of debtors (Tail Risk).
*   Economic thresholds of collection costs.
*   Legal literacy of the borrower.

Instead of relying on simple grade-based averages, risk management should focus on the strategic barriers to recovery that emerge immediately after a high-grade default event.

---

## 🛠️ Tech Stack

*   **Language:** Python
*   **Libraries:** Pandas, Matplotlib, Seaborn, SQLAlchemy
*   **Database:** MySQL (Data Filtering & Refinement)