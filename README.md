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
