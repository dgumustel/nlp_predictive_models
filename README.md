# Natural Language Processing and Predictive Classification Models

This project showcases the use of Natural Language Processing (NLP) with classification models in identifying from "which subreddit a given post came from". 

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

I used [Pushshift's](https://github.com/pushshift/api) API to full 5,000 posts from r/DMAcademy, 5,000 posts from r/truezelda, 5,000 comments from r/PoliticalDiscussion in 2012, and 5,000 comments from r/PoliticalDiscussion in 2020. 

---

### Data Cleaning and EDA

My data contained many removed, deleted, and null posts. I dropped all of these, then used regular expressions to filter out hyperlinks, digits, punctuation, and other special strings (like '&amp;). This cleaned up my data considerably. Then I identified custom stop words to use in the modeling process, plotted the distributions of number of words and characters per post for each subreddit, and performed sentiment analysis. 

Findings: r/DMAcademy had consistently wordier posts than r/truezelda, and r/DMAcademy was found to have a higher fraction of words classified as positive and negative than r/truezelda using sklearn's NLTK and `SentimentIntensityAnalyzer()`. Comment length and word count in r/PoliticalDiscussion was similar in 2012 and 2020. Positivty scores were the same for 2012 and 2020, and 2020 had a lower negativity score than 2012. 

| Score type | r/DMAcademy | r/truezelda | r/PoliticalDiscussion 2012 | r/PoliticalDiscussion 2020 |
|------------|-------------|-------------|----------------------------|----------------------------|
| Negative   | 0.079423    | 0.058906    | 0.099317                   | 0.085801                   |
| Neutral    | 0.789251    | 0.809166    | 0.775668                   | 0.791869                   |
| Positive   | 0.130916    | 0.119762    | 0.121415                   | 0.119317                   |

---

### Preprocessing and Modeling



---


### Model Evaluation

Confusion matrix from the Bernoulli Naive Bayes model had specificity of 0.5454545454545454. The confusion matrix below has true label on the y-axis and predicted label on the x-axis.

| 0 | 18 | 15 |
|---|----|----|
| 1 | 1  | 32 |
|   | 0  | 1  |

This model was great at predicting true positives (optimized for sensitivity I think?), but did not minimize false positives by far. In our context, this means the model was great at identifying oceanography posts as oceanography posts, but also misclassified many boardgame posts as oceanography posts. Based on this kind of model, why is this?



TFIDF Bernoulli Naive Bayes model had specificity of 0.9393939393939394. The confusion matrix below has true label on the y-axis and predicted label on the x-axis.

| 0 | 31 | 2  |
|---|----|----|
| 1 | 17 | 16 |
|   | 0  | 1  |

This model was optimized for true negatives instead (optimized for specificity for sure), and as a result had a larger number of false negatives than the BernoulliNB model. Based on this kind of model, why is this?

I expect to get higher accuracies when I add in more data! 


### Conclusions

---

### Avenues for Future Work
