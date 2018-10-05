# Getting Started with Text Analysis in Python

If you'd like to follow along with this presentation from PyGotham 2018, please feel free to download this repo and follow these instructions.

## Installation
To install TextBlob, simply open a shell and type:
```sh
$ pip install textblob
$ python -m textblob.download_corpora
```
If you run into an issue with certificate errors on step two above and you are on a Mac, you can see [this article](http://www.cdotson.com/2017/01/sslerror-with-python-3-6-x-on-macos-sierra/) to learn more about ways to address it.

Also, if you want to be all professional about this, you probably want to learn about installing and using [virtualenv](https://virtualenv.pypa.io/en/stable/), and probably [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/). But it's really up to you -- not required to just get going here.

## Using TextBlob
Now you're ready to get started using TextBlob! Launch a python shell to get going.
```sh
$ python
```
Let's import TextBlob:
```py
from textblob import TextBlob
```
Now let's create our first TextBlob from the text of a random tweet, and we're going to call it `tweet`:
```py
tweet = TextBlob("I am very excited about the person who will be taking the place of Don McGahn as White House Councel! I liked Don, but he was NOT responsible for me not firing Bob Mueller or Jeff Sessions. So much Fake Reporting and Fake News! ")
```
Congrats! You've made your first TextBlob. Here's some stuff you can do with it:
```py
tweet.words
tweet.sentences
tweet.tags
tweet.noun_phrases
tweet.word_counts
tweet.sentiment
```

## Naive Bayes with TextBlob
To start making a Naive Bayes classifier with TextBlob, we need to start by importing the NaiveBayesClassifier from the TextBlob package:
```py
from textblob.classifiers import NaiveBayesClassifier
```
Great. Now you can make your own NaiveBayesClassifier and train it with some training data. Assuming you've downloaded the `sample.json` file in this repo and you opened your Python shell from inside that same directory, you can type these commands to open the sample file and use it to train a new classifier we're creating and calling `cl`:
```py
with open('sample.json', 'r') as training:
  cl = NaiveBayesClassifier(training, format='json')
```
That may take a second, but now you have a `cl` object that is a NaiveBayesClassifier, and it has been trained with our `sample.json` training data. You've basically built up a big probability table of which words or "features" are more closely associated with the labels in our training data. To see some information about these probabilities, type:
```py
cl.show_informative_features()
```
Now you're ready to classify some new text that the `cl` classifier object hasn't yet seen.

Let's try to classify the `tweet` TextBlob we created earlier with this command:
```py
cl.classify(tweet)
```
That's great. It should only return the name of the label (from our training `sample.json` data) that the `tweet` text is most like. But what if you want some more information, like if it was a very strong match to one of our labels, or if it's almost 50/50?

You can use the `prob_classify` method to get some probability information about how likely each of your labels is to be used, by calling the methods this way (using the same 'labels' that are contained in the training data):
```py
cl.prob_classify(tweet).prob('Trump')
cl.prob_classify(tweet).prob('Staff')
```

Now if you want to run some real tests, you can also feed in some test data in a very similar way to how we trained our classifier. Assuming you have the `test.json` file in this repo, you can load it in and test how accurate your classifier is by running:
```py
with open('test.json', 'r') as test:
  cl.accuracy(test, format='json')
```

And that's our demo! The [TextBlob docs](https://textblob.readthedocs.io/en/dev/index.html) are really great and has a lot of the information here (along with much more). 

Enjoy getting started with text analysis and classification in Python!
