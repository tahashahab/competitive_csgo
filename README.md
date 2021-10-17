# Professional CS:GO Analysis

## Background
This repo contains multiple jupyter notebooks with analysis of datasets taken from competitive CS:GO matches over the course of 4 years (Apr 2016-Mar 2020).

All the analysis and modelling is written with Python using NumPy, Pandas, Matplotlib, Seaborn, Sci-kit Learn, TensorFLow, and Statsmodels. 

## Domain Knowledge
Counter Strike: Global Offensive (CS:GO) is a tactical first-person shooter video game released in 2012 and has since become a prominent figure in the esports scene. 

The data that I will be analyzing is taken from a 5v5, person vs. person (PVP) gamemode called Search & Destroy, where two sides, Terrorists (Ts) and Counter-Terrorists (CTs) fight against each other on a variety of different maps. The objectve of the game is for the Ts to plant a bomb on dedicated bomb sites on the map while the CTs must eliminate all the Ts and/or defuse the bomb.

The matches that are in the data come from many different professional teams playing against each other in a variety of tournaments. Generally, each year the best teams will play in a year-long circuit to get a chance to play in the largest event in the year called the Major. The winners of the Major are seen as the best team in the world in that year, similar to American sports. 

The most common way to play a proffesional match is to play a best-of-3 (BO3) series, although BO1, BO2, and BO5s are played depending on the tournament structure. A single map is structured in a way called Max Rounds 15 (MR15) where each team plays on one side (CT vs T) for 15 rounds, switching sides, then playing until one team reaches 16 round wins. If at the end of 30 rounds the game score is 15-15, there is an overtime (OT) protocol that is usually MR3, first to 4 round wins. 

Each player has statistics for how they performed in that map, a few examples: their amount of kills, deaths, average damage per round (ADR), and percentage of rounds in which the player either had a kill, assist, survived or was traded (KAST%). These can be used to assess what a player is good at, although it is certainly not enough to only look at statistics to see a player's impact in the map. 

If you are interested in learning more about the game and its intricacies I suggest checking out: 
>https://www.reddit.com/r/LearnCSGO/ 

## Data

The data is taken and scraped from hltv.org, a competitive CS:GO hub.
We are working with 3 .csv files that split different aspects of competitive CS:GO matches:
- `picks.csv`: Each team's map pick/ban before a match is played, which other columns that can be used to merge the datasets together (e.g. event_id, match_id)
- `players.csv`: Every player's statistics for each map played in the match. Other information like opponent and country of origin is in the dataset as well.
- `results.csv`: The results for every map played in the timeframe with accompanying information like round wins based on ct/t sides and which team started on what side.

## Notebooks

- `csgo_picks.ipynb`: Some visualizations and analysis of team's map picks/bans throughout the dataset.
- `csgo_players.ipynb`: Cleaned up the data, performed feature engineering, and visualized the spread of the player statistics.  
- `csgo_results.ipynb`: Some data wrangling and analysis of map winrates based on side, as well as a few classification models attempting to predict map outcomes based on a variety of features.

## Results and Conclusions
- `csgo_picks.ipynb`
  - The main goals of analysis on this dataset were to try and understand how the map picks/bans were distributed over the full dataset and in different time frames, as well as see how the landscape of picks/bans changed when Vertigo was introduced in 2019. 

Information on the frequencies of the picks of each map over the pick/ban process.
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/map_pick_count.png)

Countplot of map vetoes in the pick/ban process post Vertigo's introduction.
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/vertigo_countplot.png)

Wrote a function to create a visualization of culminative frequency based on a column and a date timeframe:

