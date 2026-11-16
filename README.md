# League of Legends: Does the Early Game Decide the Winner?

**[View the full report](https://srogle.github.io/league-early-game/)**

At the 10-minute mark of a League of Legends game, what tells you more about who
will win: **first blood** or the **gold lead**? This project uses about 10,000
high-Diamond ranked games and seven machine learning models to find out.

## Key findings

- **The gold lead matters far more than first blood.** First blood alone predicts
  the winner about 60% of the time; the gold lead alone gets about 72%.
- **First blood adds almost nothing once you know the gold.** Its value is mostly
  already counted in the gold it earns.
- **About 73% is the ceiling for 10-minute data.** Seven very different models all
  averaged between 72% and 74% across 50 cross-validation runs, so the limit is
  the data, not the model: roughly a quarter of games are decided after 10 minutes.
- **The simplest model won.** Logistic Regression had the best average accuracy (73.7%).

| Inputs | Test accuracy (Logistic Regression) |
|---|---|
| Always guessing | 50.1% |
| First blood only | 60.3% |
| Gold difference only | 71.9% |
| First blood + gold | 72.0% |
| All 12 early-game inputs | 71.5% (73.7% average in cross-validation) |

## Data

[League of Legends Diamond Ranked Games (10 min)](https://www.kaggle.com/datasets/bobbyscience/league-of-legends-diamond-ranked-games-10-min)
on Kaggle: 9,879 high-Diamond solo queue games from around 2020, with both teams'
stats at 10 minutes and whether blue won.

## Methods

- Split games 80/20 into training and test sets before any cleaning that learns
  from the data (stratified, fixed random seed)
- Capped unrealistic "wards placed" values at the training data's 99th percentile
- Turned blue and red stat pairs into differences (blue minus red), going from
  40 columns to 12 inputs and 1 target
- Compared Logistic Regression, Gradient Boosting, Random Forest, SVM, Naive Bayes,
  K-Nearest Neighbors, and a 3-level Decision Tree with repeated stratified
  cross-validation (5 folds × 10 repeats)

## Repository layout

```
docs/index.html                 The rendered report (served by GitHub Pages)
report/league_diamond_data.qmd  Quarto source with all the code
report/styles.css               Table and heading styles for the report
report/dark-pulse.scss          Dark theme with purple accents and Lux-style fonts
```

## Re-running the report

Requires Python and [Quarto](https://quarto.org/).

```
python -m pip install polars numpy lets-plot scikit-learn kagglehub jupyter
cd report
quarto render league_diamond_data.qmd
```

The dataset downloads automatically through `kagglehub` on the first run.
Rendering takes about 2 minutes, mostly the repeated cross-validation.

## Built with

Python, Polars, scikit-learn, lets-plot, and Quarto. Created for DS 250.
