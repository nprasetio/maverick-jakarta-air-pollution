# WORKPLAN — 2-Day Compressed Schedule (3 Sesi)

> **Total budget kerja: ~5,5 jam** dalam 3 sesi.
> Submit deadline: **besok jam 17:00 via email ke tim Maverick Indonesia**.
> Pembagian: kamu eksekusi notebook, aku (Claude) bikin deck final di akhir Hari 2.

---

## 📅 Schedule Overview

| Sesi | Waktu | Durasi | Goal |
|---|---|---|---|
| Hari 1 — Malam | 20:00 – 22:00 | 120 menit | Setup + Task 1.1, 1.2, 1.3 |
| Hari 2 — Pagi | 07:00 – 08:30 | 90 menit | Task 1.4 + Task 2.1, 2.2, 2.3 |
| Hari 2 — Siang | 14:00 – 15:25 | 85 menit | Task 2.4 + Task 3 + Exec Summary |
| Hari 2 — Deck & Submit | 15:25 – 17:00 | 95 menit | Claude bikin deck, kamu review, submit email |

---

## 🕗 SESI 1 · HARI 1 — Malam ini (20:00 – 22:00) · 120 menit
cc
**Goal akhir malam ini:** Notebook selesai sampai Task 1.3 (trigger investigation) dan ter-push ke GitHub.

### 20:00 – 20:20 · Setup (20 menit)

1. Buat repo GitHub baru: `maverick-jakarta-air-pollution` (private)
2. Clone ke laptop, copy semua isi folder ini ke repo lokal
3. Pindahkan dataset Excel ke `data/raw/` (otomatis di-gitignore)
4. Setup environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate            # mac/linux
   .venv\Scripts\activate               # windows
   pip install -r requirements.txt
   ```

5. Buka `notebooks/analysis.ipynb`, run **Section 0** (Setup)

✅ Output: print `Data file exists: True` + `df_raw.head(3)` keluar 3 baris

### 20:20 – 20:35 · Section 1 Data Cleaning (15 menit)

- Isi `TODO 1.1` — parsing datetime:
  ```python
  df['Published'] = pd.to_datetime(df_raw['Published'], utc=True).dt.tz_convert('Asia/Jakarta')
  df['Date']      = df['Published'].dt.date
  df['Month']     = df['Published'].dt.to_period('M')
  df['DayOfMonth']= df['Published'].dt.day
  ```
- Run TODO 1.2 dan 1.3 (sudah filled, tinggal eksekusi + baca output)
- Tulis 3-4 kalimat di markdown cell "📝 Cleaning decision"

### 20:35 – 21:00 · Task 1.1 Monthly Volume (25 menit)

```python
monthly_volume = df.groupby('Month').size()
aug_count = monthly_volume.loc[pd.Period('2023-08')]
other_avg = monthly_volume.drop(pd.Period('2023-08')).mean()
ratio     = aug_count / other_avg
```

- Plot bar chart, highlight August warna berbeda
- Save: `plt.savefig(CHARTS_DIR / '01_monthly_volume.png', dpi=150, bbox_inches='tight')`
- Tulis insight 2-3 kalimat di markdown cell

### 21:00 – 21:30 · Task 1.2 August Daily Deep-Dive (30 menit)

```python
aug = df[df['Month'] == pd.Period('2023-08')]
daily_aug = aug.groupby('Date').size()

above_100   = daily_aug[daily_aug > 100]
first_cross = above_100.index.min()

post_peak   = daily_aug[daily_aug.index > first_cross]
drop_below  = post_peak[post_peak <= 100].index.min()

