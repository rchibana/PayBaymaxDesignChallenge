# PayBaymaxDesignChallenge

- Our customer needs a service such as Google Analytics, that can provide a series of information based on their website customers' actions.

### System requirements

1. Handle large write volume: Billions write events per day.

2. Handle large read/query volume: Millions of merchants want to get a business insight. Read/Query patterns are time-series related metrics.

3. Provide metrics to customers with at most one hour delay.

4. Run with minimum downtime.

5. Have the ability to reprocess historical data in case of bugs in the processing logic.

## Solution

To solve the problem, I created a structure following the technology stack below:

- Microservices

- Kafka

- Spark

- HBase

### Microservices

I decided to use this kind of architecture because of the facility to build and maintain the product; moreover, it's simple to deliver a new feature quickly.

### Apache Kafka

Kafka provides some essential features to help build this architecture, like low latency, fault-tolerance, high-throughput, scalability, and persistency that will be helpful in case of data reprocessing necessity, such as bugs or even in a business requirement. 

### Apache Spark

Apache Spark helps with all the real-time processing(MapReduce) once that the data is already in memory. Though the processing speed, Apache Spark provides more useful features such as high-level analytics (supports machine learning, graph algorithms, SQL queries and streaming data along MapReduce), supports to many languages (Java, Scala, Python, R), and fault tolerance using RDD (maybe one of the most crucial feature of Apache Spark).
RDD(Resilient Distributed Dataset) idea is basically to distribute each dataset among many servers, and then it can be computed on different nodes of the cluster.

### HBase

Hbase database was selected for its qualities, such as linear scalability(if you need to store more data, the unique effort will be adding more nodes), automatic replication(data stored in HDFS), and high throughput.

## Project Architecture

![Project diagram](Real-Time%20Application.png)

## Flow

To describe the flow, I separated the flow through topics bellow:

1 - (begin) The flow starts when the client's websites trigger an event and send the data to our system.

2 - The system will validate the data entrance and send it forward to a Kafka's topic.

3 - The topic has the Apache Spark as a subscriber; thus, it will receive the data.

4 - Apache Spark will apply the MapReduce technique over the data, and send the data to HBase (time-series database).

5 - The client access the website(Front-End API) and requests either a report or chart

6 - The website will request the information to a backend API that will look for the data in the HBase

7 - Finally, after all the flow, the client will receive the chart/report requested
