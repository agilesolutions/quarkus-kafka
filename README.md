Quarkus Kafka Quickstart
========================

## before building this demo

- log on to [katacoda k8s course site](https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster)
- git clone https://github.com/agilesolutions/hibernate-panache-resteasy.git
- curl -O http://mirror.easyname.ch/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
- tar xvf apache-maven-3.6.0-bin.tar.gz
- mv apache-maven-3.6.0  /usr/local/apache-maven
- export M2_HOME=/usr/local/apache-maven
- export M2=$M2_HOME/bin
- export PATH=$M2:$PATH
- cd quarkus-kafka
- docker-compose up

This project illustrates how you can interact with Apache Kafka using MicroProfile Reactive Messaging.

## Kafka cluster

First you need a Kafka cluster. You can follow the instructions from the [Apache Kafka web site](https://kafka.apache.org/quickstart) or run `docker-compose up` if you have docker installed on your machine.

## Start the application

The application can be started using: 

```bash
mvn compile quarkus:dev
```  

Then, open your browser to `http://localhost:8080/prices.html`, and you should see a fluctuating price.

## Anatomy

In addition to the `prices.html` page, the application is composed by 3 components:

* `PriceGenerator` - a bean generating random price. They are sent to a Kafka topic
* `PriceConverter` - on the consuming side, the `PriceConverter` receives the Kafka message and convert the price.
The result is sent to an in-memory stream of data
* `PriceResource`  - the `PriceResource` retrieves the in-memory stream of data in which the converted prices are sent and send these prices to the browser using Server-Sent Events.

The interaction with Kafka is managed by MicroProfile Reactive Messaging.
The configuration is located in the application configuration.

## Running in native

You can compile the application into a native binary using:

`mvn clean package -Pnative`

and run with:

`./target/kafka-quickstart-1.0-SNAPSHOT-runner` 