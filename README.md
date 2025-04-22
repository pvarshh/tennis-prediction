---
title: Who is the ATP Superstar?
layout: spec
---

# Who is the ATP Superstar?

**Jeremy, Pranav**

## Introduction

The Association of Tennis Professionals (ATP) is the main governing body for men's professional tennis and the ATP tour is a worldwide tour with a series of tournaments consisting of the best tennis players around the globe. 

In our dataset, we have 74906 matches spanning across 2000-2024. 

There are 49 columns in our dataset. Many of these are already self-explanatory, but we note clarifications as necessary below:

- tourney_id
  - a unique identifier for each tournament, such as 2020-888. The exact formats are borrowed from several different sources, so while the first four characters are always the year, the rest of the ID doesn't follow a predictable structure.
- tourney_name
- surface
- draw_size
  - number of players in the draw, often rounded up to the nearest power of 2. (For instance, a tournament with 28 players may be shown as 32.)
- tourney_level
  - For men: 'G' = Grand Slams, 'M' = Masters 1000s, 'A' = other tour-level events, 'C' = Challengers, 'S' = Satellites/ITFs, 'F' = Tour finals and other season-ending events, and 'D' = Davis Cup 
  - For women, there are several additional tourney_level codes, including 'P' = Premier, 'PM' = Premier Mandatory, and 'I' = International. The various levels of ITFs are given by the prize money (in thousands), such as '15' = ITF $15,000. Other codes, such as 'T1' for Tier I (and so on) are used for older WTA tournament designations. 'D' is used for Federation/Fed/Billie Jean King Cup, and also for Wightman Cup and Bonne Bell Cup.
  - Others, eventually for both genders: 'E' = exhibition (events not sanctioned by the tour, though the definitions can be ambiguous), 'J' = juniors, and 'T' = team tennis, which does yet appear anywhere in the dataset but will at some point.
- tourney_date
  - eight digits, YYYYMMDD, usually the Monday of the tournament week.
- match_num
  - a match-specific identifier. Often starting from 1, sometimes counting down from 300, and sometimes arbitrary. 
- (winner/loser)_id
  - the player_id used in this repo for the winner / loser of the match
- (winner/loser)_seed
  - the winner / loser's seed in the tournament
- (winner/loser)_entry
  - 'WC' = wild card, 'Q' = qualifier, 'LL' = lucky loser, 'PR' = protected ranking, 'ITF' = ITF entry, and there are a few others that are occasionally used.
- (winner/loser)_name
- (winner/loser)_hand
  - R = right, L = left, U = unknown. For ambidextrous players, this is their serving hand.
- (winner/loser)_ht
  - height in centimeters, where available
- (winner/lower)_ioc
  - three-character country code
- (winner/loser)_age
  - age, in years, as of the tourney_date
- score
- best_of
  - '3' or '5', indicating the the number of sets for this match
- round
- minutes
  - match length, where available
- (w/l)_ace
  - winner / loser's number of aces
- (w/l)_df
  - winner / loser's number of doubles faults
- (w/l)_svpt
  - winner / loser's number of serve points
- (w/l)_1stIn
  - winner / loser's number of first serves made
- (w/l)_1stWon
  - winner / loser's number of first-serve points won
- (w/l)_2ndWon
  - winner / loser's number of second-serve points won
- (w/l)_SvGms
  - winner / loser's number of serve games
- (w/l)_bpSaved
  - winner / loser's number of break points saved
- (w/l)_bpFaced
  - winner / loser's number of break points faced
- (winner/loser)_rank
  - winner / loser's ATP or WTA rank, as of the tourney_date, or the most recent ranking date before the tourney_date
- (winner/loser)_rank_points
  - number of ranking points, where available

Some questions we brained storm are:
- How much does player rank predict match outcomes? Is the rank gap a significant factor?
- Does the surface type significantly impact match statistics like rally length (minutes)?
- How strongly do pre-match player statistics, such as rank, age, height, and seeding predict the match winner?
- Is there a performance difference between left-handed and right-handed players, potentially varying by surface?

Ultimately, the question we plan to investigate further in this project is: How strongly do pre-match player statistics, such as rank, age, height, and seeding predict the match winner?

