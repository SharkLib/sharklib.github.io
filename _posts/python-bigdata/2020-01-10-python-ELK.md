---
date: 2020-01-01 13:48:05
layout: post
title: Python and ELK
subtitle: Start to develop a ELK by python
description: >-
  Start to develop a ELK by python
image: >-
  /assets/img/python-training.png
optimized_image: >-
  /assets/img/python-training.png
category: Python
tags:
  - blog
  - Python
author: Ron
paginate: true
---

 
  ## Key words:PostMan python Elastic Logstash Kibana
what is the ELK Stack? "ELK" is the acronym for three open source projects: Elasticsearch, Logstash, and Kibana. Elasticsearch is a search and analytics engine. Logstash is a server‑side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and then sends it to a "stash" like Elasticsearch. Kibana lets users visualize data with charts and graphs in Elasticsearch.





Elasticsearch handles storage and provides a RESTful search and analytics endpoint.

Logstash is a server-side data processing pipeline that ingests, transforms and loads data.

Kibana lets you visualize your Elasticsearch data and navigate the Elastic Stack.


### HERE’S WHY YOU SHOULD BE USING ELK:

1 – Best in Class User Interface for Data Analysis

ELK features an elegant user interface that streamlines data analysis tasks so you can spend time exploring and reviewing data not trying to figure out how to navigate the interface. The only thing that you have to concern yourself with is the answer you are seeking.

2 – Data Visualization for the Amazing price of… Free

With ELK you can begin exploring your data right at the moment of inspiration. Kibana features out of the box visualizations, including histograms, line graphs, pie charts, sunbursts, and more. You can also choose to use Vega grammar to design custom visualizations.

3 – Extendable Source and Aggregation. Possibilities Oh my!

You can configure Logstash to accept data from a variety of sources.
Logash is essentially a streamline ETL pipeline that allows an array of options to filter and transform data to suit your needs.

### Setup your ELK



