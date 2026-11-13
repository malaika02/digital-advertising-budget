# Digital Advertising Budget Optimizer

**Complete Google Colab project built from the uploaded Project 2 brief**

The supplied `Digital_Advertising_Budget_Optimizer_Colab.ipynb` implements the instructor's management question: **How should a company allocate the next Rs. 1,000,000 across Facebook, Instagram, Google, TikTok and YouTube?**

## Getting started in Google Colab

1. Open https://colab.research.google.com/ and choose **File → Upload notebook**.
2. Upload `Digital_Advertising_Budget_Optimizer_Colab.ipynb` from this folder.
3. Choose **Runtime → Run all**. With an internet connection it automatically downloads the licensed source campaign, platform and customer CSV files from GitHub.
4. Review the displayed audit, weighted marketing KPIs, plots, allocation decision, and execution-time benchmark plots.
5. To save the calculated outputs, uncomment the last two lines of the export cell in the notebook, or download `optimizer_run_results.zip` from the Colab Files sidebar.

**Default run:** 10,000 / 100,000 / 1,000,000 simulated rows; Pandas + a local Python implementation of MapReduce + local PySpark if installation and Java are available. For the **10,000,000-row** test, change `RUN_10_MILLION = True` and rerun. It requires considerable storage/runtime and may be impractical on free Colab. Generation is chunked for memory safety.

**No internet?** Set `SOURCE_MODE = "synthetic"` to use an original fully simulated fallback. `ALLOW_SYNTHETIC_FALLBACK=True` also warns and falls back if a GitHub download fails. The sample CSV packaged here was produced from the independent fallback and is *not a copy of the original GitHub dataset*. It is supplied so you have a 10k-row project dataset immediately.

## Real public dataset selected

- Repository: https://github.com/Giftedman1/digital-marketing-campaign-analysis
- License: MIT (see the linked repository).
- Raw source table: https://raw.githubusercontent.com/Giftedman1/digital-marketing-campaign-analysis/main/Dataset/campaigns.csv
- Platform names: https://raw.githubusercontent.com/Giftedman1/digital-marketing-campaign-analysis/main/Dataset/platforms.csv
- Independent customer population: https://raw.githubusercontent.com/Giftedman1/digital-marketing-campaign-analysis/main/Dataset/customers.csv
- The source is itself **synthetic educational marketing data**, not a verified advertiser's transactions.
- The original source provides **10,000 campaign records across seven platforms**; there are **7,120** campaign records on the five required platforms after filtering. The notebook re-simulates to the precise requested scale counts.

### Five-platform source snapshot (directly checked against the upstream 10k CSV)

| Platform | Source campaign records | Historical aggregate ROAS (approximately) |
|---|---:|---:|
| Facebook | 1,420 | 2.70x |
| Instagram | 1,431 | 2.82x |
| Google Ads | 1,374 | 3.10x |
| TikTok | 1,465 | 2.93x |
| YouTube | 1,430 | 2.82x |

All five are present. This is descriptive historical performance in the **synthetic** source, *not* a guarantee of future returns. The Colab notebook calculates precise per-platform values from the CSV at runtime and derives the optimization scores with bootstrap resampling.

## Project brief coverage

| Required item | Delivered in notebook |
|---|---|
| Digital platform analysis | Five named channels, joined from the actual platform lookup |
| 12 requested columns | `date,campaign,platform,impressions,clicks,spend,conversions,revenue,age,gender,city,device` |
| Marketing KPIs | CTR, CPC, CPA, conversion rate, ROAS, revenue, campaign/objective performance; weighted aggregates |
| Campaign and audience visuals | Platform, ROAS, spend, revenue, monthly campaign-start trend, device-target and demographic summaries |
| Big Data simulation | Chunked, seeded generator with funnel-valid records at 10k, 100k, 1m and optional 10m |
| Engine and management decision | Exact Rs. 1,000,000 allocation, all 5 platforms, conservative bootstrapped score, diminishing-returns assumption, baseline and sensitivity |
| Pandas vs MapReduce vs Spark | Comparable sums, CSV read included in times, validation of numerical agreement, timing graphs |
| Save reports | Source audit, KPI CSVs, allocation CSV, sensitivity CSV, measured benchmark CSV, metadata JSON, ZIP |

### Important methodology / limitations

- The source's `StartDate` is treated as the **date of a campaign**, not a daily impression or purchase date.
- `Age` and `Gender` are **independently simulated** from an unlinked customer population, not real attributed campaign audiences. `City` is **simulated** from broad region labels, not measured city-level performance. `DeviceTarget` is campaign targeting, not an observed converting device. These fields meet the exercise's schema but are excluded from the allocation decision.
- Historical `Cost` and `Revenue` refer to **USD** in the source. The rupee allocation uses the **unitless ROAS ratio** as a *what-if assumption*; no conversion rate or PKR transaction records are asserted.
- The historical `ROAS` is descriptive, not a causal estimate of the marginal revenue from reallocating budget. A 5%-45% platform cap, bootstrap resampling, and a concave diminishing-return curve are modeling assumptions, not established market facts.
- The MapReduce benchmark is **single-machine Python map/reduce**, not Hadoop; PySpark runs on **local Spark**, not a multi-worker cluster.
- All generated large datasets are synthetic; the default run does **not** execute 10 million unless you explicitly enable it.

## Packaged contents

- `Digital_Advertising_Budget_Optimizer_Colab.ipynb`: full, independently written notebook (main deliverable)
- `Simulated_Digital_Advertising_10k.csv`: original 12-column, 10k-row fallback sample
- `Original_Project_Brief.docx`: user-supplied original brief preserved for reference
- `offline_demo_results/`: example outputs from the notebook executed **offline on the fallback**, *not from the GitHub source* (contains budget/sensitivity/benchmark results)
- `VERIFICATION.md`: exactly what was locally tested and what still needs Colab verification
- `requirements.txt`: common dependencies for running outside Colab; notebook installs PySpark when needed

## Academic submission note

Use the **GitHub-source run**, not the offline demonstration figures, when presenting empirical-looking historical platform comparisons. Cite the dataset repository and describe it as synthetic. For 10m benchmarks, cite your own Colab timing printouts and state the hardware/runtime used. Do not describe the output as real advertising revenue forecasts.
