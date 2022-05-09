# kcsyslog
Syslog with Kafka Connect Demo



## Overview

* Start with Confluent cp-all-in-one
* Create new image including syslog source connector binaries
* Modify docker-compose to use new image
* Bring up your Confluent Platform in docker-compose
* Deploy syslog source connector




## Create New Image

Create a new image to be stored locally.

The Dockerfile is sparse.
We simply add the syslog source connector.
Optionally, add the BigQuery sink connector for long-term storage of syslog messages.


docker build -t kc -f kc-with-syslog/Dockerfile .


## Modify docker-compose

First, we use our new kc image with the included connector binaries.

Second, we map the port we plan on using for syslog traffic.
In this case, 514/udp.

````
$ diff -c docker-compose.yml.dist docker-compose.yml
*** docker-compose.yml.dist     2022-05-09 05:19:00.684513033 -0500
--- docker-compose.yml  2022-05-09 05:31:45.534534799 -0500
***************
*** 54,60 ****
        SCHEMA_REGISTRY_LISTENERS: http://0.0.0.0:8081
  
    connect:
!     image: cnfldemos/cp-server-connect-datagen:0.5.3-7.1.0
      hostname: connect
      container_name: connect
      depends_on:
--- 54,61 ----
        SCHEMA_REGISTRY_LISTENERS: http://0.0.0.0:8081
  
    connect:
! #    image: cnfldemos/cp-server-connect-datagen:0.5.3-7.1.0
!     image: kc
      hostname: connect
      container_name: connect
      depends_on:
***************
*** 62,67 ****
--- 63,70 ----
        - schema-registry
      ports:
        - "8083:8083"
+ #      - "5454:5454"
+       - "514:514/udp"
      environment:
        CONNECT_BOOTSTRAP_SERVERS: 'broker:29092'
        CONNECT_REST_ADVERTISED_HOST_NAME: connect
````






## Bring It Up

````
docker-compose up
````




## Send Some Lines

