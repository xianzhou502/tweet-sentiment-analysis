# Tweet Sentiment Analysis

## Overview

This project explores tweet sentiment classification using Natural Language Processing and supervised machine learning. The main goal is to compare binary sentiment classification with multi-class sentiment classification and examine how the inclusion of a neutral class affects model performance.

The project focuses not only on predictive accuracy, but also on how classification errors can affect the interpretation of public opinion in social media analysis. In particular, the analysis investigates whether neutral tweets should be treated as noise or as a meaningful sentiment category that reveals ambiguity in short, informal texts.

## Research Questions

This project is guided by three main questions:

1. Which classifier performs more consistently across binary and multi-class sentiment classification?
2. Which feature set offers the best balance between predictive performance and interpretability?
3. What types of tweets are most frequently misclassified, and what do these errors suggest about sentiment ambiguity?

## Dataset

The project uses the `tweet_eval` sentiment dataset, which contains tweets labelled as:

* Negative
* Neutral
* Positive

The dataset is suitable for comparing two task settings:

* **Binary classification:** positive vs negative tweets
* **Multi-class classification:** negative, neutral, and positive tweets

The exploratory analysis found that the original dataset was imbalanced, with neutral and positive tweets appearing more frequently than negative tweets. To address this, the training split was under-sampled to create a more balanced training set, while the test set was left in its original distribution to reflect realistic class imbalance.

## Exploratory Data Analysis

The exploratory analysis examined several important properties of the dataset:

* Sentiment label distribution
* Tweet word-count distribution
* VADER compound score distribution
* Neutral tweet ambiguity
* Examples of factual, weakly emotional, mixed, and context-dependent neutral tweets

The analysis showed that many tweets are short and contain limited context. VADER compound scores also overlapped across sentiment classes, suggesting that classification difficulty is not caused only by class imbalance, but also by genuine ambiguity in tweet language.

## Methodology

The project compares four classification pipelines across both binary and multi-class settings.

| Pipeline   | Classifier          | Feature Set                              | Purpose                                                 |
| ---------- | ------------------- | ---------------------------------------- | ------------------------------------------------------- |
| Pipeline 1 | Logistic Regression | Token + POS + VADER Lexicon              | Interpretable baseline model                            |
| Pipeline 2 | Logistic Regression | Token + POS + VADER Lexicon + Embeddings | Tests whether embeddings improve predictive performance |
| Pipeline 3 | Decision Tree       | Token + POS + VADER Lexicon              | Alternative interpretable classifier                    |
| Pipeline 4 | Decision Tree       | Token + POS + VADER Lexicon + Embeddings | Tests decision tree performance with extended features  |

## Feature Engineering

The project uses several NLP feature types:

### Token Features

TF-IDF unigram and bigram features were used to capture individual sentiment words and short phrases. Bigrams help capture expressions involving negation and emphasis.

### POS Features

Part-of-speech features were included to capture grammatical patterns that may support sentiment classification.

### VADER Lexicon Features

VADER was used because it is designed for social media and microblogging contexts. It captures sentiment intensity, negation, degree modifiers, and contrastive expressions.

### Embedding Features

Embedding features were added to test whether semantic similarity could improve classification when tweets express similar meanings using different words.

## Preprocessing

Preprocessing was designed to preserve important sentiment signals. The project used a customised stop-word approach that retained negation terms such as:

* `not`
* `no`
* `never`
* `n't`
* `nt`
* `ever`
* `ca`

This was important because removing negation could distort the meaning of tweets such as “not good” or “never again”.

## Model Evaluation

The models were evaluated using:

* Accuracy
* Macro-F1
* Precision
* Recall
* Class-specific performance
* Misclassification patterns

Macro-F1 was especially important because accuracy alone may hide weak performance on smaller or more ambiguous classes.

## Binary Classification Results

In the binary task, the model classified only positive and negative tweets.

| Pipeline   | Accuracy | Macro-F1 | Main Finding                                                                |
| ---------- | -------: | -------: | --------------------------------------------------------------------------- |
| Pipeline 1 |    0.821 |    0.793 | Strong interpretable baseline                                               |
| Pipeline 2 |    0.854 |    0.829 | Best binary performance                                                     |
| Pipeline 3 |    0.710 |    0.683 | Decision tree underperformed                                                |
| Pipeline 4 |    0.744 |    0.708 | Embeddings helped, but performance remained weaker than logistic regression |

Pipeline 2 performed best in the binary task. The addition of embeddings improved classification performance, especially for negative tweets. However, embedding features were harder to interpret than token, POS, and lexicon features.

## Multi-class Classification Results

