Kafka
------------------------------------------
------------------------------------------

librdkafka
---------------------
https://github.com/edenhill/librdkafka

configuration variables:
---------------------
https://github.com/edenhill/librdkafka/blob/master/CONFIGURATION.md
https://github.com/edenhill/librdkafka/blob/master/INTRODUCTION.md#low-latency

commits
---------------------
https://medium.com/@luiscualquiera/c%C3%B3mo-gestionar-los-commit-en-kafka-4a9ff18763c3
https://www.logicbig.com/tutorials/misc/kafka/auto-committing-offsets.html


donet  (confluent-kafka)
---------------------
https://docs.confluent.io/current/clients/dotnet.html
https://github.com/confluentinc/confluent-kafka-dotnet/tree/master/examples
https://github.com/brianpursley/DotnetCoreKafkaExample


SlimMessageBus
---------------------
https://github.com/zarusz/SlimMessageBus
https://zarusz.github.io/SlimMessageBus/docs/provider_kafka.html


Semantics
---------------------
https://www.linkedin.com/pulse/kafka-technical-overview-sylvester-daniel/
https://www.linkedin.com/pulse/kafka-producer-overview-sylvester-daniel/
https://www.linkedin.com/pulse/kafka-producer-delivery-semantics-sylvester-daniel/
https://dzone.com/articles/exactly-once-semantics-with-apache-kafka-1
https://www.confluent.io/blog/apache-kafka-data-access-semantics-consumers-and-membership/
https://dzone.com/articles/kafka-consumer-delivery-semantics


Net.Core
---------------------
https://cinlogic.com/2020/01/10/getting-started-with-kafka-using-dotnet-core.html
https://www.c-sharpcorner.com/article/c-sharp-web-api-with-apache-kafka-integration/

Course udemy 
---------------------
https://courses.datacumulus.com/downloads/kafka-beginners-bu5/
https://github.com/simplesteph/kafka-beginners-course


Topics, Partitions and offsets
---------------------

Topics: a particular stream of data
are split in partitios
 - each partition is ordered
 - each message within a partition gets an incremental id, called offset
 - offset only have a meaning for a specific partition
 - Order is guaranteed only within a partition (no across partitions)
 - Data is kept only for a lmited time (default is one week)
 - once the data is written to a partition, it cant be changed (immutability)
 - Data is assined randomly to a partition unless a key is provided

Brokers
-----------------

 - a Kafka cluster is composed of multiple brokers (servers)
 - each broker is identified with its id (integer)
 - each broker contains certain topic partitions
 - after connecting to any broker (called a bootstrap broker), you will be connected to the entire cluster
 - a good number to get started is 3 brokers, but some big clusters have over 100 brokers

Topic Replication factor
-----------------

 - Topics should have a replication factor > 1 (usually between 2 and 3)
 - This way if a broker is down, another broker can serve the data


Leader for a Partition
-----------------

- At any time only ONE broker can be a leader for a given partition
- Only that leader can receive and serve data for a partition
- The other brokers will syncronize the data
- therefore each partition has one leader and multiple ISR (in-sync replica)

Producers
-----------------
 - Producers write data to topics (which is made of partitions)
 - Producers automatically know to which broker and partition to write to
 - In case of broker failure, Producers will automatically recover
 - Producers can choose to receive acknowledgment of data writes
    - acks=0: Producer won't wait for acknowledgement (posible data loss)
    - acks=1: Producer will wait for leader acknowledgment (limited data loss)
    - acks=all: Leader + replicas acknowledgment (no data loss)

    





