
# Problem Statement

I will use threads from the /r/college and /r/GradSchool subreddits to build a model that will help me identify the key conversations among each demographic in order to create a better schol enviornment for students

For this investigation, the /r/GradSchool thread will be our target column. 


# Data
All data was retrieved at [Reddit](https://reddit.com).
The data I sam using is from the [/r/college](https://www.reddit.com/r/college) subreddit and the [/r/GradSchool](https://www.reddit.com/r/GradSchool/) subreddit. 

Several scrapes were made during this investigation:
- [Baseline Data - College Subreddit](./data/college_oct14.csv)
- [Baseline Data - GradSchool Subreddit](./data/grad_school_oct14.csv)

- [Final Data for Model - College Subreddit](./data/college_oct17.csv)
- [Final Data for Model - GradSchool Subreddit](./data/grad_school_oct17.csv)

All of the Reddit threads for this subject only went up to the the last 950 posts. To ensure that I had as much data as possible for this investigation, I scraped all subjects available for the thread (hot, new, controversial, top, rising). 


# Baseline Model
The baseline model was created with reddit data up to October 14, 2019. The EDA that took place on this model was very basic. Scraping the data led to 3,741 threads for /r/GradSchool and 2,888 threads for /r/College making it to a total of 6,629 total threads aand a 56% / 43% split.

To make our data easier to work with, I combined the title and body columns into a column called _all_text_ since some threads were missing body content. In addition, I removed all punctuatin from the threads and created another column that calculated the word count of each column. 

Our EDA found that /r/GradSchool threds had more word count among their threads but the /r/colelge threads had more upvotes for it's word count. 

Our baseline model was ran on a Gridsearch pipeline with a Count Vectorizer Model Transformer and a Logistic Regression Model Estimator. Out of all the models ran in the Gridsearch, our best model included a minimum document frequency of 3, a maximum document frequency of 90%, a maximum features parameter of 2000, and an n-gram range of (1,1).

These features led to an accuracy score of 0.8806034482758621 . I put this data into a confusion matrix and the Baseline Model Specificity is 0.8718 and Baseline Model Sensitivity is 0.9146. So this model is pretty strong.

# Exploratory Data Analysis
EDA consisted of removing punctuation and numbers from the data. I also created a customer stop word list that included the terms: I'm, I'd, I've, grad, school, college, phd, masters, just, like dont. When searching for the most common terms in the EDA process, these words were interfering with my investigation. Because I wanted to identify the problems and most talked about conversations among the two threads, these terms made my investation hard. For example, the conversation about "masters" and "phd" is expected to show up in the /r/GradSchool threads. 

After looking at the Word Count, Number of up votes, and comments per post, we can notice that the spread participation in the grad school thread is much higher than the spread of participation in the college thread. Users in the college thread do have more points lying outside the 75th percentile compared to the Grad School student. This shows me that College students are more willing to participating in bumping or upvoting on other threads compared to putting up a thread themselves. We know that there are more grad school threads compared to college threads, so we know that grad school students particpate in posting more. With their posts, they tend to have a higher word count as well. In addition, the spread of the number of comments is slightly higher than college students. This shows that grad students take more of an acitive particpation by commenting on the threads rather than upvoting.

## Sentiment Analysis
I chose to perform sentiment analysis to understand the "attitudes' of the threads in each subreddit. This way I was able to identify the highly positive and highly negative threads. During this analysis I found that the GradSchool subreddit tend to have slightly higher rated positive posts compared to College subreddit. College subreddit has a higher IQR of negative posts but GradSchool subreddit has the highest max and mean of negative posts. The min, max, and mean compound scores are very similar between the two threads. However, the GradSchool thread seems to have IQR that leans nowards more positive posts while the college thread IQR spreads between -0.4 to 0.8.

The positive terms for college don't have much significance. We notice there is some talk about meeting new people and having frinds. Other terms include discussion about financial aid. I would be interesting to investigate the treads that include this term and see what they are actually talking about. One takeaway from the college Negative words is that there seems to be a lot of talk around financial aid and offie hours.

In the GradSchool Thread, th postive terms seem to focus on mental health and there is discussion among the users on good things that happened to themselves. THis shows me that they are going on reddit to express and shre things that are happening to them. The negative terms that appear for grad school is quite significant. Similar to their positive terms, it seems like thenegative terms are coming from users who are sharing things that are happening to themselves. There is alot of talk on mental health and it seems like grad school students are struggling with confidence in grad school


# Model / Model Evaluation

| Pipe/Grid # | Transformer   | Estimator | Stop Words | max_features | min_df | max_df | ngram_range |
|------|------| ------| ------| ------| ------| ------| ------|
| 1 | CountVectorizer | Logistic Regression | eda_words* | [3000, 4000, 5000] | [10, 25, 50] | [0.8, 0.9] | [(1,3), (1,2), (2,2) ] |
| 2 | TFi - DF | Logistic Regression | eda_words* | [1000, 1500, 1700] | [20, 25, 40] | [0.8, 0.85, 0.9] | (1,1) |
| 3 | TFi -DF | Logistic Regression | eda_words* | [1500, 1700, 2000] | [35, 37, 42] | [0.85, 0.9] | [1,1]
| param_ngram* | CountVectorizer | Logistic Regression | eda_words* | None | 1 | 1.0 | [(2,3) , (3,3)] |

I also experiement with a RandomForest model and an ADA Boost Classifer model. The Random Forest model seemed to have given me a higher sore compared to some of the other general count vectorizer models. The best accuracy score I could get from the ADA Boost Classifer model was 82.5%

eda_words* is a customer stop word list and the ENGLISH_STOP_WORDS disctionary from sklearn.feature_extraction.text library. 
param_ngram* gridsearch was made specifically to help identify the best two term and three term words for the EDA process.

In the end, our first baseline model had the best overall accuracy score. I found that using custom stop words hurt my accuracy score rather than helped it. 


# Conclusion/Recommendations 
The GradSchool students thread has the most activity when it comes to the number of posts, word counts in the post, and number of comments. Based on the sentiment analysis and the GradSchool students have more postive threads. These positive threads seem to be focused on user's experiences. They are mentioning individual experiences of them "having a good week". THe threads that were ranked the most positive and most negative seemed mentioned the term "mental health". This gives me the idea that users are seeking support for mental health. The significant average number of comments in the GradSchool thread also shows that other users are more likely to support/comment on such posts. 

# Next Steps
I would want to do further investigation on the data to create a better model. This may include makng an overall custom stop words list and not including the one from the sklearn library. I would also like to do some form of stemming or lemmatizing especailly since I noticed similar gramatical patterns among the two subreddits. 
