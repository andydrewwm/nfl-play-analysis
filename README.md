# Shotgun vs. Under Center: An NFL Play-by-Play Analysis

## Introduction
This NFL play-by-play dataset covers every play from every game (except the Super Bowl) during the 2021-2024 NFL seasons. It provides an in-depth look at the game from a variety of angles:
- Game Context: This includes information like the game ID, season, date, teams involved, down, distance, yard line, etc.
- Play Type and Outcomes: Information about whether a play is a pass or a rush, the formation used, yards gained, penalties, touchdowns, and more.
- Play Descriptions: A text field that details exactly what happened during each play, providing insights into the action on the field.

### Guiding Question
For this project, my goal is to answer:
> Do teams running primarily out of Shotgun have higher yards gained per play than those primarily under center, or vice versa?

This question aims to shed light on how the formation chosen for a play impacts the yardage gained, and whether certain formations are better suited for offensive success.

### Dataset Overview
The dataset contains **185,963 rows** across the 2021-2024 seasons, with each row representing a single play during a game. This will give me plenty of space for filtering and grouping data.

The following columns are most relevant to answering my guiding question:
- `Formation`: The formation used by the offense (e.g., Shotgun, Under Center).
- `PlayType`: Whether the play was a rush or a pass.
- `Yards`: The number of yards gained during the play.
- `OffenseTeam`: The team running the play.
- `SeasonYear`: The season during which the play occurred.
- `Down`: The down during which the play was run.
- `ToGo`: The distance the offense needed for a first down.

### Why Should You Care?
Understanding how different offensive strategies, like the choice between Shotgun and Under Center formations, influence yardage gains can have a significant impact on how teams approach games. This analysis gives insights into how often these formations are used, how effective they are, and how they vary across different teams and game situations. By predicting the success of plays based on formations, play types, and other contextual features, this project provides a deeper understanding of what works on the field, which is valuable for coaches, analysts, and even fantasy football enthusiasts (like myself) looking to understand the game at a deeper level.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
Before starting my analysis, I loaded in and cleaned the dataset. Here is a detailed breakdown of the steps I took and my reasoning:

