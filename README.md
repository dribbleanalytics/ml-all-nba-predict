# Dribble Analytics All-NBA predictions

This repository contains code, data, and results for our MVP predictions for the 2018-19 and 2019-20 seasons.

Each folder will contain an updated dataset, IPython notebook, and folder of predictions (graphs). Each folder also has a project-specific README that goes more in-depth on the methods.

[Link to most recent All-NBA predictions post.](https://dribbleanalytics.blog/2020/02/2020-all-nba-predictions)

## 2018-19 predictions

For our 2018-19 predictions, we created four classification models to predict a player's All-NBA probability. They used a variety of counting stats, and advanced stats, and team success stats (seed and wins). They correctly predicted all 15 players to make an All-NBA team. They got the order wrong in some cases. The models predicted George over Durant for first team and Westbrook over Irving for the second team. Lillard and Curry were tied, so that's technically incorrect, too.

All the code, data, and graphs are available in the "2018-19" folder.

## 2019-20 predictions

We used a much different method for our 2019-20 predictions. We used a deep neural network in Keras and many more features, along with a much larger data set.

All the code, data, and graphs are available in the "2019-20" folder.

Our most recent predictions were performed using data from 2/3. They were:

|Team|G|G|F|F|C|
--:|:--|:--|:--|:--|:--|
|1|James Harden|Damian Lillard|Giannis Antetokounmpo|LeBron James|Anthony Davis|
|2|Luka Doncic|Ben Simmons|Jimmy Butler|Kawhi Leonard|Rudy Gobert|
|3|Devin Booker|Trae Young|Pascal Siakam|DeMar DeRozan|Nikola Jokic|

To see the All-NBA probabilities for every player, click [here](https://docs.google.com/spreadsheets/d/1SdQUExlLtwTdGT-_0gMrUEv4_l0Ggvtv9c2DpIOj0tw/edit?usp=sharing).
