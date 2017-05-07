# PredictionIO
This repository contains predictionIO setup, model training and deployment details and code.

## Installing Apache PredictionIO from Source Code
- Download source code (apache-predictionio-0.10.0-incubating.tar.gz)
- Install JAVA (Note: openjdk-7 not supported)
$ sudo apt-get install openjdk-8-jdk
- Build
$ tar zxvf apache-predictionio-0.11.0-incubating.tar.gz
$ cd apache-predictionio-0.11.0-incubating
$ ./make-distribution.sh
$ tar zxvf PredictionIO-0.11.0-incubating.tar.gz
- Install dependencies
$ mkdir PredictionIO-0.11.0-incubating/vendors
**Spark setup**
$ wget http://d3kbcqa49mib13.cloudfront.net/spark-1.5.1-bin-hadoop2.6.tgz
$ tar zxvfC spark-1.6.3-bin-hadoop2.6.tgz PredictionIO-0.11.0-incubating/vendors
- Install PostgreSQL (postgresql-9.2.20-1-linux-x64.run)
$ createdb pio
$ psql -c "create user pio with password 'pio'"
- Start PredictionIO and dependent services (PredictionIO-0.11.0-incubating/bin/pio eventserver &)

## Train and deploy model

- Create a new engine from engine template (Using MyClassificion)
./pio template get apache/incubator-predictionio-template-attribute-based-classifier MyClassification1
- Generate appID and EventKey 
./pio app new predictprob
- Collect event data REST API
curl -i -X POST http://localhost:7070/events.json?accessKey=aqdapQJH5EWGq32iB93YS9zsSqIHZwtDIkI2rHyf2qiSk59BpJnLgXx5S1NtrvAa \
-H "Content-Type: application/json" \
-d '{
  "event" : "$set",
  "entityType" : "user",
  "entityId" : "u0",
  "properties" : {
    "attr0" : 0,
    "attr1" : 1,
    "attr2" : 0,
    "plan" : 1
  }
  "eventTime" : "2014-11-02T09:39:45.618-08:00"
}'
- Specify app name in Myclassification1/engine.json 
- Build
./pio build
- Train
./pio train
- Deploy
./pio deploy --port 8123 (default port did not work so specifing port, this step is optional)
