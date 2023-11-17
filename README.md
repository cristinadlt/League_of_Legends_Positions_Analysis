# League of Legends Positions Analysis
A project for DSC80 at UCSD in which we clean, perform EDA, assess missingness, and conduct a hypothesis test with 2022 League of Legends data.

## Introduction
In this project, we explore the importance of positions within League of Legends as it relates to success in the game. More formally, our question is "Which role impacts the success of the team more: Top or Support?". In League of Legends, players want to advance to higher ranks and know which position impacts their performance the most. For some players, it may impact the direction of their professional careers as different positions require different skills. By investigating this question, we can determine if certain positions, Top or Support, increase the success of a player. 

Our dataset is a collection of data on Champions played in 2022 International competitive matches. These include the Worlds 2022 Main Event, MSI 2022, Worlds 2022 Play-In, and LCK 2022 Regional Finals datasets. The dataset contains 367 rows and 6 columns.  The columns relevant to our question include `Champion`, `Pos`, `P%`, `W%`,` KDA`,  and `KP`. We selected these columns as they all describe the performance of Champions and their positions during a match in relation to the overall performance of the team.

### Columns
- `Champion`: Name of champion
- `Pos`: Position of champion
- `P%`: Percentage of games champion was picked
- `W%`: Win percentage
- `KDA`: Total Kill/Death/Assist ratio
- `KP`: Kill participation percentage of team's kills

## Data Cleaning
We started off by merging all four datasets into one. As such, we have multiple of the same Champion in our final dataset. This does not interfere with our analysis because Champion does not necessarily need to be a unique identifier. Our question centers on the performance of the Champion based on their position and not the Champion themselves. After combining the datasets, we dropped columns that were not relevant to our question, keeping only `Champion`, `Pos`, `P%`, `W%`, `KDA`,  and `KP`. 

We then identified that missing data was denoted by `-` and replaced these values with `NaN`. Further inspecting the missing data, we concluded that rows where data was missing in all columns except for the `Champion` and `P%` columns had a common pattern. All rows with the missingness described had a ‘P%’ value of 0% for the tournament it was a part of. Thus, we decided to drop these rows as a Champion that was never chosen in the tournament would not provide any relevant information to the question at hand. In total, only 12 rows were dropped due to this specific case of missingness. 

Additionally, we also converted numeric columns to their respective data types. These columns included `W%`, `KDA`,  and `KP`. After these assessments, we dropped the rows with positions that were not relevant to our question, keeping only the rows with positions of Top or Support.

### Head of our cleaned DataFrame
| Pos     |   W% |   KDA |   KP |
|:--------|-----:|------:|-----:|
| Top     |   55 |   2.8 | 55.5 |
| Support |   50 |   5.5 | 66.7 |
| Support |   33 |   2.1 | 77.6 |
| Support |  100 |   4.3 | 72.2 |
| Support |   50 |   7.5 | 55.6 |
