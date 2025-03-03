# Video Game Average User Score Classification Project 

## Contents

1. [Problem Statement](#Problem-Statement)
2. [Project Directory](#Project-Directory)
3. [Project Requirements](#Project-Requirements)
4. [Data Dictionary](#Data-Dictionary)
5. [Executive Summary](#Executive-Summary)
6. [Data Collection](#Data-Collection)
7. [Data Cleaning](#Data-Cleaning)
8. [Exploratory Data Analysis](#Exploratory-Data-Analysis)
9. [Preprocessing](#Preprocessing)
10. [Modeling](#Modeling)
11. [Conclusions & Recommendations](#Conclusions--Recommendations)

## Problem Statement

Video games take years of development time and cost anywhere from a few million to well over a hundred million dollars to produce. There are plenty of examples of video games that are poorly received and never establish a lasting player base. There are a few potential metrics that may determine how a game performs. One metric that can determine how a game performs is video game reviews, particularly user scores for a game. Using video game reviews from Metacritic I want to create a model that will help us identify the attributes of a video game that will help it get a high average user score.

## Project Directory
```
Capstone_Project
|__ assets
|   |__ avg_user_score.jpg
|   |__ categorical_consoles_eda.jpg
|   |__ categorical_developer_eda.jpg
|   |__ categorical_genre_eda.jpg
|   |__ confusion_matrix_log_regression.jpg
|   |__ continuous_critic_score_eda.jpg
|   |__ continuous_critic_score_hist_eda.jpg
|   |__ lemm_log_coefs.jpg
|   |__ log_coefs.jpg
|   |__ vectorizing_summary_eda.jpg
|__ code
|   |__ 01_Web_Scraping_Metactritic.ipynb
|   |__ 02_Data_Cleaning.ipynb
|   |__ 03_EDA_&_Preliminary_Analysis.ipynb
|   |__ 04_Preprocessing.ipynb
|   |__ 05_Modeling.ipynb
|__ data
|   |__ cleaned_all_console_reviews.csv
|   |__ df_for_further_nlp.csv
|   |__ final_all_console_reviews.csv
|   |__ final_reviews_modeling.csv
|   |__ top_100_pc_games.csv
|   |__ top_100_ps4_games.csv
|   |__ top_100_ps5_games.csv
|   |__ top_100_switch_games.csv
|   |__ top_100_xbox-series-x_games.csv
|   |__ top_100_xboxone_games.csv
|__ README.md
```

## Project Requirements
The following libraries were used in the project.
- Requests
- BeautifulSoup
- Sleep
- Datetime
- Pandas
- Numpy
- Clean
- Seaborn
- Stats
- Matplotlib
- Sklearn
- Spacy
- NLTK

If attempting to run the web scraper please note that Metacritic sometimes limits how often you can scrape. You might have to run the scraper for each console separately, not immediately one after the other. If the scraper fails please either wait or download the required files [here](https://we.tl/t-dy4FRbQ9fs). If the link has expired please contact me [here](https://www.linkedin.com/in/francisco-trejo07/). 


## Data Dictionary
|Feature|Type|Description|
|---|---|---|
| console | object |electronic device that outputs a video signal or image to display a video game|
| video_game_name |object | name of video game|
| summary |object | summary of what video game is about|
| developer |object | the developer (creator) of the video game|
| genre(s) |object | genre of video game|
| num_players* | int| max number of players that can play in the same game environment at the same time|
| esrb_rating |object | ESRB (Entertainment Software Rating Board) rating of video game|
| critic_score |int| critic review score of video game|
| avg_user_score |float| average user review score of video game|
| user_review |object | user review of video game|
| user_score |int|user score of video game |

\* values of 100 - games with online multiplayer <br>
\* values of 150 - games considered to be MMOs (massive multiplayer online games)

## Executive Summary
For this project, I set out to get various user reviews for the top 100 all-time games from [Metacritic](https://www.metacritic.com/browse/games/score/metascore/all/ps4/filtered) for 6 different consoles. Using the Requests library and BeautifulSoup I was able to scrape over 100k reviews. All of the reviews were combined into a master data frame to use in our model to help identify the attributes of a video game that will help it get a high average user score.

The next step was to clean our data. Null values were addressed, and values were imputed and cleaned. Using cleantext and the clean function all of the text values in our dataset were also cleaned. Numbers were tokenized and emojis were removed as part of the cleaning process. With a clean dataset, I was able to move on to exploratory data analysis.

During EDA every column was explored. The target column was calculated by checking if the review is for a game above/below the median average user score of 8.4. Continuous features were explored using a scatter plot and a box plot. Categorical features were explored using bar charts and grouped bar charts with the target. Once all features were explored it was time to proceed with preprocessing.

For preprocessing all categorical variables were dummied. For video game names and developers I feature engineered custom columns to deal with all the unique values, some of which had only a few occurrences. From there I vectorized our columns with text values and explored the words that had a TF-IDF score of 1. I then condensed our data into a final .csv file ready for modeling.

For modeling, I used a Logistic Regression and Random Forest model for their interpretability. A grid search was performed for both models to find the best hyperparameters. Coefficients were extracted and interpreted in relation to our target. A confusion matrix was also created along with a classification report to see what was the precision and recall. The random forests model produced the highest accuracy score but due to its lack of interpretability with its coefficients emphasis was made to improve the logistic regression model. Text features were lemmatized and vectorized and then re-run in our logistic regression model. Coefficients were once again extracted and examined, successfully identifying the top 10 features to improve the chances of a game having a score above the median average user score of 8.4.

## Data Collection
For our data, I set out to scrape the reviews from Metacritic for the top 100 games of all time for 6 different consoles. Those consoles were PlayStation 4/5, PC, Nintendo Switch, Xbox One, and Xbox Series X. Using the Requests library along with BeautifulSoup I created a function that scraped at most 1000 reviews per game for all 100 games in the all-time list for each console. Once scraped, the reviews were saved to a CSV file. The results included over 100,000 reviews that were all later merged into a final data frame. The individual CSV files for the game reviews are available in the [data](./data) folder (the larger combined CSV files are not in the repo due to size limits, see [Project Requirements](#Project-Requirements) to download the larger files).

## Data Cleaning
For data cleaning, I explored each feature in our combined data frame. Each feature was checked for any Null values, if any were present steps were taken to impute these missing values. When scraped from Metacritic some columns had unnecessary information that was later cleaned to extract the values needed for modeling. For our text columns, summary and user reviews, I used the [clean](https://pypi.org/project/clean-text/) function which helped us either get rid of or encode certain text to make it easier for vectorizing later on.

## Exploratory Data Analysis
During our EDA I explored every feature and its relationship to our target. Our target variable relied heavily on the average user score. Preliminary EDA was done on the feature such as plotting a histogram to see the distribution of the variable. ![](./assets/avg_user_score.jpg) The next thing to do was to calculate our target, which is whether or not our game is at/above or below the median average user score of 8.4. This helped formulate our problem to be categorical and helped balance out the target variable by using the median. I also created a few functions to better facilitate the EDA for the rest of our columns. With this, I was ready to continue.

For our categorical features, I used bar charts to see the value counts of the feature, for those with too many unique values I looked at the top 10. To see the relationship between a categorical feature with our target I used grouped bar charts to visualize the counts of our variables above/below our target as seen below![](./assets/categorical_consoles_eda.jpg) This helps us answer the question for each of our categorical variables, how many are above/below the target.

For our continuous variables, I used scatter plots and box plots to help us identify any outliers that are present. I also plotted multiple histograms to help us discern the density of our continuous variable with our target. ![](./assets/continuous_critic_score_hist_eda.jpg) This process helped us gain some further insight into what our data looks like and get a better grasp of how this data can help us answer our question.

## Preprocessing
The final step before modeling was to preprocess our data and get it ready to use in our models. Here I looked at some of the outliers that were present in our EDA and removed them as necessary. For our categorical variables, I created dummy columns but for categorical variables with too many unique values, I feature engineered columns. These columns consisted of “appears in top 20 or top 25 most frequent” to be able to reduce the number of columns generated. For our text columns, I vectorized the words using TfidVectorizer creating 50 features for each text column. I also plotted the 15 most frequent words to see how many times they appeared in our corpus.![](./assets/vectorizing_summary_eda.jpg) I also addressed the imbalance present in our target column and randomly sampled rows that were above the target to create a balance. Once that was done I saved our final CSV file that was ready for modeling.

## Modeling
For modeling, I used a Logistic Regression and Random Forest model for their interpretability. A grid search was performed for both models to find the best hyperparameters. Coefficients were then extracted and interpreted in relation to our target. A confusion matrix was also created along with a classification report to see what was the precision and recall.![](./assets/confusion_matrix_log_regression.jpg)
The random forests model produced the highest accuracy score but due to its lack of interpretability with its coefficients, emphasis was made to improve the logistic regression model. Since I had already grid searched the best hyperparameters the next thing was to go back and look at our data, particularly the text features, summary and user reviews.

I used spacy to be able to lemmatize our text. Both user reviews and summary columns were lemmatized, both taking a substantial amount of run time. Once that was done I then again performed a grid search for our logistic regression model. Our model score did improve with a final accuracy score of 87.11%, 37.11% better than the baseline. I also had a confidence interval of 0.8719 ± 0.0112. I also extracted our top 10 coefficients from our model which helped us identify the best features to focus on to improve the chances of a game having a score above the median average user score of 8.4. 

![](./assets/lemm_log_coefs.jpg)


## Conclusions & Recommendations
I set out to create a model that could identify the key attributes that would help a video game achieve a higher average user score. Our best model was a random forest model but due to its lack of interpretability with its coefficients, emphasis was placed on the logistic regression model. With lemmatized text, our model produced an accuracy score of 87.11% which is 37.11% better than our baseline of 50%. Our model also had coefficients and with these coefficients I identified the top 10 attributes that correlate to better chances of a video game achieving a higher average user score.

Below are the top 10 coefficients from our logistic regression model. Note that coefficients have been exponentiated for easier interpretability.
- Sum_wild
- Sum_life
- Sum_city
- Sum_combat
- Sum_return
- Sum_number
- Sum_way
- Genre_in_top_20
- Video_game_name_in_top_40
- Sum_war

8 out of the 10 coefficients have to do with the certain words used in a game summary. The words wild and life had the highest coefficients from our model, they increased the likelihood of having an above average user score by 2.54 and 2.29 times respectively. The other words used in summary increased likelihood from 1.83 times to 1.46 times going from the top down of the above list. Words that describe and emphasize a video game with vibrant life, wild settings, city scapes, varying combat, and war-like scenarios had a positive increase in a game having a higher average user score. The tokenized number also showed up as increasing likelihood by 1.53 times, this can be anywhere from descriptive summaries about the vastness of the game, potential player count, or even a certain time setting or potential sequel to a previous game. 

I also see our feature-engineered columns like genre in top 20 and video game name in top 40 increasing the likelihood of a better average user score. These features identified whether or not a review was for a game that was one of the most popular/frequent genres/video game names. A game with a genre type in our top 20 increases the likelihood of a better average user score by 1.48 times. A game that's a sequel or has some relation to a game in our top 40 list increases the likelihood of a better average user score by 1.47 times. 

Our findings show key attributes for a better average user score for video games based on the list of all-time video games from Metacritic. Like many things in pop culture, fads come and go, one way to better improve our model is to collect more data for games by certain years and see how the attributes for each year change and what stays the same. Collecting more data for both video games and user reviews is always recommended. Further steps for this project would be to streamline a process that uses a production model that can take in certain parameters for an upcoming video game and return a prediction based on the models I have made.


