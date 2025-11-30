# The Relationship Between Awards, Box Office, and IMDb Ratings

## Overview of Project

This project looks into whether big budget or award winning movies tend to get higher audience ratings.

In this project, the I will investigate the influence of *production budgets*, *box office performance*, and *awards* on public perception, through IMDb ratings, using the *Box Office Mojo dataset* enriched with external metadata from the *OMDb API*.

Apart from public data, I will include my own personal rating according to IMDb to give an individual perspective.
This comparison will reveal whether personal preferences align with global audience opinions highlighting how subjective taste differs from general trends.

The goal is to determine if financial and critical success correlate with audience approval, therefore quantifying what makes a movie "successful."


## AI Usage Disclosure
This project used a Large Language Model (LLM) for:
* Code structure suggestions
* Text polishing & formatting
* Minor refactoring (comments, backticks, plot styling)
  
All statistical decisions, data cleaning logic, feature engineering, tests, and interpretations were done by me.
Code segments directly influenced by the LLM are explicitly marked.



## Motivation

Being a movie enthusiast, I have often wondered if commercial and critical success actually correlate to the enjoyment of the people who watch it.
Some low-budget films turn out to be classics, while plenty of blockbusters fall flat with audiences despite their size and awards.
This project gives an opportunity to investigate this question through real data. It combines my interest in cinema with data analysis and visualization techniques.
By analyzing the patterns between *budget*, *box office*, and *ratings*, I try to understand whether the audience perception is influenced most by *financial investment*, *critical acclaim*, or *storytelling quality*.

## Hypotheses

### **1. Budget vs Ratings**

* **H₀₁:** There is no significant relationship between a movie’s budget and its IMDb rating or Metascore.
* **H₁₁:** Higher budget films tend to receive higher IMDb ratings and Metascores.

### **2. Awards vs Ratings**

* **H₀₂:** Awards and nominations have no significant effect on IMDb ratings or Metascores.
* **H₁₂:** Award winning movies, especially Oscar winners, tend to have higher IMDb ratings and Metascores than non-awarded films.

### **3. Box Office vs Ratings**

* **H₀₃:** Box office performance (domestic or worldwide gross) has no association with IMDb ratings or Metascores.
* **H₁₃:** There is a positive correlation between box office results and both IMDb ratings and Metascores.
  
### **4. Personal vs Public Ratings (RatingGap)**
* **H₀₄:** On average, my personal ratings do not systematically differ from IMDb ratings
* **H₁₄:** On average, my personal ratings systematically differ from IMDb ratings
  
## Data Sources

### **Box Office Mojo Dataset**

