# LoL-Comparing-Action
On examining the 2022 league of legends dataset, we aim to answer the following questions: Looking at tier-one professional leagues, which league has the most “action-packed” games? Is the amount of “action” in this league significantly different than in other leagues? 

## Introduction and Question Identification
The dataset that we have chosen is the League of Legends dataset where we chose to answer the question: Out of the tier-one professional leagues, which league has the most “action-packed” games? Is the amount of “action” in this league significantly different than in other leagues?
We chose to determine the game's action by using the team kills per minute column. By doing so, we can direct enthusiastic LoL viewers to a novel and action-heavy form of League of Leagues that they probably haven’t watched before.  In our dataset, we used the columns:
gameid: the id of the game that the row of information belongs to
league: the regional league the that match is participating in
team kpm: the kills per minute of a team

We took only the information that belongs to the teams in tier 1 leagues and combined the rows based on gameid to get a concise row of information for each match. With this we have 3039 rows or matches to analyze.


## Cleaning and EDA
Data Cleaning for Assessment of Missingness:

For the purpose of Assessment of Missingness, we extracted three columns from the original dataframe: ‘split’, ‘datacompleteness’ and ‘side’. In order to make permutation testing easier, we turned ‘datacompleteness’ and ‘side’ to bool columns where ‘complete’ and ‘Blue’ were True and ‘partial’ and ‘Red’ were False for ‘datacompleteness’ and ‘side’ respectively. This DataFrame contains 149232 rows and 4 columns.

|    | split   | datacompleteness   | side   | split_missing   |
|---:|:--------|:-------------------|:-------|:----------------|
|  0 | Spring  | True               | True   | False           |
|  1 | Spring  | True               | True   | False           |
|  2 | Spring  | True               | True   | False           |
|  3 | Spring  | True               | True   | False           |
|  4 | Spring  | True               | True   | False           |

Data Cleaning for hypothesis testing:

| league   |   gamelength |   kills |   team kpm |   barons |   damagetochampions |     dpm |   damagetakenperminute | is_vcs   |
|:---------|-------------:|--------:|-----------:|---------:|--------------------:|--------:|-----------------------:|:---------|
| LPL      |         1365 |      19 |     0.8351 |        1 |               70503 | 3099.03 |                4805.14 | False    |
| LPL      |         1444 |      30 |     1.2465 |        1 |               94875 | 3942.17 |                6133.21 | False    |
| LPL      |         1893 |      20 |     0.6339 |        2 |              102576 | 3251.22 |                4995.75 | False    |
| LPL      |         2271 |      19 |     0.502  |        2 |              135067 | 3568.48 |                5167.08 | False    |
| LPL      |         1900 |      27 |     0.8526 |        2 |              115066 | 3633.66 |                4841.34 | False    |

Each match contains 12 rows of data, 10 for players and 2 for teams. We decided to take only the rows for team data for our analysis. Then to answer the hypothesis we must only look at tier one teams and filter it by whether they were one of the 9 tier one leagues. After getting the two rows for the tier one teams, we then combined both rows to get a single row for each match. For the EDA analysis for hypothesis testing, we queried the only relevant columns that contained complete data for all tier one leagues: league, gamelength, kills, team kpm, barons, damagetochampions, damagetakenperminute. These columns were relevant in trying to find interesting differences in leagues. When combining the team data we must be wary of duplicate data and in this case gamelength. The gamelength column was the same for both teams in a match as it indicated the length of the same match that both teams participated in. To maintain correctness in our data, the resulting gamelength column was halved. During the hypothesis testing, we created a new column which contained a boolean of whether the match played was part of the VCS league or not. This was to be able to measure the difference between the distributions of VCS and non-VCS matches.

For both cases, all missing data was replaced with np.nans.

### Univariate Analysis
<iframe src="Assets/kills.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis
<iframe src="Assets/cond-dist-team-kpm.html" width=800 height=600 frameBorder=0></iframe>
We could see a large discrepancy between VCS and non VCS matches in previous bivariate analytics and wanted to visualize the conditional distributions of VCS and non VCS matches in terms of team kpm. There seems to be a difference in their distributions with VCS having more team kpm on average.

