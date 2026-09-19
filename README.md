# Google Play Store App and Review Analysis

## 1. Project Overview

This project performs exploratory data analysis on Google Play Store app metadata and user-review data.

The analysis studies:

* App categories
* Ratings
* Installs
* Free versus paid apps
* Pricing
* App size
* Content ratings
* Review volume
* User-review sentiment
* Sentiment polarity
* Sentiment subjectivity

The project uses two datasets:

1. **Play Store App Data** — app-level information such as category, rating, installs, type, size, and price.
2. **User Review Data** — translated user reviews, sentiment labels, sentiment polarity, and sentiment subjectivity.

---

## 2. Project Objectives

The project is designed to:

* Clean duplicate, missing, and inconsistent app and review records.
* Convert app attributes such as installs, reviews, size, and price into numerical variables.
* Analyse category-level app distribution and user engagement.
* Compare free and paid app behaviour across install ranges.
* Study pricing, size, rating, and install relationships.
* Analyse sentiment distribution in user reviews.
* Examine polarity, subjectivity, and review length.
* Merge app metadata with aggregated review sentiment for category-level analysis.

---

## 3. High-Level Workflow

```text
Raw Google Play Store data
        │
        ├── App metadata
        └── User reviews
        │
        ▼
Data-quality checks
        │
        ├── Duplicate analysis
        ├── Missing-value analysis
        └── Data-type inspection
        │
        ▼
Data cleaning and feature engineering
        │
        ├── Remove duplicates
        ├── Handle missing values
        ├── Convert dates and numeric fields
        ├── Create primary genre
        └── Create review length
        │
        ▼
App-level exploratory analysis
        │
        ├── Categories
        ├── Ratings
        ├── Installs
        ├── Pricing
        ├── App size
        └── Content rating
        │
        ▼
Review-level sentiment analysis
        │
        ├── Sentiment share
        ├── Polarity
        ├── Subjectivity
        └── Review length
        │
        ▼
Merge app and review data
        │
        ▼
Category-level sentiment analysis
```

---

## 4. Dataset Overview

### 4.1 Play Store App Data

The raw app dataset contains more than 10,000 records. After cleaning, the repository contains **10,358 app records**.

| Variable         | Description                      |
| ---------------- | -------------------------------- |
| `App`            | Application name                 |
| `Category`       | App category                     |
| `Rating`         | User rating on a 1–5 scale       |
| `Reviews`        | Number of reviews                |
| `Size`           | App size                         |
| `Installs`       | Install count range              |
| `Type`           | Free or paid app                 |
| `Price`          | App price                        |
| `Content Rating` | Age/content suitability category |
| `Genres`         | App genre or genres              |
| `Last Updated`   | Last update date                 |
| `Current Ver`    | Current app version              |
| `Android Ver`    | Required Android version         |

### 4.2 User Review Data

The raw review dataset contains more than 64,000 records. After duplicate removal and missing-value removal, the repository contains **29,692 review records**.

| Variable                 | Description                          |
| ------------------------ | ------------------------------------ |
| `App`                    | Application name                     |
| `Translated_Review`      | Review translated into English       |
| `Sentiment`              | Positive, Negative, or Neutral       |
| `Sentiment_Polarity`     | Strength and direction of sentiment  |
| `Sentiment_Subjectivity` | Degree of opinion versus objectivity |
| `review_len`             | Length of translated review text     |

---

## 5. Data Cleaning and Feature Engineering

### 5.1 App Data Cleaning

The project performs the following steps:

1. Removes duplicate app records.
2. Replaces placeholder values such as `-`, `no`, `null`, `unknown`, blank spaces, `miss`, and `missing` with null values.
3. Converts `Last Updated` into datetime format.
4. Extracts year and month from the updated date.
5. Creates a primary-genre field.
6. Converts review count into a numerical field.
7. Converts install ranges into numerical install values.
8. Converts app size into numerical megabytes.
9. Converts price into numerical dollars.

### 5.2 App Features Created

| New feature     | Purpose                                     |
| --------------- | ------------------------------------------- |
| `updated`       | Datetime version of Last Updated            |
| `year`          | Year extracted from updated date            |
| `month`         | Month extracted from updated date           |
| `primary_genre` | First genre extracted from the Genres field |
| `rev_num`       | Numerical review count                      |
| `install_num`   | Numerical install count                     |
| `size_mb`       | App size in megabytes                       |
| `price_dol`     | App price in dollars                        |

### 5.3 Review Data Cleaning

The review-data workflow:

1. Removes duplicate reviews.
2. Drops records with missing review text or sentiment values.
3. Creates a `review_len` feature from translated-review text length.

```python
rev["review_len"] = rev["Translated_Review"].str.len()
```

---

## 6. App Category Analysis

The project uses a bar chart to compare the number of apps in each category.

### Key Findings

| Category       | Approximate app count |
| -------------- | --------------------: |
| Family         |                 1,943 |
| Game           |                 1,121 |
| Tools          |                   843 |
| Beauty         |                    53 |
| Parenting      |                    60 |
| Comics         |                    60 |
| Events         |                    60 |
| Art and Design |                    65 |