* Source: [Kaggle – Box Office Mojo Movies Dataset](https://www.kaggle.com/datasets/igorkirko/wwwboxofficemojocom-movies-with-budget-listed)
* Scope: ~3,500 U.S. released, English language films between 1990 and 2020
* Description: Provides detailed information on movie budgets, domestic/international/worldwide grosses, release dates, MPAA ratings, and genres.
* Context:
  This dataset was created by scraping over 250,000 Box Office Mojo pages. It only contains films that have a listed production budget.
* Purpose:
  Core financial information, such as budgets and worldwide grosses, will be provided by the Box Office Mojo dataset, which acts as the backbone of this project.

### **OMDb API**

* Source: [Open Movie Database (OMDb API)](https://www.omdbapi.com/)
* Collected Fields:
  * Awards (Oscar and other wins/nominations)
  * BoxOffice (domestic revenue)
  * IMDb Rating
  * Metascore
  * IMDb Votes
* Purpose:
  The main purpose of the OMDb API is to gain access to award and critical evaluation data not available in the Box Office Mojo dataset.
  
  Specifically, it provides:
  * Awards information (to identify Oscar winners and total nominations)
  * Metascore values for critical reception

  These fields, in conjunction with the IMDb ratings, can be used to analyze how awards and critical reviews relate to audience perception and box-office performance.

### **Personal IMDb Ratings**

* Source: IMDb personal export file
* Purpose: Used for optional comparison against public ratings.
  
## Planned Analysis

**1. Budget vs Rating**

   * Explore whether movies with bigger budgets actually get higher IMDb ratings.  
   * Visualize this relationship with scatter plots and check correlations to see if spending more really pays off.

**2. Awards vs Rating**

   * Group movies into categories like Oscar winners, nominated, or non-awarded.  
   * Compare their average IMDb ratings using box plots to see if awards truly reflect audience appreciation.

**3. Box Office vs Rating**

   * Check if global box-office earnings are linked to audience ratings.  
   * Use simple regression analysis to find out whether commercial success also means higher ratings.
     
**4. Personal vs Public Ratings**

* Compare my own IMDb ratings (MyRating) with public IMDb ratings for the subset of movies I have rated.
* Analyze the RatingGap (MyRating − IMDb Rating) using a one-sample t-test and visualizations to see whether I am systematically more generous or more critical than the general audience.


## Expected Outcomes

* Higher budgets may correlate with slightly higher IMDb ratings, though not always, to suggest money is not solely a guarantee of audience appreciation.
* It is expected that award-winning films, especially Oscar winners, will always have consistently higher ratings than the non-awarded.
* The low-budget films that are rated most highly may be outliers. They may demonstrate examples of storytelling success without major production costs.
* The project will yield visual and statistical evidence on whether prestige and investment correspond with audience opinion.

## Data Cleaning & Preprocessing

**Key steps included:**

- Converting all money fields to numeric (removing `$`, `,`, whitespace)
- Standardizing column names across Mojo + OMDb
- Converting Year, IMDb rating, Metascore, and MyRating to numeric
- Removing invalid or duplicate entries
- Merging datasets using:
  - **imdbID** (preferred key)
  - **fallback:** Title + Year

## Feature Engineering

Created new variables:

- **Profit** = Worldwide – Budget  
- **ROI** = Worldwide / Budget  
- **AwardLevel** = Oscar Winner / Oscar Nominee / Other Award / No Awards  
- **AwardTier** = Winner / Nominee Only / No Award  
- **RatingGap** = MyRating – IMDbRating  

## Exploratory Data Analysis (EDA)

### **Descriptive Statistics**

- Budgets & grosses are extremely right-skewed.  
- IMDb public ratings cluster between **6.0–8.0**.  
- Awarded films have noticeably higher ratings.

### **Correlation Analysis**

- **Budget ↔ WorldwideGross:** strong positive correlation  
- **Ratings ↔ Budget:** very weak correlation (r ≈ 0.1)  
- **Awards ↔ Ratings:** moderate correlation  
- **MyRating:** aligns with IMDb but shows individual deviations  

### **Visualizations Included**

- Budget vs IMDb Rating (log-scale scatter)
- Worldwide Gross vs IMDb Rating
- Boxplots of AwardLevel vs Ratings
- Correlation heatmaps
- MyRating vs IMDb Rating scatter


**Overall Insight:**  
High-budget movies do **not** consistently receive high ratings, but award-winning films **generally do**.


## Formal Statistical Analysis

### **ANOVA: Award groups vs IMDb Rating**
- Statistically significant → **Reject H₀₂**  
- Post-hoc Welch t-tests show:  
  - Oscar Winners > Nominees  
  - Nominees > No Awards  

### **Correlation: Budget vs Ratings**
- Very weak positive correlation  
- Reject H₀₁ statistically  
- **Practical significance is extremely low**

### **Box Office vs Ratings**
- Weak positive correlation  
- Box office success ≠ audience appreciation

### **Personal Ratings vs Public IMDb Ratings**
- One-sample t-test on RatingGap (MyRating − IMDb Rating) → **Reject H₀₄**
- Mean RatingGap is significantly **negative** (I rate films lower on average)
- Personal ratings show **predictable divergences** from general audience sentiment

## Key Findings

### **Budget Matters Statistically, but Not Meaningfully**
- r ≈ 0.05–0.13  
- Effect size extremely weak  
- High budget ≠ high rating

### **Awards Are the Best Predictor of High Ratings**
- Oscar winners outperform all other groups  
- Awards correlate strongly with critical and audience scores

### **Box Office Success Is Not a Rating Predictor**
- Many high-grossing films are only mid-rated  
- Many highly rated films earn modestly

Personal Ratings Show Individual Taste
- I tend to rate movies slightly lower than IMDb users on average.
- My ratings follow the general trend but show consistent, predictable deviations (mean gap ≈ −0.19).

## Conclusions

- Financial investment alone **does not** guarantee high audience ratings.  
- Awards and critical recognition **strongly align** with higher ratings.  
- Audience perception is influenced more by **quality and storytelling** than by production scale.  
- Personal ratings reveal predictable divergences from general audience sentiment, supported by a significantly negative RatingGap.

**Overall, the analysis shows that storytelling is the real driver of what makes a movie memorable.**


  