This question delves into the core predictability of tennis matches based on readily available player information **before** the match starts. It explores fundamental factors often discussed by commentators and analysts. 

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning and Imputation

First, we need to combine our datasets. The dataset we collected from Kaggle split the matches by year, so we can simply concatenate our csv files. We will add two new columns, ``match_id`` and ``year`` to better differentiate matches.




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tourney_id</th>
      <th>tourney_name</th>
      <th>surface</th>
      <th>draw_size</th>
      <th>...</th>
      <th>loser_rank</th>
      <th>loser_rank_points</th>
      <th>year</th>
      <th>match_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-301</td>
      <td>Auckland</td>
      <td>Hard</td>
      <td>32</td>
      <td>...</td>
      <td>63.0</td>
      <td>595.0</td>
      <td>2000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-301</td>
      <td>Auckland</td>
      <td>Hard</td>
      <td>32</td>
      <td>...</td>
      <td>49.0</td>
      <td>723.0</td>
      <td>2000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-301</td>
      <td>Auckland</td>
      <td>Hard</td>
      <td>32</td>
      <td>...</td>
      <td>59.0</td>
      <td>649.0</td>
      <td>2000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>74903</th>
      <td>2024-M-DC-2024-WG2-PO-VIE-RSA-01</td>
      <td>Davis Cup WG2 PO: VIE vs RSA</td>
      <td>Hard</td>
      <td>4</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2024</td>
      <td>74903</td>
    </tr>
    <tr>
      <th>74904</th>
      <td>2024-M-DC-2024-WG2-PO-VIE-RSA-01</td>
      <td>Davis Cup WG2 PO: VIE vs RSA</td>
      <td>Hard</td>
      <td>4</td>
      <td>...</td>
      <td>416.0</td>
      <td>109.0</td>
      <td>2024</td>
      <td>74904</td>
    </tr>
    <tr>
      <th>74905</th>
      <td>2024-M-DC-2024-WG2-PO-VIE-RSA-01</td>
      <td>Davis Cup WG2 PO: VIE vs RSA</td>
      <td>Hard</td>
      <td>4</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2024</td>
      <td>74905</td>
    </tr>
  </tbody>
</table>
<p>74906 rows × 51 columns</p>
</div>


For easier analysis, we changed the tournament date to a Y/M/D format

Next, we columns with missing values that we would later have to fill in using imputation stategies ensuring consistent data across the board for our model to analyze. We found teh following columns that need to be imputed

    surface, winner_seed, winner_entry, winner_ht, winner_age, loser_seed loser_entry, loser_hand, loser_ht, loser_age, minutes, w_ace', w_df, w_svpt, w_1stIn, w_1stWon, w_2ndWon, w_SvGms, w_bpSaved, w_bpFaced,l_ace, l_df, l_svpt, l_1stIn, l_1stWon, l_2ndWon, l_SvGms, l_bpSaved, l_bpFaced, winner_rank, winner_rank_points, loser_rank, loser_rank_points


The resulting analysis of missing values in the columsn is as follows


    surface                  53
    winner_seed           43786
    winner_entry          65400
    winner_ht              1425
    winner_age                5
    loser_seed            57668
    loser_entry           59484
    loser_hand                4
    loser_ht               2909
    loser_age                 3
    minutes                8174
    w_ace                  6520
    w_df                   6520
    w_svpt                 6520
    w_1stIn                6520
    w_1stWon               6520
    w_2ndWon               6520
    w_SvGms                6520
    w_bpSaved              6520
    w_bpFaced              6520
    l_ace                  6520
    l_df                   6520
    l_svpt                 6520
    l_1stIn                6520
    l_1stWon               6520
    l_2ndWon               6520
    l_SvGms                6520
    l_bpSaved              6520
    l_bpFaced              6520
    winner_rank             573
    winner_rank_points      573
    loser_rank             1468
    loser_rank_points      1468


#### Handling Categorical Features

**Surface:** The surface is consistant across tournaments, so first layer imputation is to full surface with tournament surface. However, for some tornaments, surface wasnt reported at all, so our second layer imputation was to set match surface the most common surface of the year.

**Hand:** For the players who's dominant hand is missing, our imputation strategy was to default the dominant hand right (more common hand bias).

