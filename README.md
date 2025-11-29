# DSA 210 Project: Rocket League Positioning Analysis

* **Course:** DSA 210 Introduction to Data Science
* **Student:** [Your Name]
* **Term:** Fall 2025

---

## 1. The Idea (Motivation)

In professional Rocket League, everyone has good mechanics. You can hit the ball, you can fly, and you can dribble. What separates the winners from the losers often isn't *what* they do with the ball, but *where* they are on the field.

A classic mistake in lower-ranked lobbies is "double committing"—where two teammates rush for the ball at the same time, crash into each other, and leave the net wide open. 

**My project investigates this specific behavior.** I want to see if we can mathematically prove that "spacing out" (maintaining a wider average distance between teammates) actually leads to more wins. By connecting raw gameplay data with professional ranking data, I also want to see if the "Pros" essentially play a different game than the amateurs when it comes to positioning.

## 2. The Data

To answer this, I am combining two very different data sources.

### Part A: The Gameplay (Base Data)
I am using a [massive dataset](https://www.kaggle.com/datasets/dster/rocket-league-rlcs-timeseries) of Rocket League replays from **Kaggle**. This isn't just a scoreboard; it contains frame-by-frame telemetry.
* **Info retrieved:** The 3D coordinates ($x, y, z$) of every player, updated multiple times per second.
* **Granularity:** Roughly 3,000 matches.

### Part B: The Context (Enrichment)
The Kaggle data gives me the *moves*, but it doesn't tell me *who* is good. For that, I built a scraper for **Liquipedia**, the main wiki for Rocket League e-sports.
* **Info retrived:** The official RLCS ranking points for teams across all major regions (EU, NA, SAM, etc.).
* **Why:** This allows me to tag a match as "High Level" (Pro) vs. "Standard" based on whether the team appears in the global rankings.

## 3. The Process

This analysis is broken down into three main scripts.

### 1. Scraping the Rankings (`1_collect_data.ipynb`)
Since Liquipedia separates rankings by region and uses complex tabbed tables, I wrote a custom scraper using `BeautifulSoup`. It iterates through 7 different regions, parses the HTML tables, and extracts the highest point total for every team found. This gives me a "Global Leaderboard" CSV.

### 2. Feature Engineering: "The Spacing Metric"
For the gameplay analysis, I needed to turn raw coordinates into a single number that represents "teamwork." I defined **Team Spacing** as the average distance between every pair of teammates on the field.

For a standard 3-player team ($p_1, p_2, p_3$), the math looks like this:

$$\text{Avg Spacing} = \frac{\text{dist}(p_1, p_2) + \text{dist}(p_2, p_3) + \text{dist}(p_1, p_3)}{3}$$

I calculate this for every frame of the match and then average it to get a single "Spacing Score" for that team for that game.

### 3. Merging & Analysis (`2_analysis.ipynb`)
Finally, I merge the two datasets. I use **Fuzzy String Matching** (`thefuzz`) to link the team names from the replay files (which might be "G2 Stride") to the Liquipedia rankings (which might be "G2 Esports").

## 4. Hypothesis

My main hypothesis is simple:
> **"Winning teams will have a statistically significantly higher average spacing value than losing teams."**

I am testing this using an **Independent Two-Sample T-Test** ($p < 0.05$).

The plot is shown below:
![spacing hypothesis plot](plots/spacing_hypothesis.png)

I am also running a secondary check to see if **Ranked (Pro)** teams generally maintain wider spacing than **Unranked** teams, regardless of the match outcome.

## 5. How to Run

1.  **Install Dependencies:**
    ```bash
    pip install pandas numpy scipy matplotlib seaborn pyarrow fastparquet beautifulsoup4 requests thefuzz python-Levenshtein
    ```
2.  **Collect the data:**
    Run all the cells in `1_collect_data.ipynb`, this will download the Kaggle dataset and move the main used parquet
    file to the root. Then it will scrape all the necessary data from Liquipedia.
3.  **Run Analysis:**
    Ensure you have `frames.parquet` and `games.parquet` in the folder, then run `2_analysis.ipynb`. The output of the data is already there, but you can run it yourself as well.
4.  **View Results:**
    Check the cell outputs for the P-Value/T-statistics and the `plots/` folder for the visualizations.
