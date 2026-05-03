# Jakarta Air Pollution — Media Intelligence Analysis

> **Maverick Indonesia · Data Analyst Case Study**
> **Author:** Novindra Prasetio · novindrap@gmail.com
> **Repository:** [github.com/nprasetio/maverick-jakarta-air-pollution](https://github.com/nprasetio/maverick-jakarta-air-pollution)

---

## EN — Project Overview

End-to-end media intelligence analysis built for the Maverick Indonesia Data
Analyst assessment. The brief: a Jakarta-based startup selling a portable
PM2.5 / AQI reader has hired a PR consultancy to find out whether the August
2023 Jakarta air-pollution media wave is still an opportunity or already
past, and to recommend when, how, and through whom to launch their awareness
campaign.

The analysis works through three tasks built on **4,113 news articles**
(January – November 2023) from a media monitoring export:

1. **Coverage volume & trend** — when the wave peaked, how big it was, when it faded
2. **Engagement & source analysis** — which outlets and angles actually moved the needle
3. **Strategic recommendation** — timing, narrative, media targets, AMEC IEF measurement

The deliverable is a slide deck supported by a reproducible Jupyter notebook.

---

## ID — Ringkasan Proyek

Analisa media intelligence end-to-end untuk test Data Analyst Maverick
Indonesia. Brief-nya: startup Jakarta yang menjual alat ukur PM2.5/AQI
portable menyewa konsultan PR untuk mencari tahu apakah gelombang
pemberitaan polusi udara Jakarta Agustus 2023 masih jadi peluang atau sudah
lewat, sekaligus merekomendasikan kapan, bagaimana, dan melalui media mana
campaign awareness mereka harus diluncurkan.

Analisa menjawab tiga task berdasarkan **4.113 artikel berita**
(Januari – November 2023) dari hasil ekspor media monitoring:

1. **Volume & tren coverage** — kapan puncaknya, seberapa besar, kapan reda
2. **Engagement & analisa sumber** — outlet dan angle mana yang benar-benar berpengaruh
3. **Rekomendasi strategis** — timing, narrative, target media, pengukuran AMEC IEF

Deliverable: slide deck (.pptx + .pdf) yang didukung notebook Jupyter reproducible.

---

## Headline Findings

| Finding | Number |
|---|---|
| Total articles analyzed | **4,113** |
| August 2023 share of total volume | **68.5%** (2,819 articles) |
| August magnitude vs. average month | **21.8×** the typical month (~129 articles) |
| Post-August collapse (Aug → Nov) | **−99%** (2,819 → 716 → 142 → 19) |
| Power-law concentration | **63%** of articles get ≤10 interactions; **0.3%** capture viral mass |
| Outlier story | Kris Dayanti @ Synchronize Festival — **25,536 interactions** (6× the second article) |
| The Sweet Spot outlet | **kompas.tv** — Top-5 in both volume (#4) and engagement (#3, avg 97.8) |

## Strategic Recommendation (Headline)

> **Don't launch loud now. Don't wait silently either.**
> Use Q4 2023 – Q1 2024 to **pre-position quietly**, then **launch loud at
> the next dry-season trigger** (target: late June 2024).

- **Narrative angle:** Lifestyle-Solutions with health undertone. Avoid political/regulatory framing.
- **Media targets:** kompas.tv (Tier-1 Sweet Spot) · kompas.com (Tier-2 Mass Awareness) · cnnindonesia.com (Tier-2 Credibility) · Tempo Network metro.tempo.co (Tier-3 Policy Pressure).
- **Data-as-content:** Jakarta Pollution Live Index — embed device data into media partner sites; auto-alert journalists at fatal AQI thresholds.
- **AMEC KPIs (3-month post-launch):** 25% Share of Voice in top-5 outlets · +40% YoY brand mention growth · 500+ device pre-orders.

Full reasoning, charts, and the AMEC IEF measurement plan are in the slide deck.

---

## Repository Structure

```
maverick-jakarta-air-pollution/
├── README.md                                          # This file
├── WORKPLAN.md                                        # Execution log (Indonesian)
├── requirements.txt                                   # Python dependencies
├── .gitignore                                         # Excludes raw dataset
│
├── data/raw/                                          # Dataset goes here (not committed)
│
├── notebooks/
│   └── analysis.ipynb                                 # Main analysis (Tasks 1–3 + appendices)
│
├── outputs/
│   ├── charts/                                        # 5 PNG visualizations
│   │   ├── 01_monthly_volume.png
│   │   ├── 02_august_daily.png
│   │   ├── 03_wordcloud.png
│   │   ├── 04_engagement_tier.png
│   │   └── 05_platform_skew.png
│   ├── tables/                                        # CSV exports of source rankings
│   │   ├── top5_by_volume.csv
│   │   └── top5_by_engagement.csv
│   └── 4_5_AMEC_Plan.md                               # AMEC IEF table (also in notebook)
│
├── deck/
│   ├── Maverick_Case_Study_Novindra.pptx              # Final presentation deliverable (22 slides)
│   ├── Maverick_Case_Study_Novindra.pdf               # PDF backup
│   └── build_deck.js                                  # Reproducible deck generator (pptxgenjs)
│
└── docs/
    └── INTERVIEW_PREP.md                              # 23 practice questions for the interview
```

---

## Reproduction

**Prerequisites:** Python 3.10+, Jupyter Lab/Notebook, Node.js 18+ (only if rebuilding the deck).

```bash
# 1. Clone the repo
git clone https://github.com/nprasetio/maverick-jakarta-air-pollution.git
cd maverick-jakarta-air-pollution

# 2. Create virtual environment
python -m venv .venv
source .venv/bin/activate          # macOS / Linux
# .venv\Scripts\activate           # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Place the raw dataset in data/raw/
#    File: [Maverick Indonesia] - Air Pollution News Article Raw Data .xlsx
#    (not committed to git for confidentiality)

# 5. Launch the notebook
jupyter lab notebooks/analysis.ipynb
```

To rebuild the deck (optional):

```bash
cd deck
npm install pptxgenjs
node build_deck.js
```

---

## Methodology Notes (one-paragraph)

All article timestamps are converted to Asia/Jakarta timezone before
date-based grouping (raw export is UTC+07 ISO; mixing with naive datetime
causes silent off-by-one errors at month boundaries). LinkedIn interactions
are dropped per brief (tracking error). Articles with zero total interactions
are kept in volume counts but flagged separately when computing engagement
averages, to avoid biasing the means downward. Duplicate links (n=119) and
duplicate headlines (n=172) are inspected case-by-case — true duplicates are
dropped, partial duplicates with different timestamps/interactions are merged.
Source rankings filter to outlets with ≥20 articles to avoid one-off virals
distorting the averages. Theme tagging of headlines is manual (no NLP) for
transparency; subjective bias is acknowledged in Appendix B of the notebook.

Full methodology in [`notebooks/analysis.ipynb`](notebooks/analysis.ipynb) Appendix A.

---

## Submission Note

Per the brief:
> *"We would rather read a submission that takes a clear position and
> supports it with data, even if the conclusion is not perfect."*

This analysis takes a **clear hybrid stance**: pre-position quietly during
the off-season, then launch loudly at the next dry-season trigger (target:
late June 2024). The deck argues this position; the notebook provides the
reproducible evidence.

---

## License & Confidentiality

The raw dataset is the property of Maverick Indonesia and is **not** committed
to this repository (see `.gitignore`). All code, analysis, charts, and the
slide deck are released for review purposes related to the assessment.
