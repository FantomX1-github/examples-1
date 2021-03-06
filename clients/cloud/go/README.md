# Overview

Produce messages to and consume messages from a Kafka cluster using [Confluent Golang Client for Apache Kafka](https://github.com/confluentinc/confluent-kafka-go).

# Prerequisites

* [Confluent's Golang Client for Apache Kafka](https://github.com/confluentinc/confluent-kafka-go#getting-started) installed on your machine
* Create a local file (e.g. at `$HOME/.confluent/librdkafka.config`) with configuration parameters to connect to your Kafka cluster, which can be on your local host, [Confluent Cloud](https://www.confluent.io/confluent-cloud/?utm_source=github&utm_medium=demo&utm_campaign=ch.examples_type.community_content.clients-ccloud), or any other cluster.  Follow [these detailed instructions](https://github.com/confluentinc/configuration-templates/tree/master/README.md) to properly create this file. 
* If you are running on Confluent Cloud, you must have access to a [Confluent Cloud](https://www.confluent.io/confluent-cloud/?utm_source=github&utm_medium=demo&utm_campaign=ch.examples_type.community_content.clients-ccloud) cluster

# Example 1: Hello World!

In this example, the producer writes Kafka data to a topic in Confluent Cloud. 
Each record has a key representing a username (e.g. `alice`) and a value of a count, formatted as json (e.g. `{"Count": 0}`).
The consumer reads the same topic from Confluent Cloud and keeps a rolling sum of the counts as it processes each record.

1. Run the producer, passing in arguments for (a) the local file with configuration parameters to connect to your Confluent Cloud instance and (b) the topic name:

```bash
$ go build producer.go
$ ./producer -f $HOME/.confluent/librdkafka.config -t test1
Preparing to produce record: alice 	 {"Count": 0}
Preparing to produce record: alice 	 {"Count": 1}
Preparing to produce record: alice 	 {"Count": 2}
Preparing to produce record: alice 	 {"Count": 3}
Preparing to produce record: alice 	 {"Count": 4}
Preparing to produce record: alice 	 {"Count": 5}
Preparing to produce record: alice 	 {"Count": 6}
Preparing to produce record: alice 	 {"Count": 7}
Preparing to produce record: alice 	 {"Count": 8}
Preparing to produce record: alice 	 {"Count": 9}
Successfully produced record to topic test1 partition [0] @ offset 0
Successfully produced record to topic test1 partition [0] @ offset 1
Successfully produced record to topic test1 partition [0] @ offset 2
Successfully produced record to topic test1 partition [0] @ offset 3
Successfully produced record to topic test1 partition [0] @ offset 4
Successfully produced record to topic test1 partition [0] @ offset 5
Successfully produced record to topic test1 partition [0] @ offset 6
Successfully produced record to topic test1 partition [0] @ offset 7
Successfully produced record to topic test1 partition [0] @ offset 8
Successfully produced record to topic test1 partition [0] @ offset 9
10 messages were produced to topic test1!
```

2. Run the consumer, passing in arguments for (a) the local file with configuration parameters to connect to your Confluent Cloud instance and (b) the same topic name as used above. Verify that the consumer received all the messages:

```bash
$ go build consumer.go
$ ./consumer -f $HOME/.confluent/librdkafka.config -t test1
...
Consumed record with key alice and value {"Count":0}, and updated total count to 0
Consumed record with key alice and value {"Count":1}, and updated total count to 1
Consumed record with key alice and value {"Count":2}, and updated total count to 3
Consumed record with key alice and value {"Count":3}, and updated total count to 6
Consumed record with key alice and value {"Count":4}, and updated total count to 10
Consumed record with key alice and value {"Count":5}, and updated total count to 15
Consumed record with key alice and value {"Count":6}, and updated total count to 21
Consumed record with key alice and value {"Count":7}, and updated total count to 28
Consumed record with key alice and value {"Count":8}, and updated total count to 36
Consumed record with key alice and value {"Count":9}, and updated total count to 45
...
```
