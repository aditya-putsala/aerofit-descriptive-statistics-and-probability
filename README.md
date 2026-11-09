# Aerofit Treadmill Customer Analysis

Profiling the customers of three treadmill models (KP281, KP481 and KP781) using descriptive statistics, charts and probability tables, to see who buys which model.

---

## Table of Contents

- [Problem Statement](#problem-statement)
- [Results at a Glance](#results-at-a-glance)
- [Dataset](#dataset)
- [Data Preparation](#data-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Product Profiles](#product-profiles)
- [Notebook Recommendations, Checked Against the Data](#notebook-recommendations-checked-against-the-data)
- [Known Issues and Limitations](#known-issues-and-limitations)
- [Key Takeaways](#key-takeaways)
- [Getting Started](#getting-started)
- [Repository Structure](#repository-structure)
- [Tech Stack](#tech-stack)

---

## Problem Statement

Aerofit sells three treadmill models:

| Model | Level | Price |
|-------|-------|------:|
| KP281 | Entry | $1,500 |
| KP481 | Mid | $1,750 |
| KP781 | Advanced | $2,500 |

The business wants to know who typically buys each model, so that it can recommend the right treadmill to new customers and aim its marketing better. The notebook looks at age, gender, marital status, education, income, self-rated fitness, usage and weekly miles, and asks how each relates to the model bought.

This is a **descriptive analysis**: charts, summary tables and conditional probabilities. No predictive model is built, so there is no train/test split and no accuracy score in the notebook.

---

## Results at a Glance

| Item | Value |
|------|-------|
| Customers / columns | 180 / 9 |
| Missing values | None |
| Duplicate rows | None |
| Customers per model | KP281: 80 (44.4%); KP481: 60 (33.3%); KP781: 40 (22.2%) |
| Customers who bought KP781 and are male † | 33 of 40 (82.5%), against 50% for KP281 and 52% for KP481 |
| Mean income by model † | KP281: 46,418; KP481: 48,974; KP781: 75,442 |
| Customers with income above 68,220 † | 24, and every one bought KP781 |
| Customers rating fitness 5 † | 31; 29 of them bought KP781 |
| Features that separate the models † | Education, income, usage, fitness, miles and gender (all p < 0.01) |
| Features that do not † | Age and marital status (p = 0.96 for both) |
| KP281 against KP481 † | No feature separates them (every p above 0.14) |

Figures marked † were calculated for this README from the CSV; they are not printed in the notebook. Many sentences in the notebook's own commentary do not match its printed output (see [Known Issues and Limitations](#known-issues-and-limitations)), so this README uses the printed output and my re-calculation, not the commentary.

---

## Dataset

`aerofit_dataset.csv` has **180 rows and 9 columns**, one row per customer who bought a treadmill. The source and the period covered are not stated in the notebook.

| Column | Description | Values in the data |
|--------|-------------|--------------------|
| `Product` | Treadmill model bought | KP281: 80; KP481: 60; KP781: 40 |
| `Age` | Age in years | 18 to 50 (mean 28.8, median 26) |
| `Gender` | Gender | Male: 104; Female: 76 |
| `Education` | Years of education | 12 to 21 (mean 15.6); 16: 85; 14: 55; 18: 23; 15: 5; 13: 5; 21: 3; 12: 3; 20: 1 † |
| `MaritalStatus` | Marital status | Partnered: 107; Single: 73 |
| `Usage` | Planned or reported uses per week | 2 to 7 (mean 3.5): 2: 33; 3: 69; 4: 52; 5: 17; 6: 7; 7: 2 |
| `Fitness` | Self-rated fitness, 1 (poor) to 5 (excellent) | 1: 2; 2: 26; 3: 97; 4: 24; 5: 31 |
| `Income` | Annual income (dollars) | 29,562 to 104,581 (mean 53,720; median 50,596) |
| `Miles` | Miles run or walked per week | 21 to 360 (mean 103.2; median 94) |

The notebook does not define `Usage` and `Miles` precisely. Most of its text calls `Usage` "times per week", but one chart is labelled "Hours per Week". My understanding is that in the original case study these two columns are what the customer plans or expects to do, rather than measured behaviour, but the notebook does not say, so check this before relying on them.

**Every customer in this data bought a treadmill.** There are no people who did not buy, so the data can show how buyers differ between models, not who is likely to buy at all.

---

## Data Preparation

- **Missing values and duplicates:** none.
- **Data types:** `Product`, `Gender` and `MaritalStatus` were converted to the `category` type.
- **Binning:** six numeric columns were cut into groups for the charts and tables:

| New column | Groups (customers) |
|------------|--------------------|
| `Age_Group` | Young 18 to 25: 79 (43.9%); Adult 26 to 35: 73 (40.6%); Middle-Aged 36 to 50: 28 (15.6%); Senior over 50: 0 |
| `Education_Group` | High School 14 or fewer years: 63 (35.0%); College 15 to 16: 90 (50.0%); Advanced over 16: 27 (15.0%) |
| `Income_Group` | Low under 40K: 32 (17.8%); Medium 40K to 60K: 106 (58.9%); High 60K to 80K: 23 (12.8%); Very High over 80K: 19 (10.6%) |
| `Usage_Group` | Low 1 to 2: 33 (18.3%); Moderate 3 to 4: 121 (67.2%); High 5 to 7: 26 (14.4%) |
| `Fitness_Group` | Low 1 to 2: 28 (15.6%); Moderate 3: 97 (53.9%); High 4 to 5: 55 (30.6%) |
| `Miles_Group` | Light 0 to 50: 17 (9.4%); Moderate 51 to 100: 97 (53.9%); Active 101 to 200: 60 (33.3%); Enthusiast over 200: 6 (3.3%) |

The "Senior (over 50)" group is empty because the oldest customer is 50. The cut points are the author's choice.

---

## Exploratory Data Analysis

**Numeric summary** (as printed in the notebook)

| Column | Mean | Std | Min | Median | Max |
|--------|-----:|----:|----:|-------:|----:|
| Age | 28.79 | 6.94 | 18 | 26 | 50 |
| Education | 15.57 | 1.62 | 12 | 16 | 21 |
| Usage | 3.46 | 1.08 | 2 | 3 | 7 |
| Fitness | 3.31 | 0.96 | 1 | 3 | 5 |
| Income | 53,720 | 16,507 | 29,562 | 50,596 | 104,581 |
| Miles | 103.2 | 51.9 | 21 | 94 | 360 |

**Correlations between the numeric columns** (the notebook shows these as a heatmap)

| | Age | Education | Usage | Fitness | Income | Miles |
|---|---:|---:|---:|---:|---:|---:|
| Age | 1.00 | 0.28 | 0.02 | 0.06 | 0.51 | 0.04 |
| Education | 0.28 | 1.00 | 0.40 | 0.41 | 0.63 | 0.31 |
| Usage | 0.02 | 0.40 | 1.00 | 0.67 | 0.52 | 0.76 |
| Fitness | 0.06 | 0.41 | 0.67 | 1.00 | 0.54 | 0.79 |
| Income | 0.51 | 0.63 | 0.52 | 0.54 | 1.00 | 0.54 |
| Miles | 0.04 | 0.31 | 0.76 | 0.79 | 0.54 | 1.00 |

**Outliers** † (points beyond 1.5 times the interquartile range)

| Column | Upper limit | Outliers | Which models |
|--------|------------:|---------:|--------------|
| Income | 80,582 | 19 | All 19 are KP781 |
| Miles | 188 | 13 | 11 KP781, 1 KP281, 1 KP481 |
| Age | 46.5 | 5 | 2 KP281, 2 KP781, 1 KP481 |

**Key observations**
- **The typical customer:** young (44% are 25 or under; the median age is 26), earning about 50,000, rating fitness as 3 out of 5, and running about 94 miles a week. Age, income and miles are all right-skewed.
- **Skew and outliers:** the high-income and high-mileage tails come almost entirely from KP781 buyers.
- **Strong links between customer traits:** fitness and miles (0.79), usage and miles (0.76), usage and fitness (0.67) and income and education (0.63) are strongly related. Age and income are also moderately related (0.51), which the notebook describes as "weak correlations with Age".
- **Overlap between traits:** income, education, fitness, usage and miles all move together (0.4 to 0.8). Any single trait's link with the model bought therefore partly reflects the others.
- **Customer base:** 58% are male, 59% are partnered, and 59% are in the 40K to 60K income group.

---

## Product Profiles

**Average customer by model** †

| | KP281 | KP481 | KP781 |
|---|---:|---:|---:|
| Customers | 80 | 60 | 40 |
| Age | 28.6 | 28.9 | 29.1 |
| Years of education | 15.0 | 15.1 | 17.3 |
| Usage per week | 3.1 | 3.1 | 4.8 |
| Fitness (1 to 5) | 2.96 | 2.90 | 4.62 |
| Income | 46,418 | 48,974 | 75,442 |
| Miles per week | 82.8 | 87.9 | 166.9 |
| Share male | 50.0% | 51.7% | 82.5% |
| Share partnered | 60.0% | 60.0% | 57.5% |
| Income range | 29,562 to 68,220 | 31,836 to 67,083 | 48,556 to 104,581 |

**Share of each customer group buying each model** † (the conditional probability P(model | group); each row adds to 100%)

| Feature | Group | Customers | KP281 | KP481 | KP781 |
|---------|-------|----------:|------:|------:|------:|
| Gender | Female | 76 | 52.6% | 38.2% | 9.2% |
| | Male | 104 | 38.5% | 29.8% | 31.7% |
| Marital status | Partnered | 107 | 44.9% | 33.6% | 21.5% |
| | Single | 73 | 43.8% | 32.9% | 23.3% |
| Age group | Young (18 to 25) | 79 | 43.0% | 35.4% | 21.5% |
| | Adult (26 to 35) | 73 | 43.8% | 32.9% | 23.3% |
| | Middle-Aged (36 to 50) | 28 | 50.0% | 28.6% | 21.4% |
| Education | High School (14 or fewer) | 63 | 55.6% | 41.3% | 3.2% |
| | College (15 to 16) | 90 | 47.8% | 35.6% | 16.7% |
| | Advanced (over 16) | 27 | 7.4% | 7.4% | 85.2% |
| Income | Low (under 40K) | 32 | 71.9% | 28.1% | 0.0% |
| | Medium (40K to 60K) | 106 | 48.1% | 41.5% | 10.4% |
| | High (60K to 80K) | 23 | 26.1% | 30.4% | 43.5% |
| | Very High (over 80K) | 19 | 0.0% | 0.0% | 100.0% |
| Usage | Low (1 to 2) | 33 | 57.6% | 42.4% | 0.0% |
| | Moderate (3 to 4) | 121 | 48.8% | 35.5% | 15.7% |
| | High (5 to 7) | 26 | 7.7% | 11.5% | 80.8% |
| Fitness | Low (1 to 2) | 28 | 53.6% | 46.4% | 0.0% |
| | Moderate (3) | 97 | 55.7% | 40.2% | 4.1% |
| | High (4 to 5) | 55 | 20.0% | 14.5% | 65.5% |
| Miles | Light (0 to 50) | 17 | 70.6% | 29.4% | 0.0% |
| | Moderate (51 to 100) | 97 | 51.5% | 40.2% | 8.2% |
| | Active (101 to 200) | 60 | 30.0% | 25.0% | 45.0% |
| | Enthusiast (over 200) | 6 | 0.0% | 16.7% | 83.3% |

**Composition of each model's buyers** † (the other direction, P(group | model); each model adds to 100%)

| Feature | Group | KP281 | KP481 | KP781 |
|---------|-------|------:|------:|------:|
| Gender | Male | 50.0% | 51.7% | 82.5% |
| Education | Advanced (over 16) | 2.5% | 3.3% | 57.5% |
| Income | High or Very High (over 60K) | 7.5% | 11.7% | 72.5% |
| Usage | High (5 to 7) | 2.5% | 5.0% | 52.5% |
| Fitness | High (4 to 5) | 13.8% | 13.3% | 90.0% |
| Miles | Active or Enthusiast (over 100) | 22.5% | 26.7% | 80.0% |

**Does each feature relate to the model bought?** † (chi-square test of the binned table; Cramér's V runs from 0 for no link to 1 for a perfect link)

| Feature | p-value | Cramér's V |
|---------|--------:|-----------:|
| Income group | 3e-18 | 0.51 |
| Fitness group | 9e-18 | 0.49 |
| Education group | 8e-16 | 0.46 |
| Usage group | 4e-13 | 0.42 |
| Miles group | 8e-9 | 0.37 |
| Gender | 0.0016 | 0.27 |
| Marital status | 0.96 | 0.02 |
| Age group | 0.96 | 0.04 |

The Income and Miles tables have a few very small cells, so those p-values are approximate. The conclusion does not change.

**Key observations**
- **KP781 is the distinct model.** Its buyers earn about 75,000 on average (against about 47,000 to 49,000 for the other two), run about 167 miles a week (against about 83 to 88), use the treadmill about 4.8 times a week (against about 3.1) and rate their fitness 4.6 (against about 2.9). 90% rate fitness 4 or 5, and 82.5% are men.
- **Income:** all 19 customers with income above 80K, and in fact all 24 above 68,220, bought KP781. Nobody earning under 40K bought it.
- **Education:** 85.2% of customers with more than 16 years of education chose KP781, against 3.2% of those with 14 years or fewer. Only 27 customers are in the Advanced group.
- **Gender:** men are about three times as likely as women to buy KP781 (31.7% against 9.2%), while KP281 and KP481 sell about equally to both. Only 7 women bought KP781.
- **Age and marital status make no difference.** The model shares are nearly identical in every age group and for partnered and single customers. The notebook's claims that KP781 is bought mostly by young customers are not supported (see below).
- **KP281 and KP481 look alike.** On every column their averages are close and a statistical test finds no difference (Mann-Whitney p-values 0.14 to 0.78 for the numeric columns, and chi-square p-values of 0.98 or more for gender and marital status †). The data does not show who chooses KP481 rather than KP281.
- **Small groups:** KP781 has 40 customers, there are 6 Enthusiasts and 19 Very High earners, so the percentages in the smaller groups are rough.

---

## Notebook Recommendations, Checked Against the Data

| Recommendation in the notebook | Supported by this data? |
|--------------------------------|-------------------------|
| Target KP781 at high-income, highly educated men aged 18 to 25 | Income, education and gender: yes (see above). **Age 18 to 25: no.** KP781 shares are 21.5%, 23.3% and 21.4% across the three age groups |
| Promote KP281 to customers earning 40K to 60K with college education | Only partly. Among 40K to 60K earners, KP281 (48.1%) and KP481 (41.5%) are close, so the data does not single out KP281 for this group. Customers earning under 40K lean towards KP281 (71.9%) |
| Offer installment plans on KP481 and KP781 to middle-income customers (40K to 80K) | Plausible, but the data has nothing on payment constraints. Notably, 27.5% of KP781 buyers earn 40K to 60K |
| Run KP781 promotions and trials for women | The gap is real (9.2% of women against 31.7% of men), but the data cannot tell whether women are unreached or uninterested, and only 7 women bought KP781 |
| Build a fitness-tracking app | Cannot be tested with this data |
| Run seasonal discounts in January and February | Cannot be tested: there is no date column |
| Promote KP781 to high-mileage runners | Supported: 5 of the 6 customers over 200 miles bought it (83.3%), though the group is small |

---

## Known Issues and Limitations

These affect how far the conclusions can be trusted and are listed so that anyone reusing the analysis knows about them.

1. **Many statements in the notebook's commentary contradict its own output.** The tables and charts are correct, but the text written beside them often is not. The ones that matter most:

| Where | Notebook text | What the output shows |
|-------|---------------|-----------------------|
| Age group chart | Young about 50%, Adult about 35% | Young 43.9%, Adult 40.6% |
| Income group chart | Medium about 50%, High about 25% | Medium 58.9%, High 12.8% |
| Education group chart | College about 60%, Advanced about 30% | College 50.0%, High School 35.0%, Advanced 15.0% |
| Education chart | 16 years is 60%, and 18 or more years is about 5% | 16 years is 47.2%, and 18 or more is 15.0% |
| Usage charts | Moderate usage 60% | 67.2% |
| Fitness chart | Moderate about 50%, High about 35% | 53.9% and 30.6% |
| Product vs age | KP781 is mostly bought by young customers; P(KP781 given Young) = 0.27, Adult = 0.20 | 0.22 for Young and 0.23 for Adult: no age effect |
| Product vs education | P(KP781 given Advanced) = 0.52 | 0.85 |
| Product vs income | P(KP781 given Very High income) = 0.65 | 1.00 (19 of 19) |
| Product vs miles | 15% of KP781 buyers are in the Active group | 67.5% of KP781 buyers (the 15% is the Active KP781 buyers as a share of all customers) |
| Product vs usage | KP781 buyers have High usage | 52.5% have High usage and 47.5% Moderate |
| Marginal probabilities | 24% KP281 by males; 20% KP281 by College; 15% KP781 by Advanced; 25% KP281 by Medium income | 22%, 24%, 13% and 28% |
| Correlation matrix | Weak correlations with Age | Age and Income is 0.51 (moderate) |
| Miles boxplot | Outliers above 200 miles a week | The upper whisker is about 188, and 13 points lie above it |
| Product vs marital status | Partnered customers buy more of each product | Only because they are 59% of customers; the shares are the same for both groups |

   The "Business Insights" section repeats several of these, so its figures should not be quoted.
2. **Duplicated chart.** The second "Product vs Age Group" section plots the age chart again but its note talks about education. The Product versus Education chart is never drawn.
3. **The data only contains buyers.** The notebook's goal talks about "probabilities of purchasing each treadmill", but the tables give the share of buyers with a given trait who chose each model. They cannot say how likely someone is to buy a treadmill in the first place.
4. **Marginal and conditional probabilities are easy to mix up.** The notebook's "marginal" figures are joint shares of all 180 customers (for example, 22% are male KP281 buyers), which depend on how many customers are in each group. P(model given group) and P(group given model) answer different questions. Both are shown above and labelled.
5. **No significance tests, and small groups.** The notebook relies on bar charts. With 180 customers, only 40 for KP781, and groups as small as 6 or 7 customers, a visual difference can be chance. The chi-square tests above show that most of the differences are real, but age and marital status are not.
6. **Traits overlap, so "drivers" are not independent.** Income, education, fitness, usage and miles are correlated (0.4 to 0.8). The analysis looks at one trait at a time and cannot say which one matters most. A buyer's income may matter because richer customers can afford KP781, because they are fitter, or both.
7. **Meaning and timing of `Usage`, `Fitness` and `Miles` are unclear.** If they are the customer's plans or self-ratings at the time of purchase, a customer who chose a premium model may simply report more ambitious figures, which would make the link with product partly circular. The notebook also labels `Usage` as hours in one chart and times in the rest.
8. **The stated goal is not delivered.** The problem statement says the analysis should help recommend the right treadmill to new customers, but no recommendation rule or model is built. As a rough check (not in the notebook), a random forest with 5-fold cross-validation repeated 10 times separated KP781 buyers from the rest with 97% accuracy †, against 77.8% for always saying "not KP781". For all three models it reached only about 65% accuracy † against 44.4% for always predicting KP281, because KP481 buyers are mostly confused with KP281 buyers.
9. **Binning choices.** The group cut points are arbitrary, produce an empty Senior group, and hide detail (for example, education has only a handful of values, and usage of 4 times is grouped with 3).
10. **The saved notebook contains almost no outputs.** Only 2 of its 61 code cells have saved output. The results are visible in the PDF export (`Business_Case_Aerofit.pdf`), whose loading cells differ slightly from the notebook (it reads `aerofit_treadmill.csv` from a download link, while the notebook reads `aerofit_dataset.csv`). The analysis cells are the same.
11. **The download cell does not work as written.** `!gdown` in the notebook points to a Google Drive folder link, which cannot download a single CSV. Put `aerofit_dataset.csv` next to the notebook instead.
12. **Source and sampling unknown.** The notebook does not say where the data comes from, how customers were chosen, or the period covered, and the product counts (exactly 80, 60 and 40) are unusually round. The results describe these 180 customers and may not apply to Aerofit's customers in general.

---

## Key Takeaways

- KP281 is the most common model (44% of customers), then KP481 (33%) and KP781 (22%).
- **KP781 buyers form a distinct group:** higher income (about 75,000), more years of education (about 17), higher self-rated fitness (4.6 of 5), heavier usage and about twice the weekly miles of the other buyers. 82.5% are men.
- **Income, education, fitness, usage and miles all separate KP781 buyers from the rest. Age and marital status do not.** The notebook's idea that KP781 appeals mainly to young customers is not supported.
- **KP281 and KP481 buyers cannot be told apart** with the columns in this data. Choosing between them probably depends on things the data does not record, such as budget, features or sales advice.
- **The women's share of KP781 is low** (9.2% of women against 31.7% of men), based on only 7 women.
- Treat the notebook's written commentary and "Business Insights" with caution; use the tables in this README or the notebook's printed output. Several recommendations (age targeting, seasonal promotions, the app) are not backed by this data.

---

## Getting Started

**1. Install the libraries**
```bash
pip install pandas numpy matplotlib seaborn jupyter
```

**2. Place the dataset next to the notebook.** The notebook loads the data with `pd.read_csv('aerofit_dataset.csv')`, which matches the file in this repository. Skip or delete the `!gdown` cell (see the known issue above).

**3. Open the notebook**
```bash
jupyter notebook Business_Case_Aerofit.ipynb
```

**4. Run the cells in order.** The saved notebook has no outputs, so run it to see the charts, or open `Business_Case_Aerofit.pdf` for a saved copy.

---

## Repository Structure

```
.
├── Business_Case_Aerofit.ipynb   # Cleaning, binning, EDA and probability tables
├── Business_Case_Aerofit.pdf     # Printed copy of the notebook with outputs
├── aerofit_dataset.csv           # 180 customers x 9 columns
└── README.md
```

---

## Tech Stack

Python, pandas, NumPy, Matplotlib, Seaborn, Jupyter Notebook. (The chi-square tests and the classification check marked † in this README used SciPy and scikit-learn; the notebook itself does not.)
