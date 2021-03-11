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
 - Producers can choose to send a Key with the message ( string, number, etc)
 - if key=null, data is sent round robin (broker 101 then 102 then 102),...
 - if a key is sent, then all messages for that key will always go to the same partition
 - a key is basically sent if you need message ordering for a specific field 

Consumers
-----------------
 - Consumers read data from a topic (identified by name)
 - Consumers know which broker to read from
 - In case of broker failure, consumers know how to recover
 - Data is read in order within each partitions

 Consumer Groups
-----------------
 - Consumers read data in consumer groups
 - Each consumer within a group reads from exclusive partitions
 - if you have more consumers than partitions, some consumers will be inactive

Consumer Offsets
-----------------
 - Kafka stores the offsets at which a consumer group has been reading
 - The offsets committed live in a Kafka topic named __consumer_offsets
 - When a consumer in a group has processed data received from Kafka, it shoud be commiting the offsets
 - If a consumer dies, it will be able to read back from where it left off thanks to the committed consumer offsets

Delivery semantics for consumers
-----------------
 - Consumers choose when to commit offsets
 - There are 3 delivery semantics:
    - At most once: 
        - offsets are commited as soon as the message is received
        - if the processing goes wrong, the message will be lost (it wont be read again)
    - At least once (usually preferred)
        - offsets are committed after the message is processed
        - if the processing goes wrong, the message will be read again
        - this can result in duplicate processing of messages, Make sure your processing is idempotent
    - Exactly once:
        - Can be archieved for kafka => Kafka workflows using kafka stream api
        - For Kafka => External system workflows, use an idempotent consumer

Kafka Broker Discovery
-----------------
 - Every Kafka broker is also called a "bootstrap server"
 - That means that you only need to connect to one broker, and you will be connected to the entire cluster
 - Each broker knows about all brokers, topics and partitions (metadata) 

 Zookeper
-----------------
 - Manages brokers (keeps a list of them)
 - helps in performing leader election for partitions
 - sends notifications to kafka in case of changes (ej: new topic, broker dies, broker comes up, delete topics, etc)
 - kafka can't work without zookeeper
 - by design operates with and odd number of servers (3,5,7)
 - has a leader (handle writes) the rest of the servers are followers (handle reads)
 - Zookeeper does not store consumer offsets with kafka > v0.10


 Kafka Guarantees
-----------------
- Messages are appended to a topic-partition in the order they are sent
- Consumers read messages in the order stored in a topic-partition
- With a replication factor of N, producers and consumers can tolerate up to N-1 brokers being down
- This is why a replication factor of 3 is a good idea:
    - Allows for one broker to be taken down for maintenance
    - Allows for another broker to be taken down unexpectedly
- As long as the number of partitions remains constant for a topic (no new partitions), the same key will always go to the same partition