In the multi-class task, the neutral class was included alongside positive and negative tweets.

| Pipeline   | Accuracy | Macro-F1 | Main Finding                                              |
| ---------- | -------: | -------: | --------------------------------------------------------- |
| Pipeline 1 |    0.615 |    0.600 | Neutral class reduced performance                         |
| Pipeline 2 |    0.633 |    0.620 | Best multi-class performance                              |
| Pipeline 3 |    0.577 |    0.547 | Decision tree struggled with negative and neutral classes |
| Pipeline 4 |    0.577 |    0.549 | Embeddings slightly improved decision tree macro-F1       |

Pipeline 2 remained the strongest model in the multi-class task, but overall performance dropped after the neutral class was included. This shows that neutral tweets introduce a more difficult classification boundary between polar and non-polar sentiment.

## Binary vs Multi-class Comparison

| Pipeline   | Binary Macro-F1 | Multi-class Macro-F1 | Change |
| ---------- | --------------: | -------------------: | -----: |
| Pipeline 1 |           0.793 |                0.600 | -0.193 |
| Pipeline 2 |           0.829 |                0.620 | -0.209 |
| Pipeline 3 |           0.683 |                0.547 | -0.136 |
| Pipeline 4 |           0.708 |                0.549 | -0.159 |

All pipelines performed worse after neutral tweets were included. However, this does not mean that binary classification is always better. Instead, the neutral class exposes important ambiguity that would be hidden if neutral tweets were removed.

## Key Findings

* Logistic regression performed more consistently than decision trees across both task settings.
* Pipeline 2, using logistic regression with token, POS, VADER lexicon, and embedding features, achieved the strongest overall performance.
* Embeddings improved predictive performance but reduced interpretability.
* Pipeline 1 was slightly weaker than Pipeline 2 but more interpretable.
* The neutral class made classification significantly harder.
* Many neutral tweets were ambiguous because they were factual, weakly emotional, mixed in sentiment, or dependent on missing context.
* Positive tweets were generally easier to classify than negative or neutral tweets.
* Error analysis showed that sentiment classification should consider the consequences of misclassification, especially when used to interpret public opinion.

## Project Files

| File                                                    | Description                                                                                              |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `Tweet Sentimnet Analysis_Report.docx`               | Final written report explaining the research question, methodology, results, discussion, and limitations |
| `Tweet Sentimnet Analysis_Exploratory Analysis.ipynb`   | Jupyter Notebook for exploratory data analysis                                                           |
| `Tweet Sentimnet Analysis_Exploratory Analysis.html` | HTML export of the exploratory analysis notebook                                                         |
| `Tweet Sentimnet Analysis_Binary Task.ipynb`         | Jupyter Notebook for binary sentiment classification                                                     |
| `Tweet Sentimnet Analysis_Binary Task.html`          | HTML export of the binary classification notebook                                                        |
| `Tweet Sentimnet Analysis_Multi-class Task.ipynb`    | Jupyter Notebook for multi-class sentiment classification                                                |
| `Tweet Sentimnet Analysis_Multi-class Task.html`     | HTML export of the multi-class classification notebook                                                   |

## Tools and Technologies

* Python
* Jupyter Notebook
* Pandas
* NumPy
* Scikit-learn
* TF-IDF
* VADER Sentiment Analysis
* POS features
* Embedding features
* Logistic Regression
* Decision Tree Classification
* Macro-F1, precision, recall, and accuracy evaluation

## Skills Demonstrated

* Natural Language Processing
* Sentiment analysis
* Text classification
* Exploratory data analysis
* Feature engineering
* Binary classification
* Multi-class classification
* Model comparison
* Error analysis
* Interpretation of ambiguous text data
* Research communication
* Data storytelling

## Limitations

This project has several limitations:

* Under-sampling balanced the training data but removed many positive and neutral examples.
* Embedding features improved performance but were difficult to interpret.
* Tweets are short and often lack context, which increases label ambiguity.
* Neutral tweets are especially difficult because they may contain factual reporting, weak sentiment, mixed sentiment, or missing conversational context.
* The models should be interpreted as analytical tools rather than perfect sentiment detectors.

## Conclusion

This project shows that tweet sentiment classification is strongly affected by task framing. Binary sentiment classification produces higher scores, but it hides the ambiguity introduced by neutral tweets. Multi-class classification is more difficult, but it provides a more realistic understanding of social media sentiment.

Overall, logistic regression with token, POS, VADER lexicon, and embedding features achieved the best predictive performance. However, the project also shows that interpretability and error analysis are essential when sentiment models are used to understand public opinion.
