# Sentiment Analysis for MySejahtera App

## Background
MySejahtera is a mobile application developed by the Government of Malaysia to facilitate contact tracing efforts in response to the COVID-19 pandemic in Malaysia. It was initially released soon after the pandemic began, on April 16, 2020, and has gone through various major updates prior to its stable release on 23 July, 2022. As this was a large-scale tracking app used daily by every Malaysian resident, inevitably there were a multitude of problems faced by users. MySejahtera implemented a helpdesk function for FAQs and to contact their team for further assistance. Nonetheless, the app is currently rated 3.9 stars on the Google Play Store, which shows that there is room for improvement. This project focuses on Google Play Store reviews from Mysejahtera users from the first release until current day to analyze customer feedback and suggest potential improvements.

### Business Objectives
• To detect and understand customer feelings<br>
• To determine viable improvements based on customer feedback

## Determining Data Mining Goals
Data Mining Goal
Graded Sentiment Analysis model

### Success Criteria
The model built is evaluated to have an optimum accuracy and recall


## Data Collection
Users can rate and write reviews for MySejahtera's app on GooglePlay with a star rating and thumbs-up system. The ratings are on a 5-point scale, with 1 being the lowest rate score and 5 being the highest rate score. The goal of the project is to predict the review written is classified as positive or negative sentiment based on textual data. Our group will scrape real user reviews on GooglePlay using GooglePlay Scraper.

Google-Play-Scraper provides an API to crawl through Google Play. We used pip install google-play-scraper to install the package and scraped users' reviews and rating scores on MySejahtera's app.

The reviews were collected in batches, according to their scores (1-5). This was done in an attempt to achieve a balanced dataset with roughly the same number of reviews for each score. Also, in order to gather reviews that had more text and were written recently, we set up the google play scraper to scrape from both review types, 'Most relevant' and 'Newest'.

## Data Cleaning and Preprocessing
1. Remove duplicated reviews
2. Remove reviews that do not have any meaningful words
3. Remove reviews that are non-English or gibberish
4. Remove HTML tags
5. Remove stopwords
6. Use NLTK to remove stopwords
7. Use NLTK to stem words to root form


## Exploratory Data Analysis
1. Timeframe of the Review Written
2. Number of Thumbs Up Received
3. Number of Meaningful Words
4. Wordclouds: Most Frequently Used Words
4. Barplots (Uni-grams & Bi-grams)

## Modelling
We explored both classical machine learning and deep learning (LSTM) for sentiment analysis. The production model will be selected based on accuracy and recall on the validation set.

We conducted two experiments as following:

1. Experiment 1
- Positive: 3-star, 4-star, 5-star
- Negative: 1-star, 2-star
- Test set: 20%, Validation set: 30%, Training set: 50%

2. Experiment 2
- Positive: 4-star, 5-star
- Negative: 1-star, 2-star, 3-star
- Test set: 20%, Validation set: 30%, Training set: 50%

### Classical Machine Learning
For each of the experiement, we used the Bag of Words (BoW) representation to extract features from the text. This will be done through vectorization, specifically the CountVectorizer and TF-IDF Vectorizer. The CountVectorizer simply tokenizes and counts the word occurrences in our corpus. While on the other hand, TF-IDF tells us which words are important to one document, relative to all other documents. Words that occur often in one document but don't occur in many documents contain more predictive power.

After vectorizing, we will fit a Logistic Regression, K-NN, Decision Tree,Support Vector Machine Random Forest, XGBoost and Multinomial Naive Bayes on the training data and evaluate the models' performance on the validation set.

### Deep Learning - Long Short-Term Memory (LSTM)
Long Short-Term Memory (LSTM) is a special kind of Recurrent Neural Network (RNN) capable of learning order dependence in sequence prediction problems. We used our own corpus embedding to build a deep network as pre-trained word embeddings like Work2vec and FastText to outperform the model.

### Topic Modelling for Negative Sentiment
Next, we used Gensim's LDA to identify the key pain points that dissatisfied customers are facing.

Before running LDA, we first performed a simple pre-processing by removing stopwords, lemmatizing and using SpaCy to only keep tokens that are nouns, adjectives, verbs or adverbs. This approach gave us more distinct topics than if we were to just rely on the same pre-processing approach as that used during classification modeling


## Evaluation and Interpretation
Assessment of Data Mining Results
When reviewing the business sucess criteria, the project was able to collect 10000 rows of data. As for the pain points, the results of the topic modelling revealed that the primary pain points were:

a) Battery drain
The main concerns were internet and bluetooth connection. For Wi-Fi, GPS itself does not require an internet connection. However, the information about infectious diseases in an area is sent via the app and requires an internet connection. Additionally, from 26 December 2021, the Health Ministry had introduced a new feature in MySejahtera called ‘MySJ Trace‘ which was essentially a new-and-improved way to contact-trace. This feature required the use of Bluetooth and Location Tracking at all times. Many consumers believe that both these scenarios cause phone battery to drain quicker.

Despite common belief, it has been found that keeping a device’s Bluetooth on without connecting to another device only consumes about 1.8% more power than if it was turned off. For scale, this allows for around 10 – 15 mins of extra time for a device that typically gives five hours of screen-on time. If WiFi is not actively being used it uses no power. It scans for networks every 15 seconds when the phone is not asleep, but that's just a receiver, and uses no measurable power (less than 1 mw). This pain point can potentially be overcome by releasing a breakdown of MySejahtera's average battery usage to consumers.

