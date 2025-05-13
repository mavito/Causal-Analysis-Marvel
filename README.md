
# 🎬 MCU Actor Causal Impact Analysis

This project investigates the **causal effect** of joining the Marvel Cinematic Universe (MCU) on an actor's career — measured primarily via **opening weekend box office revenue**. It leverages **staggered Difference-in-Differences (DiD)** methodology, fixed effects regression, and exploratory data analysis, combining real-world movie metadata with rigorous statistical modeling.

---

## 📌 Objective

To evaluate whether appearing in MCU films causes a statistically significant increase in an actor’s box office performance, specifically focusing on **log of opening weekend revenue**. Control variables include number of lead roles, average IMDb ratings, and film frequency.

---

## 🧩 Data Collection

All data is collected using public APIs (TMDb, OMDb, and Box Office Mojo). The process includes:

- **Fetching MCU Movie Metadata** using TMDb (company ID: 420)
- **Filtering out** irrelevant genres (e.g., documentaries, one-shots)
- **Fetching Actor Filmographies** with budget, revenue, runtime, and cast order
- **Scraping Opening Weekend Data** using IMDb IDs via Box Office Mojo
- **Ratings from OMDb**, including IMDb, Rotten Tomatoes, and Metacritic

> 📌 *Only actors with a high popularity score (above a defined threshold, main Avengers) are selected as leads.*

---

## 🧪 Data Preprocessing & Engineering

- Created binary flags for **MCU entry**, **Treatment**, **Lead Role**, and **Supporting Role**
- Extracted **release year**, **quarter**, and **entry year**
- Applied log transformation to `opening_weekend` to stabilize variance
- Constructed panel dataset: actor × year

---

## 🔬 Methodology

### 1. **Treatment Definition**
Actors are labeled as treated if they joined MCU. Control group includes actors of similar genre participation who didn’t.

### 2. **Staggered Difference-in-Differences (DiD)**
Visual and statistical models account for actors entering MCU at different times, using:
- Event study plots
- Parallel trend checks
- Heatmaps of treatment timing

### 3. **Fixed Effects Model (TWFE)**
```python
TWFE Model:
opening_weekend_log ~ Treatment + C(actor_name) + C(release_year)
```

### 4. **Covariate Controls**
Additional model includes:
- `count_lead_roles`
- `average_rating`
- `film_count`

### 5. **Propensity Score Matching (PSM)**
Logistic regression is used to compute treatment propensity. Plots and KDEs visualize covariate balance across groups.

---

## 📊 Visualizations

- 📈 **Line Plots**: Pre-trend checks across years
- 🧊 **Heatmaps**: Actor-year matrix for treatment exposure
- 📦 **Boxplots**: Revenue distribution by treatment status
- 🔄 **Partial Regression**: Isolating treatment effect
- 🧬 **Genre Distribution**: Treated vs. control group

---

## 📈 Key Results

- **TWFE Coefficient**: +1.91 log points (p = 0.003)
- **Large treatment effect** observed in post-entry period
- **Robust to controls** like lead roles and average ratings

---

## 📚 Dependencies

```bash
pip install pandas numpy matplotlib seaborn plotly statsmodels scikit-learn tqdm beautifulsoup4 requests
```

---

## 📌 Usage

### Step 1: Data Collection
Run `data_collection.ipynb` to fetch and export data into `treatment_control.json`.

### Step 2: Causal Analysis
Run `causal_analysis.ipynb` to generate visualizations and statistical results.

---

## 🧠 Authors & Credits

- **Manas Tokale, Robin Luke, Srijan Shetty, Yu Zhuoran**
- BADM 590: Marketing Analytics — Final Project
- UIUC, May 2025

---

## 🔖 License

This project is for educational purposes only. Use of TMDb, OMDb, and Box Office Mojo data must comply with their terms of service.
