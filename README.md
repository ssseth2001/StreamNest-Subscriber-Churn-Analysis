# StreamNest-Subscriber-Churn-Analysis
An end-to-end churn analysis for a fictional streaming service: messy raw data cleaned in Excel and Power Query, a churn definition built from billing behavior, and an interactive Excel dashboard that answers eight business questions and ends in costed recommendations.
 
> **Note:** StreamNest is a synthetic practice dataset (5,000 subscribers, 30,000+ payment attempts, Jan 2024 to Jun 2026). It was built to practice a full analytics workflow, so patterns are cleaner than real-world data.
 
<img width="721" height="650" alt="dashboard" src="https://github.com/user-attachments/assets/76bebef8-1ad6-41d7-ac41-d8d07b568b3d" />

 
**Interactive version:** download `[streamnest_Project.xlsx](streamnest_Project.xlsx)` and open it in desktop Excel to use the slicers (Plan, Billing, Country, Signup source). Every KPI, chart, and table updates when you click a filter.
 
---
 
## The business problem
 
**Half of StreamNest's subscribers have left, and leadership wants to know what to do about it.**
 
To answer that, the analysis looks at four questions:
 
1. **Who is leaving?** (plan, billing type, signup channel, engagement)
2. **Why and when do they leave?** (cancel reasons, timing, failed payments)
3. **What is it costing us, and where should we grow?** (lost revenue, country value)
4. **What should we do, and what is it worth?** (costed recommendations)
---
 
## Tools
 
- **Excel** (formulas, pivot tables, pivot charts, slicers, conditional formatting, linked pictures)
- **Power Query** (cleaning, deduplication, joins, Group By, custom columns in M)
---
 
## Data cleaning
 
Raw files: `streamnest_subscribers.csv` (5,085 rows) and `streamnest_payments.csv` (30,501 rows).
 
| Issue found | Action |
|---|---|
| 70 duplicate subscriber rows, 150 duplicate payment rows | Removed |
| 15 internal test accounts and their payments | Removed |
| 55 orphan payments with no matching subscriber | Removed (found with a Left Anti join) |
| Countries spelled many ways (USA, U.S., Brasil, DE...) | Mapped to one name per country |
| Plan, billing, device, free-trial values in many formats | Standardized |
| Impossible ages (0, 7, 999) and watch hours (negative, 9999) | Set to blank, row kept |
| Prices stored as text ($12.99, 12.99 USD) | Converted to numbers |
| Mixed date formats, swapped signup/cancel dates, future signups | Converted, fixed, or blanked |
| Free-text cancel reasons | Grouped into 6 categories; 77 blanks labeled "No reason given" |
| 81 annual subscribers labeled "Monthly" | Detected by payment size (average payment > 5x monthly price) and relabeled |
| Cancel dates after the June 30, 2026 snapshot | Kept as churned, excluded from the monthly trend |
 
**Result:** 5,000 subscribers and 30,296 payment attempts (27,814 successful, 2,206 failed, 276 refunded).
 
---
 
## Defining churn
 
The data has no churn column, so churn was defined from behavior, measured at the snapshot date (June 30, 2026):
 
- **Voluntary churn:** the subscriber has a cancel date
- **Involuntary churn:** no cancel date, but no successful payment for more than 45 days (monthly plans) or 380 days (annual plans)
- **Active:** everyone else, including people still on a free trial
---
 
## Key findings
 
| Question | Finding |
|---|---|
| How big is the problem? | **50% churn** (2,487 of 5,000). 1 in 4 churned subscribers never chose to leave: **645 left because a payment failed.** |
| Who leaves? | Students churn **70%** vs Family **37%**. Monthly churns **55%** vs Annual **27%**. Students have no annual option. |
| Which channels? | Promo signups churn **64%**, referrals **40%**. Free-trial signups churn **57%** vs **44%**. |
| Engagement | Subscribers watching under 5 hours a month churn most. |
| Why? | **Too expensive** is the top cancel reason (31% of cancellations); "not using it" and "nothing to watch" add 43%. |
| When? | Cancellations spike every **May** (end of school year) and after the **January 2025 US price increase**. |
| Failed payments | Phone-bill and debit card payments fail about **3x** more than PayPal. Failed payments cost at least **$86K a year**. Those subscribers watched as much as active users (~25 hrs/month). |
| Where to grow | **India** is #2 in subscribers but lowest in revenue per subscriber ($30 vs $128 in the US) with the highest churn. **UK, Germany, Canada** are small but high value (~$115 to $119 per subscriber). |
 
---
 
## Recommendations and estimated impact
 
| Action | Est. subscribers kept | Est. revenue / year |
|---|---|---|
| 1. Fix failed payments: grace period, one-tap card update, reminders by email, text, and in-app | 323 | $43K |
| 2. Convert free trials: show what's coming and offer a discount before the trial ends | 135 | $19K |
| 3. Move monthly subscribers to annual, including a new student annual plan | 114 | $13K |
| 4. Help people find shows: personal picks for light viewers, using the existing catalog | 78 | $10K |
| **Total** | **~650** | **~$85K** |
 
Together these could bring churn from **50% to about 37%**.
 
**Assumptions:** half of failed payments recovered, half of the free-trial churn gap closed, 10% of monthly subscribers switch to annual, 10% of disengaged cancellers kept. Groups can overlap, so the total is an upper estimate.
 
**Growth idea:** a carrier bundle partnership (similar to phone plans that include a streaming service) would remove failed payments and tie the subscription to a service people rarely cancel.
 
---
 
## Limitations and next steps
 
- The data shows *which* payment methods fail, not *why*. Payment processor decline codes would confirm the cause.
- Country analysis covers existing customers only, not market size. Population, internet usage, and competitor pricing would be needed to size each market.
- Content licensing costs are not in the data, so new content was not recommended without a cost-benefit check

## Files
 
| File | Description |
|---|---|
| [streamnest_Project.xlsx](streamnest_Project.xlsx) | Full workbook: cleaned data, Power Query steps, pivots, and the interactive dashboard |
| [StreamNest_Cleaning_Log.xlsx](StreamNest_Cleaning_Log.xlsx) | Every cleaning step and how many rows it affected |
| [data/streamnest_subscribers.csv](data/streamnest_subscribers.csv) | Raw subscriber data (messy) |
| [data/streamnest_payments.csv](data/streamnest_payments.csv) | Raw payment data (messy) |
| [data/README_data_dictionary.txt](data/README_data_dictionary.txt) | Column descriptions |
