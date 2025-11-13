# Yankees-2025-2026-Offseason-Analysis

This case study by Samuel Anglim is a "Track B" capstone project for the Google Professional Data Analytics certificate. It details a data-driven analysis of the New York Yankees' 2025 season to identify key statistical inefficiencies by benchmarking performance against a 10-year advanced statistical model.

## Executive Summary: The "Patient" Philosophy

This project's core analysis compares the Yankees' 2025 offensive "process" (**Pitches per Plate Appearance, or P/PA**) and "outcome" (**Expected Weighted On-Base Average, or xwOBA**) against a 10-year historical benchmark (2015-2024) built from the same data sources.

The reason these two statistics were chosen for study is what stood out among the top 2025 postseason teams. All of these teams (Seattle, Toronto, Milwaukee, Los Angeles) had great success because they were able to extend at-bats and knock pitchers out of the game early. This put pressure on the bullpen, and in a 7-game series, you need your pitching staff as well-rested as possible.

In the 2025 American League Division Series between the Toronto Blue Jays and the New York Yankees, the Blue Jays took a more patient approach to their at-bats, and the Yankees often found themselves relying on their bullpen as soon as the 5th inning in nearly every single game.

This case study will analyze if our devised metric (**P/PA**), which was created using mathematical modeling in R Studio, has a verifiable impact on winning statistics (**xwOBA**), and how the New York Yankees should approach this offseason with their current financial flexibility.

---

## Phase 1: Ask

**Business Task:** To conduct a comprehensive financial and advanced statistical analysis of the 2025 Yankees, benchmarking on-field performance against a 10-year historical model of successful teams. The goal is to identify the root cause of any performance gaps, assess the viability of the team's current offensive philosophy, and provide a list of cost-effective acquisitions.

**Stakeholders:** Hal Steinbrenner (Owner), Brian Cashman (General Manager), Aaron Boone (Manager), and the Scouting Department.

**Key-Guiding Questions:**

1.  **Advanced Statistical Benchmarking (Offense):** What is the 10-year historical benchmark for "playoff-caliber" offenses, specifically in **P/PA** and **xwOBA**?
2.  **Gap Analysis (Offense - Process):** How did the 2025 Yankees' batting P/PA compare to the 10-year historical average?
3.  **Gap Analysis (Offense - Outcome):** How did the 2025 Yankees' batting xwOBA compare to the 10-year benchmark for top-tier offenses?
4.  **2026 Financial Outlook:** What is the Yankees' projected 2026 payroll, and how does their 2025 revenue provide financial flexibility?
5.  **Targeted Acquisitions:** Which available 2025 players (non-Yankees) fit the successful profile of high P/PA and high xwOBA?

---

## Phase 2: Prepare

To answer our questions, data was aggregated from several credible, public sources.

* **Financials (Revenue):** `2025_team_revenue.csv` from Forbes' MLB Team Valuations.
* **Financials (Payroll):** `2026_team_payroll.csv` from Spotrac's 2026 Projections.
* **2025 Advanced Stats (Batting):**
    * `expected_stats_2025.csv`: A **team-level** file from the Baseball Savant "Expected Statistics" Leaderboard. This was our "outcome" file, providing **xwOBA**.
    * `savant_player_swingtake_2025.csv`: A **player-level** file from the Baseball Savant "Swing-Take" Leaderboard. This was our "process" file, providing `pitches` and `pa` to calculate **P/PA**.
* **2025 Advanced Stats (Pitching):**
    * `team_pitching_2025.csv`: A **player-level** file from the Baseball Savant "Swing-Take" Leaderboard, used to analyze the pitching staff's P/PA.
* **2025 Player Target Data:**
    * `player_expected_stats_2025.csv`: A **player-level** file from the "Expected Statistics" leaderboard, used to get the `xwOBA` for individual players to build our target list.
* **Historical Advanced Stats (2015-2024):**
    * This was the most intensive data collection step. To build an "apples-to-apples" benchmark, **20 separate CSV files** were manually downloaded from Baseball Savant:
        * **10 `xwoba_*.csv` files:** One for each year (2015-2024) from the **team-level** "Expected Statistics" leaderboard.
        * **10 `ppa_*.csv` files:** One for each year (2015-2024) from the **player-level** "Swing-Take" leaderboard.

---

## Phase 3: Process

All data was processed and analyzed using R in RStudio. The R script is structured to logically move from data loading to final analysis.

First, all necessary libraries (`tidyverse`, `janitor`, `ggrepel`) are loaded.

The **data loading** step reads all 25+ data files from the `data/` folder. A key technical step here is using `list.files()` and `map_dfr()` to automatically find all 10 historical `xwoba_` files and all 10 historical `ppa_` files, and then stack them into two master historical data frames: `hist_team_xwoba` and `hist_player_ppa`.

