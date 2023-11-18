# League of Legends Positions Analysis

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

## Cleaning and EDA 

### Data Cleaning
We started off by merging all four datasets into one. As such, we have multiple of the same Champion in our final dataset. This does not interfere with our analysis because Champion does not necessarily need to be a unique identifier. Our question centers on the performance of the Champion based on their position and not the Champion themselves. After combining the datasets, we dropped columns that were not relevant to our question, keeping only `Champion`, `Pos`, `P%`, `W%`, `KDA`,  and `KP`. 

We then identified that missing data was denoted by `-` and replaced these values with `NaN`. Further inspecting the missing data, we concluded that rows where data was missing in all columns except for the `Champion` and `P%` columns had a common pattern. All rows with the missingness described had a ‘P%’ value of 0% for the tournament it was a part of. Thus, we decided to drop these rows as a Champion that was never chosen in the tournament would not provide any relevant information to the question at hand. In total, only 12 rows were dropped due to this specific case of missingness. 

Additionally, we also converted numeric columns to their respective data types. These columns included `W%`, `KDA`,  and `KP`. After these assessments, we dropped the rows with positions that were not relevant to our question, keeping only the rows with positions of Top or Support.

#### Head of our cleaned DataFrame

| Pos     |   W% |   KDA |   KP |
|:--------|-----:|------:|-----:|
| Top     |   55 |   2.8 | 55.5 |
| Support |   50 |   5.5 | 66.7 |
| Support |   33 |   2.1 | 77.6 |
| Support |  100 |   4.3 | 72.2 |
| Support |   50 |   7.5 | 55.6 |

### Univariate Analysis

<iframe src="assets/kda-ratio.html" width=700 height=600 frameBorder=0></iframe>

This is the distribution of KDA ratios for the cleaned dataset. The plotted distribution is skewed to the right due to outliers, showing that most KDA ratios fall between 0-5.


### Biveriate Analysis

<iframe src="assets/position-vs-kda-ratio.html" width=700 height=600 frameBorder=0></iframe>

This histogram displays the distribution of KDA by position. By doing so, we can see that although both have a similar shape, the KDA for those with the position of “Support” had higher ratios more so than “Tops”.

<iframe src="assets/win-percentage-kda-ratio.html" width=700 height=600 frameBorder=0></iframe>

The scatterplot below displays a positive relationship between `W%` and `KDA`. This confirms that `KDA` can be used as a measure of success in future testing. 


### Interesting Aggregates

| Pos     |      W% |     KDA |      KP |
|:--------|--------:|--------:|--------:|
| Support | 49.4935 | 4.70649 | 67.4065 |
| Top     | 40.2143 | 2.64405 | 53.4548 |

We grouped our dataset by position to determine the means of `W%`, `KDA`, and `KP` for “Support” and “Top” individually. Looking at our grouped table, we can see that the `W%`, `KDA`, and `KP` were all higher on average for the “Support” position than for the “Top” position.

## Assessment of Missingness 

### NMAR Analysis
We do not believe any column in our cleaned dataset is Not Missing at Random (NMAR). Rather than NMAR, we believe that the columns that are missing are Missing by Design(MD). This is due to the fact that we can infer the missingness of other columns from values of  the `P%` column. As mentioned previously in details of our data cleaning, all the missing values in columns could be traced back to the same value in  `P%` , where all values were 0%. Therefore, missingness in the dataset can be described as MAR rather than NMAR. 

### Missingness Dependency
**Assessing the Missingness of  `Pos` on `Champion`**

We want to assess if the missingness of the column `Pos` is dependent on the column `Champion`. If the `Pos` column is dependent on `Champion` then it would be Missing at Random(MAR) else we would conclude that it is Completely Missing at Random(MCAR). 

To evaluate this dependency, we conducted a permutation test with the following pair of  hypotheses:

**H<sub>0</sub>**: The distribution of `Champion` when `Position` is missing is the **same** as the distribution of `Champion` when `Pos` is not missing.

**H<sub>1</sub>**: The distribution of `Champion` when `Position` is missing is **not the same** as the distribution of `Champion` when `Pos` is not missing.

We chose to use the Total Variation Distance (TVD) as the test statistic because our columns contain categorical data. The observed test statistic was calculated as the following: 

<p style="text-align: center;">t = 0.386</p>

Along with the empirical distribution of the TVD: 

<iframe src="assets/champ-pos-tvd.html" width=700 height=600 frameBorder=0></iframe>

After generating 100,000 simulations the resulting p-value was:

<p style="text-align: center;">p-value = 0.770</p>

Thus, at a significance level of 0.05 we fail to reject the null hypothesis and conclude that the missingness of the column `Pos` is not dependent on the column `Champion`. As such, we can say that `Pos` is MCAR  and not MAR in this case. 

**Assessing the Missingness of  `Pos` on `P%`**

We also want to assess if the missingness of the column `Pos` is dependent on the column `P%`. If the `Pos` column is dependent on `P%` then we would conclude that it is MAR and MCAR otherwise. 

To assess this, we conducted a permutation kolmogorov-smirnov test with the pair of  hypotheses below:

**H<sub>0</sub>**: The distribution of `P%` when `Position` is missing is the **same** as the distribution of `P%` when `Pos` is not missing.

**H<sub>1</sub>**: The distribution of `P%` when `Position` is missing is  **not the same** as the distribution of `P%` when `Pos` is not missing.

We chose to use the k-s test statistic to carry out this evaluation as the distributions of the columns are both centered closely around each other but their shapes may still be different. The distributions of the columns are displayed below: 

<iframe src="assets/p-by-missingness-of-position.html" width=700 height=600 frameBorder=0></iframe>

After conducting our test the following is the resulting p-value:

<p style="text-align: center;">p-value = 1.92*10<sup>-22</sup></p>

Thus, at a significance level of 0.05 we reject the null hypothesis and conclude that the missingness of the column `Pos` is dependent on the column `P%`. As such, we can say that in this case `Pos` is MAR  and not MCAR. 
