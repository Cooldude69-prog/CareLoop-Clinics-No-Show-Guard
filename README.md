## PART 1, 3, AND 4 COLAB LINK BELOW
[click here](https://colab.research.google.com/drive/1cxBkBp-7lb8ubswT9mTxo0Fpno1Sp2gU?usp=sharing.md)

# CareLoop-Clinics-No-Show-Guard: PROJECT SUMMARY

### 1. Four-Line Summary (One finding per Part)
* **Part 1:** Ingested and cleaned 1,020 raw bookings, resulting in 990 valid records, 52 capped outliers, and 8 live-enriched climate records.
* **Part 2:** Identified through SQL that Hadapsar Care Centre (Pune) has the highest no-show rate at 27.3%, and that 31+ day lead times surge to a 60.5% no-show rate.
* **Part 3:** Built a Logistic Regression model achieving a Test ROC-AUC of 0.79 and selected a 0.35 threshold to flag high-risk bookings.
* **Part 4:** Extracted structured reasons using Gemini Flash and Pydantic validation, confirming that missed appointments due to forgetfulness strictly occurred when reminders were not sent.

---

### 2. Management Recommendation
CareLoop Clinics should prioritize automated front-desk interventions for appointments booked more than 15 days in advance, as lead times exceeding 15 days experience a severe no-show rate of 30.0% to 60.5%. By deploying the logistic regression risk model at a 0.35 probability threshold on the evening before each clinic day, the front desk can proactively target the 49 high-risk patients out of 198 test bookings. Because qualitative call-centre notes demonstrate that forgetting appointments is directly tied to a lack of automated text reminders (accounting for 2 of the missed visits in our sample), mandating SMS reminders combined with a quick follow-up call for high-lead-time bookings will capture missed slots efficiently while minimizing empty doctor schedules.

---

### 3. Model Selection & Performance
* **Chosen Model:** Logistic Regression (with `StandardScaler` inside a scikit-learn Pipeline).
* **Why Chosen:** It achieved the highest 5-fold cross-validated ROC-AUC on the training set (~0.77) compared to Decision Trees and Random Forests, while offering highly interpretable coefficients.
* **Test Performance:** Test ROC-AUC is **0.7918**, achieving a precision of **0.57** and a recall of **0.57** at the operational threshold of **0.35**.

---

### 4. Limitations
1. **Observational Data:** The data are observational, meaning estimated associations (such as reminder impact) do not definitively prove direct causation.
2. **Cleaning Leakage:** Statistical summaries like quartiles and IQR caps were computed on all 990 rows prior to the train/test split rather than restricted solely to training data.
3. **Random Splitting:** A random train/test split allows multiple bookings from the same patient ID to appear in both training and test partitions, rather than a strict past-to-future chronological split.