Next, the **2025 data is cleaned**.
* The financial files are processed using `clean_names()` and `parse_number()` to convert text like "$680 million" into numeric values (680000000) for calculation.
* The advanced 2025 stats are prepared. This required handling two different data granularities:
    1.  **P/PA:** The **player-level** `batting_2025_ppa` file is aggregated using `group_by(team_numeric)` and `summarize(weighted.mean(...))` to create a single, team-level P/PA score for all 30 teams.
    2.  **xwOBA:** The **team-level** `batting_2025_xwoba` file is cleaned using `str_trim()` to remove hidden whitespace from the `team_id` column (e.g., "NYY " -> "NYY"), which is critical for correctly filtering for the Yankees.

---

## Phase 4: Analyze

This is where the benchmark and the 2025 performance are calculated, providing the core answers to our questions.

First, the **10-Year Advanced Benchmark** is built from the historical data. We discarded the idea of a simple traditional benchmark (like OBP or ERA) in favor of a true "apples-to-apples" comparison.
1.  **`xwoba_benchmark`:** The script groups the `hist_team_xwoba` data by year, uses `slice_max(n=10)` to find the **Top 10 offenses** for each of the last 10 years, and then calculates the 10-year weighted average `xwOBA` of *only this elite group*.
2.  **`ppa_benchmark`:** The script aggregates the `hist_player_ppa` data by team and year, then calculates the 10-year **league-wide average** for P/PA. This is a robust benchmark for a "philosophy" metric.

Next, the **Gap Analysis** is performed. The R script calculates the Yankees' 2025 `P/PA` and `xwOBA` and prints them directly next to our new 10-year benchmarks.

### Key Analytical Findings:

* **Finding 1: Offensive Process (P/PA):** The Yankees' 2025 batting P/PA was **4.053**. This is measurably higher than the 10-year historical league average of **3.917**.
* **Finding 2: Offensive Outcome (xwOBA):** The Yankees' 2025 batting xwOBA was **0.340**. This is **above** the 10-year benchmark for an elite, "Top 10" offense, which was **0.329**.
* **Finding 3: Financial Power:** The Yankees' 2026 projected payroll of **$192.3M** confirms they have significant financial flexibility.

---

## Phase 5: Share (Stakeholder Presentation)

This is the narrative presented to stakeholders, using the plots generated by the R Markdown file.

**Slide 1: The "Process" (Using Plot 1: Offensive P/PA Gap)**
* **Title:** Finding 1: We Have a Clear (and Patient) Offensive Philosophy
* **Key Insight:** "Our analysis shows our 2025 batting P/PA was **4.053**, which is 3.5% higher than the 10-Year Historical Average of **3.917**."
* **The "So What?":** "This confirms our patient, 'Blue Jays' style philosophy is not just a feeling; it is a measurable fact. But is it *working*?"

**Slide 2: The "Outcome" (Using Plot 2: Offensive xwOBA Gap)**
* **Title:** Finding 2: Our Philosophy Is Working
* **Key Insight:** "Our 2025 offensive xwOBA was **0.340**. This is significantly **above** the 10-Year Benchmark for an elite, Top-10 offense, which is **0.329**."
* **The "So What?":** "This is our key finding. Our patient process (high P/PA) *is* leading to elite outcomes (high xwOBA). We have the right strategy."

**Slide 3: The Opportunity (Using Plot 3: Financials)**
* **Title:** Finding 3: We Have the Power to Double Down
* **Key Insight:** "Our 2026 projected payroll is **$192.3 million**, which is well below other top-market teams like the Dodgers ($240M) and Mets ($212M)."
* **The "So What?":** "We have the financial flexibility to be aggressive and acquire the *right* players who fit our successful, data-driven model."

---

## Phase 6: Act (The Final Recommendation)

This is the final, actionable recommendation to Brian Cashman and Hal Steinbrenner, derived directly from the R script's final output.

### 1. The Finding:
My analysis shows the Yankees are committed to a high P/PA offensive philosophy. By comparing our 2025 performance to a 10-year advanced benchmark, we found that this process is **successfully leading to elite xwOBA outcomes**.

### 2. The Recommendation:
We must **double down on our successful philosophy**. The 2026 offseason goal is not to find "good hitters," but to find players who are *proven* to excel in a high-P/PA, high-xwOBA system. We must acquire players who fit our model.

### 3. Targeted Acquisitions (Answering the Key Question):
The final step of the R script was to build this exact list. It loaded the **2025 player-level `xwOBA` file** (`player_expected_stats_2025.csv`) and joined it with the **player-level `P/PA` file** (`savant_player_swingtake_2025.csv`).

It then filtered this master list to find players who:
1.  Had an `xwOBA` **above** our 10-year elite benchmark (0.329).
2.  Had a `P/PA` **above** our 10-year average benchmark (3.917).
3.  Were **not** already on the Yankees (`team_numeric != 147`).

The script generated a list of these targets, sorted by P/PA. This list includes (but is not limited to) notable names like **DH Kyle Schwarber, 2B Gleyber Torres, 1B Pete Alonso, SS J.P. Crawford, SS Geraldo Perdomo, 3B Manny Machado, OF Steven Kwan, and SS Zach Neto**.

By using our financial power to pursue these specific, data-vetted players, we can strengthen our already successful model and build a roster capable of winning the 2026 World Series.
 
