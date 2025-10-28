# DSA 210: Introduction to Data Science - Project Proposal
---

## 1. Motivation (Project Proposal)

This project aims to build a machine learning model to predict the winner of a professional Rocket League (RLCS) match.

The central research question is: **Does enriching a model with human-generated "power rankings" lead to a statistically significant improvement in predictive accuracy compared to a model trained *only* on raw, in-game performance statistics?**

This project will compare two models to answer this question:

* **Model A (Baseline):** Trained only on in-game stats (boost, shots, saves, etc.).
* **Model B (Enriched):** Trained on in-game stats *plus* the team's official regional ranking from Liquipedia.

The findings will help determine which features (raw performance or expert consensus) are more predictive of success in e-sports.

## 2. Data Sources

This project will use two distinct, publicly available datasets as required by the course guidelines.

### Base Data: In-Game Match Statistics

* **Source:** `ballchasing.com` API
* **Description:** I will collect granular, match-level replay data for recent RLCS tournaments. Key metrics will include boost usage (BPM), positioning (time spent in offensive/defensive thirds), shots, saves, assists, and demolition counts for each team.

### Enrichment Data: E-Sports Rankings

* **Source:** Liquipedia Rocket League Wiki (e.g., RLCS Rankings pages)
* **Description:** I will collect the official team rankings and point standings for the corresponding tournaments. This data represents the "human expert" or "public consensus" ranking of a team's strength going into a match.

## 3. Data Collection and Analysis Plan

The entire project will be conducted in **Python 3**.

**Data Collection:**
    * **`ballchasing.com` API:** I will use the Python `requests` library to query the API for replay group data from specific RLCS events. The JSON responses will be parsed and loaded into a `pandas` DataFrame.
    * **Liquipedia Scraping:** I will use the `pandas.read_html()` function to directly scrape the ranking tables from the relevant Liquipedia tournament pages. If this fails due to dynamic JavaScript content, I will use `selenium` as a fallback.
    * **Data Merging:** The two datasets will be cleaned and merged on team name and date to create a final, unified feature set for each match.
## 4. Initial `requirements.txt`

This file will be included in the repository to ensure reproducibility:
```
pandas numpy requests scikit-learn lxml selenium jupyterlab matplotlib seaborn
```
