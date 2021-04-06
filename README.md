# Natural Language Processing and Predictive Classification Models

This project showcases the use of Natural Language Processing (NLP) with classification models from the [scikit-learn library](https://scikit-learn.org/stable/index.html) in identifying which subreddit a given post came from. 

This project is currently under development! - 04/03/21

---

### Project Goals

1. Use Pushshift's API to collect posts and comments from subreddits
2. Train and compare two models that can predict which subreddit a post came from - uses 2+ subreddits
3. Train and compare two models that can predict which year a post came from - uses 2+ time periods of one subreddit

---

### Notebooks in this repo:

I separated each piece of my process for this project into a different notebook:

[01_data_collection.ipynb](https://github.com/dgumustel/nlp_predictive_models/blob/master/notebooks/01_data_collection.ipynb) - This notebook uses [Pushshift's](https://github.com/pushshift/api) API to retrieve posts and comments from subreddits. 

[02_text_cleaning.ipynb](https://github.com/dgumustel/nlp_predictive_models/blob/master/notebooks/02_text_cleaning.ipynb) - Drop any posts that are removed, deleted, or null, and use regular expressions to filter out unwanted strings and characters.

[03_EDA.ipynb](https://github.com/dgumustel/nlp_predictive_models/blob/master/notebooks/03_EDA.ipynb) - Identify custom stop words, plot distributions of number of words and characters per post for each subreddit, and perform sentiment analysis.

[04_dnd_zelda_model.ipynb](https://github.com/dgumustel/nlp_predictive_models/blob/master/notebooks/04_dnd_zelda_model.ipynb) - Train a Bernoulli Naive Bayes and an SVM classifier to predict whether a post came from r/DMAcademy or r/truezelda.

[05_political_disc_model.ipynb](https://github.com/dgumustel/nlp_predictive_models/blob/master/notebooks/05_political_disc_model.ipynb) - Train a Bernoulli Naive Bayes and an SVM classifier to predict which year a post came from on r/PolitlcalDiscussion.

---

### Data Collection

I used [Pushshift's](https://github.com/pushshift/api) API to pull 5,000 posts from r/DMAcademy, 5,000 posts from r/truezelda, 5,000 comments from r/PoliticalDiscussion in 2012, and 5,000 comments from r/PoliticalDiscussion in 2020. 

---

### Data Cleaning and EDA

My data contained many removed, deleted, and null posts. I dropped all of these, then used regular expressions to filter out hyperlinks, digits, punctuation, and other special strings. This cleaned up my data considerably. Then I identified custom stop words to use in the modeling process, plotted the distributions of number of words and characters per post for each subreddit, and performed sentiment analysis. 

Findings: r/DMAcademy had consistently wordier posts than r/truezelda, and r/DMAcademy was found to have a higher fraction of words classified as positive and negative than r/truezelda using sklearn's NLTK and `SentimentIntensityAnalyzer()`. Comment length and word count in r/PoliticalDiscussion was similar in 2012 and 2020. Positivty scores were about the same for 2012 and 2020, and 2020 had a lower negativity score than 2012. 

| Score type | r/DMAcademy | r/truezelda | r/PoliticalDiscussion 2012 | r/PoliticalDiscussion 2020 |
|------------|-------------|-------------|----------------------------|----------------------------|
| Negative   | 0.079423    | 0.058906    | 0.099317                   | 0.085801                   |
| Neutral    | 0.789251    | 0.809166    | 0.775668                   | 0.791869                   |
| Positive   | 0.130916    | 0.119762    | 0.121415                   | 0.119317                   |

---

### Preprocessing and Modeling

I chose to use sklearn's [Bernoulli Naive Bayes](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.BernoulliNB.html) and [SVM classifier](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html) as the two models for this project. For preprocessing, I ran the BernoulliNB model twice, first with [CountVectorizer()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html), and then with [TfidfVectorizer()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html). I wanted to compare the impact that each vectorizer had on predictions, but each of them performed exceptionally with a very small difference in the number of misclassifications. The TF-IDF vectorizer produced the smallest number of misclassifications, so I selected that vectorizer to use with my SVM classifier. These models performed very well (>98% accuracy on unseen data) with minimal hyperparameter tuning. 

---

### Model Evaluation

#### Predicting which subreddit a post came from

The confusion matrices below have true label on the y-axis and predicted label on the x-axis.


Below are the predictions from the `BernoulliNB` classifier with `CountVectorizer()`

|             | r/DMAcademy | r/truezelda |
|-------------|-------------|-------------|
| r/DMAcademy | 1222        | 0           |
| r/truezelda | 15          | 1013        |

This model had an accuracy of 99.33%! The high precision and recall scores also reflect how few misclassifications this model made.

Below are the prediction from the `BernoulliNB` with `TfidfVectorizer()`. 


|             | r/DMAcademy | r/truezelda |
|-------------|-------------|-------------|
| r/DMAcademy | 1220        | 2           |
| r/truezelda | 7           | 1021        |

With the TF-IDF vectorizer, the BernoulliNB model made even fewer errors. 

Below are the predictions from the `SVC` with `TfidfVectorizer()`. 


|             | r/DMAcademy | r/truezelda |
|-------------|-------------|-------------|
| r/DMAcademy | 1218        | 4           |
| r/truezelda | 11          | 1017        |

This model performs almost as well as the BernoulliNB model with the TF-IDF vectorizer. 



#### Predicting which year a post on r/PoliticalDiscussion came from

I need to finish tuning hyperparameters for these models, expect results soon.


### Conclusions

With a large amount of data, thorough cleaning, and custom stop words, the binary classification of subreddit posts can be really effective! Each model created here performed exceptionally well, with train and test accuracies above 98% for each one. The Bernoulli Naive Bayes model with the TF-IDF vectorizer performed the best, though the differences in misclassifications between it and the SVM classifier were extremely minimal. The subreddits I chose for this project were quite distinct, so I'd be curious to see how these models perform with subreddits that are less distinguied from one another. I'd also like to see how well different classification models would do on this problem, like a logistic regression classifier or KNN. 

---