### Interesting Aggregates
1. Average vision score by position and side - This could give you insight into which positions and sides tend to place more wards and control wards, which can be a valuable piece of information in the game. Here we see that ‘teams’ tend to place more wards and control wards by an extremely high margin. But, we cannot consider them because it is not an actual position. So, we look at the next highest -  ‘jng’. Thus, the position ’jng’ tends to place and control the most wards. We also see that blue teams appear to consistently place more wards and control wards.

|                  |   visionscore |
|:-----------------|--------------:|
| ('bot', 'Blue')  |       38.6675 |
| ('bot', 'Red')   |       37.0821 |
| ('jng', 'Blue')  |       43.4252 |
| ('jng', 'Red')   |       42.8077 |
| ('mid', 'Blue')  |       33.8845 |
| ('mid', 'Red')   |       33.7007 |
| ('sup', 'Blue')  |       79.7652 |
| ('sup', 'Red')   |       78.7803 |
| ('team', 'Blue') |      226.785  |
| ('team', 'Red')  |      223.075  |
| ('top', 'Blue')  |       31.0384 |
| ('top', 'Red')   |       30.7046 |

2. Win rate by champion and patch - By grouping the data by champion and patch, you can see how often each champion was played in each patch and how often they won. This could give you insight into which champions were strongest in each patch. Since this dataset is extremely big, we have just included the top 10 champions, i.e those who had a win rate of 100%. This table shows the first 10 entries.

|                         |   result |
|:------------------------|---------:|
| ('Zyra', 12.21)         |        1 |
| ('Tryndamere', 12.2)    |        1 |
| ('Twisted Fate', 12.16) |        1 |
| ('Aphelios', 12.17)     |        1 |
| ('Twisted Fate', 12.18) |        1 |
| ('Twitch', 12.01)       |        1 |
| ('Samira', 12.21)       |        1 |
| ('Zed', 12.23)          |        1 |
| ('Zed', 12.21)          |        1 |
| ('Jinx', 12.23)         |        1 |

## Assessment of Missingness
### NMAR Analysis
We believe that the “monster kills own jungle”, “monster kills enemy jungle” column is NMAR. On a cursory glance, it appears that those two columns only have missing values when the ‘datacompleteness’ is complete. However, when we compare the number of rows when the df’s rows are complete (127404)  vs when the two columns have missing values (126888), they are not the same. Thus, while it is reasonable to assume that most missing values for these columns happen when the datacomleteness is complete, one cannot say this definitely. Thus, these two columns must be NMAR. This is because the data in this dataset was made from compiling data from different sources like Match History pages, lolesports.com, lpl.QQ.com, Leaguepedia, the Riot Games solo queue APIs, and more, so different sources may have collected different types of data leading to discrepancies between different leagues. Furthermore, some of this data might have been manually entered rather than API-sourced. 
Some additional data we might want to obtain is “player feedback”: If we have access to the players who participated in the matches, we could ask them for additional information on how the game was played. They may be able to provide more context on why certain data points are missing, and may be able to provide additional information on monster kills. Furthermore, game replay files could provide more detailed information on how the game was played, including information on which monsters were killed and when. By analyzing the game replay files, we may be able to fill in some of the missing values or atleast find the cause.

### Permutation testing
For our Assessment of missingness, we decided to look at the “split” column. We believe this column has non trivial missingness because: 1. There is no way to determine the value of column based on other columns. For example, it does not happen that the split values are missing only when data is partial. 2. There are 12 ('Spring', 'Split 1', 'Winter', 'Opening', 'Summer', 'Champ 1','Split 2', 'Closing', '2022', 'Pro-Am', 'Fall', 'Champ 2') different options available for the split entry. Although we notice that they tend to fall in groups, there is still no way to determine what the values may be as they do not follow a particular order (eg. alphabetical). For the two columns to test the dependence on, we decided to look at ‘datacompleteness’ and ‘side’.

Let’s examine the ‘datacompleteness’ case.
We first examine the proportion of complete values when split is null and split is false.

| null_split   |   datacompleteness |
|:-------------|-------------------:|
| False        |           0.798296 |
| True         |           0.97648  |

