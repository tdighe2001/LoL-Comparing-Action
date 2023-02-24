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