**Seed:** Missing seed for a player means they were not seeded for that tournament, so to maiintain consistency with seed values (1-32), we set default seed to unseeded players as 99.

**Entry:** Missing entry into the tournament for a player means they entered via main draw, so we can impute with the string "MD" which simply means main draw.

#### Handling Numerical Features

**Age, Height:** For the sake of simplicity and ease, we imputed missing player age / height with the median age / height of all players recorded.

**Duration:** Imputed match duration based on the average duration of matches given the round of the match.

**In Match Stats:** For stats that we typically see during the match, our imputation strategy used median imputation. 

**Rank / Rank Points:** Logically, players who are consistently ranked are less likely to have missing rank data. So, we can impute unranked players with the worst rank.



### Univariate Analysis

<iframe
src="assets/freq_rank.html"
width="800"
height="400"
frameborder="0"
></iframe>

This plot shows the frequency of different ranks among match winners. We expect a skew towards lower ranks because better players win more often. The log scale helps better visualize the right tail of higher-ranked winners.

<iframe
src="assets/freq_surface.html"
width="800"
height="400"
frameborder="0"
></iframe>

This plot shows the frequency of matches played on different surfaces in the dataset. Hard courts are typically the most common, followed by Clay, Grass, and Carpet.

### Bivariate Analysis

TODO: DROPDOWN

<iframe
src="assets/height_diff.html"
width="1200"
height="400"
frameborder="0"
></iframe>

This plot investigates if height difference correlates with winning, especially in the context of rank difference. Points in the bottom-left quadrant represent shorter players winning against higher-ranked opponents (upset). Points in the top-right represent taller players winning against lower-ranked opponents.

### Interesting Aggregates


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>surface</th>
      <th>Carpet</th>
      <th>Clay</th>
      <th>Grass</th>
      <th>Hard</th>
    </tr>
    <tr>
      <th>tourney_level</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>92.8</td>
      <td>99.7</td>
      <td>92.1</td>
      <td>95.9</td>
    </tr>
    <tr>
      <th>D</th>
      <td>101.6</td>
      <td>103.9</td>
      <td>103.6</td>
      <td>104.0</td>
    </tr>
    <tr>
      <th>F</th>
      <td>103.8</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>103.5</td>
    </tr>
    <tr>
      <th>G</th>
      <td>0.0</td>
      <td>147.9</td>
      <td>141.7</td>
      <td>149.9</td>
    </tr>
    <tr>
      <th>M</th>
      <td>93.8</td>
      <td>102.2</td>
      <td>0.0</td>
      <td>99.2</td>
    </tr>
    <tr>
      <th>O</th>
      <td>0.0</td>
      <td>99.6</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>


This table compares the average length of matches across different tournament levels (i.e. Grand Slams 'G', Masters 'M', etc.) and surfaces. we can observe that Grand Slam matches on Clay, Grass, and Hard surfaces are typically longer than other typical ATP matches."


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>surface</th>
      <th>Carpet</th>
      <th>Clay</th>
      <th>Grass</th>
      <th>Hard</th>
    </tr>
    <tr>
      <th>tourney_level</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>38.1</td>
      <td>37.8</td>
      <td>37.9</td>
      <td>35.6</td>
    </tr>
    <tr>
      <th>D</th>
      <td>30.0</td>
      <td>24.9</td>
      <td>32.3</td>
      <td>26.2</td>
    </tr>
    <tr>
      <th>F</th>
      <td>46.7</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>35.3</td>
    </tr>
    <tr>
      <th>G</th>
      <td>0.0</td>
      <td>28.6</td>
      <td>30.3</td>
      <td>27.8</td>
    </tr>
    <tr>
      <th>M</th>
      <td>39.2</td>
      <td>35.1</td>
      <td>0.0</td>
      <td>35.9</td>
    </tr>
    <tr>
      <th>O</th>
      <td>0.0</td>
      <td>25.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



This table shows the percentage of matches won by the lower-ranked player ('upsets') across different tournament levels and surfaces. the is_upset variable becomes True only for those rows in matches_df where the player listed as the winner actually had a worse rank (reminder: higher rank value means actual lower rank) than the player listed as the loser. overall, this table provides insight into identifying environments where rankings might be less predictive and upsets are more common.

## Framing a Prediction Problem

