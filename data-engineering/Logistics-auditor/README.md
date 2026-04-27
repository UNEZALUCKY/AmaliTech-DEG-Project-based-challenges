# 🚚 Last Mile Logistics Auditor — Veridi Logistics
**AmaliTech Data Engineering Challenge — Project 1**

---

## A. Executive Summary

An audit of **~100,000 delivered orders** from the Olist Brazilian E-Commerce dataset reveals that a significant percentage of deliveries arrive later than the estimated date provided to customers, directly confirming the CEO's suspicion of systematic over-promising. The problem is strongly regional — remote northern and northeastern Brazilian states show disproportionately higher late delivery rates compared to southeastern states near major distribution centers. Statistical analysis proves a clear causal link between delivery lateness and customer review scores: on-time customers rate their experience significantly higher than customers who receive super-late orders. A seller-level risk scoring system further reveals that the problem is not solely a regional logistics failure — specific high-risk sellers consistently set unrealistic ETAs, providing Veridi with targeted intervention options beyond system-wide logistics reform.

---

## B. Project Links

| Deliverable | Link |
|---|---|
| 📓 Notebook (Google Colab) | [https://colab.research.google.com/drive/1hBHuvKl_vPyBLldeUUv3ueWdc6SVfI-6?usp=sharing] |
| 📊 Dashboard (Looker Studio) | [https://datastudio.google.com/reporting/a7c7939b-be49-4193-8a57-dde151735bb7] |
| 🎞️ Presentation (Slides) | [https://docs.google.com/presentation/d/1-Y1E3LURQo1bICamZ-NTjrLpOoklrUJ7CtJWGBWGDeE/edit?usp=sharing] |
| 🎥 Video Walkthrough (Optional) | [Add your YouTube link here] |


---

## C. Technical Explanation

### Data Cleaning
- **Date parsing:** All 5 timestamp columns converted from string to `datetime64` using `pd.to_datetime()`.
- **Deduplication of reviews:** The reviews table had multiple entries per `order_id` (1-to-many relationship). Resolved by sorting on `review_answer_timestamp` descending and calling `.drop_duplicates(subset='order_id', keep='first')` — retaining the most recent review per order.
- **Row duplication check:** Verified after each join that `master.shape[0] == orders.shape[0]` (left joins only). No duplication occurred.
- **Excluded orders:** Orders with `order_status` of `canceled` or `unavailable`, or with null `order_delivered_customer_date` / `order_estimated_delivery_date`, were excluded from the delay analysis and flagged separately. These represent ~3% of the dataset.
- **Outlier handling:** No extreme outliers were found in the delay distribution; data was clipped to ±30 days for visualization purposes only.

### Candidate's Choice: Seller Risk Score
**What:** A composite risk score (0–100) per seller, weighted as:
- 40% Late Delivery Rate
- 35% Inverse Average Review Score (penalty for low ratings)
- 25% Super-Late Delivery Rate (>5 days late)

**Why it matters:** The CEO's instinct was that ETA promises are the root problem. This analysis proves that the issue is partly *seller-driven* — some sellers consistently over-promise delivery windows regardless of geography. By identifying "High Risk" sellers, Veridi can take targeted action (retraining, contractual penalties, or removal) without overhauling the entire national logistics network. This is a more cost-effective and precise intervention.

---


