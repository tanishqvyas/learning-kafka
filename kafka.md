# What is Kafka

Kafka is just like a messaging system.

Producers send messages to kafka server. There can be multiple producers.

All consumers can consume the messages. Kafka acts as message broker


Kafka is distributed platform.

In production kafka is referred as Kafka cluster
A cluster is made up of more than 1 kafka server

Each Kafka server is referred to ass **Broker**

Kafka is fault tolerant : ability of system to continue operating even when one or more component of the setup fails.

In kafka cluster messages are replicated in multiple brokers based on replication factor.


Kafka is scalable system and can be scaled based on the load requirement by adding new brokers. We can increase the number of consumers.

We can roughly have 1Million requests per second. This kind of throughput can be expected of a kafka cluster.


# Kafka Architecture

Kafka server >> can have multiple Topics >> Each topic can have multiple partitions.

We have something called as a publisher or producer. We can publish data to the topic (the partitions of the topic)

We also have a consumer group (one or more than one groups) and each group can have more than one consumer. They consume directly from the topics.

A consumer cannot hang independently. It has to be associated with a consumer group.

## Zookeeper

It is an open source, distributed configuration synchronization service. If there is any config change then it synchronizes the cluster producers and the consumers

We need to keep a count of

1. Which messages are read by the consumer
2. Cluster information
3. Topic information

It acts as data and control plane.

