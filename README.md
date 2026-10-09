# Companion Diagnostics

**Every trial on ClinicalTrials.gov that states a companion diagnostic, or names an actual CDx product — worldwide.**

### → **[Open the site](https://chenru1z.github.io/companion-diagnostics/)**

---

| | |
|---|---|
| **652** | CDx trials, 0.11% of the registry |
| **200** | industry-led (31%) |
| **538** | oncology (83%) |
| **166** | say "companion diagnostic" explicitly |
| **486** | name a CDx product without using the phrase |

Derived from the ClinicalTrials.gov bulk download, snapshot **2026-09-25** (604,566 studies).

Most-named products: **PD-L1 IHC 22C3** (157) · **FoundationOne** (130) · **pharmDx** (74) · **HercepTest** (51) · **PD-L1 SP263** (45) · **Guardant360 CDx** (41) · **Vysis FISH** (36).

---

## How a trial qualifies

ClinicalTrials.gov has **no companion-diagnostic field**, no intervention type and no flag. A trial appears here only on one of two signals:

1. **The record says so** — "companion diagnostic" or "CDx".
2. **It names an actual CDx product** — FoundationOne, 22C3, 28-8, SP142, SP263, pharmDx, therascreen, Oncomine Dx, Guardant360, BRACAnalysis, myChoice, HercepTest, Vysis, AmoyDx, Idylla and others.

Precision was chosen over volume. Two broader rules were tried and rejected:

**Drug + diagnostic-test intervention.** This looks like the structural fingerprint of co-development and is not. It returns 810 trials that are **91% academic**, dominated by oral glucose tolerance tests, brain MRIs and cardiac MR — trials logging ordinary clinical assessments, not developing a diagnostic.

**Vendor names alone.** Bare "Ventana", "Dako" and "cobas" are IHC and PCR platforms behind hundreds of routine stains and assays. Only their named CDx products count here, which is why the patterns are specific (`cobas EGFR`, not `cobas`).

The composition is the evidence that it worked: 83% oncology and 31% industry-led, against 20% and 23% for a general device dataset.

### One bug worth recording

"CDx" is standard shorthand for a companion diagnostic, but it is also the prefix of several **drug** codenames — Celldex's CDX-011, CDX-301, CDX-1140, Codexis's CDX-6114. Without a guard, 72 of 178 phrase-only matches were drug trials with no diagnostic in them. A digit after the token now means product code, not abbreviation, and the count fell from 735 to 652.

---

## What it does

- **Dashboard** — which CDx products are named, trials per year, study phase, therapeutic area, the companies and academic centres running them, and where.
- **Search** — full-text across titles, conditions, interventions and sponsors, with filters for phase, status, sponsor, country, therapeutic area and start year.
- **Trial pages** — design, eligibility, outcome measures, every site, collaborators, MeSH terms and linked publications, linking back to the ClinicalTrials.gov record.

Unlike a device dataset, **phase applies here** — these are drug trials, and Phase I–IV is meaningful.

---

## Limitations

**This undercounts, and the gap is structural, not fixable by better matching.**

- **Companion diagnostics are frequently co-developed without a separate trial record.** The assay is validated against banked samples from the drug trial; no distinct NCT ID exists.
- **Many drug trials rely on a CDx without naming it** — "PD-L1 positive" with no mention of which assay established it.
- **The authoritative source is the FDA's list of cleared and approved companion diagnostic devices**, which carries the actual drug–diagnostic pairings. This site infers from trial text; it is not a substitute.
- **Registration is self-reported**, and 2026 is a partial year.

Treat 652 as a floor on CDx-related trial activity, not a census.

---

## How it is built

The same pipeline that builds the [China trials](https://chenru1z.github.io/china-clinical-trials/) and [global device](https://chenru1z.github.io/global-device-trials/) sites, with one step swapped:

1. **Index all 604,566 studies**, recording intervention types and CDx signals for each.
2. **Select** the studies carrying a CDx signal.
3. **Build a SQLite database** — 9 normalised tables plus an FTS5 index.
4. **Export to static JSON** — 0.2 MB search index, so the whole site is about 9 MB.

Python standard library only. No frameworks, no build step, no dependencies.

---

## Attribution

Data courtesy of the **U.S. National Library of Medicine**, from [ClinicalTrials.gov](https://clinicaltrials.gov). ClinicalTrials.gov data is in the public domain.

**NLM does not endorse or verify this site.** This is an independent project and is not affiliated with the NLM, the NIH, or any sponsor or manufacturer listed in the data. Product names are the trademarks of their owners and appear here only as they do in trial records.

Nothing here is medical advice.

---

## Accuracy

If you find a trial wrongly included or wrongly missing, please [open an issue](../../issues) with the NCT ID. The matching rules are in `pipeline/cdx.py` and are deliberately easy to tighten.