duration = (drop_below - first_cross).days if drop_below else (daily_aug.index.max() - first_cross).days
```

- Line chart daily count, tambah `axhline(100, color='red', linestyle='--')`
- Save `02_august_daily.png`
- Insight 2-3 kalimat

### 21:30 – 22:00 · Task 1.3 Trigger Investigation (30 menit)

⚠️ **Section paling penting untuk storytelling — jangan rush.**

1. Run code window Aug 11-14 yang sudah disiapkan
2. **Baca pelan** top 30 headline berdasarkan Total Interactions
3. Tag setiap headline dengan tema (di kepala/di kertas, gak perlu code):
   - `politics` (pernyataan/penunjukan pejabat)
   - `iqair-data` (Jakarta worst air quality #1 dunia)
   - `wfh-policy` (wacana WFH PNS)
   - `health-impact` (ISPA, anak-anak, korban)
   - `weather` (kemarau, El Niño)
4. Tulis 1 paragraf insight: event apa yang paling sering muncul, kombinasi apa yang memicu spike

### Sebelum tidur · Push ke GitHub

```bash
git add .
git commit -m "Day 1 night: setup + Task 1.1-1.3 (volume, daily, trigger)"
git push origin main
```

---

## 🕖 SESI 2 · HARI 2 — Pagi (07:00 – 08:30) · 90 menit

**Goal:** Task 1.4 + 3 dari 4 sub-task di Task 2 selesai sebelum mulai aktivitas hari.

### 07:00 – 07:20 · Task 1.4 September Wave (20 menit)

```python
sep = df[df['Month'] == pd.Period('2023-09')]
top_sep = sep.nlargest(20, 'Total Interactions')[['Date','Headline','Website','Total Interactions']]
```

- Baca 20 headline teratas
- Compare tema dengan Aug 11-14 — masih politics, atau pivot ke health-impact / lifestyle?
- Tulis 1 paragraf insight

### 07:20 – 07:35 · Task 2.1 Engagement Tier (15 menit)

- Run code yang sudah disiapkan
- Verify counts cocok dengan tabel brief (>1K=11, 101-1K=97, 11-100=1422, 1-10=2101, 0=482)
- Plot horizontal bar chart, save `03_engagement_tier.png`
- Insight 2 kalimat: shape distribusi = power-law, implikasi strategis

### 07:35 – 07:55 · Task 2.2 Outlier Deep-Dive (20 menit)

- `df.nlargest(10, 'Total Interactions')` → konfirmasi Kris Dayanti #1 dengan 25,536
- Konteks yang harus disebut di insight:
  - Synchronize Festival = music festival besar Jakarta
  - "Singgung" = casual comment, bukan press conference
  - 100% engagement dari Facebook → audience massa
  - Bandingkan: artikel #2 (Anies politik) = 4,214 = 6× lebih kecil
- Insight punchline: *"Jakarta audience respond ke human moment > policy statement"*

### 07:55 – 08:20 · Task 2.3 Facebook vs Twitter Influencer (25 menit)

- Run code yang sudah disiapkan (lengkap, tinggal interpret)
- Pilih 3 outlet skew Twitter + 3 outlet skew FB
- Insight: Twitter-skewed = audience elite/jurnalis (credibility), FB-skewed = audience massa (awareness)

### 08:20 – 08:30 · Push pagi (10 menit)

```bash
git add .
git commit -m "Day 2 morning: Task 1.4 + Task 2.1-2.3 (september, tier, outlier, FB-vs-TW)"
git push
```

✅ **Sampai jam 08:30** — sisanya tinggal Task 2.4, Task 3, Exec Summary di sesi siang.

---

## 🕑 SESI 3 · HARI 2 — Siang (14:00 – 15:25) · 85 menit

**Goal:** Notebook 100% selesai jam 15:25, langsung trigger Claude untuk bikin deck.

### 14:00 – 14:20 · Task 2.4 Source Ranking (20 menit)

- Run code yang sudah disiapkan, 2 CSV otomatis ter-save
- **Pertanyaan kunci**: outlet mana yang muncul di KEDUA top-5 (volume DAN avg engagement)? → itu yang harus klien approach pertama
- Insight + tabel di markdown

### 14:20 – 15:05 · Task 3 Strategic Recommendation (45 menit)

Re-baca insight Task 1.3, 1.4, 2.2, 2.3, 2.4. Tulis 5 sub-jawaban dengan **referensi angka eksplisit**.

- **3.1 Timing** (10 menit) — stance: HYBRID. Pre-position Q4 2023–Q1 2024, launch loud Juni 2024. Argumen: data Sept-Nov collapse (716 → 142 → 19), pola musiman dry season, hindari mati pelan-pelan.
- **3.2 Narrative** (10 menit) — stance: LIFESTYLE-SOLUTIONS dengan health undertone. Bukti: outlier Kris Dayanti 9× artikel #2, top 11 (>1K) tidak murni politik.
- **3.3 Media Targets** (10 menit) — 3-5 outlet, masing-masing 1 kalimat alasan berbasis ranking dari 2.4.
- **3.4 Data as Content** (10 menit) — pilih 1 dari 3 ide (Live Index / Pollution Diary / Annual Report) + 3 langkah eksekusi.
- **3.5 AMEC IEF** (5 menit) — isi tabel Outputs/Outtakes/Outcomes, propose 3 KPI konkret dengan angka.

### 15:05 – 15:20 · Executive Summary (15 menit)

7 kalimat summary di Section 5 — ini akan jadi slide pertama deck.

### 15:20 – 15:25 · Final push + Trigger Claude (5 menit)

```bash
git add .
git commit -m "Day 2: Task 2.4 + Task 3 complete + executive summary"
git push
```

Lalu kirim ke chat: **"Notebook done, build the deck"**

---

## 🕒 SESI 4 · HARI 2 — Deck & Submit (15:25 – 17:00) · 95 menit

### 15:25 – 16:10 · Claude bikin deck (45 menit)

- Aku akan baca notebook akhir kamu + bikin `.pptx` di `deck/Maverick_Case_Study_Novindra.pptx`
- Selama menunggu, kamu bisa: refresh + minum kopi, jalankan ulang notebook 1× untuk verifikasi, atau scan ulang INTERVIEW_PREP.md untuk persiapan

### 16:10 – 16:40 · Review deck + revisi (30 menit)

- Buka deck, review setiap slide
- Kirim feedback ke chat (mis. *"slide 5 ganti title jadi X"*, *"chart Task 2 tambahin caption"*) — aku revisi cepat
- Final touch: pastikan nama, kontak, dan tanggal benar di cover slide

### 16:40 – 16:55 · Submit prep (15 menit)

- Export deck ke PDF (back-up untuk attach email)
- Tag release di GitHub: `git tag v1.0-submission && git push --tags`
- Pastikan repo: dataset TIDAK ke-commit, README + WORKPLAN + INTERVIEW_PREP visible
- Set repo ke **public** (kalau diminta) atau invite reviewer Maverick sebagai collaborator
- Siapkan email ke tim Maverick:
  - **To:** [email recruiter / HR Maverick]
  - **Subject:** `Data Analyst Case Study Submission — Novindra`
  - **Body:** singkat — link GitHub repo + 1 paragraf headline finding + attach PDF deck
  - **Attach:** `Maverick_Case_Study_Novindra.pdf`

### 16:55 – 17:00 · Send email · ✅ DONE

Kirim email **sebelum jam 17:00** sharp.

---

## ⚠️ Contingency — Kalau di Tengah Jalan Telat

| Situasi | Tindakan |
|---|---|
| Sesi 1 (malam) selesai jam 22:00 baru sampai Task 1.2 | Geser Task 1.3 ke pagi 07:00, pagi pad jadi lebih padat — skip Task 2.3 ke siang. |
| Sesi 2 (pagi) selesai jam 08:30 baru sampai Task 2.1 | Mulai sesi siang lebih awal jam 13:30 kalau bisa, atau kompres Task 3 jadi 30 menit. |
| Sesi 3 (siang) jam 15:00 baru selesai Task 2.4 | **Skip Task 3.4 (data as content)** — sebut singkat saja di deck. Prioritaskan 3.1-3.3 + AMEC. |
| Sesi 3 jam 15:15 belum mulai exec summary | Kirim ke Claude: *"Buatkan exec summary dari notebook saat ini"* — aku draft cepat. |
| Deck belum selesai jam 16:30 | Aku kirim versi minimum-viable, kamu submit dulu, revisi versi v1.1 menyusul (gak masalah jika sebelum jam 17:00). |
| Stuck di code | Paste output error/output kamu ke chat, aku unblock dalam <2 menit. |

🚨 **Hard rule:** kalau jam 16:50 dan deck belum siap, submit notebook + README + 1 page summary saja. Lebih baik submit imperfect daripada miss deadline.

---

## 📌 Aturan Main Selama 5,5 Jam Eksekusi

1. **Commit kecil-kecil** setiap selesai 1 task — jangan numpuk.
2. **Setiap claim di markdown harus ada referensi angka** dari output code di atasnya.
3. **Stop polish kalau sudah cukup baik (80/20)**. Test ini menilai *thinking*, bukan *aesthetic*.
4. **Jangan over-explain chart** — 1 kalimat insight + 1 kalimat implikasi sudah cukup.
5. **Stuck >5 menit?** Paste ke chat, jangan habis waktu sendirian.
6. **Submit-anxiety mitigation:** prep email draft dari sekarang (template di bawah), jadi tinggal paste link + attach PDF.

---

## 📧 Template Email untuk Submit (siapkan dari sekarang)

```
To:      [email recruiter Maverick Indonesia]
Cc:      novindrap@gmail.com
Subject: Data Analyst Case Study Submission — Novindra