As hinted in the introduction, our prediction problem is to predict the outcome of a specific ATP tour tennis match between two designated players ('player 1' and 'player 2'), using only information that would be known before the match commences (to make our model actually useful). This prediction problem is a **Binary Classification** task because the model will predict one of two possible classes: either player 1 wins, or player 1 loses (so player 2 wins).

The response variable (the variable being predicted) is **outcome**. This will be a binary variable where: 
- outcome = 1 signifies that Player 1 won the match.
- outcome = 0 signifies that Player 1 lost the match.

This variable directly represents the result we set out to predict (win/loss for a designated 'player 1'). It also simplifies the prediction task compared to predicting the winner's name (which would be a high-cardinality multiclass problem).

We will primarily use **accuracy** as the main metric to evaluate our model. Recall that accuracy measures the overall proportion of matches correctly predicted, so i.e. both player 1 wins and player 2 wins. It provides a clear baseline measure of model performance. Since our data preparation method (detailed in the next section) involves duplicating matches (once with winner as player 1, once with loser as player 1), the overall target variable distribution in the training set is perfectly balanced (50% wins, 50% losses). There won't be skewedness by class imbalance in the overall dataset.

Information that we would know at "time of prediction" include:
- player attributes: id, age, hand, height, nationality (IOC).
- player status: rank, rank points, tournament seed, tournament entry.
- match context: tournament name, tournament level, surface, round within the tournament, format ('best_of' 3 or 5 sets).
- historical performance (calculated before the current match. can be found using feature engineering):
  - head-to-head (H2H) win/loss record between the two specific players prior to this match.
  - recent player form (e.g., win percentage over the last N matches or M months).
  - career or recent performance metrics on the specific surface of the upcoming match.
  - career or recent performance metrics in similar tournament levels or rounds.

# Data Preprocessing for Matchup Data

This document explains the steps performed by the `create_matchup_df` function, which transforms raw match records into a clean, structured DataFrame suitable for modeling head-to-head player matchups.

**Goal:** Build a DataFrame containing all matches between two specified players, labeling each row with which player won.
- **Input Arguments:**
  - `player1_name` (str): Name of the first player.
  - `player2_name` (str): Name of the second player.
- **Output:** A pandas DataFrame (`matchup_df`) sorted by match date, with standardized column names and correct data types.


Selecting Relevant Columns
- **Define Expected Columns**: A list of column names (`extracted_cols`) that includes match metadata, winner attributes (age, rank, seed, etc.), and loser attributes.
- **Filter by Availability**: Create `available_cols` by intersecting `extracted_cols` with the actual columns in the source DataFrame `matches_df`. This ensures compatibility even if some columns are missing.

Filtering Matches Between the Two Players

- **Player1 Wins**: Rows where `winner_name == player1_name` and `loser_name == player2_name`.
- **Player2 Wins**: Rows where `winner_name == player2_name` and `loser_name == player1_name`.
- Extract these subsets into `p1_wins_df` and `p2_wins_df`, respectively, using only `available_cols`.

Renaming Columns for Consistency

**Objective:** Standardize column names so that columns always refer to “player1” or “player2”, regardless of who actually won in the raw data.

- Player1-Win Subset (`p1_wins_df`)
- Map raw columns:  
  - `winner_name` → `p1_name`  
  - `loser_name`  → `p2_name`  
  - `winner_age`, `winner_rank`, … → `p1_age`, `p1_rank`, …  
  - `loser_age`, `loser_rank`, … → `p2_age`, `p2_rank`, …
- Add binary label: `did_player1_win = 1`

- Player2-Win Subset (`p2_wins_df`)
- Swap roles in the rename map:
  - `loser_name` → `p1_name` (because player1 lost)
  - `winner_name` → `p2_name`
  - Rename other attributes accordingly.
- Add binary label: `did_player1_win = 0`

Combining and Cleaning Data
-  **Concatenate Subsets**: Merge `p1_wins_df` and `p2_wins_df` into a single DataFrame `matchup_df`.
- **Date Parsing**: Convert the `match_date` column (originally in `YYYYMMDD` integer format) into pandas `datetime` objects.
- **Type Casting**:
   - Convert age, rank, and IDs to integer types.
   - Convert rank points, heights, and seeds to numeric (float) types, allowing for missing values.
   - Ensure categorical fields (e.g., `p1_hand`, `p2_entry`, `p1_ioc`) are strings.
   - Enforce the victory indicator `did_player1_win` as integer.

