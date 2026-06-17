# Azure Pricing Models — What a Three-Tier Web App Actually Costs
This is the coursework submission for the **Azure Pricing Models Learning Program**. It walks through building a monthly cost estimate for a three-tier web application hosted in West Europe, then runs a set of what-if comparisons across billing approaches (pay-as-you-go vs Reserved Instances), licensing (Azure Hybrid Benefit), storage redundancy (LRS vs GRS), and IaaS vs PaaS choices.

## The bottom line
| | USD/month |
|---|---|
| Pay-as-you-go baseline | **$679.58** |
| Optimized (3-yr RI + Hybrid Benefit + LRS) | **$421.96** |
| Saving | **$257.61 (37.9%)** |
The single biggest lever turned out to be stacking Azure Hybrid Benefit on top of a 3-year reservation, which knocks 78.6% off the web-tier VM cost alone, with no changes to the architecture itself.

## What's in this repo
```
azure-pricing-models/
├── README.md
├── docs/
│   ├── ARCHITECTURE.md            # Components, SKU choices, usage assumptions
│   └── OPTIMIZATION_ANALYSIS.md   # What-if scenario results and discussion
└── estimate/
    └── azure-cost-estimate-west-europe.xlsx   # Cost estimate report (3 sheets)
```
The spreadsheet is split into three sheets. `Assumptions` lists every unit price along with where it came from and when it was captured (the blue cells are the editable inputs). `Cost Estimate` works like a line-item calculator for the baseline. `Optimization Scenarios` runs the five what-if comparisons using live formulas, so updating a price on the Assumptions sheet ripples through the whole workbook automatically.

## Regenerating the numbers from the official Azure calculator
The prices baked into the workbook are USD list prices captured on 11 June 2026, and they won't stay accurate forever. To pull a fresh, official export:
1. Go to https://azure.microsoft.com/pricing/calculator/, set the region to **West Europe** and the currency to USD.
2. Add the following: Virtual Machines (2× D2s_v5, Windows, 730 hrs), Managed Disks (2× P10), Load Balancer (Standard, 5 rules, 1,536 GB processed), Azure Cache for Redis (Standard C1), Azure SQL Database (Single DB, DTU, Standard S2), Storage Account (GPv2, Hot, GRS, 1,024 GB), Bandwidth (1,024 GB internet egress).
3. Flip on "Savings options" under the VM block to see the 1-yr/3-yr reservation rates, and check Azure Hybrid Benefit to see how much the license switch saves.
4. Export the result to Excel from the calculator and place that file in `estimate/` next to this workbook.

## Where each assignment task is covered
| Task | Where |
|---|---|
| 1. Define architecture | `docs/ARCHITECTURE.md` (diagram + component list) |
| 2. Configure compute | Workbook `Assumptions` + `Cost Estimate`; B- vs D-series rationale in ARCHITECTURE.md |
| 3. Data and storage (DTU vs vCore, LRS/GRS) | ARCHITECTURE.md "Why these SKUs"; Scenario 2 in OPTIMIZATION_ANALYSIS.md |
| 4. Networking estimate (1 TB egress) | `Cost Estimate` bandwidth line |
| 5. PAYG vs RI, IaaS vs PaaS | Scenarios 1 and 4 in OPTIMIZATION_ANALYSIS.md |
| 6. Azure Hybrid Benefit | Scenario 3 |
| 7. Report and export | `estimate/azure-cost-estimate-west-europe.xlsx` |