1, Downlaod [ELK] (https://www.elastic.co/)

2, Run elasticsearch:

  go to elasticsearch-version/bin/

  run elasticsearch

  if you only want to use elasticsearch on locally, nothing need to change, other wise, 

  go to elasticsearch-version/config/

  edit elasticsearch.yml.

  change #network.host: localhost to network.host: your IP address.

  change #http.port: 9200 to http:port: any port you wnat.

3, run kibana:

go to kibana-version/config/

edit kibana.yml according to step 2.

elasticsearch.url: "elastic_IP:elastic_Port"

go to kibana-version/bin/

run kibana

if you only want to use kibana on locally, nothing need to change, other wise, 

  go to elasticsearch-version/config/

  edit elasticsearch.yml.

  change #server.host: localhost: to network.host: your IP address.

  change #server.port: 5601 to server:port: any port you wnat.





It is important to run elastics before kibana.



After that, open your browser, and input localhost:9200 and localhost:5601 (base on step 2 and 3).





You will see:
```
localhost:9200

{

 "name" : "tUdd_fq",

 "cluster_name" : "elasticsearch",

 "cluster_uuid" : "TpckuXLLREGlZ1p9RuqB7g",

 "version" : {

  "number" : "6.5.4",

  "build_flavor" : "default",

  "build_type" : "tar",

  "build_hash" : "d2ef93d",

  "build_date" : "2018-12-17T21:17:40.758843Z",

  "build_snapshot" : false,

  "lucene_version" : "7.5.0",

  "minimum_wire_compatibility_version" : "5.6.0",

  "minimum_index_compatibility_version" : "5.0.0"

 },

 "tagline" : "You Know, for Search"


 localhost:5601:

 Kibana home page.

 Congurations, you ELK is running now.
```

Now it is the time to start your journey.

To do any data analysis, the most important thing is data, data, and data.

How to define your own data?



There are a lot of methods you can find from the internet, but here I will introduce PostMan and python to have your data.

### Why PostMan: it is a GUI application and easy to use.

### Why python: python is popular in data analysis.



PostMan:

Create DB

1: Create a new request:


Method: put

URL: localhost:9200/testdb

Header: 

Content-Type:application/json

Body:

select "raw" and "json"

input:
```
{

   "mappings": {

   "testtable": {

    "properties": {

    "id": {"type": "keyword" },

    "location": { "type": "geo_point"},

    "strong": { "type": "integer"},

    "time": { "type": "date"}

    }

   }

   }

  }
```
If you used DB before, you will find, the testdb is some like a db name, and the testtable is some like a table name.

2, press "Send"

3, If you find succeed message in the output, Congurations, your database is created.



Add a record:

1: Create a new request:


Method: post

URL: localhost:9200/testdb/testtable/1

Header: 

Content-Type:application/json

Body:

select "raw" and "json"

input:
```
  {

  "id":"Orchard",

  "location":{"lat":43.390987,"lon":-80.807945},

  "time":"2015-08-18",

  "strong":12

  }
```
in the URL, the number 1 means you will put the data as record 1, so you can input 2, 3,... for other.



2, press "Send"



Add records:

1: Create a new request:

Method: post

URL: localhost:9200/testdb/testtable/_bulk?pretty

Header: 
```
Content-Type:application/json
```
Body:

select "binary"  

select a json file from "Choose files"

file is like:
```
{"index":{"_id":"3"}}

{"id":"oakwhite",  "location":{"lat":43.300987,"lon":-79.817945},  "time":"2015-08-17",  "strong":20}

{"index":{"_id":"4"}}

{"id":"sixline",  "location":{"lat":43.310987,"lon":-79.807945},  "time":"2015-08-18",  "strong":10}

{"index":{"_id":"5"}}
```
2, press "Send"



List all records:

1: Create a new request:


Method: put/post

URL: http://localhost:9200/testdb/_search?pretty



2, press "Send"





For python, I will give a sample then you will know every thing:



Create DB:

import requests

```

url = "http://localhost:9200/abc"



payload = "{\n \"mappings\":{\n  \"ltem-track\":

{\n   \"properties\":\n    

 {\n    \"device_id\":{\"type\": \"integer\"},\n    

 \"Serving eNBID\": {\"type\": \"text\"},\n    

 \"location\":{\"type\": \"geo_point\"},\n    

 \"mRsrp\": {\"type\": \"integer\"},\n    \"mRsrq\": {\"type\": \"integer\"},\n    \"mBattery\": {\"type\": \"integer\"},\n    \"mMemory\": {\"type\": \"integer\"}\n   }\n  }\n }\n}"

headers = {

  'Content-Type': "application/json",

  'cache-control': "no-cache",

  'Postman-Token': "cf35767d-6a27-4e9a-964d-1cec4f02943f"

  }

```

```
response = requests.request("PUT", url, data=payload, headers=headers)



print(response.text)

Create records:

import requests

import os

import datetime

import json

import random

import time







def getCount():

  url = "http://192.168.4.227:9200/testdb/devices/_search"



  payload = ""

  headers = {

    'cache-control': "no-cache",

    'Postman-Token': "3d66f7bf-825c-4e6c-819b-2b5ad71a4b9e"

    }



  response = requests.request("GET", url, data=payload, headers=headers)



  all_data_obj = response.json()

  return all_data_obj['hits']['total']



def insertOne(lat,lng,total,iddd, num):   

  url = "http://192.168.4.227:9200/testdb
/devices/" + str(total)

  data_obj = {}

  data_obj['id'] = iddd

  data_obj['time'] = datetime.datetime.now().strftime("%Y-%m-%d")

  data_obj['strong'] = num

  pt ={}

  pt['lat'] = lat

  pt['lon'] = lng



  data_obj['location'] = pt



  headers = {

    'Content-Type': "application/json",

    'cache-control': "no-cache",

    'Postman-Token': "5903c58a-624c-46cb-98a4-fe6f90746b04"

    }



  #response = requests.request("POST", url, data  ​=json.dumps(data_obj), headers=headers)

  response = requests.post(url, headers=headers, params='', data  ​=json.dumps(data_obj))

  print(response.text)

   

pt1X = 43.390987

pt1Y = -80.807945



pt2X = 43.753992

pt2Y = -79.425480



x = (pt2X - pt1X)/100

y = (pt2Y - pt1Y)/100



for i in range(100):

  print(getCount())

  time.sleep(2)

  insertOne(pt1X+ x*i,pt1Y + y*i, getCount()+1,"iddd", random.randint(0,30))

  time.sleep(2)
``` 

 ###using data in kibana:
 1, Create kibana index.

 2, go to visualize to format you charts.

 3, add your charts to dashboard. 
 Now, you have:
 <img src="https://media.licdn.com/dms/image/C4E12AQH0TiKGK5DZuw/article-inline_image-shrink_1500_2232/0?e=1575504000&v=beta&t=9plwaAohVWTFbFFYzAVeImxW7XAutS1JMBJcQinL43I">


