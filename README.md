# poetry-mathematics
# Quantifying Beauty in Poetry with Math + Customer Insights

A data analytics project exploring whether poetic beauty can be quantified using **mathematical structure**, **information-theoretic surprise**, and **reader taste segmentation**.

This project combines:
- **Poetry feature engineering** (line structure, syllables, entropy)
- **Behavioral ratings analysis** (aesthetic appeal scores from participants)
- **Custom “Order vs Surprise” framework**
- **Customer insights segmentation** (poem styles and reader taste clusters)

---

## Project Idea

There is a long tradition of describing mathematics as beautiful — elegance, symmetry, balance, surprise. Poetry is often described in similar terms.

This project asks:

> Can we quantify aspects of poetic beauty using mathematical features such as structure, regularity, and surprise?

And beyond that:

> Do different readers reward different kinds of poetic beauty?

---

## Dataset

The dataset contains:

- **210 poems** (across 7 blocks)
- **51 participants**
- **10,710 total ratings** (51 × 210)

Each rating row includes:
- `aesthetic_appeal` (target)
- `Imagery`
- `Moved`
- `Originality`
- `Creativity`

Poems are labeled by type:
- `H` = Haiku
- `S` = Senryu
- `C` = Contemporary (3-line form)

---

## Pipeline Overview

### 1) Build poem lookup (text reconstruction)
Poems were reconstructed from block Excel files and normalized into a master lookup table.

**Output**
- `data_processed/poem_lookup_full.csv`

---

### 2) Build ratings master table
All participant rating files were combined into one behavioral ratings table.

**Output**
- `data_processed/ratings_master.csv`

---

### 3) Merge ratings + poem text
Ratings were matched to poem text using normalized keys.

**Result**
- **100% match rate**

**Output**
- `data_processed/poetry_dataset_merged.csv`

---

## Feature Engineering

### A. Structural features
- `num_words`
- `avg_word_length`
- `num_lines`
- `line_length_mean`
- `line_length_variance`

### B. Syllable features
- `syllables_total`
- `syllables_per_line_mean`
- `syllables_per_line_variance`

### C. Entropy / surprise features
- `word_entropy` (lexical unpredictability)
- `char_entropy` (character-level complexity)
- `line_ending_novelty` (line-end variation; low-utility in this dataset due to low variation)

---

## Order vs Surprise Framework (v1)

A custom framework was created to summarize poetic style into two dimensions:

### Order Score
Higher values indicate greater structural regularity / clarity (v1 approximation), derived from:
- lower line-length variance
- lower syllable variance
- shorter average word length

### Surprise Score
Higher values indicate greater novelty / complexity, derived from:
- higher word entropy
- higher line-length variance
- higher syllable variance

**Output**
- `data_processed/poetry_features_with_scores_v1.csv`

---

## Key Findings

## 1) Lexical surprise is strongly associated with beauty
`word_entropy` had a strong positive correlation with `aesthetic_appeal`.

Beauty increased steadily across **word-entropy quartiles**, suggesting that poems with more lexical variety/unpredictability were rated as more beautiful.

---

## 2) Word-level structure mattered more than syllable totals
- `syllables_total` had almost no relationship with beauty
- `syllables_per_line_variance` had a small positive relationship
- `avg_word_length` showed a strong negative relationship with beauty

This suggests readers may prefer **lexically simpler but structurally/lexically more dynamic** poems.

---

## 3) Beauty is highest when poems combine order and surprise
The strongest conceptual result came from the 4-zone “Order vs Surprise” map.

### Average beauty by zone
1. **High Order + High Surprise** (highest)
2. **Low Order + High Surprise**
3. **High Order + Low Surprise**
4. **Low Order + Low Surprise** (lowest)

This supports the thesis that:

> Beauty is not just structure or novelty alone — it is strongest when both are present.

---

## 4) Poem style segmentation reveals distinct aesthetic clusters
Using KMeans clustering on poem-level math features, 4 style segments emerged:

- **Elegant Complexity** (highest beauty; high order + high surprise)
- **Wild Beauty** (high beauty; low order + high surprise)
- **Controlled Simplicity** (moderate beauty; high order + low surprise)
- **Plain / Low Signal** (lowest beauty; low order + low surprise)

---

## 5) Reader taste segmentation shows multiple “beauty audiences”
Participants were segmented into 4 taste clusters using:
- rating generosity
- type preferences (H/S/C)
- order affinity
- surprise affinity

Segments included:
- **Surprise-Seeking Purists**
- **Balanced Appreciators**
- **High-Rating Generalists**
- **Tough Critics**

This shows beauty judgment is not uniform — different readers reward different poetic properties.

---

## Visual Outputs

Generated charts include:

- `beauty_by_order_surprise_zone.png`
- `cluster_beauty_comparison_step1.png`
- `participant_segments_map_step1.png`

These visualizations summarize the core project findings for portfolio or presentation use.

---

## How to Run (Script Order)

Run scripts in this order:

### Data preparation
1. `scripts/build_poem_lookup.py`
2. `scripts/build_ratings_master.py`
3. `scripts/merge_poetry_dataset.py`

### Feature engineering
4. `scripts/build_features_step1_full.py`
5. `scripts/build_features_step2_syllables.py`
6. `scripts/build_features_step3_entropy.py`
7. `scripts/build_scores_order_surprise.py`

### Analysis
8. `scripts/analyze_step1_full.py`
9. `scripts/analyze_step2_syllables.py`
10. `scripts/analyze_step3_entropy.py`
11. `scripts/analyze_order_surprise_zones.py`

### Poem segmentation (content insights)
12. `scripts/cluster_poem_styles_step1.py`
13. `scripts/export_cluster_summary_step1.py`
14. `scripts/plot_cluster_beauty_step1.py`

### Participant segmentation (customer insights)
15. `scripts/build_participant_profiles_step1.py`
16. `scripts/cluster_participants_taste_step1.py`
17. `scripts/export_participant_cluster_summary_step1.py`
18. `scripts/plot_participant_segments_step1.py`

---

## Tech Stack

- Python
- pandas
- NumPy
- matplotlib
- scikit-learn
- pronouncing (CMU Pronouncing Dictionary)

---

## Next Steps

Planned improvements:
- Add richer rhythm features (stress patterns, meter approximations)
- Build a predictive model for `aesthetic_appeal`
- Compare feature importance across reader segments
- Add interactive visualization (Plotly)
- Package the pipeline into notebooks / modules for cleaner reuse

---

## Author Notes

This project was designed as a hybrid of:
- **mathematical aesthetics**
- **poetry analysis**
- **customer insight segmentation**

The goal is not to reduce poetry to numbers, but to explore how measurable patterns interact with human judgments of beauty.
