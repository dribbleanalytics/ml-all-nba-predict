# METHODOLOGY: Predicting the 2019 All-NBA teams with machine learning

If the normal .ipynb does not open, try opening the "NO-OUTPUTS" file.

**All the data, code, and graphs for the final predictions is contained in the end-of-season-predictions folder**

[Link to mid-season predictions blog post.](https://dribbleanalytics.blogm/2019/03/ml-all-nba-predict)

[Link to final predictions blog post.](https://dribbleanalytics.blog/2019/04/ml-mvp-all-nba-predict-2019)

## Dates of data collection

Data for the mid-season predictions was collected on 2/7 before any games were played.

Data for the final post was collected after the end of the regular season.

## Data collection

First, I created a database of every All-Star and All-NBA player in the 3-point era (1979-1980 season until now). For the All-Stars, I collected data for everyone who either played in the All-Star game or did not play in the All-Star game due to injury but still made an All-NBA team. So, I did not record data for players who were selected as All-Stars, did not play in the All-Star game, and subsequently did not make an All-NBA team.

The only players who played in the All-Star game and were not in the dataset were Dirk Nowitzki, Dwyane Wade, and Magic Johnson. Dirk and Wade were removed this year because they have no chance at an All-NBA team. Magic was removed in 1991-1992 because he did not play in the regular season.

The following stats were recorded for all these players:

|Counting stats|Advanced stats|Team stats|
:--|:--|:--|
|G|WS|Wins|
|MP|WS/48|Overall seed|
|PTS|VORP||
|TRB|BPM||
|AST|||
|STL|||
|BLK|||
|FG%|||
|3P%|||
|FT%|||

I then classified the players by their All-Star and All-NBA selections. A 1 indicates the "positive" class (the player made the All-Star/All-NBA team), while a 0 indicates the "negative" class.

There was only one class created for the All-NBA column even though there are 3 All-NBA teams. This will be discussed in the model creation section.

For lockout years - 1998-1999 and 2011-2012 - I scaled up the win shares, VORP, and team wins according to the number of games in the lockout year; 1998-1999 was scaled up from 50 to 82, and 2011-2012 was scaled up from 66 to 82.

For this year's predictions, I scaled these stats according to the number of games the team has played so far. The data was collected during the All-Star break.

All data was taken from [Basketball Reference](http://basketball-reference.com/).

## Model creation

My goal was to predict which players would make which All-NBA teams. I did this using classification models with a single class and then assigning players to the 1st, 2nd, or 3rd team based on positional availability and prediction certainty. 

Given that each team needs 2 guards, 2 forwards, and 1 big, I used only 1 class. The models don't know there need to be 5 players on each team that follow this position. Therefore, to make the teams, I looked at the probability of the positive class (i.e. the player will make an All-NBA team) and sorted the players accordingly.

Using scikit-learn, I created four models: a support vector classifier (SVC), a random forest classifier (RF), a k-nearest neighbors classifier (k-NN), and a multi-layer perceptron classifier (or deep neural network, DNN). Each model used scikit-learn's train test split function with a test size of 25%.

The models used the following parameters.

|Counting stats|Advanced stats|Team stats|Other|
:--|:--|:--|:--|
|PTS/G|VORP|Wins|All-Star *|
|TRB/G|WS|Overall seed||
|AST/G||||

* All-Star = 1 (made the All-Star team) or 0 (did not make the All-Star team)

Each model's accuracy was tested in various ways which are discussed in [the blog post](https://dribbleanalytics.blogspot.com/2019/02/ml-all-nba-predict.html).

## Predicting the All-NBA teams

As mentioned above, I first looked at the probability of the positive class (sklearn's predict_proba function). Then, I filled in the players with the highest probability according to their position in the highest available slots.

The prediction data set consisted of every player who played in the All-Star game, and one player who was not selected as an All-Star: Rudy Gobert. Many view Gobert as a big All-Star snub. This combined with his impressive stats made me include him in the prediction set.

## Results

The table below shows the average predicted results for the initial mid-season predictions.

|Average Predictions|Guard|Guard|Forward|Forward|Center|
:--|:--|:--|:--|:--|:--|
|1st team|Stephen Curry (0.974)|James Harden (1.000)|Paul George (0.996)|Giannis Antetokounmpo (1.000)|Nikola Jokic (0.939)|
|2nd team|Damian Lillard (0.901)|Russell Westbrook (0.799)|Kevin Durant (0.994)|Kawhi Leonard (0.871)|Joel Embiid (0.919)|
|3rd team|Kyrie Irving (0.783)|Ben Simmons (0.441)|LeBron James (0.679)|Blake Griffin (0.335)|Rudy Gobert (0.863)|

The table below shows the average predicted results for the final predictions.

|Average Predictions|Guard|Guard|Forward|Forward|Center|
:--|:--|:--|:--|:--|:--|
|1st team|Damian Lillard (0.947)\*\|James Harden (0.997)|Giannis Antetokounmpo (1.000)|Kevin Durant (0.984)|Joel Embiid (0.900)|
|2nd team|Stephen Curry (0.947)\*\|Russell Westbrook (0.811)|Paul George (0.963)|Kawhi Leonard (0.884)|Nikola Jokic (0.897)|
|3rd team|Kyrie Irving (0.713)|Kemba Walker (0.417)|LeBron James (0.551)|Blake Griffin (0.420)|Rudy Gobert (0.854)|