```python 
def plot_maps(date_range: list = None, column='t1_picked_1'):
    """ Plot column frequency by a date range. Default will be team 1 map picks with the full dataset.
    """
    assert column in ['t1_removed_1', 't1_removed_2', 't2_removed_1', 't2_removed_2', 't1_picked_1', 't2_picked_2', 'left_over']
    new_df = pd.DataFrame(None, columns=['Mirage', 'Inferno', 'Train', 'Overpass', 'Nuke', 'Dust2', 'Cache', 'Cobblestone', 'Vertigo'])
    if date_range is None:
        for index, row in picks.sort_values(by='date',ascending=True).iterrows():
            map_pick = row[column]
            date = row['date']
            if math.isnan(new_df[map_pick].max()):
                new_df.loc[date, map_pick] = 1
            else:
                new_df.loc[date, map_pick] = new_df[map_pick].max() + 1
    else:
        df = picks[(picks['date'] > date_range[0]) & (picks['date'] < date_range[1])].sort_values(by='date',ascending=True)
        for index, row in df.sort_values(by='date',ascending=True).iterrows():
            map_pick = row[column]
            date = row['date']
            if math.isnan(new_df[map_pick].max()):
                new_df.loc[date, map_pick] = 1
            else:
                new_df.loc[date, map_pick] = new_df[map_pick].max() + 1
    return new_df
```    
Culminative frequency of map picks over the full dataset visualizing the prevalence of some maps as popular picks and showing how other maps were not as prioritized.
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/map_pick_cfreq.png)

Culminative frequency of map vetoes since Vertigo's introduction visualizing how Vertigo was far and away the most vetoed map as it was introduced.
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/vertigo_map_veto_freq.png)

- `csgo_players.ipynb`
  - The main goal for this dataset was to visualize player statistics and explore how the statistics related to eachother.

After cleaning and feature engineering, I visualized the statistics as frequency distributions.
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/distplot_players.png)

Similarly, I looked at the boxplot distributions of the player statistics separated by maps played in the series.
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/boxplot_players.png)

Finally, a heatmap of the correlations between the statistics.
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/corr_heatmap_players.png)

- `csgo_results.ipynb`
  - The major goals in this dataset were to visualization map winrates on both CT and T sides, team specific map winrates, and a classification predictive model attempting to predict map wins based on a number of features in the `results.csv` dataset.

This plot is an exmaple output I wrote in the results notebook of a function that returns a graph of a specific map's round wins separated by side.
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/nuke_round_wins.png)

This is a table that shows CT and T winrates based on the map.

![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/map_win_pct.png)


The code below is a function that will output a dataframe of the map winrates of a specific team:
```python
def win_rate_per_map(team: str):
    assert (team in results['team_1'].unique() or team in results['team_2'].unique())
    team_df = results[(results['team_1'] == team) | (results['team_2'] == team)]
    
    win_count = {'Inferno': 0, 'Overpass': 0, 'Train': 0, 'Mirage': 0, 'Nuke': 0, 'Dust2': 0, 'Cache': 0,
            'Vertigo': 0, 'Cobblestone': 0}
    map_count = {'Inferno': 0, 'Overpass': 0, 'Train': 0, 'Mirage': 0, 'Nuke': 0, 'Dust2': 0, 'Cache': 0,
            'Vertigo': 0, 'Cobblestone': 0}
    maps_played = 0
    
    for index, row in team_df.iterrows():
        if row['team_1'] == team:
            team_df = 1
        else:
            team_df = 2
        current_map = row['_map']
        if row['map_winner'] == team_df:
            win_count[current_map] += 1
        maps_played += 1
        map_count[current_map] += 1
    
    map_winrate = {key: (100*np.round(value/map_count[key], 4)) for key, value in win_count.items()}
    
    return pd.DataFrame([map_winrate, win_count, map_count], index=['Win %', 'Win Count','Map Count']).transpose()
```
An example of this with popular and perennial winners Astralis:
```python
win_rate_per_map('Astralis')
```
![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/astralis_map_wins.png)

Finally, the final goal was to build a predictive model to predict map wins. 

This is a confustion matrix of a logistic regression model as a base model.

![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/logreg_matrix.png)

The matrix shows that the model performed very poorly and the accuracy score was only just over 50%. In order to improve this I performed hyper-parameter optimation using Scikit-Learn's GridSearchCV model selection package to determine the optimal parameters for logistic regression.

After the optimization the resulting confusion matrix shows that this model performed much better.

![plot](https://github.com/tahashahab/competitive_csgo/blob/main/Graphs/logreg_matriix_hpo.png)

# Thank you for reading!
