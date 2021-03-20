# BDA_Mahout_Tutorial
This contains the steps and scripts of building a recommendation tutorial provided by AWS

1.  Initiallize the cluster using Quick options in AWS as hown in the video part1
  Then SSH to the cluster using putty

2.  Get the MovieLens data
    wget http://files.grouplens.org/datasets/movielens/ml-1m.zip
    unzip ml-1m.zip

3.  Convert ratings.dat, trade “::” for “,”, and take only the first three columns:
    cat ml-1m/ratings.dat | sed 's/::/,/g' | cut -f1-3 -d, > ratings.csv

4.  Put ratings file into HDFS:
    hadoop fs -put ratings.csv /ratings.csv

5.  Run the recommender job: 
    mahout recommenditembased --input /ratings.csv --output recommendations --numRecommendations 10 --outputPathForSimilarityMatrix similarity-matrix --similarityClassname       SIMILARITY_COSINE

6.  Look for the results in the part-files containing the recommendations:
    hadoop fs -ls recommendations
    hadoop fs -cat recommendations/part-r-00000 | head

**Building a Service**

1.Get Twisted, and Klein and Redis modules for Python.
  sudo pip install twisted
  sudo pip install klein
  sudo pip install redis

2. Install Redis and start up the server
   wget http://download.redis.io/releases/redis-2.8.7.tar.gz
   tar xzf redis-2.8.7.tar.gz
   cd redis-2.8.7
   make
   ./src/redis-server &
   
 3. Build a web service that pulls the recommendations into Redis and responds to queries using script “hello.py”
    
 4.Start the web service.
   twistd -noy hello.py &
 
 5.Test the web service with user id “37”:
   curl localhost:8080/37
 
