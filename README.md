# Who is the ATP Superstar? (change this lol)

**Jeremy, Pranav**

**[Github](https://github.com/pvarshh/tennis-prediction)**

## Step 1: Introduction

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
- winner_id
  - the player_id used in this repo for the winner of the match
- winner_seed
- winner_entry
  - 'WC' = wild card, 'Q' = qualifier, 'LL' = lucky loser, 'PR' = protected ranking, 'ITF' = ITF entry, and there are a few others that are occasionally used.
- winner_name
- winner_hand
  - R = right, L = left, U = unknown. For ambidextrous players, this is their serving hand.
- winner_ht
  - height in centimeters, where available
- winner_ioc
  - three-character country code
- winner_age
  - age, in years, as of the tourney_date

- loser_id
- loser_seed
- loser_entry
- loser_name
- loser_hand
- loser_ht
- loser_ioc
- loser_age
- score
- best_of
  - '3' or '5', indicating the the number of sets for this match

- round
- minutes
  - match length, where available

- w_ace
  - winner's number of aces
- w_df
  - winner's number of doubles faults
- w_svpt
  - winner's number of serve points
- w_1stIn
  - winner's number of first serves made
- w_1stWon
  - winner's number of first-serve points won
- w_2ndWon
  - winner's number of second-serve points won
- w_SvGms
  - winner's number of serve games
- w_bpSaved
  - winner's number of break points saved
- w_bpFaced
  - winner's number of break points faced

- l_ace
- l_df
- _svpt
- l_1stIn
- l_1stWon
- l_2ndWon
- l_SvGms
- l_bpSaved
- l_bpFaced

- winner_rank
  - winner's ATP or WTA rank, as of the tourney_date, or the most recent ranking date before the tourney_date
- winner_rank_points
  - number of ranking points, where available
- loser_rank
- loser_rank_points

(Courtesy of https://github.com/JeffSackmann/tennis_atp/blob/master/matches_data_dictionary.txt)

Some questions we brained storm are:
- How much does player rank predict match outcomes? Is the rank gap a significant factor?
- Does the surface type significantly impact match statistics like rally length (minutes)?
- How strongly do pre-match player statistics, such as rank, age, height, and seeding predict the match winner?
- Is there a performance difference between left-handed and right-handed players, potentially varying by surface?

Ultimately, the question we plan to investigate further in this project is: How strongly do pre-match player statistics, such as rank, age, height, and seeding predict the match winner?

This question delves into the core predictability of tennis matches based on readily available player information **before** the match starts. It explores fundamental factors often discussed by commentators and analysts. 

## Step 2: Data Cleaning and Exploratory Data Analysis

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
height="600"
frameborder="0"
></iframe>

This plot shows the frequency of different ranks among match winners. We expect a skew towards lower ranks because better players win more often. The log scale helps better visualize the right tail of higher-ranked winners.

<iframe
src="assets/freq_surface.html"
width="800"
height="600"
frameborder="0"
></iframe>

This plot shows the frequency of matches played on different surfaces in the dataset. Hard courts are typically the most common, followed by Clay, Grass, and Carpet.

### Bivariate Analysis

TODO: DROPDOWN


<iframe
src="assets/height_diff.html"
width="800"
height="600"
frameborder="0"
></iframe>

This plot investigates if height difference correlates with winning, especially in the context of rank difference. Points in the bottom-left quadrant represent shorter players winning against higher-ranked opponents (upset). Points in the top-right represent taller players winning against lower-ranked opponents.

### Interesting Aggregates


```python
duration_pivot = matches_df.pivot_table(
    values='minutes',
    index='tourney_level',
    columns='surface',
    aggfunc='mean',
    fill_value=0
)

duration_pivot = duration_pivot.round(1)
duration_pivot
```




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



this table compares the average length of matches across different tournament levels (i.e. Grand Slams 'G', Masters 'M', etc.) and surfaces. we can observe that Grand Slam matches on Clay, Grass, and Hard surfaces are typically longer than other typical ATP matches."


```python
unranked_rank = 9999
matches_df['is_upset'] = (matches_df['winner_rank'] > matches_df['loser_rank']) & \
                          (matches_df['loser_rank'] != unranked_rank) & \
                          (matches_df['winner_rank'] != unranked_rank)

upset_pivot = matches_df.pivot_table(
    values='is_upset',
    index='tourney_level',
    columns='surface',
    aggfunc='mean',
    fill_value=0
)

upset_pivot = (upset_pivot * 100).round(1)
upset_pivot
```




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



this table shows the percentage of matches won by the lower-ranked player ('upsets') across different tournament levels and surfaces. the is_upset variable becomes True only for those rows in matches_df where the player listed as the winner actually had a worse rank (reminder: higher rank value means actual lower rank) than the player listed as the loser. overall, this table provides insight into identifying environments where rankings might be less predictive and upsets are more common.

## Step 3: Framing a Prediction Problem

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

## Step 3.5: Preparing Data for a Baseline Model

### Data Preprocessing


```python
def create_matchup_df(player1_name, player2_name):
    extracted_cols = [
        'tourney_date', 
        'winner_name', 'winner_age', 'winner_rank', 'winner_seed', 'winner_entry', 
        'winner_hand', 'winner_ht', 'winner_ioc', 'winner_rank_points', 'winner_id',
        'loser_name', 'loser_age', 'loser_rank', 'loser_seed', 'loser_entry', 
        'loser_hand', 'loser_ht', 'loser_ioc', 'loser_rank_points', 'loser_id'
    ]

    # Ensure we only use columns that exist in matches_df
    available_cols = [col for col in extracted_cols if col in matches_df.columns]
    
    # condition 1: player 1 won against player 2
    p1_wins_condition = (matches_df['winner_name'] == player1_name) & (matches_df['loser_name'] == player2_name)

    # condition 2: player 2 wins against player 1
    p2_wins_condition = (matches_df['winner_name'] == player2_name) & (matches_df['loser_name'] == player1_name)

    p1_wins_df = matches_df.loc[p1_wins_condition, available_cols].copy()
    p2_wins_df = matches_df.loc[p2_wins_condition, available_cols].copy()

    # Create rename dictionaries dynamically based on available columns
    p1_rename_dict = {
        'tourney_date': 'match_date',
        'winner_name': 'p1_name',
        'winner_age': 'p1_age',
        'winner_rank': 'p1_rank',
        'winner_seed': 'p1_seed',
        'winner_entry': 'p1_entry',
        'winner_hand': 'p1_hand',
        'winner_ht': 'p1_ht',
        'winner_ioc': 'p1_ioc',
        'winner_rank_points': 'p1_rank_points',
        'winner_id': 'p1_id',
        'loser_name': 'p2_name',
        'loser_age': 'p2_age',
        'loser_rank': 'p2_rank',
        'loser_seed': 'p2_seed',
        'loser_entry': 'p2_entry',
        'loser_hand': 'p2_hand',
        'loser_ht': 'p2_ht',
        'loser_ioc': 'p2_ioc',
        'loser_rank_points': 'p2_rank_points',
        'loser_id': 'p2_id'
    }
    # Only keep rename pairs for columns that exist
    p1_rename_dict = {k: v for k, v in p1_rename_dict.items() if k in available_cols}
    
    p1_wins_df = p1_wins_df.rename(columns=p1_rename_dict)
    p1_wins_df['did_player1_win'] = 1

    p2_rename_dict = {
        'tourney_date': 'match_date',
        'loser_name': 'p1_name',
        'loser_age': 'p1_age',
        'loser_rank': 'p1_rank',
        'loser_seed': 'p1_seed',
        'loser_entry': 'p1_entry',
        'loser_hand': 'p1_hand',
        'loser_ht': 'p1_ht',
        'loser_ioc': 'p1_ioc',
        'loser_rank_points': 'p1_rank_points',
        'loser_id': 'p1_id',
        'winner_name': 'p2_name',
        'winner_age': 'p2_age',
        'winner_rank': 'p2_rank',
        'winner_seed': 'p2_seed',
        'winner_entry': 'p2_entry',
        'winner_hand': 'p2_hand',
        'winner_ht': 'p2_ht',
        'winner_ioc': 'p2_ioc',
        'winner_rank_points': 'p2_rank_points',
        'winner_id': 'p2_id'
    }
    # Only keep rename pairs for columns that exist
    p2_rename_dict = {k: v for k, v in p2_rename_dict.items() if k in available_cols}
    
    p2_wins_df = p2_wins_df.rename(columns=p2_rename_dict)
    p2_wins_df['did_player1_win'] = 0

    matchup_df = pd.concat([p1_wins_df, p2_wins_df], ignore_index=True)

    matchup_df['match_date'] = pd.to_datetime(matchup_df['match_date'], format='%Y%m%d')
    matchup_df['did_player1_win'] = matchup_df['did_player1_win'].astype(int)
    
    # Convert numeric columns to appropriate types
    numeric_cols = {
        'p1_age': 'int', 'p2_age': 'int',
        'p1_rank': 'int', 'p2_rank': 'int',
        'p1_rank_points': 'float', 'p2_rank_points': 'float',
        'p1_ht': 'float', 'p2_ht': 'float',
        'p1_seed': 'float', 'p2_seed': 'float', # seeds can be NaN for unseeded players
        'p1_id': 'int', 'p2_id': 'int', 
        'p1_ioc': 'str', 'p2_ioc': 'str',
        'p1_entry': 'str', 'p2_entry': 'str',
        'p1_hand': 'str', 'p2_hand': 'str',
        'did_player1_win': 'int'
    }
    
    for col, dtype in numeric_cols.items():
        if col in matchup_df.columns:
            matchup_df[col] = pd.to_numeric(matchup_df[col], errors='coerce').astype(dtype)
    
    matchup_df = matchup_df.sort_values(by='match_date').reset_index(drop=True)
    print(f"Found {matchup_df.shape[0]} matches between {player1_name} and {player2_name}.")

    return matchup_df
```


```python
head_to_head_data = create_matchup_df('Roger Federer', 'Fernando Gonzalez')
head_to_head_data
```

    Found 13 matches between Roger Federer and Fernando Gonzalez.





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


```python
player_cols = ['id', 'seed', 'entry', 'name', 'hand', 'ht', 'ioc', 'age', 'rank', 'rank_points']
match_context_cols = ['tourney_id', 'tourney_name', 'tourney_level', 'tourney_date', 'surface', 'draw_size', 'match_num', 'best_of', 'round', 'minutes']

# player 1
df1 = pd.DataFrame()
for col in player_cols:
    df1[f'p1_{col}'] = matches_df[f'winner_{col}']
    df1[f'p2_{col}'] = matches_df[f'loser_{col}']
for col in match_context_cols:
    if col in matches_df.columns:
      df1[col] = matches_df[col]
df1['outcome'] = 1

# player 2
df2 = pd.DataFrame()
for col in player_cols:
    df2[f'p1_{col}'] = matches_df[f'loser_{col}']
    df2[f'p2_{col}'] = matches_df[f'winner_{col}']
for col in match_context_cols:
     if col in matches_df.columns:
       df2[col] = matches_df[col]
df2['outcome'] = 0

model_df = pd.concat([df1, df2], ignore_index=True)

print(f"created model_df that has {model_df.shape[0]} rows and {model_df.shape[1]} columns.")
```

    created model_df that has 149812 rows and 31 columns.


### Train-Validation-Test Split

we filter the training_df into train_df, val_df, and test_df based on the specified year ranges using the tourney_date. this creates a natural "future" prediction problem where we used historical data to predict matches that occur in the future. we then separate the features and target for each set.


```python
model_df['year'] = model_df['tourney_date'].dt.year

TRAIN_END_YEAR = 2019
VAL_START_YEAR = 2020
VAL_END_YEAR = 2022
TEST_START_YEAR = 2023

train_df = model_df[model_df['year'] <= TRAIN_END_YEAR]
val_df = model_df[(model_df['year'] >= VAL_START_YEAR) & (model_df['year'] <= VAL_END_YEAR)]
test_df= model_df[model_df['year'] >= TEST_START_YEAR]

y_train = train_df['outcome']
y_val = val_df['outcome']
y_test= test_df['outcome']

X_train_all = train_df.drop(columns=['outcome'])
X_val_all = val_df.drop(columns=['outcome'])
X_test_all= test_df.drop(columns=['outcome'])
```

## Step 4: Baseline Model


```python
baseline_features = ['p1_rank', 'p2_rank', 'surface']
X_train_baseline = X_train_all[baseline_features]
X_val_baseline = X_val_all[baseline_features]
X_test_baseline = X_test_all[baseline_features]

numerical_features_baseline = ['p1_rank', 'p2_rank']
categorical_features_baseline = ['surface']

preprocessor_base = ColumnTransformer(
    transformers=[
        ('num', StandardScaler(), numerical_features_baseline),
        ('cat', OneHotEncoder(handle_unknown='ignore', drop='first'), categorical_features_baseline)
    ],
    remainder='drop'
)

baseline_pipeline = make_pipeline(
    preprocessor_base,
    LogisticRegression()
)

baseline_pipeline.fit(X_train_baseline, y_train)

y_pred_val_base = baseline_pipeline.predict(X_val_baseline)
val_accuracy_base = accuracy_score(y_val, y_pred_val_base)
print(f"   Base Model Accuracy on Validation Set ({VAL_START_YEAR}-{VAL_END_YEAR}): {val_accuracy_base:.4f}")

y_pred_test_base = baseline_pipeline.predict(X_test_baseline)
test_accuracy_base = accuracy_score(y_test, y_pred_test_base)
print(f"   Base Model Accuracy on Test Set (>= {TEST_START_YEAR}): {test_accuracy_base:.4f}")
```

       Base Model Accuracy on Validation Set (2020-2022): 0.6436
       Base Model Accuracy on Test Set (>= 2023): 0.6371



```python
cm_base = confusion_matrix(y_test, y_pred_test_base)
print("baseline model classification report (on the testing set):\n", classification_report(y_test, y_pred_test_base))

labels_base = [str(label) for label in baseline_pipeline.classes_]
text_annotations_base = [[f"{val}" for val in row] for row in cm_base]

fig_base = ff.create_annotated_heatmap(
    z=cm_base,
    x=labels_base,
    y=labels_base,
    annotation_text=text_annotations_base,
    colorscale="Blues",
    showscale=True
)

fig_base.update_layout(
    title=f"Baseline Model Confusion Matrix (Test Set, >= {TEST_START_YEAR})",
    xaxis=dict(title="Predicted Label", side="top"),
    yaxis=dict(title="True Label"),
    margin=dict(l=40, r=40, t=60, b=40),
    font=dict(size=12)
)
fig_base.show()
```

    baseline model classification report (on the testing set):
                   precision    recall  f1-score   support
    
               0       0.64      0.64      0.64      6062
               1       0.64      0.64      0.64      6062
    
        accuracy                           0.64     12124
       macro avg       0.64      0.64      0.64     12124
    weighted avg       0.64      0.64      0.64     12124
    




we begin our prediction model using simple Logistic Regression, passing in player ranks (p1_rank, p2_rank) and the match surface. this baseline model achieved an overall accuracy of 64% on the testing set. 

the accompanying classification report shows identical precision, recall, and f1-scores of 0.64 for predicting both class 0 (player 1 lost) and class 1 (player 1 won). this uniformity is expected, because given the perfectly balanced nature of the test set (in the preprocessing step, we mirrored p1 winning and losing, creating symmetrical entries for each match outcome). hence, we place high important on accuracy being the main evaluation criteria for our model.

as such, the confusion matrix visually confirms this balanced performance: the number of correctly predicted wins (true positives for class 1, 3871) is very close to the number of correctly predicted losses (true positives for class 0, 3853). similarly, the number of false positives, 2209 is nearly identical to the number of false negatives, 2191.

## Step 5: Final Model


```python
def feature_engineering(df):
    df = df.copy()

    df['age_diff'] = df['p1_age'] - df['p2_age']
    df['rank_diff'] = df['p1_rank'] - df['p2_rank']
    df['seed_diff'] = df['p1_seed'] - df['p2_seed']

    round_order = ['R128', 'R64', 'R32', 'R16', 'QF', 'SF', 'F']
    df['round_num'] = df['round'].map({r: i + 1 for i, r in enumerate(round_order)}).fillna(0).astype(int)

    h2h_grouped = df.groupby(['p1_id', 'p2_id'])
    if not h2h_grouped.groups:
          df['h2h'] = 0.5
    else:
        h2h = h2h_grouped['outcome'].agg(['sum', 'count']).rename(columns={'sum': 'wins', 'count': 'total'})
        h2h['ratio'] = h2h['wins'] / h2h['total']
        df = df.merge(h2h['ratio'].reset_index().rename(columns={'ratio': 'h2h'}), on=['p1_id', 'p2_id'], how='left')
        df['h2h'].fillna(0.5, inplace=True)
    
    return df

X_train_eng = feature_engineering(train_df)
X_val_eng  = feature_engineering(val_df)
X_test_eng  = feature_engineering(test_df)
```


```python
numerical_features_final = ['age_diff', 'rank_diff', 'seed_diff', 'round_num', 'best_of', 'minutes', 'h2h']
categorical_features_final = ['surface', 'tourney_level']
features_all_final = numerical_features_final + categorical_features_final

X_train_final = X_train_eng[features_all_final]
X_val_final = X_val_eng[features_all_final]
X_test_final = X_test_eng[features_all_final]

preprocessor_final = ColumnTransformer(
    transformers=[
        ('ohe', OneHotEncoder(handle_unknown='ignore', drop='first'), categorical_features_final),
        ('std', StandardScaler(), numerical_features_final),
    ],
    remainder='drop'
)
final_pipeline = make_pipeline(
    preprocessor_final,
    HistGradientBoostingClassifier(random_state=42)
)

param_dist = {
    'histgradientboostingclassifier__learning_rate':     [0.01, 0.05, 0.1, 0.2],
    'histgradientboostingclassifier__max_iter':          [100, 200, 300, 400],
    'histgradientboostingclassifier__max_depth':         [3, 5, 7, 9, None],
    'histgradientboostingclassifier__l2_regularization': [0.0, 0.1, 0.5, 1.0]
}

rs = RandomizedSearchCV(
    final_pipeline,
    param_distributions=param_dist,
    n_iter=30,
    cv=5,
    scoring='accuracy',
    n_jobs=-1,
    random_state=42,
    verbose=1
)
rs.fit(X_train_final, y_train)

val_accuracy_final = rs.score(X_val_final, y_val)
print(f"   Final Model Accuracy on Validation Set ({VAL_START_YEAR}-{VAL_END_YEAR}): {val_accuracy_final:.4f}")

y_pred_test_final = rs.predict(X_test_final)
test_accuracy_final = rs.score(X_test_final, y_test)
print(f"   Final Model Accuracy on Test Set (>= {TEST_START_YEAR}): {test_accuracy_final:.4f}")
```

    Fitting 5 folds for each of 30 candidates, totalling 150 fits
       Final Model Accuracy on Validation Set (2020-2022): 0.9138
       Final Model Accuracy on Test Set (>= 2023): 0.9201



```python
print("final model classification report (on the testing set):\n", classification_report(y_test, y_pred_test_final))

cm_final = confusion_matrix(y_test, y_pred_test_final)
labels_final = [str(label) for label in rs.best_estimator_.classes_]

text_annotations_final = [[f"{val}" for val in row] for row in cm_final]

fig_final = ff.create_annotated_heatmap(
    z=cm_final,
    x=labels_final,
    y=labels_final,
    annotation_text=text_annotations_final,
    colorscale="Greens",
    showscale=True
)

fig_final.update_layout(
    title=f"Final Model Confusion Matrix (Test Set, >= {TEST_START_YEAR})",
    xaxis=dict(title="Predicted Label", side="top"),
    yaxis=dict(title="True Label"),
    margin=dict(l=40, r=40, t=60, b=40),
    font=dict(size=12)
)
fig_final.show()
```

    final model classification report (on the testing set):
                   precision    recall  f1-score   support
    
               0       0.92      0.92      0.92      6062
               1       0.92      0.92      0.92      6062
    
        accuracy                           0.92     12124
       macro avg       0.92      0.92      0.92     12124
    weighted avg       0.92      0.92      0.92     12124
    



