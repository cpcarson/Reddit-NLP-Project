# Problem Statement

________________________

Although political groups often have distinct language surrounding issues and policy, many of the topics they discuss are very similar: Trump, United States domestic policy, and how wrong the opposite political party often is. In this notebook, I explore the language of two very distinct schools of thought, conservatism and socialism, using comments from their respective Reddit cohorts by using Natural Language Processing, or NLP. The goal of this exercise is to both differentiate the overall sentiment of each subreddit and create a model that can accurately predict to which subreddit a particular comment belongs.

# Project Directory

________________________

```
project_3
|__code
|  |__01_Data Mining.ipynb
|  |__02_Basic Sentiment Analysis.ipynb
|  |__03_Amazon_Modeling.ipynb
|  |__04_Results.ipynb
|__data
|  |__data.csv
|  |__df_con.csv
|  |__df_export.csv
|  |__df_lib.csv
|__results
|  |__scores_dataframe.csv
|  |__word_coefs.csv

```
# Data Dictionary
________________________

| Column| Data Type | Description |
| --- | --- | --- |
| created_utc | int64 | Time post was created in UTC |
| --- | --- | --- |
| body | object | Body of comment |
| --- | --- | --- |
| subreddit | object | Subreddit to which comment was posted, later modified to binary classification |
| --- | --- | --- |
| author | object | Author of post |

# Executive Summary
__________________

In this project, I collect text comments from r/conservatives and r/socialism using Reddit's Pushshift API. The resulting dataframe contains 20,000 comments from each respective community, as well as their subreddit classification. In order to analyze the sentiment of the comments, I needed a dictionary of words to which I could compare the text. Such a dictionary was created by scraping three distinct sets of words. One [positive](http://www.creativeaffirmations.com/positive-words.html), one [negative](https://www.enchantedlearning.com/wordlist/negativewords.shtml), and one set of [superlatives](https://www.easypacelearning.com/all-lessons/grammar/1436-comparative-superlative-adjectives-list-from-a-to-z). I chose superlatives as a third option for classifying text data because it is an efficient way of analyzing the extremity of the language. 

Before determining the rate of appearance for each category of word, I had to tokenize and lemmatize the words, essentially splitting each word from each comment into their individual roots. For example, "has", "having", and "has" all become "have." Then, each comment is compared against the respective dictionaries and considered in relation to the entire body of words to render the positive, negative, and superlative word rates. I found that the word rates for each subreddit were essentially the same: positive words were around 2%, superlatives .12%, and negative words at 3.3%.

After analyzing the sentiment of each subreddit, I utilized an EC2 instance from Amazon Web Services in order to efficiently create and evaluate different classification models. I used an instance with 48 cores and 256 gb of RAM-- 6x and 16x more than my personal computer, respectively. This allowed me to fit over 1,000 models analyzing 40,000 comments in just over 45 minutes. From best to worse score, they are as follows: 
1. Random Forest Classifier
2. Extra Trees Classifier
3. Bagging Classifier
4. Support Vector Classifier
5. Gradient Boosting Classifier
6. Logistic Regression
7. Multinomial Naive Bayes Classifier
8. K-Nearest Neighbor Classifier

The Random Forest Classifier, while yielding the best test results with 82.3% accuracy, was considerably overfit, even after considerable hyperparameter tuning using a gridsearch. 

For the resulting analysis, I moved the model from the EC2 cloud back to my local machine. I was then able to analyze the most important features from the Random Forest Classifier in order to get an idea as to which particular language was the most distinct in predicting a comment's appropriate subreddit. The word with the highest coefficient was, not surprisingly, "Trump". A few of the more distinct words with high importance were "capitalism", "nazi", "revolution", "worker", "class", and "rich".  

Had I more time, I would have liked to continue tuning the hyperparameters of the model to attempt to combat its overfitting characteristic while also potentially rendering a better classifying test score, even though it was successful in classifying subreddit comments. Additionally, I would have better curated the dictionary of words used in sentiment comparison for a more robust analysis.