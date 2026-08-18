# La Liga Team Clustering Analysis

Unsupervised learning project that analyzes **La Liga teams from the 2015/16 season** using event-level football data from **StatsBomb Open Data**.

The main goal is to identify groups of teams with similar tactical and performance profiles across attacking, build-up, and defensive phases of play.

The project compares three clustering approaches:

* **K-Means**
* **Gaussian Mixture Models (GMM)**
* **Agglomerative Hierarchical Clustering**

**Principal Component Analysis (PCA)** is also used to visualize the resulting cluster structures.

## Project Overview

Traditional league tables summarize results, but they do not necessarily capture *how* teams play.

This project explores whether La Liga teams can instead be grouped according to their underlying playing characteristics and tactical profiles.

The analysis focuses on team-level metrics derived from StatsBomb event data, including:

* Attacking volume and efficiency
* Passing and build-up behavior
* Defensive activity and effectiveness
* Per-90 metrics
* Efficiency ratios and percentages

After feature engineering and correlation-based variable selection, the final clustering dataset contains **17 features**.

## Main Findings

The analysis shows that:

* **FC Barcelona and Real Madrid** present clearly differentiated performance profiles compared with the rest of the league.
* **Build-up variables**, particularly passing volume and passing efficiency, are the strongest drivers of cluster formation.
* **K-Means** identifies an optimal solution of **4 clusters**.
* **Gaussian Mixture Models** also support a **4-cluster solution**.
* **Agglomerative Hierarchical Clustering** identifies a more general **2-cluster structure**.
* The hierarchical model mainly separates Barcelona and Real Madrid from the remaining teams.
* GMM reveals greater overlap between some team profiles, reflecting its probabilistic clustering approach.

## Repository Structure

```text
.
├── DataCollection_EDA.ipynb
├── Clustering.ipynb
├── DataMining_Project_Report_CristinaMorillo.pdf
├── requirements.txt
└── README.md
```

### `DataCollection_EDA.ipynb`

Data collection, feature engineering, and exploratory data analysis.

Main steps:

1. Retrieve La Liga 2015/16 event and match data using `statsbombpy`.
2. Construct team-level attacking, build-up, and defensive metrics.
3. Normalize count-based indicators per 90 minutes.
4. Explore distributions and relationships between variables.
5. Analyze team rankings across different performance metrics.
6. Study correlations between features.
7. Export the engineered datasets used in the clustering notebook.

The notebook generates:

```text
dfShotsSpain.csv
dfPassSpain.csv
dfDefenseSpain.csv
```

### `Clustering.ipynb`

Preprocessing, clustering, model comparison, and interpretation.

Main steps:

1. Load the engineered datasets.
2. Select the final clustering variables.
3. Remove redundant and highly correlated features.
4. Standardize the variables using `StandardScaler`.
5. Apply different clustering algorithms.
6. Determine the optimal number of clusters.
7. Visualize the results using PCA.
8. Analyze the features that contribute most to cluster separation.

The clustering algorithms used are:

* K-Means
* Gaussian Mixture Models
* Agglomerative Hierarchical Clustering

The notebook also creates the combined modeling dataset:

```text
dfModel.csv
```

## Data Source

The project uses **StatsBomb Open Data**, accessed through the Python package `statsbombpy`.

The analysis covers the **Spanish La Liga 2015/16 season**.

StatsBomb provides detailed event-level football data including:

* Passes
* Shots
* Duels
* Recoveries
* Fouls
* Pressures
* Expected Goals
* Match metadata

## Methodology

### Feature Engineering

The variables are organized into three main tactical dimensions.

#### Attack

Attacking metrics include variables such as:

* Shots per 90
* Shots on target per 90
* Goals per 90
* Expected Goals (xG)
* Goals per shot
* Goals per shot on target
* xG per shot
* Shots from outside the box
* Shooting efficiency

#### Build-up

Build-up metrics describe passing behavior and ball circulation, including:

* Total passes
* Completed passes
* Final-third passes
* Short passes
* Long passes
* Crosses
* Pass completion percentages
* Average pass length

#### Defense

Defensive performance is represented using metrics such as:

