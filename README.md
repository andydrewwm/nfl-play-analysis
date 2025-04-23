# Shotgun vs. Under Center: An NFL Play-by-Play Analysis
A project for EECS 398 at U-M

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
If you're into football, strategy, or sports analytics, understanding the effectiveness of formations, like Shotgun vs. Under Center, could reveal insights into what makes an offense successful. This project focuses on answering a simple yet intriguing question: does the formation you choose influence the number of yards you gain on each play? For football fans and analysts, this knowledge could help inform game strategies and highlight successful offensive trends across seasons.

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


## Framing a Prediction Problem

## Baseline Model

## Final Model
