# Natural Language Processing and Predictive Classification Models

This project showcases the use of Natural Language Processing (NLP) with classification models in identifying from "which subreddit a given post came from". 

---

### Notebooks in this repo:

[data_collection.ipynb](https://github.com/dgumustel/nlp_predictive_models/blob/master/data_collection.ipynb) - This notebook uses [Pushshift's](https://github.com/pushshift/api) API to retrieve posts and comments from subreddits. 
[dnd_zelda_model.ipynb]() - This notebook holds a classification model that can identify whether a post came from the r/DMAcademy subreddit or the r/truezelda subreddit. 
[poli_discussion_model.ipynb](https://github.com/dgumustel/nlp_predictive_models/blob/master/poli_discussion_model.ipynb) - This notebook holds a classification model that can predict whether a post on the r/politicaldiscussion subreddit came from the year 2012 or 2020.


### Overview

This project showcases the use of Natural Language Processing (NLP) classification models in identifying from "which subreddit a given post came from". 

---

### Problem Statement

The goal is to use NLP to train at least two models to identify 

---

### Data

Pulled 200 files total via [Pushshift's](https://github.com/pushshift/api) API. 100 came from the boardgames reddit and 100 came from the oceanography reddit. I used the created_utc times to pull in two batches of 50 from separate time periods. Then I selected just the 'subreddit' and 'selftext' columns. I used df.replace to remove special characters like \\t \\n \\r, might need to do more of this though! Then I binarized the 'subreddit' column by mapping oceanography to 1 and boardgames to 0. 

### TODO
* get MORE DATA - source for pulling data on time delay: https://gist.github.com/tecoholic/1242694
* try pulling in comments, too! try using titles alongside selftext! 

---

### Data Preprocessing

X here was the 'selftext' of the subreddit submissions, and y was the binarized 'subreddit'. I stratified on y on the train_test_split to ensure the balance between oceanography and boardgames posts was equal in the train and test data.  
Then I used CountVectorizer to transform X_train and X_test to sparse matrices. Made a couple bar plots to show that the top 10 words were stop words, then reset X_train and X_test to dataframes for pipeline and gridsearchCV fitting. 

---

### Model Selection and Tuning

Started with GridSearchCV and Bernoulli Naive Bayes model because that was the first we did in Grant's lesson 5.04 on NLP (2). Then did GridSearchCV and TFIDF multinomial Naive Bayes model. Learning to use pipelines, yay! 

---

### TODO 
* reexamine the pipe_params in both models
* try knn model! 

---

### Model Evaluation

Bernoulli Naive Bayes model had specificity of 0.5454545454545454. The confusion matrix below has true label on the y-axis and predicted label on the x-axis.

| 0 | 18 | 15 |
|---|----|----|
| 1 | 1  | 32 |
|   | 0  | 1  |

This model was great at predicting true positives (optimized for sensitivity I think?), but did not minimize false positives by far. In our context, this means the model was great at identifying oceanography posts as oceanography posts, but also misclassified many boardgame posts as oceanography posts. Based on this kind of model, why is this?



TFIDF multinomial Naive Bayes model had specificity of 0.9393939393939394. The confusion matrix below has true label on the y-axis and predicted label on the x-axis.

| 0 | 31 | 2  |
|---|----|----|
| 1 | 17 | 16 |
|   | 0  | 1  |

This model was optimized for true negatives instead (optimized for specificity for sure), and as a result had a larger number of false negatives than the BernoulliNB model. Based on this kind of model, why is this?

I expect to get higher accuracies when I add in more data! 



### TODO
* calculate more classification metrics - sensitivity, what else was there??? don't even remember

### Conclusions

---

### Next Steps