Halo Tim Maverick Indonesia,

Berikut submission untuk case study Data Analyst — Jakarta Air Pollution
Media Intelligence.

📎 Lampiran: Maverick_Case_Study_Novindra.pdf (slide deck)
🔗 GitHub repo (kode + notebook): https://github.com/<username>/maverick-jakarta-air-pollution

Headline finding:
Gelombang pemberitaan polusi Agustus 2023 (68.5% dari total 4,113 artikel
selama 11 bulan) sudah berakhir — kurva collapse jelas dari 2,819 → 716 →
142 → 19 artikel. Rekomendasi: pre-position quietly Q4 2023–Q1 2024, launch
loud di trigger berikutnya (target: dry season Juni–September 2024). Angle
yang terbukti paling beresonansi adalah lifestyle-solutions, bukan policy
(outlier #1 = Kris Dayanti di Synchronize Festival, 25,536 interaksi —
9× lebih besar dari artikel politik tertinggi).

Detail metodologi, chart, dan AMEC IEF measurement plan ada di deck.
Notebook reproducible tersedia di repo (tanpa dataset asli, sesuai
confidentiality).

Mohon review-nya. Saya available untuk klarifikasi atau interview lanjutan
kapan pun nyaman untuk tim.

Terima kasih,
Novindra
novindrap@gmail.com
```

---

## 🔢 Quick Reference — Anchor Numbers (hafal!)

- Total artikel: **4,113**
- Range: **1 Jan – 8 Nov 2023** (11 bulan)
- Agustus: **2,819 artikel = 68.5%** dari total
- Rata-rata 10 bulan lain: **~129 artikel/bulan** ⇒ Agustus = **~21.8× rata-rata**
- 0-interaction articles: **482 (11.7%)**
- Outlier #1: **Kris Dayanti** — Synchronize Festival, kompas.com, 1 Sep 2023, **25,536 interaksi**
- Top 11 articles (>1,000 interactions) = **0.3%** of dataset
- Curve collapse: **Aug 2,819 → Sep 716 → Oct 142 → Nov 19**

---

## ✅ Definition of Done — Sebelum Jam 17:00

- [ ] Notebook run end-to-end tanpa error
- [ ] 3 charts ter-save di `outputs/charts/`
- [ ] 2 CSV ter-save di `outputs/tables/`
- [ ] Semua "📝 Insight" markdown cells terisi (tidak ada `_[isi di sini]_` tertinggal)
- [ ] Executive Summary (Section 5) terisi 7 kalimat
- [ ] Repo di GitHub bersih: dataset tidak ke-commit, README + WORKPLAN + INTERVIEW_PREP visible
- [ ] Tag release `v1.0-submission` di-push
- [ ] Deck `.pptx` ada di `deck/`
- [ ] PDF backup deck disiapkan
- [ ] Email submit terkirim ke tim Maverick Indonesia **sebelum jam 17:00**

Good luck, Novindra. Kamu sudah punya tools, plan, dan insight awal. Tinggal eksekusi disiplin sesuai jadwal.
