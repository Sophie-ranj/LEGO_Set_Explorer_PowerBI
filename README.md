Sure — based on your screenshots, your field names are **exactly**: `category`, `themeGroup`, `theme`, `name`, `set_id` (and likely `price`, `pieces`, `year`, `age`, `imageURL`).
Here’s the **entire README.md as one file**, with the DAX updated to match that naming:

````md
# LEGO Sets Explorer Dashboard by Power BI

A Power BI report built from `lego_sets.csv` to analyze LEGO sets by **category**, **themeGroup/theme**, **age**, **price**, and **pieces**, with a **Max Price** what-if parameter, image tooltips, bookmarks (reset), and a decomposition tree drill page.

<img src="images/LegoDashboard.png" alt="Dashboard preview" width="800">

## ⬇️ Download
- <a href="./LEGO%20Sets%20Dashboard.pbix?raw=1" target="_blank" rel="noopener noreferrer">Download the PBIX</a>

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

## DAX (Measures + Interactivity)

```DAX
-- Core KPIs (match your card titles)
total sets = DISTINCTCOUNT(lego_sets[set_id])
total groups = DISTINCTCOUNT(lego_sets[themeGroup])

average pieces = AVERAGE(lego_sets[pieces])
average price = AVERAGE(lego_sets[price])
avg. age = AVERAGE(lego_sets[age])

-- What-if parameter filter (Max Price: 0–850 step 5)
-- Use this as a visual-level filter on the table: Show Under Max Price = 1
Show Under Max Price =
VAR MaxP = SELECTEDVALUE('Max Price'[Max Price], 850)
VAR ThisPrice = SELECTEDVALUE(lego_sets[price])
RETURN
IF( NOT ISBLANK(ThisPrice) && ThisPrice <= MaxP, 1, 0 )

-- Selected-set details (show placeholders when multiple sets are selected)
Selected Set Name =
IF(
    HASONEVALUE(lego_sets[set_id]),
    SELECTEDVALUE(lego_sets[name]),
    "Multiple sets selected"
)

Selected Price =
IF( HASONEVALUE(lego_sets[set_id]), SELECTEDVALUE(lego_sets[price]) )

Selected Year =
IF( HASONEVALUE(lego_sets[set_id]), SELECTEDVALUE(lego_sets[year]) )

Selected Pieces =
IF( HASONEVALUE(lego_sets[set_id]), SELECTEDVALUE(lego_sets[pieces]) )

Selected Age =
IF( HASONEVALUE(lego_sets[set_id]), SELECTEDVALUE(lego_sets[age]) )

Selected Image URL =
IF( HASONEVALUE(lego_sets[set_id]), SELECTEDVALUE(lego_sets[imageURL]) )

-- Optional formatted price for a big card
Selected Price (Formatted) =
IF(
    HASONEVALUE(lego_sets[set_id]),
    FORMAT( SELECTEDVALUE(lego_sets[price]), "$#,0.00" ),
    "—"
)
```

---

## Report Pages & UX
- Page 1: KPI cards + slicers (**themeGroup**, **theme**, **Age Range**, **Max Price**) + table with tooltip image + selected-set detail panel
- **Reset Filters** button built with a **Bookmark** (restores default/unfiltered state)
- Visual interactions adjusted so **table selections don’t filter** the top KPI cards
- Page 2: **Decomposition Tree** using `category → themeGroup → theme → name` with navigation buttons

<img src="images/DecompositionTree.png" alt="Decomposition Tree preview" width="800">

---

## Author
**Sophie Ranj** — ranj.sophie@outlook.com  
LinkedIn: <a href="https://linkedin.com/in/sophie-ranj" target="_blank" rel="noopener noreferrer">linkedin.com/in/sophie-ranj</a>  
Portfolio: <a href="https://sophie-ranj.github.io/" target="_blank" rel="noopener noreferrer">sophie-ranj.github.io</a>
````

If any one of these columns is named slightly differently in your model (most commonly `imageURL` vs `imageUrl` or `themeGroup` vs `theme_group`), tell me the exact spelling and I’ll patch the DAX in one go.
