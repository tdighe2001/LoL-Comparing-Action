# LoL-Comparing-Action
On examining the 2022 league of legends dataset, we aim to answer the following questions: Looking at tier-one professional leagues, which league has the most “action-packed” games? Is the amount of “action” in this league significantly different than in other leagues? 

## Introduction and Question Identification

## Cleaning and EDA
Data Cleaning for Assessment of Missingness:
For the purpose of Assessment of Missingness, we extracted three columns from the original dataframe: ‘split’, ‘datacompleteness’ and ‘side’. In order to make permutation testing easier, we turned ‘datacompleteness’ and ‘side’ to bool columns where ‘complete’ and ‘Blue’ were True and ‘partial’ and ‘Red’ were False for ‘datacompleteness’ and ‘side’ respectively. 
Data Cleaning for hypothesis testing:

For both cases, all missing data was replaced with np.nans.
### Univariate Analysis
### Bivariate Analysis
### Interesting Aggregates
1. Average vision score by position and side - This could give you insight into which positions and sides tend to place more wards and control wards, which can be a valuable piece of information in the game. Here we see that ‘teams’ tend to place more wards and control wards by an extremely high margin. But, we cannot consider them because it is not an actual position. So, we look at the next highest -  ‘jng’. Thus, the position ’jng’ tends to place and control the most wards. We also see that blue teams appear to consistently place more wards and control wards. **still need to embed plot**
2. Win rate by champion and patch - By grouping the data by champion and patch, you can see how often each champion was played in each patch and how often they won. This could give you insight into which champions were strongest in each patch. Since this dataset is extremely big, we have just included the top 10 champions, i.e those who had a win rate of 100%. **still need to embed plot**

## Assessment of Missingness
### NMAR Analysis
We believe that the “monster kills own jungle”, “monster kills enemy jungle” column is NMAR. On a cursory glance, it appears that those two columns only have missing values when the ‘datacompleteness’ is complete. However, when we compare the number of rows when the df’s rows are complete (127404)  vs when the two columns have missing values (126888), they are not the same. Thus, while it is reasonable to assume that most missing values for these columns happen when the datacomleteness is complete, one cannot say this definitely. Thus, these two columns must be NMAR. This is because the data in this dataset was made from compiling data from different sources like Match History pages, lolesports.com, lpl.QQ.com, Leaguepedia, the Riot Games solo queue APIs, and more, so different sources may have collected different types of data leading to discrepancies between different leagues. Furthermore, some of this data might have been manually entered rather than API-sourced. 
Some additional data we might want to obtain is “player feedback”: If we have access to the players who participated in the matches, we could ask them for additional information on how the game was played. They may be able to provide more context on why certain data points are missing, and may be able to provide additional information on monster kills. Furthermore, game replay files could provide more detailed information on how the game was played, including information on which monsters were killed and when. By analyzing the game replay files, we may be able to fill in some of the missing values or atleast find the cause.

### Permutation testing
For our Assessment of missingness, we decided to look at the “split” column. We believe this column has non trivial missingness because: 1. There is no way to determine the value of column based on other columns. For example, it does not happen that the split values are missing only when data is partial. 2. There are **6-7 (list them)** different options available for the split entry. Although we notice that they tend to fall in groups, there is still no way to determine what the values may be as they do not follow a particular order (eg. alphabetical). For the two columns to test the dependence on, we decided to look at ‘datacompleteness’ and ‘side’.

Let’s examine the ‘datacompleteness’ case.
We first examine the proportion of complete values when split is null and split is false. **embed plot**


