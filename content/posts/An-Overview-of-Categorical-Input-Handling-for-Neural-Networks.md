---
title: An Overview of Categorical Input Handling for Neural Networks
date: '2019-01-15T23:36:22.803Z'
subtitle: >-
  A quick guide to summarize many approaches for handling categorical data (both
  low and high cardinality) when preprocessing data for neural network based
  predictors
excerpt: >-
  A quick guide to summarize many approaches for handling categorical data (both
  low and high cardinality) when preprocessing data for…
thumb_img_path: >-
  images/An-Overview-of-Categorical-Input-Handling-for-Neural-Networks/1*THwLoeqaLjU3IRE83n8TAA.jpeg
layout: post
---
![](/images/An-Overview-of-Categorical-Input-Handling-for-Neural-Networks/1*THwLoeqaLjU3IRE83n8TAA.jpeg)

<figcaption>image by <a href="https://unsplash.com/@sanwaldeen" data-href="https://unsplash.com/@sanwaldeen" class="markup--anchor markup--figure-anchor" rel="noopener" target="_blank">sanwaldeen</a></figcaption>

In the context of a coding exercise in 2018, I was asked to write a sklearn pipeline and a tensorflow estimator for a [dataset](https://www.kaggle.com/uciml/adult-census-income) that describes employees and their wages. The goal: Create a predictor to predict if someone earns more or less than 50k a year.

![](/images/An-Overview-of-Categorical-Input-Handling-for-Neural-Networks/1*9FTGrvRUbmIAS0aZwehe5A.jpeg)

One of the issues I had was the **handling of categorical values**. While a decision tree or forest has no issues with such data (they actually work really well with it), it’s a bit more tricky to handle with a NN. Of course, we all learned One-Hot-Encoding is a way to map this kind of data into a NN passable format. But I was asked to develop an exhaustive list of many ways of handling a categorical column. The column with the highest number of categories is the `native-country` with 41 unique occurring values.

Let’s assume for a minute the dataset had an additional column called “email domain” which holds domains such as “@gmail.com” “@hotmail.com” and private domains such as “@pascalbrokmeier.de”. This list may hold thousands of unique values and these values are very difficult to handle by a neural network. A good tool would encode the meaning of the categories in some meaningful way while keeping the number of dimensions relatively low.

It turns out there are a number of ways to approach this problem. In the following article I will therefore **create an overview of many ways to handle categorical data with neural networks.** I will split the list into two categories: Generic approaches that can map any column of arbitrary symbols into appropriate representations and domain specific ones. For the domain specific example I will describe ways of representing the country column in some meaningful way.

#### What is categorical data?

Before diving into ways of handling categorical data and passing it to a neural network, I want to loose a few words to describe what it is. In my own words, categorical data is a set of symbols that describe a certain higher level attribute of some object, system or entity of interest. As an example, the concepts “red”, “blue” and “green” describe the fact that some physical objects radiate a specific distribution of the electromagnetic spectrum which turns into an observation of a conscious human being that can still observe the world through the visual sense. This is a whole lot more to handle than a simple “1, 2, 3”. How would you order the colors? How would you put them into relation with each other? What is belongs to what? Which of the following four words would you assign a lower distance between each other? “Green, blue, sky, leaf”.

Categorical data *can, but doesn’t have to*, follow some sort of ordering. Often however, categorical data may be distinct or even overlapping. Sometimes possible values of a category set can have relations between each other, sometimes they have nothing to do with each other whatsoever. All of these concepts need to be kept in mind when transferring categorical input into a numeric vector space.

In the words of Wikipedia:

> a categorical variable is a variable that can take on **one of a limited**, and usually fixed number of possible values, assigning each individual or other unit of observation **to a particular group or nominal category** on the basis of some **qualitative property**.

Okay enough taking credit for other peoples work. Let’s get into it:

### Domain agnostic solutions

The following ways of encoding categorical data are agnostic to the type of categories we’re interacting with.

#### Ordinal Encoding

Let’s start with the simplest form: Assigning each possible category an integer and pass it along. This is an enormously naive way of handling the data and it usually serves no good other than to *make it work,* meaning the program won’t crash anymore. When looking at the country column, one may then expect something like this:

<script src="https://gist.github.com/pascalwhoop/ebebfafb5dfe6b5569befcb7cd828def.js"></script>

There are several downsides to this approach: `Canada != 1/2 * China` and `Vietnam != 40/39*United-States` . Any higher-level information about these countries is *lost in translation*. The results show less performance when using my [benchmark predictor network](https://github.com/pascalwhoop/categorical-demo):

    >>>> ordinal_pp = helpers.to_pipeline(("ordinalencoder", preprocessing.OrdinalEncoder()))
    >>>> ordinal_pipeline = pipeline.get_pipeline(ordinal_pp, input_dim=14)
    >>>> helpers.execute_on_pipeline(ordinal_pipeline, X_train, y_train, X_test, y_test)
    Epoch 1/3  
     - 1s - loss: 0.9855 - mean_absolute_error: 0.3605 - acc: 0.7362  
    Epoch 2/3  
     - 1s - loss: 0.4939 - mean_absolute_error: 0.3108 - acc: 0.7741  
    Epoch 3/3  
     - 1s - loss: 0.4665 - mean_absolute_error: 0.2840 - acc: 0.7970
    **0.8492152641473572**

#### One Hot Encoding

The one that I found most often to be the “recommended approach” is OHE, also called “Dummy Encoding”. It’s explained on nearly every page that pops up when searching for “categorical data neural networks”. It’s also part of sklearn and therefore very quick to apply to a dataset. The principle is simple and best shown with a bit of code:

    >>>> import helpers  
    >>>> from sklearn import preprocessing  
    >>>> import numpy as np  
    >>>> X_test, y_test = helpers.get_data(subset="test")
    >>>> ohe = preprocessing.OneHotEncoder()  
    >>>> categories = np.array(list(set(X_test['workclass'].astype(str).values))).reshape(-1,1)  
    >>>> ohe.fit(categories)
    OneHotEncoder(categorical_features=None, categories=None,  
           dtype=<class 'numpy.float64'>, handle_unknown='error',  
           n_values=None, sparse=True)
    >>>> categories
    array([['Self-emp-inc'],  
           ['Local-gov'],  
           ['Private'],  
           ['State-gov'],  
           ['Never-worked'],  
           ['Without-pay'],  
           ['Federal-gov'],  
           ['Self-emp-not-inc'],  
           ['nan']], dtype='<U16')
    >>>> ohe.transform(categories).todense()
    matrix([[0., 0., 0., 0., 1., 0., 0., 0., 0.],  
            [0., 1., 0., 0., 0., 0., 0., 0., 0.],  
            [0., 0., 0., 1., 0., 0., 0., 0., 0.],  
            [0., 0., 0., 0., 0., 0., 1., 0., 0.],  
            [0., 0., 1., 0., 0., 0., 0., 0., 0.],  
            [0., 0., 0., 0., 0., 0., 0., 1., 0.],  
            [1., 0., 0., 0., 0., 0., 0., 0., 0.],  
            [0., 0., 0., 0., 0., 1., 0., 0., 0.],  
            [0., 0., 0., 0., 0., 0., 0., 0., 1.]])

*Excuse me for not having it shared as a gist. For some reason, medium turns my pasted gist links into screenshots of the gist. Very useless*

The results are an improvement over the previous variant:

    >>> ohe_encoder_pp = helpers.to_pipeline(("ohe", preprocessing.OneHotEncoder(handle_unknown='ignore', categories=categories.get_categories())))
    >>> ohe_pipeline = pipeline.get_pipeline(ohe_encoder_pp, input_dim=112)
    >>> helpers.execute_on_pipeline(ohe_pipeline, X_train, y_train, X_test, y_test)
    Epoch 1/3  
     - 2s - loss: 0.3824 - mean_absolute_error: 0.2332 - acc: 0.8358  
    Epoch 2/3  
     - 1s - loss: 0.3601 - mean_absolute_error: 0.2117 - acc: 0.8530  
    Epoch 3/3  
     - 1s - loss: 0.3547 - mean_absolute_error: 0.2125 - acc: 0.8526
    **0.9069985244122271**

#### Embedding Categorical data

[This paper](https://arxiv.org/pdf/1604.06737v1.pdf) and [this post about it](https://www.fast.ai/2018/04/29/categorical-embeddings/) both describe how to turn tabular data into something a neural network can manage.

![](/images/An-Overview-of-Categorical-Input-Handling-for-Neural-Networks/1*9ZnLAmslQj_neKc6Opl6uw.png)

<figcaption>Source: <a href="https://tech.instacart.com/deep-learning-with-emojis-not-math-660ba1ad6cdc" data-href="https://tech.instacart.com/deep-learning-with-emojis-not-math-660ba1ad6cdc" class="markup--anchor markup--figure-anchor" rel="nofollow noopener" target="_blank">https://tech.instacart.com/deep-learning-with-emojis-not-math-660ba1ad6cdc</a></figcaption>

What this does is allow you to *pre-train train* an embedding layer for each category that you intend to convert into something a NN can consume. In the graphic above, the instacart team used an [embedding layer](https://keras.io/layers/embeddings/) to convert any of their 10 million products into a 10 dimensional embedding. The following hidden layers then only need to handle a much smaller input size. Also, these lower dimensions are then of *fixed size* which is important for building models, as the input size of the first layer needs to be set during training time and the later prediction values must also adhere to this size. The concept is very similar to [natural language processing approaches](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py) and we may even make use of the pre-trained embeddings. More on that later

What does this mean for our categorical values? Well, for each categorical column in the dataset, we’d have to create an embedding network that learns embeddings for that category. If the categorical column contains words from a pre-trained embedding vocabulary (such as a colors embedding or another subdomain), it may be beneficial to [adopt such a pretrained embedding](https://www.tensorflow.org/guide/embedding). If the vocabulary in the category is out of the ordinary, it may be better to train (or adapt) a [domain-specific embedding](http://ruder.io/word-embeddings-2017/index.html#taskanddomainspecificembeddings). This has the added benefit of much lower dimensionality. A vector space that only needs to describe colors in relation to each other may be much smaller than one that tries to embed all words in the English language.

### Domain specific solutions

#### Difference Coding

[Difference Coding](https://stats.idre.ucla.edu/spss/faq/coding-systems-for-categorical-variables-in-regression-analysis-2/) may be helpful for a categorical value that describes ordinal properties. Let’s assume there are just three forms of education: *None, School, University.* In this example, the result would be three new variables with the following values:

*   None: 0,0,0
*   School: -0.5,0.5,0
*   University: -0.25,-0.25, 0.5

Basically, the higher in the order, the more expressive are the values as more of the columns move away from a 0 value.

#### String metadata

How many words are in the category string, how long is the string, which letters occur how often? While this may be extremely noisy information, a NN with a sufficiently large dataset may be able to extract some small predictive power from such information. I remember Katie from [linear digressions](https://lineardigressions.com/) mentioning an intuition of hers once that went something along the lines of “*if you can imagine a human deriving some value out of the data, a machine learning algorithm may very well do too”.* What does that mean for our dataset? Well, a long job title may be indicative of a higher rank in a (arguably too deep) hierarchy chain, thus increasing the probability of earning a higher salary. Quick detour: [This page takes it to the extreme](http://www.bullshitjob.com/title/) (and distracts from my bad example).

#### Sub information (Edit: Or N-grams)

Let’s jump back to the hypothetical email column. A domain has a [TLD](https://en.wikipedia.org/wiki/Top-level_domain) which may hold information about the origin of the individual. It may also give us an indicator about their affiliations with non-profit organizations (.org) or if they work in an educational institution (.edu). This information may be valuable for the training of a NN. Splitting the category into sub-columns may therefore be valuable.

**Edit**: Probably the most powerful technique of all though is the usage of n-grams, specifically character level n-grams. The above paragraph already eludes to this but a the n-grams are definitely the more generic approach. Let’s say you take each email and create a set of 2 — 10-grams. Now, these n-grams can be encoded and passed to a ML algorithm. They will contain several grams such as “web.de”, “gmail.c” or “@apple.com”. Surely, one can get a good amount of information from such substrings. Also, you now have a way of passing information to ML algorithms that contains all those cases where people *don’t* have a `gmail` or `hotmail` account. People with personal domains certainly fall into a different category as `jonny95@gmail.com` (sorry Jonny).

#### Data enrichment

As noted, while many email domains such as those from Google Mail or Hotmail are very widespread over the population (and thus don’t contain a lot of information), custom domains may be a strong indicator for a persons salary, especially if those custom domains are not personal domains but belong to some organization. There is also a plethora of data extractable from the web based on the domain name. A crawler may be added to the pipeline that, for each encountered domain name, performs a HTTP call to the domain, extracts some keywords and uses them during training.

* * *

### Recap

Converting categories to something a neural network can process is a common problem but finding more than a few ways to approach the problem seems to be hard. [This,](https://stats.stackexchange.com/questions/36303/regression-model-with-categorical-values) [this,](https://stats.stackexchange.com/questions/298479/how-to-handle-large-categorical-values-in-data-frame) [this](https://stats.stackexchange.com/questions/313715/one-hot-encoding-alternatives-for-large-categorical-values) question suggest that there really aren’t many alternatives. Ultimately, any method that requires a NN or a regression to convert the categories into some vector representation requires itself a numeric input to begin with. There simply aren’t many other ways to map a set of alternative values other than simply numbering ( `OrdinalEncoder` ) or turning each possible value into its own binary dimension ( `OneHotEncoder` ). Even the embeddings take the detour of a OneHotEncoder before passing a fixed-size vector into the prediction layers.

If you know of any other ways of encoding such values, please share it with me! I’ll update the article accordingly and credit any author of course.

#### Further Reading

*   Summary of many coding systems: [https://stats.idre.ucla.edu/spss/faq/coding-systems-for-categorical-variables-in-regression-analysis-2/](https://stats.idre.ucla.edu/spss/faq/coding-systems-for-categorical-variables-in-regression-analysis-2/)
*   Autoencoders, which may also offer a way to encode the inputs [https://en.wikipedia.org/wiki/Autoencoder](https://en.wikipedia.org/wiki/Autoencoder)
*   Categorical Values Wikipedia: [https://en.wikipedia.org/wiki/Categorical\_variable#Categorical\_variables\_and\_regression](https://en.wikipedia.org/wiki/Categorical_variable#Categorical_variables_and_regression)

[**List of analyses of categorical data - Wikipedia**  
*This a list of statistical procedures which can be used for the analysis of categorical data, also known as data on the…*en.wikipedia.org](https://en.wikipedia.org/wiki/List_of_analyses_of_categorical_data "https://en.wikipedia.org/wiki/List_of_analyses_of_categorical_data")[](https://en.wikipedia.org/wiki/List_of_analyses_of_categorical_data)

[**Tutorial - What is a variational autoencoder? - Jaan Altosaar**  
*Understanding Variational Autoencoders (VAEs) from two perspectives: deep learning and graphical models.*jaan.io](https://jaan.io/what-is-variational-autoencoder-vae-tutorial/ "https://jaan.io/what-is-variational-autoencoder-vae-tutorial/")[](https://jaan.io/what-is-variational-autoencoder-vae-tutorial/)

[**On learning embeddings for categorical data using Keras**  
*(This is a breakdown and understanding of the implementation of Joe Eddy solution to Kaggle’s Safe Driver Prediction…*medium.com](https://medium.com/@satnalikamayank12/on-learning-embeddings-for-categorical-data-using-keras-165ff2773fc9 "https://medium.com/@satnalikamayank12/on-learning-embeddings-for-categorical-data-using-keras-165ff2773fc9")[](https://medium.com/@satnalikamayank12/on-learning-embeddings-for-categorical-data-using-keras-165ff2773fc9)

[**How to handle different input sizes of an NN when One-Hot-Encoding a categorical input?**  
*Thanks for contributing an answer to Data Science Stack Exchange! Some of your past answers have not been…*datascience.stackexchange.com](https://datascience.stackexchange.com/questions/43397/how-to-handle-different-input-sizes-of-an-nn-when-one-hot-encoding-a-categorical/43399#43399 "https://datascience.stackexchange.com/questions/43397/how-to-handle-different-input-sizes-of-an-nn-when-one-hot-encoding-a-categorical/43399#43399")[](https://datascience.stackexchange.com/questions/43397/how-to-handle-different-input-sizes-of-an-nn-when-one-hot-encoding-a-categorical/43399#43399)

[**Why One-Hot Encode Data in Machine Learning?**  
*Getting started in applied machine learning can be difficult, especially when working with real-world data. Often…*machinelearningmastery.com](https://machinelearningmastery.com/why-one-hot-encode-data-in-machine-learning/ "https://machinelearningmastery.com/why-one-hot-encode-data-in-machine-learning/")[](https://machinelearningmastery.com/why-one-hot-encode-data-in-machine-learning/)