b) COVID vaccine status update problems
Some users had trouble with updating their vaccine statuses after getting vaccinated. Uninstalling and reinstalling the app did not fix the issue. MySejahtera was a necessity to be able to enter premises and keep the citizens safe. Thus, not being able to access vaccination records was a understandably huge concern to users.

One way to overcome this would be by having the personnel at the vaccination centers be trained in updating the vaccination status of users manually. This would also reduce the number of complaints that are directed towards the HelpDesk team to allow them time and resources to deal with other user issues.

c) Sign-in problems
Several consumers reported sign-in issues, primarily that the password reset option does not work as expected. This is either due to not receiving a reset link or the link leading to the login page instead of the password reset page. Many of these reviews were responded to with an auto-reply that requested the user to reset their password or register new ID at https://mysejahtera.malaysia.gov.my/register/. Users further reported that this solution did not work for them.

Both the responses on the MySejahtera HelpDesk and the Google PlayStore review section lead to the same solution that did not help the consumers. As such, the developers need to create a better architecture for the way passwords are managed. There is also an option to have the MySJ ID be connected to the user's IC instead of their phone numbers/e-mails which could be changed anytime. Additionally, mutiple different solutions should be made available for the users to try based on their specific situation/problem.


### Approved Model
As the performance of most machine learning models performed poorer in Experiment 1 than in Experiment 2, an overview of the models' performance in only Experiment 2 are shown below.

<br>

|                                                            	| Accuracy on Training Set 	| Accuracy on Validation Set| Recall on Validation Set     	| 
|:-------------------------------------------------------------	|:-------------------------:|:-------------------------:|:----------------------------:	|
| Count Vectorizer & Linear Regression                       	| 0.81285                  	| 0.76426                   | 0.77046                      	| 
| Count Vectorizer & K Nearest Neighbor                        	| 0.75763                  	| 0.64839                   | 0.73675                      	| 
| Count Vectorizer & Decision Tree                             	| 0.97345                  	| 0.67669                   | 0.74745                      	| 
| Count Vectorizer & Support Vector Machine                    	| 0.85745                  	| 0.73375                   | 0.76629                      	| 
| Count Vectorizer & Random Forest                             	| 0.97345                  	| 0.76205                   | 0.77155                      	| 
| Count Vectorizer & XGBoost                                   	| 0.88426                  	| 0.75763                   | 0.76145                      	|
| Count Vectorizer & Multinomial Naive Bayes                   	| 0.76905                  	| 0.74613                   | 0.79774                      	| 
| TF-IDF Vectorizer & Logistic Regression                      	| 0.75259                  	| 0.73994                   | 0.71137                      	| 
| TF-IDF Vectorizer & K Nearest Neighbor                        | 0.81364                  	| 0.66077                   | 0.65116                      	| 
| TF-IDF Vectorizer & Decision Tree                            	| 0.97345                  	| 0.68819                   | 0.74796                      	| 
| TF-IDF Vectorizer & Support Vector Machine                   	| 0.82559                  	| 0.75409                   | 0.76340                     	| 
| TF-IDF Vectorizer & Random Forest                            	| 0.97345                  	| 0.75542                   | 0.76723                      	| 
| TF-IDF Vectorizer & XGBoost                                  	| 0.91426                  	| 0.75409                   | 0.76085                      	| 
| TF-IDF Vectorizer & Multinomial Naive Bayes                  	| 0.78975                  	| 0.76471                   | 0.77251                      	|                   	|

<br>

|                                                            	| Accuracy on Test Set 	| Recall on Test Set    	| 
|:-------------------------------------------------------------	|:-------------------------:|:-------------------------:|
| **Production Model:** TF-IDF Vectorizer & Multinomial Naive Bayes       | 0.7752                 	| 0.8607                 | 
<br>


The TF-IDF Vectorizer & Multinomial Naive Bayes was chosen as our production model as it achieved the highest accuracy and recall on the validation set. The model attained an accuracy of 0.76471 on the validation data and a recall of 0.77251. When scored on the test set, the production model attained an accuracy of 0.7752 and recall of 0.8607. 


## Deployment
Multinomial Naive Bayes has been selected as our production model in this project. In the deployment stage, we deployed our model to an Web API. To do this, we chose Gradio as our deployment library. Gradio is a free and open-source Python library that allows us to develop an easy-to-use customizable component demo for our machine learning model that anyone can use anywhere.

This Web API will prompt an input from user. User are allow to key in their review just like giving review on the Google Play Store. Once the submit button is clicked, the sentence/input will send to our model. After the model predict the result for the input, the result will be shown at output tab by telling the users that "Positive Sentiment Detected" or Negative Sentiment Detected".

Gradio is a free library and it allow us to generate a public URL that last for 72 hours. In future, to host the Web API permanently, 'Hugging Face'(a community and data science platform that provides hosting for Gradio) can be consider. It has various of pricing starts from 0.6 USD. This will allow us to host our model Web API permanently and also recorded all the users input.

### Review
Future improvement can be made on the project such as increasing the data size, increasing stopwords and meaningful words and decreasing the confuse reviews that will cause false positive and negative. These improvement will increase the accuracy of our model. 'Neutral' result can consider to be add for those review that are not positive and negative. Going forward, hosting and dedicated database will be necessary as the scope of the project expands.
