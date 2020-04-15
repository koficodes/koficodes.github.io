---
title: 'Build, Train and Deploy Tensorflow Deep Learning Models on Amazon SageMaker: A Complete Workflow Guide'
excerpt: "Deploy ML/Deep learning models easily on Amazon Sagemaker."
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
  overlay_image: https://cdn-images-1.medium.com/max/2000/0*pPRpm1VjoMktq1nR.jpeg
---


## Introduction

Machine learning(ML) projects mostly follow a workflow that involves generating example data, training a model and deploying the model.
These steps have subtasks and are iterative.
More often ML engineers and data scientists need an environment where they can experiment and prototype ideas faster.
After prototyping, deploying and scaling machine learning models is also a mystery that is known to few.

It will be ideal and convenient if without any tiresome setup ML engineers and data scientists can easily go from experimentation or prototyping to deploying production-ready and scalable ML models. This is where **Amazon SageMaker** comes in.

![machine-learning workflow](https://cdn-images-1.medium.com/max/2000/0*eH3exQLyFHE8Swdp.png)

## What is Amazon SageMaker:

Sagemaker was built to provide a platform to support the development and deployment of machine learning models.
Quoting from the official website:
>  Amazon SageMaker is a fully managed service that provides every developer and data scientist with the ability to build, train, and deploy machine learning (ML) models quickly. SageMaker removes the heavy lifting from each step of the machine learning process to make it easier to develop high quality models.
>  Traditional ML development is a complex, expensive, iterative process made even harder because there are no integrated tools for the entire machine learning workflow. You need to stitch together tools and workflows, which is time-consuming and error-prone. SageMaker solves this challenge by providing all of the components used for machine learning in a single toolset so models get to production faster with much less effort and at lower cost.
*source: [https://aws.amazon.com/sagemaker/](https://aws.amazon.com/sagemaker/)*

## **Features of Amazon SageMaker:**

* Sagemaker Provides customizable Amazon ML instances with developer-friendly notebook environment preloaded with ML frameworks and libraries.

* Seamless integration with AWS storage services such as( s3, RDS DynamoDB, Redshift, etc) for analysis.

* SageMaker provides 15+ most commonly used [ML algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html) and also supports building custom algorithms.

![SageMaker features](https://cdn-images-1.medium.com/max/2000/1*Jgyk2EiCrwtl2FSzCPNs6w.png)

## How SageMaker Works

![](https://cdn-images-1.medium.com/max/2000/0*gXfeKtFap01dpxVk.png)

To train models on sagemaker , you will have to create a training job by specifying the path to your training data on s3, the training script or built-in algorithm and the EC2 container for training.

After training, the Model artifacts are uploaded to s3. From this artifact, a model can be created and deployed on EC2 containers with endpoint configuration for prediction or inference.

## What we will build

In this tutorial, we will build an ML model to predict the sentiment of a text.
The details of processing the data and building the model are well explained in my previous [tutorial](https://medium.com/datadriveninvestor/deep-learning-lstm-for-sentiment-analysis-in-tensorflow-with-keras-api-92e62cde7626). We will focus on training and deploying the model on Amazon Sagemaker. 
Optionally, I accompanied this tutorial with a [complete notebook](https://github.com/paulkarikari/sentiment_analysis_deployment/blob/master/sentiment_analysis_tensorflow_keras_LSTM.ipynb) to upload in your Sagemaker notebook instance to run alongside this tutorial if you want.

We are building a custom model and it’s much more convenient to use the [sagemaker python SDK](https://sagemaker.readthedocs.io/en/stable/index.html) for training and deploying the model.
Same tasks can be accomplished by using the web UI of sagemaker, mostly when using built-in algorithms.

### Steps:

* Step 1: Create an Amazon S3 Bucket

* Step 2: Create an Amazon SageMaker Notebook Instance

* Step 3: Create a Jupyter Notebook

* Step 4: Download, Explore, and Transform the Training Data (refer to the previous [tutorial](https://medium.com/datadriveninvestor/deep-learning-lstm-for-sentiment-analysis-in-tensorflow-with-keras-api-92e62cde7626))

* Step 5: Train a Model

* Step 6: Deploy the Model to Amazon SageMaker

* Step 7: Validate the Model

* Step 8: Integrating Amazon SageMaker Endpoints into Internet-facing Applications

* Step 9: Clean Up

### Create an Amazon s3 Bucket:

First, we create an s3 bucket. This is where we will store the training data and also where the model artifacts will be saved later.
Create a bucket called tensorflow_sentiment_analysis

### Create an Amazon SageMaker Notebook Instance:

Go to Sagemaker in the AWS console on the left panel click on *Notebook instance (1) *and then click on create *Notebook instance (2)*.

![](https://cdn-images-1.medium.com/max/3472/1*cG8NIywcetTbjk0l9HCMbQ.png)

on the next page enter the name of the notebook, any name of your choice will work. You can leave the rest as default for the purpose of this tutorial. After that click on create notebook instance

![Creating a notebook instance](https://cdn-images-1.medium.com/max/2000/1*EqTCgsP8fbg_gg07m4vgZw.png)

The notebook will be created and the status will be pending for a short period of time and then will switch to InService. At this stage, you can click on open Jupyter or Open Jupyter lab. The difference between the two are differences in UI.
I prefer to use Jupyter lab because it has file explorer and supports multiple tabs for opened files and also feels more like an IDE

![Pending status](https://cdn-images-1.medium.com/max/3280/1*U5sP3Hq5HHFNkMRFUeYCDA.png)

![InService status](https://cdn-images-1.medium.com/max/2768/1*luUaJwx-zMUONyL1_8DXbA.png)

### Download, Explore, and Transform the Training Data

Download the [dataset](https://www.kaggle.com/crowdflower/twitter-airline-sentiment) and upload it to your notebook instance. Refer to this [tutorial](https://medium.com/datadriveninvestor/deep-learning-lstm-for-sentiment-analysis-in-tensorflow-with-keras-api-92e62cde7626) for the explanation of the exploration and transformation of data.

The data is transformed and saved into s3.

![](https://cdn-images-1.medium.com/max/2000/1*-iMWq64MDPbe3w28dGoCxg.png)

Before you can use the sagemaker SDK API you have to create a session,
then you call the upload_data with the name of the data and key prefix which is the path of the s3 bucket.
This returns the complete s3 path of the data file. You can query to verify as shown above.

### Training the Model

To Train a TensorFlow model you have to use TensorFlow estimator from the sagemaker SDK

![TensorFlow estimator](https://cdn-images-1.medium.com/max/2000/1*Fnbw0g2aBwHda-E7HcWuMg.png)

***entry_point:*** This is the script for defining and training your model. This script will be run in a container. (more on this later)

***role:*** The role assigned to the running notebook. you get that by running the coderole = sagemaker.get_execution_role()

***train_instance_count***: The number of container instances to spin up for training the model.

***train_instance_type:*** The [instance type](https://aws.amazon.com/sagemaker/pricing/instance-types/) of container to be used for training the model.

***framwork_version:*** TensorFlow version used in the training script. you get that by running tf_version = tf.__version__

***py_version*:** Python version used.

***script_mode*:** If set to True the estimator will use the Script Mode containers (default: False). This will be ignored if py_version is set to ‘py3’.
This allows for running arbitrary script code in a container.

***hyperparameters***: The are parameters needed to run the training script.

Now that you know what each parameter means, let’s understand the content of the training script.
```python
  %%writefile train.py

  import argparse
  import os
  import tensorflow as tf
  from tensorflow.keras.preprocessing.text import Tokenizer
  from tensorflow.keras.preprocessing.sequence import pad_sequences
  from tensorflow.keras.models import Sequential
  from tensorflow.keras.layers import LSTM,Dense
  from tensorflow.keras.layers import Embedding, Dropout
  import pandas as pd

  if __name__ == '__main__':
     
    parser = argparse.ArgumentParser()

    # hyperparameters sent by the client are passed as command-line    arguments to the script.

    parser.add_argument(‘--pochs’, type=int, default=10)
    parser.add_argument(‘--batch-size’, type=int, default=100)
    parser.add_argument(’--learning-rate’, type=float, default=0.1)

    parser.add_argument(‘--gpu-count’, type=int, 
                                      default=os.environ['SM_NUM_GPUS'])

    # input data and model directories
     parser.add_argument(‘--model-dir’, type=str, 
                                     default=os.environ['SM_MODEL_DIR'])

     parser.add_argument(‘--train’, type=str, 
                                default=os.environ['SM_CHANNEL_TRAIN'])

     args, _ = parser.parse_known_args()
     
     epochs = args.epochs
     lr = args.learning_rate
     batch_size = args.batch_size
     gpu_count = args.gpu_count
     model_dir = args.model_dir
     training_dir = args.train
     
     training_data = pd.read_csv(training_dir+’/train.csv’,sep=’,’)
     tweet = training_data.text.values
     labels = training_data.airline_sentiment.values
     
     num_of_words = 5000
     token = Tokenizer(num_words=num_of_words)
     token.fit_on_texts(tweet)
     
     vocab_size = len(token.word_index) + 1 # 1 is added due to 0 index
     
     tweet_sequence = token.texts_to_sequences(tweet)
     
     max_len = 200
     padded_tweet_sequence = pad_sequences(tweet_sequence, 
                                                     maxlen=max_len)
     
     # Build the model
     embedding_vector_length = 32
     model = Sequential() 
     model.add(Embedding(vocab_size, embedding_vector_length,   
                                            input_length=max_len))
     model.add(Dropout(0.2))
     model.add(LSTM(100)) 
     model.add(Dropout(0.2))
     model.add(Dense(1, activation=’sigmoid’)) 
     model.compile(loss=’binary_crossentropy’,optimizer=’adam’,
                                               metrics=[‘accuracy’]) 
     
     model.fit(padded_tweet_sequence,labels,validation_split=0.3,    
                        epochs=epochs, batch_size=batch_size, verbose=2)
     
     tf.saved_model.simple_save(
     tf.keras.backend.get_session(),
     os.path.join(model_dir, ‘1’),
     inputs={‘inputs’: model.input},
     outputs={t.name: t for t in model.outputs})
```

The first line is a command to write the content of the cell to a file train.py.

Because SageMaker imports your training script, you should put your training code in a main guard `(if __name__=='__main__':) `so that SageMaker does not inadvertently run your training code at the wrong point in execution.

All hyperparameters are passed to the script as command-line arguments.
The training script also gets access to environment variables in the training container instance. Such as the following

* **SM_MODEL_DIR**: A string that represents the path where the training job writes the model artifacts to. After training, artifacts in this directory are uploaded to S3 for model hosting.

* **SM_NUM_GPUS**: An integer representing the number of GPUs available to the host.

* **SM_CHANNEL_XXXX**: A string that represents the path to the directory that contains the input data for the specified channel. For example, if you specify two input channels in the Tensorflow estimator’s fit call, named ‘train’ and ‘test’, the environment variables **SM_CHANNEL_TRAIN** and **SM_CHANNEL_TEST** are set.

The midsection of the script is the usual model definition and training.
The last part of the saves the model artifacts to the s3 path provided. Take note of how the path is created by appending a numeric to it.

To start training call the fit method and pass the training data path to it to start training. This creates a training job on sagemaker. You can check the training jobs section to see the job created.

![Start Training](https://cdn-images-1.medium.com/max/2000/1*mgs7iiVBzGxe6XAhkzcIyA.png)

![Training Job in progress](https://cdn-images-1.medium.com/max/3838/1*BNhyQdBA9eFBvGY9ETx_fA.png)

If everything goes well you should see the output below at the last section of the output logs.

![](https://cdn-images-1.medium.com/max/2494/1*EDdEkvMRDAJJNCi0gmzqAQ.png)

### Deploy the Model to Amazon SageMaker

To deploy we call the deploy method on the estimator by passing the following parameters.

***initial_instance_count:*** The initial number of inference instance to lunch.
This can be scaled up if the request load increases.

***instance_type:*** The instance type for the inference container.

***endpoint_name:*** A unique name for the model endpoint.

![](https://cdn-images-1.medium.com/max/2000/1*Ip00HazJKdG8V5_LeT-OHA.png)

### Validating the Model

After calling the deploy method, the endpoint for the model is returned and this can be used to validate the model by using test data as shown below.

![](https://cdn-images-1.medium.com/max/2106/1*kdSGr6FcXXnf512Yr1FSMQ.png)

![](https://cdn-images-1.medium.com/max/2380/1*1IpefzCURpeOPdKJtDbeWA.png)

### Integrating Amazon SageMaker Endpoints into Internet-facing Applications.

The end use of ML models is for applications to send requests to it for inference/prediction. This can be accomplished you using API gateway and lambda function.

![architecture for application integration](https://cdn-images-1.medium.com/max/2000/1*4tal3jzl3C459XnAb37q3Q.png)

Applications will make requests to the API endpoint, this will trigger a lambda function, the lambda function will preprocess the data to what the input model expects. ie convert text input to numeric representation and then send this to the model for prediction. 
The prediction result is received by the lambda function which is then returned to the API gateway to be sent to the users.

### Clean Up

Make sure to call end_point.delete_endpoint()to delete the model endpoint.
After go ahead and delete any files uploaded by sagemaker from your s3 bucket.

## Conclusion

In this tutorial, you learned how to train and deploy deep learning models on Amazon Sagemaker.
Here is a [link](https://github.com/paulkarikari/sentiment_analysis_deployment/blob/master/sentiment_analysis_tensorflow_keras_LSTM.ipynb) to the complete Notebook.

## Resources

* [https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html)

* [https://www.tensorflow.org/tfx/serving/serving_basic](https://www.tensorflow.org/tfx/serving/serving_basic)

* [https://sagemaker.readthedocs.io/en/stable/index.html#](https://sagemaker.readthedocs.io/en/stable/index.html#)

* [https://medium.com/r/?url=https%3A%2F%2Fwww.kaggle.com%2Fcrowdflower%2Ftwitter-airline-sentiment](https://www.kaggle.com/crowdflower/twitter-airline-sentiment)

* [https://medium.com/datadriveninvestor/deep-learning-lstm-for-sentiment-analysis-in-tensorflow-with-keras-api-92e62cde7626](https://medium.com/datadriveninvestor/deep-learning-lstm-for-sentiment-analysis-in-tensorflow-with-keras-api-92e62cde7626)
