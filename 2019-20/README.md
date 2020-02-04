# METHODOLOGY: Predicting the 2020 All-NBA teams with a deep neural network

[Link to blog post.](https://dribbleanalytics.blog/2020/02/2020-all-nba-predictions)

## Data collection

First, we created a database of every player who started their career in the 3-point era (rookie season on or after 1979-1980 season). We set this boundary so that every player played their entire career with the 3-point line.

The following stats were recorded for all these players:

|Other|Shooting|Rebounding|Passing/ball control|Defense|Catch-alls|
:--|:--|:--|:--|:--|:--|
|G|FG, FGA, FG%|ORB, ORB%|AST, AST%|STL, STL%|PER|
|GS|2P, 2PA, 2P%|DRB, DRB%|TOV, TOV%|BLK, BLK%|OWS, DWS, WS, WS/48|
|MP|3P, 3PA, 3P%|TRB, TRB%|USG%|PF|OBPM, DBPM, BPM|
||FT, FTA, FT%||||VORP|
||eFG%||||RAPTOR (off. and def.)|
||TS%|||||
||3PAr, FTr|||||
||PTS|||||

All data was taken from Basketball-Reference, except for the RAPTOR data, which was taken from 538.

## Model creation

Using the above 47 features, we created a deep neural network in Keras to predict a player's All-NBA probability. Because the data is imbalanced (about 500 All-NBA players vs. 15,000 total samples), we performed borderline-SMOTE on the training data. It's very important that we only did this on the training data, as doing it before splitting the data causes the models to bleed information. This makes them artificially accurate and not predictive.

We split up the initial data into 50% training, 25% validation, and 25% testing. SMOTE was done on the training data.

The model had 6 layers. The first layer had 47 nodes (accounting for 47 features). The next layer had 32, then each subsequent layer had half as many nodes as the previous until we reached 4 nodes. After 4 nodes, our next layer was the output layer (1 node).

The first 3 layers had L1 regularization, and the final 3 layers had L2 regularization. This helps avoid overfitting.

Each layer except the output layer had a leaky ReLU activation function. The output layer had sigmoid activation such that all our probabilities fall between 0 and 1.

We used early stopping to minimize validation loss (our loss function was binary cross-entropy) with a look forward of 25 epochs to avoid overfitting. The fitted model is available in the "fitted-models" folder. The model titled "es_model" is the model with early stopping, while the "full_model" contains the model with no early stopping.

## Predicting the All-NBA teams

We collected data for the 2019-20 season on 2/3/2020 before any games were played. After collecting the necessary data, we made predictions from the model for this year's All-NBA teams.

We sort the teams by placing the highest probability player into the highest available slot for his position. If a player's position is unclear, we use their All-Star game listed position. If the player did not make the All-Star game, we use the position where they play the most minutes. So, Doncic is a guard, and Butler and Derozan are forwards.

Each time must have 2 guards, 2 forwards, and 1 center.

## Results

The table below shows the model's predictions:

|Team|G|G|F|F|C|
--:|:--|:--|:--|:--|:--|
|1|James Harden|Damian Lillard|Giannis Antetokounmpo|LeBron James|Anthony Davis|
|2|Luka Doncic|Ben Simmons|Jimmy Butler|Kawhi Leonard|Rudy Gobert|
|3|Devin Booker|Trae Young|Pascal Siakam|DeMar DeRozan|Nikola Jokic|

The All-NBA probabilities for every player are available [here](https://docs.google.com/spreadsheets/d/1SdQUExlLtwTdGT-_0gMrUEv4_l0Ggvtv9c2DpIOj0tw/edit?usp=sharing).