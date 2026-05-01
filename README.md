# Jakarta Air Pollution — Media Intelligence Analysis

> **Maverick Indonesia · Data Analyst Case Study (2026)**
> Author: Novindra · novindrap@gmail.com

---

## EN — Project Overview

This repository contains an end-to-end media intelligence analysis built for the
Maverick Indonesia Data Analyst assessment. The brief: a Jakarta-based startup
selling a portable PM2.5 / AQI reader has hired a PR consultancy to find out
whether the August 2023 Jakarta air-pollution media wave is still an opportunity
or already past, and to recommend when, how, and through whom to launch their
awareness campaign.

The analysis works through three tasks built on **4,113 news articles**
(January – November 2023) from a media monitoring export:

1. **Coverage volume & trend** — when the wave peaked, how big it was, when it faded
2. **Engagement & source analysis** — which outlets and angles actually moved the needle
3. **Strategic recommendation** — timing, narrative, media targets, AMEC IEF measurement

The deliverable is a slide deck supported by a reproducible Jupyter notebook.

---

## ID — Ringkasan Proyek

Repositori ini berisi analisa media intelligence end-to-end untuk test Data
Analyst Maverick Indonesia. Brief-nya: startup Jakarta yang menjual alat ukur
PM2.5/AQI portable menyewa konsultan PR untuk mencari tahu apakah gelombang
pemberitaan polusi udara Jakarta Agustus 2023 masih jadi peluang atau sudah
lewat, sekaligus merekomendasikan kapan, bagaimana, dan melalui media mana
campaign awareness mereka harus diluncurkan.

Analisa menjawab tiga task berdasarkan **4,113 artikel berita**
(Januari – November 2023) dari hasil ekspor media monitoring:

1. **Volume & tren coverage** — kapan puncaknya, seberapa besar, kapan reda
2. **Engagement & analisa sumber** — outlet dan angle mana yang benar-benar berpengaruh
3. **Rekomendasi strategis** — timing, narrative, target media, pengukuran AMEC IEF

Deliverable berupa slide deck yang didukung notebook Jupyter yang reproducible.

---

## Repository Structure

```
maverick-jakarta-air-pollution/
├── README.md                  # This file
├── WORKPLAN.md                # Day-by-day execution guide (Indonesian)
├── requirements.txt           # Python dependencies
├── .gitignore
│
├── data/
│   └── raw/
│       └── (dataset not committed — see Reproduction below)
│
├── notebooks/
│   └── analysis.ipynb         # Main analysis notebook (Tasks 1–3)
│
├── outputs/
│   ├── charts/                # Exported PNGs used in the deck
│   └── tables/                # Exported CSVs (rankings, distributions)
│
├── deck/
│   └── (final presentation .pptx)
│
└── docs/
    └── (interview prep, methodology notes, references)
```

---

## Reproduction

**Prerequisites:** Python 3.10+, Jupyter Lab/Notebook.

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/maverick-jakarta-air-pollution.git
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

---

## Methodology Notes (one-paragraph version)

All article timestamps are converted to Asia/Jakarta timezone before any
date-based grouping (the raw export is in UTC+07 ISO format but mixing with
naive datetime causes silent off-by-one errors at month boundaries). LinkedIn
interactions are dropped per the brief (tracking error). Articles with zero
total interactions are kept, but flagged separately when discussing engagement
to avoid biasing the average. Duplicate links (n=119) and duplicate headlines
(n=172) are inspected — most are legitimate cross-publishing across
sub-domains, so they are kept but documented in the cleaning section.

---

## Headline Findings (preview)

- **August 2023 = 68.5%** of all coverage in eleven months
  (2,819 of 4,113 articles)
- The trigger: mid-August 2023 — Jakarta hit "world's worst air quality" status
  on IQAir; political response (Jokowi → Luhut appointment) amplified the wave
- Engagement is brutally skewed: **51%** of articles get 1–10 interactions;
  the top 11 articles (0.3%) capture a disproportionate share of all engagement
- The single biggest article (25,536 interactions) is a celebrity / lifestyle
  story (Kris Dayanti at Synchronize Festival), not a policy story
- The wave is over: Sept 716 → Oct 142 → Nov 19 articles

---

## Submission Note

Per the brief:
> *"We would rather read a submission that takes a clear position and supports
> it with data, even if the conclusion is not perfect."*

The final recommendation in this analysis takes a **clear hybrid stance**:
pre-position quietly during the off-season, then launch loudly at the next
dry-season trigger (target: late June 2024). The deck argues this position;
the notebook provides the reproducible evidence behind it.

---

## License & Confidentiality

The dataset is the property of Maverick Indonesia and is **not** committed to
this public repository. Code and analysis are released for review purposes
related to the assessment.
