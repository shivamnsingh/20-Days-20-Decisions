# "20 Days, 20 Decisions" — Full Roadmap (v2, reordered)
### Structure: Applied Work First → Concepts → Business Metrics → Dashboards → Capstone

This version leads with your five existing notebooks (strongest work, hooks an audience early),
then teaches the underlying concepts as callbacks, then layers on business metrics/frameworks,
then converts the strongest analyses into Power BI dashboards, and closes with one integrated
capstone case study.

**One dataset thread:** Online Retail II is introduced on Day 6 and deliberately reused on
Days 11, 12, 13, 16, 19, and 20 — this is what makes the back half of the series feel like one
consulting engagement instead of disconnected posts.

**Repo structure:** one repo `20days20decisions`, one folder per day, root `README.md` as a
clickable table of contents, `capstone/` folder for Day 20.

---

## Quick Reference

| Day | Title | Type |
|---|---|---|
| 1 | Used Cars — the accident insight | Applied Analysis |
| 2 | IPL Toss — does it actually matter? | Applied Analysis |
| 3 | Product Demand — the accounting bug | Applied Analysis |
| 4 | Startup Success — recall over accuracy | Applied Analysis |
| 5 | Divorce Prediction — when a model is "too good" | Applied Analysis |
| 6 | Pareto Principle | Concept |
| 7 | Correlation vs. Causation | Concept |
| 8 | Simpson's Paradox | Concept |
| 9 | A/B Testing | Concept |
| 10 | Forecasting Basics | Concept |
| 11 | Customer Lifetime Value (CLV) | Business Metric |
| 12 | Cohort Analysis | Business Metric |
| 13 | RFM Analysis | Business Metric |
| 14 | Funnel Analysis | Business Metric |
| 15 | CAC, Churn Rate & NPS | Business Metric |
| 16 | Dashboard: Sales & CLV Executive View | Power BI |
| 17 | Dashboard: Retention & Cohort | Power BI |
| 18 | Dashboard: Funnel & Conversion | Power BI |
| 19 | Dashboard: Interactive RFM Segmentation | Power BI |
| 20 | Capstone: Full Case Study | Capstone |

---

## Days 1–5: Applied Analysis (your existing notebooks)

