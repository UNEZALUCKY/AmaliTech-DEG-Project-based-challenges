# 🥗 The "Sugar Trap" — Market Gap Analysis
**AmaliTech Data Engineering Challenge — Project 2**

---

## A. Executive Summary

An analysis of over **500,000 food products** from the Open Food Facts global database reveals a striking market imbalance in the snack sector: the vast majority of products cluster in the "Low Protein / High Sugar" quadrant, while the "High Protein / Low Sugar" space — the true Blue Ocean — is severely under-served. The biggest opportunity lies in the **Protein & Health Bars** and **Nuts & Seeds** categories, where demand signals for health-forward products are highest but current supply is limited. Ingredient analysis of the best-performing products in the target zone consistently highlights **whey, peanuts, and soy protein** as the foundational protein sources. Additionally, cross-referencing with official Nutri-Score grades confirms that a product targeting the Blue Ocean quadrant can realistically achieve a Grade A or B rating, making it commercially viable in regulated European markets where health labeling is mandatory.

---

## B. Project Links

| Deliverable | Link |
|---|---|
| 📓 Notebook (Google Colab) | [Add your Colab link here] |
| 📊 Dashboard (Streamlit Cloud) | [Add your Streamlit link here] |
| 🎞️ Presentation (Slides) | [Add your Google Slides / PDF link here] |
| 🎥 Video Walkthrough (Optional) | [Add your YouTube link here] |

> ⚠️ **Before submitting:** Test all links in an Incognito/Private browser window to verify public access.

---

## C. Technical Explanation

### Data Cleaning
- **Selective loading:** The full Open Food Facts dataset exceeds 3GB. Only the first 500,000 rows were loaded using `nrows=500_000`, and only 12 relevant columns were selected via `usecols` — reducing memory usage by ~90%.
- **Missing values:** Rows missing `product_name`, `sugars_100g`, or `proteins_100g` were dropped, as these are the three essential fields for the analysis. Other nutrition columns with missing values (fiber, fat) were retained and handled per-chart.
- **Impossible value filtering:** A hard biological constraint was applied — no nutrient can exceed 100g per 100g of product. Any row violating this was removed as a data entry error.
- **Outlier removal:** Z-score filtering (threshold: 4 standard deviations) was applied to `sugars_100g` and `proteins_100g` to remove extreme outliers that survive the hard-limit filter.
- **Category parsing:** The `categories_tags` field is a comma-separated string with language prefixes (e.g., `en:snacks`). These were parsed into lists, language prefixes stripped, and keywords matched using a priority-ordered dictionary to assign a single `primary_category` per product. Priority ordering ensures that a product tagged as both "protein bar" and "chocolate" is correctly classified as a protein bar.

### Candidate's Choice: Nutri-Score vs Market Positioning
**What:** A cross-tabulation of official Nutri-Score grades (A–E) against the Sugar/Protein quadrant for each product, shown as a stacked bar chart.

**Why it matters:** Several European countries (France, Germany, Belgium) already mandate Nutri-Score labels on packaged food, and EU-wide adoption is progressing. A new product that targets the High Protein / Low Sugar quadrant must *also* achieve a Nutri-Score of A or B to succeed at retail in these high-value markets. This analysis shows that the Blue Ocean quadrant strongly correlates with Grade A and B scores — meaning the nutritional strategy and the regulatory strategy are aligned. This de-risks the product launch and provides the R&D team with a concrete nutritional benchmark that satisfies both market positioning and compliance goals simultaneously.

---

## Pre-Submission Checklist

- [ ] GitHub repo is **Public** (verify in Incognito mode)
- [ ] `.ipynb` notebook file uploaded
- [ ] HTML or PDF export of notebook uploaded
- [ ] Raw dataset CSV **NOT** committed (added to `.gitignore`)
- [ ] Code uses **relative paths only** (no `C:/...` paths)
- [ ] Dashboard link is **publicly accessible** (no login required)
- [ ] Presentation link is **publicly accessible**
- [ ] README updated with Executive Summary and links
- [ ] All 4 User Stories completed
- [ ] Candidate's Choice completed and explained
