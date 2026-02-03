# LEGO Sets Explorer Dashboard by Power BI

A Power BI report built from `lego_sets.csv` to analyze LEGO sets by **category**, **themeGroup/theme**, **age**, **price**, and **pieces**, with a **Max Price** what-if parameter, image tooltips, bookmarks (reset), and a decomposition tree drill page.

<img src="images/lego 1.png" alt="Dashboard preview" width="800">

## ⬇️ Download

- <a href="./lego.pbix?raw=1" target="_blank" rel="noopener noreferrer">Download the PBIX (LEGO SET)</a>

---

## 🔎 Findings (Insights)

- After cleaning, the dataset includes **4,385** sets with **411** average pieces and **$45** average price—yet pricing is **strongly right-skewed** (premium tail up to ~**$850**). The **Max Price** parameter is key to analyzing “typical” sets without premium outliers dominating the table.
- The catalog is highly concentrated in **Normal** (**4,289 / 4,385 ≈ 97.8%**), meaning most insights come from this category—while smaller categories represent niche subsets.
- The **decomposition tree** surfaces rare combinations quickly: drilling **Extended → Historical → Castle** narrows down to just **6** sets, which is useful for spotting thin segments and niche themes.
- The selected-set panel helps validate outliers instantly (e.g., **Table Football (2022)**: **$249.99**, **2,339 pieces**, **Age 18+**)—clearly above the overall averages.

---

## Data Prep (Power Query)

- Loaded `lego_sets.csv`, removed: `minifigs`, `bricksetURL`, `thumbnailURL`
- Fixed data types (notably `price`, `age`, `pieces`, `year`)
- Filtered out missing values for `price`, `age`, `pieces`, `imageURL`
- Final cleaned row count used in the report: **4,385** sets

## Derived Columns

- **Age Range**: `Over 18`, `10 to 17`, `5 to 9`, `1 to 4`
- **Price Range**: `$`, `$$`, `$$$`, `$$$$`, `$$$$$` (thresholds: 25/50/100/500)

---

## Report Pages & UX

- Page 1: KPI cards + slicers (**themeGroup**, **theme**, **Age Range**, **Max Price**) + table with tooltip image + selected-set detail panel
- **Reset Filters** button built with a **Bookmark** (restores default/unfiltered state)
- Visual interactions adjusted so **table selections don’t filter** the top KPI cards
- Page 2: **Decomposition Tree** using `category → themeGroup → theme → name` with navigation buttons

<img src="images/lego 2.png" alt="Decomposition Tree preview" width="800">

---

## Author

**Sophie Ranj** ranj.sophie@outlook.com  
LinkedIn: <a href="https://linkedin.com/in/sophie-ranj" target="_blank" rel="noopener noreferrer">linkedin.com/in/sophie-ranj</a>  
Portfolio: <a href="https://sophie-ranj.github.io/" target="_blank" rel="noopener noreferrer">sophie-ranj.github.io</a>