### Day 1 — Used Cars: the accident insight
**Hook:** "A car with an accident on record loses $20,000 in resale value — more than most people realize. Here's the data."
**Deliverable:** EDA + Random Forest price driver analysis, headline reframed around the accident-history finding rather than R².
**Business example:** Dealership pricing / insurer total-loss decisions.
**Dataset:** [Used Car Price Dataset (Kaggle)](https://www.kaggle.com/datasets/taeefnajib/used-car-price-prediction-dataset)
**GitHub:** `day01-used-cars` — notebook + `RESULTS.md` stating the accident gap and honest model limitation (R²=0.68, MAE ~$13.8k).
**Portfolio idea:** Add the missing takeaways section; end on the Decision Card, not a bare chart.
**Decision Card:**
- Insight: No-accident listings average $45,823 vs $25,862 with accident history ($19,961 gap).
- Recommendation: Weight accident disclosure more heavily than brand alone in pricing.
- Confidence: Medium-high on the gap; low on precise price prediction (32% of variance unexplained).
- Next: Control for brand mix within accident/no-accident groups — currently a raw comparison.
**CTA:** "Would you pay full price for a car with a disclosed accident if the discount was smaller than $20k?"

### Day 2 — IPL Toss: does it actually matter?
**Hook:** "I ran the same hypothesis test 8 times and found 1 'significant' result. Then I remembered why that's exactly what random noise looks like."
**Deliverable:** Chi-square testing across IPL venues, corrected for multiple comparisons.
**Business example:** Broadcasters/fantasy-sports platforms deciding whether toss result belongs in win-probability models.
**Dataset:** [IPL Complete Dataset (Kaggle)](https://www.kaggle.com/datasets/patrickb1912/ipl-complete-dataset-20082020)
**GitHub:** `day02-ipl-toss` — add `multiple_comparisons_fix.ipynb` with before/after Bonferroni correction.
**Portfolio idea:** Apply Bonferroni correction (α/8 = 0.00625) — highest-value single edit in the set.
**Decision Card:**
- Insight: Overall toss-win rate 52.2% (p=0.234). One venue looked significant (p=0.02) but that's within the false-positive rate expected from testing 8 venues; doesn't survive correction.
- Recommendation: Don't weight toss result in win-probability or fantasy-scoring models.
- Confidence: High — stable before and after correction.
- Next: Test whether toss *decision* (bat/field), independent of who won it, matters.
**CTA:** "Have you seen a 'finding' in your own work that was actually multiple-testing noise?"

### Day 3 — Product Demand: the accounting bug
**Hook:** "One line of pandas code almost turned every return into a phantom sale. Here's the bug — and the $ impact of getting it wrong."
**Deliverable:** Data cleaning case study (accounting notation, duplicate-order handling, per-product outliers) + 4-week moving average forecast, tone cleaned up.
**Business example:** Inventory/working-capital planning.
**Dataset:** [Historical Product Demand (Kaggle)](https://www.kaggle.com/datasets/felixzhao/productdemandforecasting)
**GitHub:** `day03-demand-forecasting` — split into `01_cleaning.ipynb` and `02_forecast.ipynb`.
**Portfolio idea:** Biggest tone rewrite needed of the five (drop casual phrasing); add $-quantified cost estimate for the MAE.
**Decision Card:**
- Insight: `(1000)` = -1000 accounting notation, bracket-stripped without sign-flip would silently convert returns into phantom sales; 634k "duplicate" rows were legitimate repeat orders.
- Recommendation: Forecast at category level (median product has only ~150 data points); use 4-week over 12-week MA for this volatile category.
- Confidence: Medium — MA is a floor, not a ceiling.
- Next: Test exponential smoothing; check if Warehouse A's missing dates undercounts true demand.
**CTA:** "What's a data-cleaning bug you almost shipped without noticing?"

### Day 4 — Startup Success: recall over accuracy
**Hook:** "My model was 75% accurate at predicting startup success. It also missed 42% of the startups that actually failed. Here's why that matters more."
**Deliverable:** Random Forest with Gini vs. permutation importance comparison, reframed around the recall gap.
**Business example:** Accelerator/VC screening — the cost of missing a failure vs. a success.
**Dataset:** [Startup Success Prediction (Kaggle)](https://www.kaggle.com/datasets/manishkc06/startup-success-prediction)
**GitHub:** `day04-startup-success` — add `threshold_tuning.ipynb`.
**Portfolio idea:** Surface the 0.58 recall on failures explicitly instead of leading with "solid signal."
**Decision Card:**
- Insight: Recall on failed startups only 58%; `relationships` (network size) is the most robust predictor across two importance methods.
- Recommendation: Don't deploy as-is for screening; tune threshold for recall, not accuracy.
- Confidence: Medium — network signal is robust; dataset is US-centric, pre-2014.
- Next: Threshold tuning; test on a more recent cohort.
**CTA:** "If you were screening startups, would you rather miss a future unicorn or greenlight a future failure?"

### Day 5 — Divorce Prediction: when a model is "too good"
**Hook:** "This model hit 94% accuracy predicting divorce from a survey. That's not impressive — it's a red flag."
**Deliverable:** Classification with leakage-safe feature selection (train/test split before feature selection), reframed around questioning the high accuracy.
**Business example:** Relationship-counseling app intake design.
**Dataset:** [Divorce Predictors Dataset (Kaggle)](https://www.kaggle.com/datasets/andrewmvd/divorce-prediction)
**GitHub:** `day05-divorce-prediction` — feature the leakage-safe split prominently in the README.
**Portfolio idea:** Explain *why* accuracy is inflated (top predictors correlate ≥0.90 with outcome).
**Decision Card:**
- Insight: 11 of 54 questions carry nearly all predictive signal.
- Recommendation: A 54-question intake could be cut to ~11 questions without losing signal.
- Confidence: Low-medium on general "prediction" claim (n=170); high on the "fewer questions, same signal" recommendation.
- Next: Validate on a larger, independent sample.
**CTA:** "Have you ever seen a model that was 'too accurate' to trust?"

---

## Days 6–10: Statistical Concepts

### Day 6 — Pareto Principle
**Hook:** "18% of customers in this dataset generate 74% of revenue. If you're spending equally on all of them, you're leaving money on the table."
**Deliverable:** Pareto analysis — cumulative % revenue vs. cumulative % customers, 80/20 line marked on chart.
**Business example:** Retail customer prioritization — account management vs. automated marketing.
**Dataset:** [Online Retail II (Kaggle)](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) — introduced here, reused through Day 20.
**GitHub:** `day06-pareto-retail` — reusable `pareto_chart()` function.
**Portfolio idea:** Make the cumulative curve the hero image with the crossover point annotated directly.
**Decision Card:**
- Insight: State the real concentration split from the data (don't assume literal 80/20).
- Recommendation: Segment spend to match — proactive outreach for top tier, automated for long tail.
- Confidence: High on the pattern; check whether the exact ratio holds before generalizing.
- Next: Whether the same customers stay in the top tier month over month (seeds Day 12 cohort work).
**CTA:** "Guess before you scroll: what % of customers drives 80% of revenue — more or less than 20%?"

### Day 7 — Correlation vs. Causation
**Hook:** "Marketing spend and sales moved together for 18 months straight. I still wouldn't tell a CMO to increase the budget based on that alone."
**Deliverable:** Correlation between ad spend and sales, testing for a seasonality confound before any causal claim.
**Business example:** Marketing budget allocation.
**Dataset:** [Advertising Dataset (Kaggle)](https://www.kaggle.com/datasets/bumba5341/advertisingcsv)
**GitHub:** `day07-correlation-causation` — naive vs. seasonality-controlled correlation side by side.
**Portfolio idea:** Pin this post; callback explicitly to Day 2's statistical-trap post.
**Decision Card:**
- Insight: Raw correlation is strong; a large share disappears once seasonality is controlled for (state the real number).
- Recommendation: Don't greenlight budget increases off correlation alone — isolate the seasonal component or run a real test.
- Confidence: High on the general trap; medium on exact magnitude.
- Next: A proper A/B or geo-holdout test (sets up Day 9).
**CTA:** "What's a correlation you've seen mistaken for causation at work?"

### Day 8 — Simpson's Paradox
**Hook:** "On Day 1 I said Porsches sell for way more than Toyotas on average. That's true — and also misleading. Here's what happens once you control for mileage and model year."
**Deliverable:** Re-segment Day 1's used-cars dataset by mileage/year bands within brand.
**Business example:** Dealership pricing — is "brand premium" real, or partly a listing-mix artifact?
**Dataset:** Same `used_cars.csv` from Day 1 (deliberate reuse).
**GitHub:** Add `day08-simpsons-paradox.ipynb` inside the `day01-used-cars` folder.
**Portfolio idea:** Before/after chart pair (aggregate vs. segmented) — the single strongest visual for this concept.
**Decision Card:**
- Insight: State whichever is true once run — premium holds, shrinks, or reverses after controlling for mileage/year.
- Recommendation: Weight mileage/year more heavily relative to brand if the premium shrinks.
- Confidence: Medium — single dataset, one point in time.
- Next: Check for the same reversal when segmenting by accident history.
**CTA:** "Always ask: what happens to this comparison if I control for one more variable?"

### Day 9 — A/B Testing
**Hook:** "I designed an A/B test the way most people actually run them — then showed why the 'winning' result probably wouldn't replicate."
**Deliverable:** A/B test design — sample size/power check, significance test, discussion of what "significant" does and doesn't guarantee.
**Business example:** Website CTA button test.
**Dataset:** [A/B Testing Dataset (Kaggle)](https://www.kaggle.com/datasets/zhangluyuan/ab-testing)
**GitHub:** `day09-ab-testing` — small reusable significance/sample-size calculator function.
**Portfolio idea:** Open with a callback: "Day 2 taught me to distrust a single significant p-value — here's the test design that avoids that trap from the start."
**Decision Card:**
- Insight: State the winning variant, p-value, and sample size.
- Recommendation: Only roll out if the sample size could reliably detect an effect of that size.
- Confidence: Depends on the power calculation — make that the headline, not the p-value.
- Next: Replicate in a second independent test window before permanent rollout.
**CTA:** "Would you ship a change based on one significant A/B test, or wait for it to replicate?"

### Day 10 — Forecasting Basics
**Hook:** "I forecasted 4 weeks of product demand using nothing but a moving average — then showed why the error compounds the further out you look."
**Deliverable:** Callback to Day 3 — reframe the MA forecast as a Forecasting Basics explainer, with an error-growth-by-horizon visual.
**Business example:** Retail/inventory demand planning.
**Dataset:** Same Historical Product Demand dataset from Day 3.
**GitHub:** Add `day10-forecasting-basics.ipynb` inside the `day03-demand-forecasting` folder.
**Portfolio idea:** Chart showing forecast error growing week+1 through week+4.
**Decision Card:**
- Insight: Week+1 forecast is most trustworthy; error compounds because each later forecast is built on the prior forecast, not real data.
- Recommendation: Treat only 1-week-ahead as tightly actionable; build wider safety-stock buffers for week 3–4.
- Confidence: High on the qualitative pattern; medium on exact magnitude.
- Next: Compare against exponential smoothing for slower error growth.
**CTA:** "How far ahead would you trust a forecast for your own business?"

---

## Days 11–15: Business Metrics & Frameworks

### Day 11 — Customer Lifetime Value (CLV)
**Hook:** "Not all customers are worth chasing. One CLV calculation told me exactly which ones are — and which cost more to keep than they're worth."
**Deliverable:** CLV calculation and segmentation.
**Business example:** Retention budget allocation.
**Dataset:** Online Retail II (reused from Day 6).
**GitHub:** `day11-clv-retail` — reusable CLV calculator function.
**Portfolio idea:** Package the calculator as a standalone, reusable template.
**Decision Card:**
- Insight: State top-decile CLV contribution relative to acquisition cost.
- Recommendation: Prioritize retention spend on top-CLV segment; automate/deprioritize the rest.
- Confidence: Medium — sensitive to time horizon and churn assumptions.
- Next: Compare simple historical CLV vs. a predictive CLV model.
**CTA:** "Would you rather have 1,000 low-CLV customers or 100 high-CLV ones?"

### Day 12 — Cohort Analysis
**Hook:** "I grouped customers by the month they first purchased — and found exactly when they stop coming back."
**Deliverable:** Cohort retention heatmap.
**Business example:** Retention tracking by signup/first-purchase month.
**Dataset:** Online Retail II (reused).
**GitHub:** `day12-cohort-analysis` — reusable cohort heatmap function.
**Portfolio idea:** Explicit callback to Day 6's "what I'd check next."
**Decision Card:**
- Insight: State the steepest drop-off point found.
- Recommendation: Focus onboarding/retention interventions at that specific point.
- Confidence: Medium-high — pattern consistent across cohorts.
- Next: Check whether acquisition channel affects retention curve shape.
**CTA:** "What's your guess — do most customers churn in month 1, or later?"

### Day 13 — RFM Analysis
**Hook:** "Not every customer deserves the same email. RFM segmentation told me exactly who to prioritize — and who to stop emailing."
**Deliverable:** RFM (Recency, Frequency, Monetary) scoring and segmentation.
**Business example:** Marketing segmentation.
**Dataset:** Online Retail II (reused).
**GitHub:** `day13-rfm-segmentation` — reusable scoring template.
**Portfolio idea:** Segment table becomes a reusable template others can apply to their own data.
**Decision Card:**
- Insight: State segment sizes (e.g., Champions vs. At-Risk vs. Lost).
- Recommendation: Different messaging/cadence per segment — win-back for At-Risk, VIP treatment for Champions.
- Confidence: High on segment definitions; medium on whether the intervention actually moves behavior.
- Next: Run the win-back campaign as an A/B test (callback to Day 9).
**CTA:** "Champions, At-Risk, or Lost — which segment do you think is biggest?"

### Day 14 — Funnel Analysis
**Hook:** "70% of users dropped off at one single step. Funnel analysis showed exactly where — and why fixing it matters more than driving more traffic."
**Deliverable:** Funnel drop-off analysis.
**Business example:** E-commerce checkout funnel.
**Dataset:** [Google Analytics Sample / GA4 BigQuery public dataset (Kaggle)](https://www.kaggle.com/datasets/bigquery/google-analytics-sample)
**GitHub:** `day14-funnel-analysis`.
**Portfolio idea:** This funnel visual becomes the base for Day 18's dashboard.
**Decision Card:**
- Insight: State the exact step with the biggest drop-off.
- Recommendation: Fixing that step likely has more ROI than increasing top-of-funnel traffic.
- Confidence: Medium — correlational; needs a test to confirm the causal fix.
- Next: A/B test a simplified version of that step (callback to Day 9).
**CTA:** "Where do you think most users drop off in an online checkout?"

### Day 15 — CAC, Churn Rate & NPS
**Hook:** "This business looked like it was growing. Once I combined CAC with churn rate, the real story was it's losing money on every new customer."
**Deliverable:** Combined CAC + churn rate unit economics analysis.
**Business example:** SaaS unit economics.
**Dataset:** [Telco Customer Churn (Kaggle)](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)
**GitHub:** `day15-unit-economics`.
**Portfolio idea:** Short "unit economics 101" write-up tied to this post.
**Decision Card:**
- Insight: State actual CAC payback period vs. average customer lifetime given churn rate.
- Recommendation: If payback exceeds lifetime, growth spend is destroying value — fix CAC or retention before scaling.
- Confidence: Medium — based on averages; real decision needs cohort-level economics.
- Next: Check whether NPS correlates with churn speed (ties to Day 12).
**CTA:** "Would you keep scaling acquisition spend on a business with these unit economics?"

---

## Days 16–19: Power BI Dashboards

### Day 16 — Dashboard: Sales & CLV Executive View
**Hook:** "I turned 5 days of analysis into one dashboard a VP could actually use in 30 seconds."
**Deliverable:** Executive dashboard combining Day 6 Pareto + Day 11 CLV findings.
**Business example:** Retail exec summary view.
**Dataset:** Online Retail II.
**GitHub:** `day16-exec-dashboard` — `.pbix` file + walkthrough GIF.
**Portfolio idea:** Publish via Power BI Service and embed the public link.
**Decision Card:** Dashboard surfaces top-CLV segment and Pareto concentration in one filterable view — built so a VP can self-serve daily without touching Python.
**CTA:** "What's the ONE metric you'd want on your homepage dashboard?"

### Day 17 — Dashboard: Retention & Cohort
**Hook:** "A churn dashboard that tells you WHO to call today, not just the churn rate."
**Deliverable:** Combined cohort (Day 12) + churn (Day 15) dashboard.
**Business example:** SaaS retention ops.
**Dataset:** Telco Churn + Online Retail cohort data.
**GitHub:** `day17-retention-dashboard`.
**Portfolio idea:** Add drill-through to individual customer level — mention this explicitly in the post, it signals UX thinking.
**Decision Card:** Flags specific at-risk segments an ops team could act on today, not just a lagging churn %.
**CTA:** "Would your team actually use a dashboard like this daily?"

### Day 18 — Dashboard: Funnel & Conversion
**Hook:** "A funnel dashboard that shows exactly where users — and marketing budget — are leaking out."
**Deliverable:** Combined funnel (Day 14) + CAC (Day 15) dashboard.
**Business example:** Marketing/growth ops.
**Dataset:** GA4 sample + Telco/Advertising data.
**GitHub:** `day18-funnel-dashboard`.
**Portfolio idea:** Add a "what I'd build next" note — shows product thinking, not just BI execution.
**Decision Card:** Pinpoints the exact funnel stage burning the most budget relative to conversion lift.
**CTA:** "Tag someone in marketing ops who needs this."

### Day 19 — Dashboard: Interactive RFM Segmentation
**Hook:** "RFM segmentation, but clickable — filter by segment and watch revenue shift live."
**Deliverable:** Interactive RFM dashboard (Day 13).
**Business example:** Customer segmentation for marketing.
**Dataset:** Online Retail II.
**GitHub:** `day19-rfm-dashboard`.
**Portfolio idea:** Feature this dashboard first in your portfolio — highest "wow factor" of the four.
**Decision Card:** Lets a marketer self-serve "which segment should I target this week" without a data request.
**CTA:** "Which segment would you spend your next marketing rupee on?"

---

## Day 20: Capstone

**Hook:** "20 days ago I could run a t-test. Today I can tell a VP what to do with the results. Here's the full case study."
**Deliverable:** One integrated case study on Online Retail II — chains Day 6 (Pareto) → Day 11 (CLV) → Day 12 (Cohort) → Day 13 (RFM) → Day 16/19 (dashboards) into a single "consulting engagement" narrative, written as a client deliverable, plus a one-page executive summary PDF (zero code, business language only — the exact artifact a Big 4 analyst would produce for a partner).
**Business example:** Full consulting engagement simulation for one retail business.
**Dataset:** Online Retail II.
**GitHub:** `capstone/` folder in the main repo, linking back to every day that fed into it.
**Portfolio idea:** This is your pinned LinkedIn post, resume link, and GitHub README hero.
**Decision Card:** Summarize the top 3 recommendations across the whole series in one place — this is the single artifact you want a recruiter to open first.
**CTA:** "If you're hiring Data Analysts or know someone who is — I'd love an intro. Here's 20 days of proof I can do the job."
