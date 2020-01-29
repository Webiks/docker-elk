## Getting Started With Kafka with ELK STACK 

Config Information for KAFKA container + config beats to output to kafka
 
### Prerequisites

You will need to have the files of the kafka branch in this repo.

Inside `docker-compose.yml` Under `kafka` service 

you will find 

``
  environment:
      KAFKA_ADVERTISED_HOST_NAME: kafka
``

this settings will foward the zookeeper to connect to host named kafka.

FOR THIS TO WORK IN LOCAL ENV You need to edit the `etc/hosts` in your system

and add this line

`
127.0.0.1 kafka
`

### Config Beat to output to KAFKA 
under the beat.yml for example `metricbeat.yml`

change output to 
```
output.kafka:
  hosts: ["kafka:9092"]
  topic: beats
  partition.round_robin:
    reachable_only: false
  required_acks: 1
  compression: gzip
  max_message_bytes: 1000000
```
be sure to select topic , to a topic name you desire.

also under the logstash config you need to edit the topic name if changed.


###Logstash Input from Kafka

you can find inside `logstash\pipeline\logstash.conf`

under input kafka you will to config topics name:

for example
```
input {
  kafka {
    bootstrap_servers => "kafka:9092"
    topics => "beats"
	codec => "json"
    }
}

```
here we set the topic to be "beats", be sure that the same output in your metricbeat/beats/shipper topic
is the same in the input config in logstash.
