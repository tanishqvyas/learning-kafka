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


