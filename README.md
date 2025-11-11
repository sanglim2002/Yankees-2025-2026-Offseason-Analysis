# Yankees-2025-2026-Offseason-Analysis

This case study by Samuel Anglim is a "Track B" capstone project for the Google Professional Data Analytics certificate. It details a data-driven analysis of the New York Yankees' 2025 season to identify key statistical inefficiencies and financial flexibility.

This project focuses on **Pitches per Plate Appearance (P/PA)** as a primary indicator of offensive and pitching philosophy. It then benchmarks the team's traditional performance against a 10-year historical model of playoff-caliber teams.

---

## Phase 1: Ask

**Business Task:** To conduct a focused statistical analysis of the 2025 Yankees, identifying performance gaps and assessing financial flexibility for the 2026 offseason.

**Key-Guiding Questions:**

1.  **Statistical Benchmarking:** What traditional statistical profile (Runs, OBP, ERA, and K/9) has defined playoff-caliber teams over the last 10 years?
2.  **Gap Analysis (Offense):** How does the 2025 Yankees' batting P/PA (Pitches per Plate Appearance) compare to the league average?
3.  **Gap Analysis (Pitching):** How does the 2025 Yankees' pitching P/PA compare to the league average?
4.  **2026 Financial Outlook:** What is the Yankees' projected 2026 payroll, and how does their 2025 revenue provide financial flexibility?

---

## Phase 2: Prepare

**Data Sources:** Data was aggregated from several credible, public sources.

* **Financials (Revenue):** Forbes MLB Team Valuations (2025)
* **Financials (Payroll):** Spotrac (2026 Projections)
* **Advanced Stats (P/PA):** Baseball Savant ("Run Value" / "Swing-Take" Leaderboards for 2025)
* **Historical Data:** Sean Lahman Baseball Database (2015-2024)

**Data Organization & Real-World Challenges:**
A primary challenge of this analysis was data integration. The three core data sources used different team identifiers, making a single merged table impossible:
* **Financials (Forbes/Spotrac):** Used full team names (e.g., "New York Yankees")
* **Historical (Lahman):** Used team abbreviations (e.g., "NYA")
* **Modern Stats (Savant):** Used numeric team IDs (e.g., "147")

This project required cleaning and analyzing these datasets in parallel.

---

## Phase 3: Process

All data was processed and analyzed using R in RStudio. The R script (available in this repository) executes the following steps:

1.  **Load** all five raw CSV files.
2.  **Clean Financials:** Convert text-based numbers (e.g., "$680 million" and "$192,333,333") into numeric values using `parse_number()`.
3.  **Clean & Aggregate Modern Stats:**
    * Load the **player-level** P/PA data from Baseball Savant.
    * Aggregate this data to the **team-level** by calculating a *weighted average* for P/PA, grouped by `team_numeric`.
4.  **Clean & Aggregate Historical Stats:**
    * Filter the 100+ years of Lahman data to a 10-year benchmark (2015-2024).
    * Engineer new features (`OBP`, `K_per_9`) from the raw data.
    * Create the final `playoff_benchmark` by summarizing the stats for all playoff teams.

---

## Phase 4: Analyze & Share

The final R Markdown file (`.html` output) generates all key findings and visualizations.

### Key Analytical Findings:

* **Finding 1: Offensive Gap:** The analysis identified a clear gap in offensive philosophy. The 2025 Yankees' batting P/PA was **4.053**, measurably less efficient than the league average of **3.897**.
* **Finding 2: Historical Benchmark:** The 10-year benchmark for playoff teams was successfully created, establishing targets like an average OBP of **0.327** and an average ERA of **3.79**.
* **Finding 3: Financial Power:** The Yankees' 2026 projected payroll of **$192.3M** (against a 2025 revenue of $680M) confirms they have significant financial flexibility to address performance gaps in the offseason.

### Key Visualizations:

1.  **Offensive P/PA Gap:** A bar chart comparing the Yankees' batting P/PA to the league average, highlighting the inefficiency.
2.  **Pitching P/PA Efficiency:** A bar chart ranking all 30 MLB teams by pitching P/PA, with the Yankees (Team 147) highlighted.
3.  **Financial Outlook:** A bar chart of all 30 teams' 2026 payroll projections, illustrating the Yankees' relative spending room.
 
