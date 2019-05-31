# Project Sentiment

Using Natural Language Processing to predict Tesla stock movement based on news article sentiment from the New York Times

## Data Gathering

News articles from The New York Times discussing Tesla were sourced for sentiment analysis.  Article information was retrieved from API requests and web scraping.

### The New York Times: API Requests

**The New York Times Developer Network: Article Search API**
https://developer.nytimes.com/docs/articlesearch-product/1/overview

The Article Search API of The New York Time was queried for articles containing the term 'tesla' between January 1, 2010 (the year that Tesla launched its IPO) and May 31, 2019.  The search term returned a total of 2,540 article hits.

The following information was requested for each article document:

* Web URL
* Snippet (Headline)
* Publication Date
* Identifier
* Lead Paragraph

### The New York Times: Web Scraping

The text of the article body was retrieved by accessing each web URL and extracting the body main body.  Of the 2,540 web URLs, 1,829 full text articles were captured.  The remaining 711 articles, generally before 2013, were not included in the dataset.

## Natural Language Processing

Natural Language Processing (NLP) was applied to each article snippet (headline), lead paragraph and article body.  Two sentiment analysis toolsets were applied to the article texts.

### VADER Sentiment Analysis

**Valence Aware Dictionary and Sentiment Reasoner (VADER)**
https://www.nltk.org/_modules/nltk/sentiment/vader.html

Based on a design to evaluate short sentences, VADER Sentiment Analysis was applied to the snippet (headline) and lead paragraph of each article.  Negative, neutral and positive percentages were recorded for each article, including a compound score.

### TextBlob Sentiment Analysis

**TextBlob**
https://textblob.readthedocs.io/en/dev/

TextBlob Sentiment Analysis was applied to the snippet (headline), lead paragraph and article body of each article.  Polarity and subjectivity scores were recorded for each article.

## Aggegating Records by Date

In order to merge with the daily closing stock price of Tesla, articles were grouped by date.  For dates with multiple articles, the mean sentiment score of all articles was aggregated for each date.  Additionally, the total number of articles retrieved on a given day was recorded to quantify news intensity.

### Final DataFrame with Sentiment Analysis by Date

The final Pandas DataFrame of sentiment analysis for The New York Times discussing Tesla contains 1,829 articles grouped on 1,032 unique days.  15 feature columns were engineered from article information using NLP:

**Daily Records Beginning January 25, 2013**
* TextBlob polarity and subjectivity: article body, lead paragraph and snippet (headline)
* VADER negative, neutral, positive and compound scores: lead paragraph and snippet (headline)
* Total article count

![NYT_API_Web_Scraping_Sentiment_DataFrame](https://user-images.githubusercontent.com/44821660/58707339-9f1d5e80-8382-11e9-86f7-b8ad4b71ee13.png)

The distribution of each sentiment feature is shown below:

![Sentiment_Feature_Distribution](https://user-images.githubusercontent.com/44821660/58707350-a8a6c680-8382-11e9-9d6b-bf106de1d654.png)

## Baseline Models

Classification models are used to predict the binary outcome of whether the stock price of Tesla moved up (1) or down(0) for that day.

#### Features
- Vader compound of snippet (continuous)
- Vader positive sentiment of snippet (continuous)
- Vader negative sentiment of snippet (continuous)
- Vader neutral sentiment of snippet (continuous)
- TextBlob article polarity (continuous)
- TextBlob article subjectivity (continuous)
- Article count for the day (continuous)
- Daily trading volumne of Tesla (continuous)
- Nasdaq movement (binary)

### Logistics Model

The model provided the following coefficients:

<img width="446" alt="Screen Shot 2019-05-31 at 9 10 47 AM" src="https://user-images.githubusercontent.com/44821660/58707842-10114600-8384-11e9-9770-575ef5311236.png">

Accuracy and F1 score:

<img width="324" alt="Screen Shot 2019-05-31 at 9 10 53 AM" src="https://user-images.githubusercontent.com/44821660/58707890-2cad7e00-8384-11e9-933c-c357f9ff3896.png">

### Gaussian Naives Baye Model

The Gaussian Naives Baye model provided the following results:

<img width="324" alt="Screen Shot 2019-05-31 at 9 10 53 AM" src="https://user-images.githubusercontent.com/44821660/58707867-1c959e80-8384-11e9-9ef9-f35cb159db79.png">

<img width="354" alt="Screen Shot 2019-05-31 at 9 11 15 AM" src="https://user-images.githubusercontent.com/44821660/58707905-3cc55d80-8384-11e9-9b15-a4cc749997a3.png">


## Final Model

Several versions of the models were tested with additions and deletions of various features. The best results were yielded by the Logistics Model with the following features:

- TextBlob article polarity (continuous)
- TextBlob article subjectivity (continuous)
- Article count for the day (continuous)
- Daily trading volumne of Tesla (continuous)
- Nasdaq movement (binary)

In this case, TextBlob sentiment analysis on article bodies proved to have better predictive power than Vader on article headlines/snippets.

### Results

<img width="468" alt="Screen Shot 2019-05-31 at 9 18 50 AM" src="https://user-images.githubusercontent.com/44821660/58708238-3388c080-8385-11e9-9910-4fe8a67ba7f5.png">

<img width="346" alt="Screen Shot 2019-05-31 at 9 18 55 AM" src="https://user-images.githubusercontent.com/44821660/58708241-3683b100-8385-11e9-9912-22dc6be1bf91.png">

<img width="325" alt="Screen Shot 2019-05-31 at 9 19 17 AM" src="https://user-images.githubusercontent.com/44821660/58708243-38e60b00-8385-11e9-922e-7cd7d5d32056.png">



