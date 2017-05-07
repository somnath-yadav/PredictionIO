# PredictionIO
This repository contains predictionIO setup, model training and deployment details and code.

## Train and deploy model
- Start event server (pio everntserver &)
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
