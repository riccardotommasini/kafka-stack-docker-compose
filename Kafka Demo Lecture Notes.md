
# Kafka Demo Lecture Notes

based on the guides at [https://learn.conduktor.io/kafka/](https://learn.conduktor.io/kafka/)

## Before everything

Download kafka CLI tools [here](https://kafka.apache.org/downloads)
We are using kafka version 3.

if you are on apple silicon

```export DOCKER_DEFAULT_PLATFORM=linux/arm64```

## Starting the cluster

```docker compose -f zk-single-kafka-multiple-edit.yml up -d```

Kafka and the conduktor UI will run in docker, while our consumers/producers will be in the host machine, so we will talk to the cluster via localhost.

here there is an image from Robin Moffat that can explain

![cluster config](./docker01.png)

[source](https://rmoff.net/2018/08/02/kafka-listeners-explained/)

## Creating Topics

```kafka-topics.sh --bootstrap-server localhost:9092 --topic first_topic --create --partitions 3 --replication-factor 1```

```kafka-topics.sh --bootstrap-server localhost:9092 --topic first_topic_with_keys --create --partitions 3 --replication-factor 1```

With replications

```kafka-topics.sh --bootstrap-server localhost:9092 --topic first_topic_with_replications --create --partitions 3 --replication-factor 3```

## Producing Data

```kafka-console-producer.sh --bootstrap-server localhost:9092 --topic first_topic```

sample data

```Hi
my
name
is
```

With keys
```kafka-console-producer.sh --bootstrap-server localhost:9092 --topic first_topic_with_keys --property parse.key=true --property key```

sample data

```
name:ric
last:tom
```

Show how different consumer read different partitions

With acks

```kafka-console-producer.sh --bootstrap-server localhost:9092 --topic first_topic_with_replications --property parse.key=true --property key.separator=: --request-required-acks X```

X=0 or 1 or -1

## Consuming Data

```kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first_topic --from-beginning```

### make the message prietty

```kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first_topic --formatter kafka.tools.DefaultMessageFormatter --property print.timestamp=true --property print.key=true --property print.value=true --property print.partition=true --from-beginning```

### Consumer Group

```kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first_topic --formatter kafka.tools.DefaultMessageFormatter --property print.timestamp=true --property print.key=true --property print.value=true --property print.partition=true --group first-group --from-beginning```

- show rescaling
- show relationship with number of partitions
- show assignment with Key hasing is consistent minus failure of the consumer.