![image](https://user-images.githubusercontent.com/43847978/183649558-e8c8b305-ded9-494c-a214-0ba0160d4bf9.png)


# Installation & Setup

[Video Link](https://www.youtube.com/watch?v=jY5fzVCkABg&list=PLxoOrmZMsAWxXBF8h_TPqYJNsh3x4GyO4&index=3&ab_channel=SelfTuts)

[Apache Kafka Quickstart](https://kafka.apache.org/quickstart)


For kafka the config file is `server.properties` and for zookeeper it is `zookeeper.properties`


## for kafka

Make the edit

```bash
advertised.listeners=PLAINTEXT://<ipaddress>:9092
```

Make the edit and add ip

```bash
zookerper.connect=localhost:2181
```
Start the kafka server using the command

```bash
JMX_PORT=8004 bin/kafka-server-start.sh config/server.properties
```

## For zookeeper

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```



# Installing Kafka Manager

It is a GUI to view our kafka cluster. It is a tool to manage the kafka cluster.

**Prereq** : You need to have java11 installed on your machine. Follow the instruction overe here to [install java](https://www.makeuseof.com/install-java-ubuntu/).

this link helped too [install java11](https://stackoverflow.com/questions/52504825/how-to-install-jdk-11-under-ubuntu)



[Kafka Cluster manager](https://github.com/yahoo/CMAK)

Clone this repo

then run the command

```bash
cd CMAK
```

```bash
./sbt clean dist
```

This tell the path to the zip file which can then be used to do the launches

```bash
[info] Your package is ready in /home/tanishq/.sbt/1.0/staging/9fe122a9540185ff93da/cmak/target/universal/cmak-3.0.0.6.zip
```

Go to this folder

then we notice that a `target` folder is created in the folder as per [this video](https://www.youtube.com/watch?v=AlQfpG10vAc&list=PLxoOrmZMsAWxXBF8h_TPqYJNsh3x4GyO4&index=4&ab_channel=SelfTuts)


Once you have ran

```bash
unzip cmak-3.0.0.6.zip
```

The folder `cmak-3.0.0.6` would be created

Then type

```bash
cd cmak-3.0.0.6/conf
```

Here we notice bunch of configuration files. We need to edit `application.conf`

and provide the ip address

cmak.zkhosts="<IP address of zookeeper host>:2181"

which is basically ip address of the zookeeper i.e. where the zookeeper is running.

### Starting the kafka manager

Execute this command from the unzipped folder

```bash
bin/cmak -Dconfig.file=conf/application.conf -Dhttp.port=8080
```

The kafka manager cna then be accessed at <IP>:8080

[Kafka manager](http://192.168.1.8:8080/)


# What is Kafka Topic ?

Topic is a kafka component where producers are connected to. Role of a publisher is to publish a message to the topic. Topics in kafka is multi subscriber.

Topic can be considered as a logical entity. The messages produced by the publisher is stored inside partitions.

In kafka each topic is present in each of the cluster node (aka broker)


# What is kafka topic partition ?

A kafka topic is divided into mutiple parts called as **Partition**
Partitions can be considered as a linear data structure. Just like an array.

These are also called as commit log

Messages are actually published to a topic partition.

every partitions has a partition number or Topic partition number.

Each partition has an increasing index called **offset** or ***partition offset**.

New messages are always added to the end of the partition.

Data is immutable after publishing.

In case of multi broker kafka cluster partitons for a topic are distributed across the whole cluster. 
  
 ![image](https://user-images.githubusercontent.com/43847978/183670531-ac2347ed-703c-4faf-9dc4-a63ac2186fa0.png)

Producers publish the messages to the topics of their own choice. In reality the messages are published to the topic partition.

Publisher can send the messages to any partition. there is no specific order in which this has to c=be carried out.

configuration is needed by Producer/Publisher so it can send the message

- bootstrap_server : basically ip address and the port (default 9092 port). Because we need to know about kafka server address to be able to connect

- topic : the producer needs to connect to a particular topic so we need the topic name 

- value_serializer : When we send data over the wire it should be serialized (i.e. ascii converted or simple string). A method which returns the serialized value.

We use `send` method to send the message to the topic.

**In python**

1. `pip install kafka-python`
2. `pip install Faker` to fake the data (random n stuff)



# Sending message to a particular partition of a topic

When we are defining a kafka producer we can get a partitioner method with following three parameters:

- key_bytes : basically the value of the key (of the messages)

- all_partition : total number of partitions for that topic

- available_partition : which partitions are currently available when the producer is trying to send the message to the topic


# What is Consumer in Kafka

Consumer are the kafka component that consumes message from kafka topic. Internally consumer consume message from kafka topic partition. 

**note** : Every consumer is always assigned a consumer group. It can never exist independently. If no group id is provided while creating a consumer then a random group_id is assigned.

## Configuration needed by a consumer

- topic : the consumer needs to connect to a particular topic so we need the topic name.

- bootstrap_servers : basically ip address and the port (default 9092 port). Because we need to know about kafka server address to be able to connect

- group_id : the consumer needs a group_id so they can register themselves to a consumer group. If not provided it is assigned randomly. Kafka internally creates a random group_id.

## What is Consumer Group in Kafka

Consumer group is a logical grouping of one or more consumers. It is mandatory for a consumer to register itself for a consumer group.

**note** : Consumer instances are separate process. Consumer instance of same consumer group can be running on different machines/nodes/brokers.

![image](https://user-images.githubusercontent.com/43847978/183916057-7b010aea-5d15-40c7-b239-50ad06e56d2a.png)




## Please note scenario 1

When we have

1 Topic

2 Partitions

1 Consumer group

1 Consumer

In this case the message will be published to the partitions randomly but the message will be consumed in round robin fashion from the partitions.

## Please note scenario 2

1 Topic

1 Partition

1 Consumer Group

2 Consumers

In this case

**IMP** : Same partition cannot be assigned to multiple consumer in same group. If we have 2 consumers in same group then one consumer will be assigned to one partition and other consumer will be assigned to other partition or will sit idle if there is only one partition.

## Please note scenario 3

1 Topic

2 Partitions

1 consumer Group

2 Consumers

In this case the message will be published to the partitions randomly. The consumers will be assigned to separate partitions.



# Replication factor in Kafka

Replication factor is the number of replicas that are maintained for each partition.

Each partition has a replication factor. And is replicated across multiple server for fault tolerance.

Only one partition will be active at a time and is called as Leader.

The other partitions will only replicate messages and are called followers.

The leader handles all the read and write requests while the followers passively replicates the leader.



**I INSTALLED KAFKA IN**

```bash
tanishq@MAVIS:/opt/kafka_2.13-3.2.1$ sudo bin/zookeeper-server-start.sh config/zookeeper.properties
```

```bash
tanishq@MAVIS:/opt/kafka_2.13-3.2.1$ sudo JMX_PORT=8004 bin/kafka-server-start.sh config/server.properties
```

```bash
tanishq@MAVIS:~/.sbt/1.0/staging/9fe122a9540185ff93da/cmak/target/universal/cmak-3.0.0.6$ 
```