### Insights

* **Family** is the largest category in the dataset.
* **Game** and **Tools** are also high-volume categories.
* Business, Medical, and Productivity show moderate app volume.
* Beauty, Parenting, Comics, Events, and Art and Design have relatively low app counts.
* Large categories may have higher demand but also stronger competition.
* Smaller categories may offer niche opportunities with lower competition.

---

## 7. Rating Analysis

The project calculates the average rating for each app category.

### Key Findings

* Most categories have an average rating above **4.0**.
* Dating apps have the lowest reported average rating, approximately **3.97**.
* Events, Education, Art and Design, Books and Reference, Personalization, and Parenting have average ratings above approximately **4.3**.
* Ratings do not show a dramatic difference across categories.

### Insight

Ratings alone may not be enough to differentiate app categories because most categories have relatively high average ratings.

---

## 8. Free Versus Paid App Analysis

The project compares app type across install ranges.

### Key Findings

* Free apps dominate every install bucket.
* Apps with approximately 1M installs are the largest group.
* Apps with 10M and 100K installs are also common.
* Very high install ranges, such as 500M and 1B, are mainly achieved by free apps.
* Paid apps have limited presence across most install buckets.

### Business Interpretation

| App model    | Potential advantage                       | Potential challenge                           |
| ------------ | ----------------------------------------- | --------------------------------------------- |
| Free apps    | Faster user acquisition and higher reach  | Revenue pressure from ads or in-app purchases |
| Paid apps    | Can target committed, value-focused users | Lower discoverability and slower scaling      |
| Hybrid model | Combines wide reach with monetisation     | Requires a balanced free and paid experience  |

---

## 9. Review Trends Over Time

The project aggregates review count by year.

### Key Findings

* Review volume increases over time.
* Review counts remain below approximately 250K until 2012.
* Review volume grows into millions after 2012.
* The notebook reports approximately 103M reviews in 2017.
* Review counts enter the billions after 2017.

### Interpretation

The growth in reviews suggests increasing Android-device adoption, stronger internet access, more applications, and higher platform engagement.

---

## 10. Category-Level Review Analysis

The project compares total review count across app categories.

| Category      | Approximate review count |
| ------------- | -----------------------: |
| Game          |                     1.4B |
| Communication |                    600M+ |
| Social        |                    500M+ |
| Family        |                    390M+ |

### Key Findings

* Games generate the highest review volume.
* Communication, Social, and Family also show high engagement.
* Beauty, Events, Parenting, and Medical show comparatively low review volume.
* High-review categories provide rich user-feedback data but may dominate platform visibility.

---

## 11. Paid-App Price Analysis

The project studies the price distribution of paid apps using box plots.

### Key Findings

| Category                            | Notebook observation                                     |
| ----------------------------------- | -------------------------------------------------------- |
| Lifestyle                           | Median price around $4.99; maximum price around $400     |
| Finance                             | Median price around $28.99; maximum price around $399.99 |
| Games, Family, Photography, Medical | Some extreme price outliers near $400                    |

### Interpretation

* Most paid-app categories have lower median prices.
* Lifestyle and Finance show the widest price variation.
* Premium pricing is more visible in specialised categories.
* Lower pricing can reduce the entry barrier and improve market reach.

---

## 12. Price Versus Installs

The project compares average paid-app price across install buckets.

### Key Findings

* Apps with zero installs have the highest average prices.
* Apps with high install counts, including 1M and 10M installs, generally have lower prices.
* Apps with 1B and 500M installs are free in the dataset.
* Moderate install ranges such as 5K, 10K, 50K, and 100K show moderate average prices.

### Insight

The notebook identifies a negative relationship between pricing and install scale:

```text
Higher price → lower installs
Lower price → higher reach and installs
```

---

## 13. App Size and Rating Analysis

The project uses a scatter plot to study app size and user ratings.

### Key Findings

* Most apps have ratings above 3.5.
* Most paid apps have sizes below approximately 120 MB.
* Free apps dominate across app sizes and ratings.
* Large paid apps are relatively uncommon.
* There is no clear correlation between app size and rating.
* Highly rated apps can exist across a wide range of app sizes.

### Interpretation

Users may value functionality and content more than app size alone.

---

## 14. Platform Growth and Content Rating

The project uses a multi-panel visualisation to study app count, average rating, and content rating over time.

### Key Findings

| Metric                 | Notebook finding    |
| ---------------------- | ------------------- |
| App count in 2010      | 1                   |
| App count in 2018      | 6,935               |
| Average rating in 2012 | Approximately 3.78  |
| Average rating in 2018 | Approximately 4.245 |
| Everyone-rated apps    | 8,393               |
| Teen-rated apps        | 1,146               |
| Mature 17+-rated apps  | 447                 |

### Interpretation

* The number of apps rises strongly after 2014.
* App ratings remain broadly stable, mainly between approximately 3.8 and 4.2.
* Everyone-rated apps dominate the dataset.
* The content-rating distribution has limited diversity compared with the number of Everyone-rated apps.

---

