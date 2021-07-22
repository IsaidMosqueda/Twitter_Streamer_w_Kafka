# Twitter_Streamer_w_Kafka
This repository holds the notebooks and code needed to stream and visualize tweets sentiment in live time using Kafka, Dash and Tweepy.

- In order to use these notebooks a developer Twitter account is needed, for more information about developers accounts refer to the twitter documentation: https://developer.twitter.com/en/apply-for-access.

- Apache Kafka is also needed for this proyect, for installing instructions refer to the following tutorial: https://kafka.apache.org/quickstart.

- Textblob is used to calculate the sentiment of tweets, it's installation can be found on the following tutorial: https://textblob.readthedocs.io/en/dev/quickstart.html.

The notebooks in this repository peform a conection to the Twitter API via Tweepy, stream tweets based on a list of topics (in this case, those topics refer to the current main cryptocurrencies: Bitcoin, Etherum and Dogecoin) and send the information recieved to a Kafka topic, then the data in the topic is proccessed wich in this case is calculate the sentiment of the tweet using the TextBlob library and ploted using a Dash live app. 

Specifically, the workflow of the proyect must be runed between the Streamer and Listener notebooks where each one peforms the following tasks:

## Streammer:
1. Define classes that read autorization keys from twitter_credentials.py, define streaming parameters and topics to stream. Inside these classes there's the option to write down in a file all the streamed data, but notice that writting data in a file may slow down the processing process and considerably, since the objective of this project is to visualize the sentimen in live time, data will not be written in a file.
2. Initialize Kafka and Zookeper topics in the current folder and define as well as configure the Topic in the server that will be used to process the streamed data. 
3. Initialize streamer and begin proccess of streaming, here the streaming time is given and during that time span the program will be quering tweets from the API(seconds).

## Listener:
1. Initialize Dash live graph.
2. subscribe a Kafka consumer to the topic where tweets are being fetched and use the data to visualize the sentiment. 
3. Define a listener function that processes the fetched tweet data in the Kafka topic. The sentiment is given by the Textblob library wich returns the sentiment from the text of the tweet, it gives values in the range [-1.0, 1.0]. The sentiment may be used as the proyect developer thinks better, in this case in order to account the person who wrote the tweet, the metric to use will be log(followers+1)*Sentiment wich gives more weight to those users with a greater following.
4. Using Python's Multiprocessing module the listener funciton is run while the live visualization is also run, using Multiproccessing allows the program to write down the sentiment metric in the list that the graph reads as the same time that the graph is reading and updating. 

The proper way of executing both notebooks in order for a fluid reading and ploting is executing all but the last cells from both notebooks, then in order to stream the tweets while ploting them at the same time, the streamer and listener cell (last one ond both notebooks) must be run at the same time, that will print the information of the tweets in the console:
![Users_sample](https://user-images.githubusercontent.com/71615610/126583221-9fdc985f-1367-46d8-8e74-6383ce881d5a.png)

and plot in a flask page loaded in a port, the sentiment metric:
![Graph_example](https://user-images.githubusercontent.com/71615610/126583297-610800c7-dc59-4eb8-84df-796b12fffdae.png)

which will be upadating in live time.