Final Touches

- **Sorting**: Sort the DataFrame by `match_date` in ascending order.
- **Index Reset**: Reset the DataFrame index to ensure a clean, continuous index after sorting.
- **Logging**: Print the total number of head-to-head matches found.

Result: The returned DataFrame has one row per match, with columns:

- **`match_date`**: Date of the match (`datetime`)
- **`p1_name`, `p1_age`, `p1_rank`, etc.**: Attributes for player1
- **`p2_name`, `p2_age`, `p2_rank`, etc.**: Attributes for player2
- **`did_player1_win`**: Binary target variable (1 if player1 won, 0 otherwise)

This structured DataFrame can now be used as input for modeling tasks (e.g., predicting head-to-head outcomes) or for exploratory analysis of player performance over time.


For example, the following is between Roger Federer and Fernando Gonzalez.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>match_date</th>
      <th>p1_name</th>
      <th>p1_age</th>
      <th>p1_rank</th>
      <th>...</th>
      <th>p2_ioc</th>
      <th>p2_rank_points</th>
      <th>p2_id</th>
      <th>did_player1_win</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2004-03-08</td>
      <td>Roger Federer</td>
      <td>22</td>
      <td>1</td>
      <td>...</td>
      <td>nan</td>
      <td>1120.0</td>
      <td>103602</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2004-05-10</td>
      <td>Roger Federer</td>
      <td>22</td>
      <td>1</td>
      <td>...</td>
      <td>nan</td>
      <td>1430.0</td>
      <td>103602</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2005-04-11</td>
      <td>Roger Federer</td>
      <td>23</td>
      <td>1</td>
      <td>...</td>
      <td>nan</td>
      <td>1200.0</td>
      <td>103602</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2007-11-12</td>
      <td>Roger Federer</td>
      <td>26</td>
      <td>1</td>
      <td>...</td>
      <td>nan</td>
      <td>1905.0</td>
      <td>103602</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2008-05-25</td>
      <td>Roger Federer</td>
      <td>26</td>
      <td>1</td>
      <td>...</td>
      <td>nan</td>
      <td>1160.0</td>
      <td>103602</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2009-03-12</td>
      <td>Roger Federer</td>
      <td>27</td>
      <td>2</td>
      <td>...</td>
      <td>nan</td>
      <td>2650.0</td>
      <td>103602</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>13 rows × 22 columns</p>
</div>



This matchup_df acts as an intermediate step for our final DataFrame for prediction. It currently has the following issues that we need to solve:
1. scarcity in the data: most players have only met each other a handful of times from 2000-2024. training a model on 10-40 data points will likely lead to poor performance and overfitting, even for predicting future matches between those same two players.
2. lack of generalizability: a model trained only on (for instance) Nadal vs. Federer data will learn patterns specific to their interactions (e.g., the effect of surface, maybe specific psychological edges). it will lack predictive power for a match between any other two players because it hasn't seen data representing their skills, ranks, or interactions.

Solution: instead of subsetting to one specific pair (of players), use the entire matches_df. we need to restructure it so that each row represents a match with p1/p2 features, alongside the outcome relative to p1 (as professor suraj suggested).


**Train-Validation-Test Split**

Ee filter the training_df into train_df, val_df, and test_df based on the specified year ranges using the tourney_date. this creates a natural "future" prediction problem where we used historical data to predict matches that occur in the future. we then separate the features and target for each set.



## Baseline Model

We begin our prediction model using simple Logistic Regression, passing in player ranks (p1_rank, p2_rank) and the match surface. this baseline model achieved an overall accuracy of 64% on the testing set. 

The accompanying classification report shows identical precision, recall, and f1-scores of 0.64 for predicting both class 0 (player 1 lost) and class 1 (player 1 won). this uniformity is expected, because given the perfectly balanced nature of the test set (in the preprocessing step, we mirrored p1 winning and losing, creating symmetrical entries for each match outcome). hence, we place high important on accuracy being the main evaluation criteria for our model.