* Recoveries
* Final-third recoveries
* Defensive duels
* Duels won
* Fouls committed
* Shots conceded
* Shots on target conceded
* Expected Goals Against (xGA)
* Goals conceded
* Clean sheets

Absolute indicators are normalized using **per-90-minute values** to ensure comparability between teams.

Percentage and ratio variables are also included to better represent efficiency and tactical style.

## Data Preprocessing

Before clustering, the dataset is processed to reduce redundancy and prevent highly correlated variables from dominating the models.

Correlation matrices are used to identify overlapping features.

When multiple variables represent similar tactical concepts, only the most informative variable is retained.

Relative and efficiency-based metrics are prioritized because they better capture playing style rather than simply overall activity levels.

The final dataset contains **17 selected features**.

All variables are standardized before clustering to ensure that differences in numerical scale do not influence distance calculations.

## Clustering Models

### K-Means

K-Means clustering is used to divide teams into groups based on similarity in their tactical and performance metrics.

The optimal number of clusters is evaluated using the **Elbow Method**.

The selected solution is:

```text
k = 4
```

The results show a meaningful separation of different team profiles, with Barcelona and Real Madrid clearly differentiated from most other teams.

### Gaussian Mixture Models

Gaussian Mixture Models provide a probabilistic alternative to K-Means.

Instead of assigning each team deterministically to one cluster, GMM estimates the probability that each observation belongs to each component.

The number of components is evaluated using:

* Akaike Information Criterion (AIC)
* Bayesian Information Criterion (BIC)

The selected solution is:

```text
n_components = 4
```

The GMM results show greater overlap between some clusters, suggesting that certain teams share characteristics from multiple tactical profiles.

### Agglomerative Hierarchical Clustering

Agglomerative Hierarchical Clustering is applied using **Ward linkage**.

The cluster structure is analyzed through:

* Dendrograms
* Silhouette Score

The selected solution is:

```text
k = 2
```

The main distinction identified by the model separates **FC Barcelona and Real Madrid** from the rest of the league.

## Principal Component Analysis

Principal Component Analysis is used for visualization purposes.

The original feature space is projected onto two principal components so that the clustering results can be visualized in two dimensions.

The first two components explain approximately:

```text
PC1: 47.7%
PC2: 13.1%

Total explained variance: 60.8%
```

This visualization helps assess cluster separation and identify teams with particularly distinctive performance profiles.

## Feature Importance

Feature influence is analyzed by comparing differences between cluster centers or component means.

Across the different clustering models, some of the most influential variables include:

* Shots per 90 minutes
* Duel success percentage
* Long-pass completion percentage
* Total passes per 90 minutes
* Goals per shot percentage

When grouped by tactical dimension, **build-up variables consistently emerge as the most important drivers of cluster separation**.

This suggests that differences in passing structure and ball circulation are particularly relevant when distinguishing La Liga team profiles.

## Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/la-liga-team-clustering.git
cd la-liga-team-clustering
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it.

### macOS / Linux

```bash
source .venv/bin/activate
```

### Windows

```bash
.venv\Scripts\activate
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

## Usage

The notebooks were originally developed using **Google Colab**.

Run them in the following order:

```text
1. DataCollection_EDA.ipynb
2. Clustering.ipynb
```

The first notebook performs data collection, feature engineering, and exploratory analysis.

The second notebook uses the generated datasets to perform preprocessing and clustering.

Some paths inside the notebooks may refer to Google Drive locations such as:

```text
/content/drive/MyDrive/
```

If running the project locally, these paths should be replaced with local relative paths.

## Technologies

The project uses:

* Python
* pandas
* NumPy
* Matplotlib
* Seaborn
* SciPy
* scikit-learn
* StatsBombPy
* Google Colab

## Report

A complete description of the methodology, exploratory analysis, clustering models, visualizations, results, and conclusions is available in:

```text
DataMining_Project_Report_CristinaMorillo.pdf
```

## Author

**Cristina Morillo Leal**

Master's Degree in Data Science
University of Trento

## Acknowledgments

Football event data used in this project comes from **StatsBomb Open Data**.

## License

This repository contains an academic data mining project.

The football data used in the analysis is provided by **StatsBomb Open Data** and remains subject to StatsBomb's corresponding terms of use.