Since ‘datacompleteness’ values vary significantly, we can rule out MCAR. We can also rule out MD because there is no way to predict what the value in 'split' may be depending on the data completeness. This leaves MAR and NMAR. We hypothesize that the data is MAR because even though the ‘datacompleteness’ is only partial when the split is either spring/summer, all of the missing values occur when ‘datacomplenetess’ is complete. Thus, the missingness of the value is not related to the actual unreported value nor does there appear to be a relationship between the propensity of a value to be missing and its value.

| datacompleteness   |   split_missing = False |   split_missing = True |
|:-------------------|------------------------:|-----------------------:|
| False              |                0.201704 |              0.0235203 |
| True               |                0.798296 |              0.97648   |

On grouping the data differently, we see that the missing split values tend to disproportionately occur when data completeness is complete. The two columns look very different, which is evidence that 'split''s missingness does depend on 'datacompleteness’.
Here is the data in the form of a bar chart
<iframe src="Assets/datacomp_dist.html" width=800 height=600 frameBorder=0></iframe>
Thus, we hypothesisize that the missing values are MAR. 

Null Hypothesis: The missingness of values in the column ‘split’ is independent of the values in the column ‘datacompleteness’. 

Test Statistic: Since we have only two categories (complete/partial) using TVD is the same taking the absolute difference in proportions. Thus, for our test statistic, we will be using the absolute difference in proportions.

On conducting the permutation test, we obtain the p-value of 0 for a 5% significance level. Thus, we reject the null hypothesis and say that the missingness of ‘split’ values does depend on the values in ‘datacompleteness’.

Now, we examine the ‘side’ case.
Like the ‘datacompleteness’ case, we first examine the proportion of complete values when split is null and split is false.

| null_split   |   side |
|:-------------|-------:|
| False        |    0.5 |
| True         |    0.5 |

We see that ‘side’ values have the exact same proportions. Since the proportion of blue/reds appears to be the same whether or not the split values are missing, we hypothesize that 'split' is MCAR , i.e does not depend on team. This is further illustrated by the following aggregate and bar graph:

| side   |   split_missing = False |   split_missing = True |
|:-------|------------------------:|-----------------------:|
| False  |                     0.5 |                    0.5 |
| True   |                     0.5 |                    0.5 |

<iframe src="Assets/side_dist.html" width=800 height=600 frameBorder=0></iframe>
We see that the values are exactly evenly split.
Null Hypothesis: The missingness of values in the column ‘split’ is independent of the values in the column ‘side’.

Like in the ‘datacompleteness’ case, we use absolute difference in proportions as our test statistic.

On conducting the permutation test, we obtain the p-value of 1 for a 5% significance level. Thus, we fail to reject the null hypothesis and say that the missingness of ‘split’ values does not depend on the values in ‘side’.

## Hypothesis Testing


| is_vcs   |   team kpm |
|:---------|-----------:|
| False    |     0.8351 |
| False    |     1.2465 |
| False    |     0.6339 |
| False    |     0.502  |
| False    |     0.8526 |


Null Hypothesis: In tier 1 leagues, the action (team kpm/kills per minute) of non VCS games and VCS games have the same distribution.

Alternative Hypothesis: In tier 1 leagues, the VCS league has more action (team kpm/kills per minute) than non VCS games.

Test Statistic: Difference in group means
<h3 style="text-align: center;">mean teamkpm in vcs matches - mean teamkpm in non vcs matches</h3>

Significance Level: 0.05

To conduct the permutation test we accessed only the columns of is_vcs and teamkpm. The numerical difference between the distributions of the two groups was calculated with the test statistic of the difference in group means. The permutation testing consisted of 500 repetitions of shuffling the teamkpm column and calculating the difference in group means between non vcs and vcs teamkpms. We chose a significance level of 0.05 to be the threshold of the permutation test and ended up getting a p-value of 0.00.

<iframe src="Assets/diff_group_mean.html" width=800 height=600 frameBorder=0></iframe>

**Based on the results of the permutation test, we can reject the null hypothesis that the two groups of team kpm come from the same distribution.**