As such, the confusion matrix visually confirms this balanced performance: the number of correctly predicted wins (true positives for class 1, 3871) is very close to the number of correctly predicted losses (true positives for class 0, 3853). similarly, the number of false positives, 2209 is nearly identical to the number of false negatives, 2191.

```
    baseline model classification report (on the testing set):
                   precision    recall  f1-score   support
    
               0       0.64      0.64      0.64      6062
               1       0.64      0.64      0.64      6062
    
        accuracy                           0.64     12124
       macro avg       0.64      0.64      0.64     12124
    weighted avg       0.64      0.64      0.64     12124
```

<iframe
src="assets/baseline.html"
width="800"
height="400"
frameborder="0"
></iframe>


## Final Model

In this final step, a robust predictive model is developed to forecast the outcome of tennis matches. The procedure involves several key stages, detailed clearly below:

**Feature Engineering**

Before training the model, the data undergoes significant feature engineering, which enhances predictive accuracy:
- **Differences in Player Statistics:**
    - Three critical new features are created:
        - `age_diff`: The difference in age between the two players.
        - `rank_diff`: The difference in player rankings, indicating relative skill level.
        - `seed_diff`: The difference in tournament seedings, reflecting tournament expectations.
    - Tournament Round Encoding:
        - The round of the tournament (`R128`, `R64`, ..., `SF`, `F`) is transformed into a numeric value (`round_num`), with higher rounds assigned greater values, capturing the importance of match context.
    - Head-to-Head (H2H) Performance:
        - For each player pair, a historical winning ratio (h2h) is computed based on their past encounters.
        - If there are no previous matches, a neutral value (0.5) is assigned, reflecting no bias toward either player.

**Preprocessing Pipeline**

After engineering these features, the data is processed for model training:

- Numerical Features (`age_diff`, `rank_diff`, `seed_diff`, `round_num`, `best_of`, `minutes`, `h2h`) are standardized using StandardScaler. This ensures each feature contributes equally, preventing features with larger numeric scales from disproportionately influencing the model.
- Categorical Features (`surface`, `tourney_level`) undergo One-Hot Encoding (`OneHotEncoder`) to convert them into binary features, allowing the model to interpret categorical distinctions effectively.
- These preprocessing steps are combined into a cohesive pipeline, ensuring consistency and reproducibility.

**Model Selection and Training**

The predictive model selected is a HistGradientBoostingClassifier, a robust, gradient-boosting algorithm ideal for structured tabular data:

- Hyperparameter Optimization: A randomized hyperparameter search (RandomizedSearchCV) explores multiple parameter combinations to identify optimal model settings:
    - `learning_rate`: Controls how quickly the model learns.
    - `max_iter`: Determines the maximum number of iterations for training.
    - `max_depth`: Limits the complexity of decision trees to prevent overfitting.
    - `l2_regularization`: Reduces model complexity and overfitting by penalizing complex solutions.
- Cross-Validation: The search employs 5-fold cross-validation to objectively evaluate and select the best model configuration based on accuracy.

**Model Evaluation**

- Validation Set Performance (2020-2022): The optimized model achieves an accuracy of 91.38%, demonstrating strong predictive capability on recent matches not seen during training.
- Test Set Performance (2023 and onward): Evaluated on entirely unseen future data, the model achieves an impressive 92.01% accuracy, confirming its robustness and generalization.
- Classification Report: The detailed classification report shows high precision, recall, and F1-score (~92%) for both outcomes (player 1 wins and player 2 wins), indicating consistent performance.
- Confusion Matrix: The confusion matrix visually summarizes prediction accuracy, clearly illustrating that the model reliably distinguishes match outcomes.

```

    final model classification report (on the testing set):
                   precision    recall  f1-score   support
    
               0       0.92      0.92      0.92      6062
               1       0.92      0.92      0.92      6062
    
        accuracy                           0.92     12124
       macro avg       0.92      0.92      0.92     12124
    weighted avg       0.92      0.92      0.92     12124
```

<iframe
src="assets/final.html"
width="800"
height="400"
frameborder="0"
></iframe>

**Conclusion and Practical Implications:**
This carefully crafted predictive modeling approach provides a powerful tool for accurately forecasting tennis match outcomes. Such a model is valuable for sports analytics, betting strategies, player assessments, and understanding factors driving match results.
