---
title: 'Deep Learning LSTM for Sentiment Analysis in Tensorflow with Keras API'
excerpt: "Learn how to use LSTM for text classification in Tensorflow using Keras API"
comments: true
categories:
  - Machine-learning
tags:
  - Deep-learning
  - Artificial intelligence
  - Neural networks
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: https://miro.medium.com/max/4400/1*Gn6dE4Om0Ai-RNTP7YGmQg.jpeg
---


### **Introduction**

Sentiment analysis is the process of determining whether language reflects a positive, negative, or neutral sentiment.
Analyzing the sentiment of customers has many benefits for businesses. eg.

* A company can filter customer feedback based on sentiments to identify things they have to improve about their services.

* A company can manage their online reputation easily by monitoring the sentiment of comments customers write about their products

In this tutorial, we will build a [Deep learning](https://en.wikipedia.org/wiki/Deep_learning) model to classify text as either negative or positive.

### **Requirements**

* **Data**: The data used is a collection of tweets about a major U.S airline available on [Kaggle](https://www.kaggle.com/crowdflower/twitter-airline-sentiment).

* [Tensorflow](https://www.tensorflow.org) version 1.15.0 or higher with [Keras](https://www.tensorflow.org/api_docs/python/tf/keras) API

* [Pandas](https://pandas.pydata.org/)

* [Numpy](https://numpy.org/)

### Data Preparation

let’s see how the data looks like:

```python
import pandas as pd

df= pd.read_csv('Tweets.csv', sep=',')
df.head(10)
```

![Data preview](https://cdn-images-1.medium.com/max/3680/1*f-66q6ix-gByre1QZ2U-fg.png)

***Steps to prepare the data:***

* Select relevant columns:
The data columns needed for this project are the ***airline_sentiment*** and ***text*** columns. we are solving a classification problem so **text** will be our features and ***airline_sentiment*** will be the labels.

Machine learning models work best when inputs are numerical. we will convert all the chosen columns to their needed numerical formats.

* Transform ***airline_sentiment*** column to numerical category:

* Transform ***text*** column to a vector of numbers. (*more on this later*)

```python
    #select relavant columns
    tweet_df = df[['text','airline_sentiment']]
```

![Selected relevant columns](https://cdn-images-1.medium.com/max/2000/1*9Z0sgkVK_OoZI1Waqxb7lA.png)

We need to classify tweets as either negative or positive, so we will filter out rows with neutral sentiment.

```python
    tweet_df = tweet_df[tweet_df['airline_sentiment'] != 'neutral']
```

![Data without neutral sentiment](https://cdn-images-1.medium.com/max/2000/1*x2GyENOsZ6UrE2ENSt5mHA.png)
```python
    # convert airline_seentiment to numeric
    sentiment_label = tweet_df.airline_sentiment.factorize()
```
![](https://cdn-images-1.medium.com/max/2000/1*r7MDtV8mxqDAaAGlClT0JQ.png)

Calling the **factorize** method returns an array of numeric categories and an index of the categories. In this case, index 0 is positive and index 1 is negative sentiment respectively.


### Preparing text for NLP:

As I said earlier, Inputs to machine learning models need to be in numeric formats.
This can be achieved by the following:

* Assign a number to each word in the sentences and replace each word with their respective assigned numbers.

* Use word [embeddings](https://towardsdatascience.com/introduction-to-word-embedding-and-word2vec-652d0c2060fa). This is capable of capturing the context of a word in a sentence or document.

```python
    from tensorflow.keras.preprocessing.text import Tokenizer
    from tensorflow.keras.preprocessing.sequence import pad_sequences

    tweet = tweet_df.text.values
    tokenizer = Tokenizer(num_words=5000)
    tokenizer.fit_on_texts(tweet)

    vocab_size = len(tokenizer.word_index) + 1

    encoded_docs = tokenizer.texts_to_sequences(tweet)

    padded_sequence = pad_sequences(encoded_docs, maxlen=200)
```

From the above code:

* we get the actual texts from the data frame

* Initialize the tokenizer with a 5000 word limit. This is the number of words we would like to encode.

* we call **fit_on_texts** to create associations of words and numbers as shown in the image below.

```python
    print(tokenizer.word_index)
```

![word index](https://cdn-images-1.medium.com/max/2484/1*AFVz2GXT-HNF7KUzCDjUlw.png)

* calling **text_to_sequence** replaces the words in a sentence with their respective associated numbers. This transforms each sentence into sequences of numbers.

```python
    print(tweet[0])
    print(encoded_docs[0])
```

![A tweet and it’s encoded version](https://cdn-images-1.medium.com/max/2000/1*Ro12i0MMmfv7iEr6zGiFTQ.png)

From the above result, you can see the tweet is encoded as a sequence of numbers. eg. ***to*** and ***the*** are converted to **1** and **2** respectively. 
Check the word index above to verify.

The sentences or tweets have different number of words, therefore, the length of the sequence of numbers will be different. 
Our model requires inputs to have equal lengths, so we will have to pad the sequence to have the chosen length of inputs. This is done by calling the **pad_sequence** method with a length of 200.
All input sequences will have a length of 200.

```python
    print(padded_sequence[0])
```

![Padded Sequence.](https://cdn-images-1.medium.com/max/2000/1*ZW2I34dsu5CT-bABfElEWQ.png)

## Build Model

Now that we have the inputs processed. It's time to build the model.

```python
    # Build the model

    from tensorflow.keras.models import Sequential
    from tensorflow.keras.layers import LSTM,Dense, Dropout,
    from tensorflow.keras.layers import SpatialDropout1D
    from tensorflow.keras.layers import Embedding

    embedding_vector_length = 32

    model = Sequential()

    model.add(Embedding(vocab_size, embedding_vector_length,     
                                         input_length=200) )

    model.add(SpatialDropout1D(0.25))

    model.add(LSTM(50, dropout=0.5, recurrent_dropout=0.5))

    model.add(Dropout(0.2))

    model.add(Dense(1, activation='sigmoid'))

    model.compile(loss='binary_crossentropy',optimizer='adam', 
                               metrics=['accuracy'])

    print(model.summary())
```
![Model Summary](https://cdn-images-1.medium.com/max/2000/1*m4owvpCTfcXdqUF8NoM8qg.png)

![Model Structure](https://cdn-images-1.medium.com/max/2000/1*cWfWK1xZQB9EtDvukRGHRA.png)

This is where we get to use the **LSTM** layer. The model consists of an embedding layer, **LSTM** layer and a Dense layer which is a fully connected neural network with sigmoid as the [activation function](https://medium.com/datadriveninvestor/a-gentle-introduction-to-activation-functions-in-deep-learning-5d5402fcb033).

Dropouts are added in-between layers and also on the **LSTM** layer to avoid overfitting.

## LSTM

![source: [http://colah.github.io/posts/2015-08-Understanding-LSTMs/](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)](https://cdn-images-1.medium.com/max/3960/0*CXFcIW6nFgIlP2j4.png)
>  Long Short Term Memory networks — usually just called “LSTMs” — are a special kind of RNN, capable of learning long-term dependencies. They were introduced by [Hochreiter & Schmidhuber (1997)](http://www.bioinf.jku.at/publications/older/2604.pdf), and were refined and popularized by many people in following work.[1](http://colah.github.io/posts/2015-08-Understanding-LSTMs/#fn1) They work tremendously well on a large variety of problems, and are now widely used.
>  LSTMs are explicitly designed to avoid the long-term dependency problem. Remembering information for long periods of time is practically their default behavior, not something they struggle to learn!
>  **source**: [http://colah.github.io/posts/2015-08-Understanding-LSTMs/](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)

## Train Model

```python
    history = model.fit(padded_sequence,sentiment_label[0],
                      validation_split=0.2, epochs=5, batch_size=32)
```

![](https://cdn-images-1.medium.com/max/2192/1*2JQYyPccGG25fahc_daffA.png)

The model is trained for 5 epochs which attains a validation accuracy of ~92%.

**Note:** *Your result may vary slightly due to the stochastic nature of the model, try to run it a couple of times and you will have averagely about the same validation accuracy.*

## Testing Model

```python
    test_word ="This is soo sad"

    tw = tokenizer.texts_to_sequences([test_word])
    tw = pad_sequences(tw,maxlen=200)
    prediction = int(model.predict(tw).round().item())

    sentiment_label[1][prediction]
```

![Prediction result](https://cdn-images-1.medium.com/max/2000/1*Vb7lXWn4-C5fak4okU7KYg.png)

The model is tested with a sample text to see how it predicts sentiment and we can see that it predicted the right sentiment for the sentence.

You can run the entire notebook on Google Colab [here](https://colab.research.google.com/github/paulkarikari/LSTM-sentiment-analysis-with-tensorflow-keras-api/blob/master/Tutorial_sentiment_analysis.ipynb) or check the entire notebook on [Github](https://github.com/paulkarikari/LSTM-sentiment-analysis-with-tensorflow-keras-api/blob/master/Tutorial_sentiment_analysis.ipynb)

**Resources**

* [http://colah.github.io/posts/2015-08-Understanding-LSTMs/](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)

* [https://keras.io/examples/imdb_lstm/](https://keras.io/examples/imdb_lstm/)

* [https://keras.io/layers/recurrent/](https://keras.io/layers/recurrent/)

In this tutorial, you learned how to use Deep learning LSTM for sentiment analysis in Tensorflow with Keras API.