## 15. App-Level Correlation Analysis

The project calculates correlations among:

* Numerical review count
* App size in MB
* Price in dollars
* Rating

### Finding

The notebook reports very low or near-zero correlations among the analysed app variables.

This means the selected variables do not show strong linear relationships in the dataset.

---

## 16. User-Review Sentiment Analysis

The project analyses the distribution of Positive, Negative, and Neutral reviews.

| Sentiment | Share of reviews |
| --------- | ---------------: |
| Positive  |            64.0% |
| Negative  |            21.3% |
| Neutral   |            14.7% |

### Key Findings

* Positive reviews form the majority.
* Negative reviews remain significant and may highlight user dissatisfaction.
* Neutral reviews form the smallest group.

---

## 17. Sentiment Polarity and Subjectivity

The project analyses sentiment polarity, sentiment subjectivity, and review length.

### Polarity Findings

| Review sentiment | Notebook observation          |
| ---------------- | ----------------------------- |
| Positive         | Median polarity around 0.35   |
| Negative         | Median polarity around -0.193 |
| Neutral          | Polarity around 0             |

### Subjectivity Findings

* Positive and negative reviews show median subjectivity around 0.51.
* Neutral reviews are generally less subjective.
* Most reviews have moderate subjectivity.
* Highly subjective reviews can reveal user experience and emotional response.
* More objective reviews can provide specific feedback for product and engineering teams.

---

## 18. App and Review Data Integration

The project aggregates reviews by app and merges them with app metadata.

### Review Aggregation

The review data is aggregated by app using:

* Average review length
* Average sentiment polarity
* Average sentiment subjectivity
* Most frequent sentiment category

### Merge Process

```python
agg = app.merge(rev_agg, on="App", how="inner")
```

After merging, the project analyses sentiment distribution by app category and calculates correlations among:

* Rating
* Review count
* Price
* Review length
* Sentiment polarity
* Sentiment subjectivity

---

## 19. Repository Structure

```text
PLAY-STORE-APP-DATA-EDA/
│
├── DATA/
│   ├── cw app data (1).csv
│   └── cw user_rev data (1).csv
│
├── NOTEBOOK/
│   └── PLAYSTORE.ipynb
│
├── GRAPHS/
│   ├── app category vs rating.png
│   ├── app count wrt categories.png
│   ├── installs vs type.png
│   ├── apps over time.png
│   ├── app size vs rating.png
│   ├── polarity vs subjectivity.png
│   ├── sentiment charts
│   ├── correlation heatmaps
│   └── other exported visualisations
│
└── README.md
```

---

## 20. Tools and Technologies

| Category                  | Tools                                |
| ------------------------- | ------------------------------------ |
| Programming language      | Python                               |
| Data handling             | Pandas, NumPy                        |
| Static visualisation      | Matplotlib, Seaborn                  |
| Interactive visualisation | Plotly Express, Plotly Graph Objects |
| Notebook environment      | Google Colab / Jupyter Notebook      |
| Dataset format            | CSV                                  |

---

## 21. How to Run the Notebook

### 21.1 Clone the Repository

```bash
git clone https://github.com/Pranaybannu/PLAY-STORE-APP-DATA-EDA.git
cd PLAY-STORE-APP-DATA-EDA
```

### 21.2 Install Dependencies

```bash
pip install pandas numpy matplotlib seaborn plotly jupyter
```

### 21.3 Open the Notebook

```bash
jupyter notebook NOTEBOOK/PLAYSTORE.ipynb
```

### 21.4 Update Dataset Paths

The notebook currently loads data from Google Drive paths.

```python
app = pd.read_csv("/content/drive/MyDrive/PLAYSTORE/Play Store Data.csv")
rev = pd.read_csv("/content/drive/MyDrive/PLAYSTORE/User Reviews.csv")
```

For local execution, update the paths to point to the dataset files in the `DATA` folder.

```python
app = pd.read_csv("DATA/cw app data (1).csv")
rev = pd.read_csv("DATA/cw user_rev data (1).csv")
```

The cleaned app file includes an `Unnamed: 0` index column. The notebook removes this column before analysis.

```python
del app["Unnamed: 0"]
```

---

## 22. Limitations

1. This project performs exploratory analysis and does not train a predictive model.
2. The analysis identifies patterns and correlations, not causal relationships.
3. The data is a static sample and may not represent the current Google Play Store.
4. Ratings, installs, and review counts can change over time.
5. The repository contains cleaned datasets, while the notebook initially expects raw Google Drive files.
6. Review sentiment is based on the provided sentiment labels and translated-review data.
7. The app and review datasets are merged only where app names match.

---

## 23. Future Improvements

1. Build an interactive Streamlit or Power BI dashboard from the analysis.
2. Add trend forecasting for app installs and review volume.
3. Analyse topic themes in negative reviews using natural-language processing.
4. Build a recommendation system for app categories or similar applications.
5. Study rating changes over time for individual apps.
6. Add statistical tests to validate category differences.
7. Create a reproducible pipeline that starts from raw data files.
8. Use current Google Play Store data for refreshed analysis.