#### 1. Combining the CSV Files
I began by combining four separate CSV files I had downloaded from [NFLsavant.com](https://nflsavant.com/about.php) to combine the 2021-2024 NFL seasons into a single DataFrame. This allowed me to take advantage of the `SeasonYear` column and observe trends and patterns across seasons.

#### 2. Dropping Unnecessary Columns
After loading the dataset, I discovered several columns with the prefix `Unnamed...` and a column named `Challenger`, which were empty. These seemed to be caused by a formatting issue with extra commas in the original CSVs. I decided to drop these columns since they were irrelevant to my analysis and empty.

#### 3. Removing Unnecessary Play Types
Next, I filtered out irrelevant types of plays. This eliminated rows where the `PlayType` column contained values such as "NO PLAY", "TIMEOUT", "QB KNEEL", etc. Including these plays would have distorted my analysis since they do not contribute to offensive performance in terms of yards gained.

#### 4. Handling Incorrect and Inconsistent Values
Right before moving on, I noticed issues with the `Down` and `YardLineFixed` columns. Some `Down` values were incorrectly labeled as 0, which seemed to be due to mis-labeled kicking plays. These were simply dropped since they are special teams plays. Also, some `YardLineFixed` values were 0 or 100, which is impossible since those would be automatic touchdowns or safeties. Upon investigation, I realized that all these plays really occurred at the 50-yard line and must have been a computation error. I made the correction by replacing all 0's and 100's in `YardLineFixed` with 50.

#### What's Leftover
After cleaning, I still had **130,937 rows** in the dataset! Here is the head of the Dataframe:

| GameId    | GameDate  | Quarter | Minute | ... | PenaltyTeam | IsNoPlay | PenaltyType | PenaltyYards |
|-----------|-----------|---------|--------|-----|-------------|----------|-------------|--------------|
| 2021092612 | 2021-09-26 | 1       | 9      | ... | NaN         | 0        | NaN         | 0            |
| 2021092612 | 2021-09-26 | 1       | 8      | ... | NaN         | 0        | NaN         | 0            |
| 2021092612 | 2021-09-26 | 1       | 7      | ... | NaN         | 0        | NaN         | 0            |
| 2021092612 | 2021-09-26 | 1       | 7      | ... | NaN         | 0        | NaN         | 0            |
| 2021101008 | 2021-10-10 | 1       | 9      | ... | NaN         | 0        | NaN         | 0            |


### Univariate Analysis
Right away I filtered the dataset by `SeasonYear` to work with only plays from the 2024 season to look at trends from this past year. I wanted to get a sense for what how often teams ran plays from the Shotgun formation, so I began by making a bar chart of formation counts.

<iframe
 src="assets/distrib_formation_2024.html"
 width="800"
 height="600"
 frameborder="0"
></iframe>

It is immediately obvious that Shotgun is the most popular formation from the 2024 NFL season. Teams played from Shotgun more than 21,000 times, which more than doubled plays from Under Center. Not to mention, "no huddle" plays were even more less common. Since No Huddle Shotgun is just fast-tempo Shotgun and vice versa for No Huddle and Under Center, I decided to make a new column at this point called `CombinedFormations` that strictly labeled plays Shotgun or Under Center. I believe the fast-tempo counterparts are similar enough to be considered the same for my analysis.

This was not what I expected. I'm familiar with Shotgun being the most popular formation in college football, but not the NFL. This dataset could be generalizing many types of backfield formations to be Shotgun. At the same time, the Philadelphia Eagle's Superbowl win with a shotgun-heavy offesnse this year shows how effective the formation is.

### Bivariate Analysis
Next, I wanted to consider situations where teams are running the ball from Shotgun and analyze its success. The first situation that came to mind was *3rd Down*, the money down. Converting on 3rd Down reliably is vital to a team's success, so I wanted to gauge how effective it has been to run from Shotgun on any given down.

<iframe
 src="assets/rush_yds_down_2024.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

Surprisingly, it seems like Shotgun runs are frequently more successful that Under Center runs on all downs. The median yards gained on downs is at least a full yard higher from Shotgun than Under Center on all four downs. Most notably, Shotgun gains between 2-8 yards within the Interquartile Range (IQR) while Under Center only manages 0-3 yards.

So Shotgun plays have much more potential for a big run of 5+ yards on the most crucial down. This indicates teams running from Shotgun more frequently succeeding on drives more often.

### Interesting Aggregates
To better compare teams Shotgun rushing success, I created a pivot table. I grouped by the `CombinedFormation` and `OffenseTeam`, then computed the average yards gained by each team from each formation.

| CombinedFormation | ARI  | ATL  | BAL  | BUF  | ... | SF   | TB   | TEN  | WAS  |
|-------------------|------|------|------|------|-----|------|------|------|------|
| SHOTGUN           | 6.45 | 5.10 | 6.85 | 5.39 | ... | 5.46 | 6.75 | 4.58 | 5.01 |
| UNDER CENTER      | 4.36 | 4.34 | 4.88 | 4.47 | ... | 4.77 | 4.59 | 3.83 | 4.32 |

The resulting table helped me to understand which teams were especially successful from Shotgun by comparing average yards gained from Shotgun across teams. This also allowed me to check a team's Shotgun average yardage against their own success from Under Center. As the table shows, it is common to gain more yards from Shotgun than Under Center.

It is important to note that I **did not impute any values** during this analysis. After data cleaning, I did have any missing values within columns I used.

## Framing a Prediction Problem
My prediction problem is a regression problem, where the goal is to predict the number of yards gained on the next play, given various features of a football play such as down, distance, formation, and play type.

The response variable in this case is `Yards`, which represents the yards gained on the play. I chose this variable because it is directly related to offensive efficiency and decision-making. The number of yards gained is a key metric for understanding the success of a play, which is central to analyzing team strategies and performance.

To evaluate my model, I will use Mean Squared Error (MSE). MSE is appropriate for this regression problem because it measures the average squared difference between the predicted and actual values, providing a clear indication of how well the model is performing in predicting the number of yards gained. I prefer MSE over other metrics like R-squared because it directly penalizes large prediction errors, which is particularly useful for a problem where predicting exact yardage matters for evaluating performance.

## Baseline Model
I'm using a Linear Regression model to predict the number of yards gained on the next play.

Features in the model are:
- `OffenseTeam` (Nominal): The team running the play (one-hot encoded).
- `YardLine` (Quantitative): The starting yard line.
- `Down` (Ordinal): The current down (one-hot encoded).
- `ToGo` (Quantitative): The distance to a first down.
- `Formation` (Nominal): The offensive formation (one-hot encoded).
- `PlayType` (Nominal): The type of play (one-hot encoded).

Model performance was:
```
Training MSE: 68.28998192392213
Test MSE 70.62823826546288
```

The slight difference between training and test MSE suggests that the model is generalizing well, without overfitting. But is the model good? It does a decent job, but not very good. This baseline model is built on basic features and a basic linear regression model, so adding more features or trying more advanced models could  make it better.

## Final Model
To improve the model, I added the following features:
- `Formation_PlayType`: This feature combines the formation and play type (e.g., Shotgun_Pass, UnderCenter_Rush). This allows us to capture nuance like how oftentimes a defense expects a run from Under Center and a pass from Shotgun.

- `Team_Down`: This feature combines the team and the down (e.g., ARI_1, BAL_2). This is useful because a team's strategy on a given down may vary from another team's, so it gives additional context to the team's play call.

These features are good for the model because they provide more context about the team's decisions during a play, while still containing information known to the offense. By capturing interactions between the formation and play type, as well as the team's decision-making on specific downs, these features allow the model to better understand the dynamics of offensive plays and predict yardage more effectively.

For the Final Model, I chose the Random Forest Regressor. This is a non-linear model that can capture complex interactions between features, which is crucial for predicting yardage in football plays where relationships are not strictly linear.

The best hyperparameters were:
- `n_estimators`: 200
- `max_depth`: 10
- `min_samples_leaf`: 4

I selected these hyperparameters using GridSearchCV, which performs an exhaustive search over a grid of hyperparameter values and selects the best combination using cross-validation.

Final model performance was:
```
Training MSE: 65.02
Test MSE: 69.72
```

Compared to the baseline model, which had a Test MSE of 70.63, the final model shows a small improvement, with a lower MSE on both the training and test sets. This indicates that the Random Forest model, with its added complexity and tuned hyperparameters, does a better job of predicting yardage than the simpler linear regression model used in the baseline. The improvement suggests that the added features and model complexity help capture the non-linear relationships in the data more effectively, even if small.
