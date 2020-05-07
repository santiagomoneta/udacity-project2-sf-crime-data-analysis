
# Udacity streaming data Nanodegree project: SF Crime Analisys

## Project's goal / Summary
To create an hourly based, Crime type analisys with realtime events (San Francisco Police calls) using Kafka to produce these events and Spark Streaming to aggreate, filter, process and present the data.
 
## Diagram

 JSON -> KAFKA -> SPARK STREAMING

## How to run
**Once you have all the tasks completed,  follow steps to run this project from the *Udacity's workspace.***

- Start the Zookeeper service: 
`/usr/bin/zookeeper-server-start config/zookeeper.properties`

Screenshot:
![Started zookeeper service](https://i.imgur.com/SGe7wpU.jpg)
- Start the Kafka service: 
	`/usr/bin/kafka-server-start config/server.properties`

Screenshot:
![Kafka service started](https://i.imgur.com/LIxbMXa.jpg)
- Start the Kafka producer: 
`python kafka-server.py`

Screenshoot:
![Events been produced into kafka topic](https://i.imgur.com/1XO2bPY.jpg)
- Confirm events are been sent: 
`kafka-console-consumer --bootstrap-server localhost:9092 --topic police.service.calls —from-beginning`

Screenshot:
![messages been consumed from kafka topic](https://i.imgur.com/1LmCco9.jpg)
- Run the Spark streaming script(*):
`spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.4 --master local[*] data_stream.py`  

Screenshot:
![Once a batch is completed the results are posted and a new batch start](https://i.imgur.com/1Nxe7Kn.jpg)
(*) The data will be process and the result table will be displayed in the terminal, you may want to CTRL-C here since the terminal have a limited capacity. You can leave it running and see how the diferent batches show updated numbers.

During this process you will also see the **progress reporter**:
![enter image description here](https://i.imgur.com/wegttEC.jpg)

Batch 1 vs Batch 15
![enter image description here](https://i.imgur.com/YMY5pSu.jpg)

SparkUI: On the lower right section of the workspace screen, click on "PREVIEW" to access **SparkUI**

Screenshot:
![enter image description here](https://i.imgur.com/zQnBixW.jpg)
## Questions
1.  **How did changing values on the SparkSession property parameters affect the throughput and latency of the data?**

This is directly attached to the overall througput performance, for example:
- The parameter **maxRatePerPartition** Indicates thatthe maximum number of messages per partition, per batch that can be loaded.
- The parameter **maxOffsetsPerTrigger** limit the number of records to fetch per trigger (not to be confused with *max.poll.records*)
    
2.  **What were the 2-3 most efficient SparkSession property key/value pairs? Through testing multiple variations on values, how can you tell these were the most optimal?**

To ensure the values I was playing around were the most optimal I used the value of  `processedRowsPerSecond`  obtaining  `341.8551515158866`

The params I found more relevant:

spark.sql.shuffle.partitions                
spark.streaming.kafka.maxRatePerPartition   