# moviEharmony
[www.moviEharmony.com](http://movieharmony.com)

[Slideshow](https://docs.google.com/presentation/d/1-A-qJugSwMEH5jcRqVluHKBr8kAQCnNkOQnQCe6Hrvo/pub?start=false&loop=false&delayms=3000)

[video](http://youtu.be/I7VbJ0PlI9I)

moviEharmony.com is a data platform which can finds movies
which 2 people may like to watch together.
It is completely open-source and uses the following technologies:

- Apache Kafka
- Python 
- Amazon S3
- Spark / Spark MLlib
- Apache Cassandra
- Flask

## The moviEharmony Website

![](https://github.com/patrickzheng/movieHarmony/blob/master/ref/s1.png?raw=true)

moviEharmony.com is currently batch processing (as of Oct 7, 2015) Amazon review dataset. These reviews provide the data which drive the following components of moviEharmony.com:

- _MovieSearch_: Allows 2 users to find movies that they may like to watch together.

![](https://github.com/patrickzheng/movieHarmony/blob/master/ref/s2.png?raw=true)

- _QueryUser_: Allows users to find what they have reviewed in the past

![](https://github.com/patrickzheng/movieHarmony/blob/master/ref/s3.png?raw=true)

- _EnterReview_: Allows users to add movie reviews

![](https://github.com/patrickzheng/movieHarmony/blob/master/ref/s4.png)

## moviEharmony InANutshell

![](https://github.com/patrickzheng/movieHarmony/blob/master/ref/s5.png)

This is my pipeline, the first step of this pipeline is to ingest user’s input movie review.
A webpage is created so user can submit their movie reviews from their web browser. 
These reviews will be transformed to a json message and be sent to Kafka. A batch consumer job to save these messages from Kafka to S3.
And combining these new reviews with all the historical reviews from amazon dataset, 
I can train a collaborative filtering model with my spark cluster. Spark machine learning library currently use a model based alternating least squares algorithm to learn latent factors and then use these latent factors to predict missing movie ratings for the users. The model will be saved to S3 and estimated ratings will be saved to cassandra.
At the end, flask will be querying cassandra to get movie recommendations return to the users.

