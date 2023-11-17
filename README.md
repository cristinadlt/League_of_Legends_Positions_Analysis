# League_of_Legends_Positions_Analysis
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