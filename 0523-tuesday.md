### Monitoring @ CERN
_Pedro Andrade (CERN)_

CERN operates its own data centers. The main site was built in the 1970s.
After hitting power capacity problems, a new extension site has been constructed in Budapest, Hungray.

They work with data center metrics (hardware and OS), data coming from site/services, and job monitoring and data management.

Data is transported via Flume and Kafka. They run 10 different types of Flume agents (e.g. JMS/AVRO, AVRO/KAFKA, KAFKA/ESSINK).
"Kafka is the rock solid core of the transport layer." Each data source is stored in a separate topic.

Data processing jobs are implemented in Scala and orchestrated by Marathon, Mesos, etc.

Data is stored in Hadoop/HDFS (mostly for archiving metadata for DR), ES (for data discovery), InfluxDB (plots, dashboards, viz).

All data is written to HDFS by default.

Data access tools are primarily Zeppelin, Kibana, Grafana.

Kafka requires careful resource planning (topic and partition splits). Control consumer group and offsets are key.

Scala was the right choice for Spark. Keeping batch/stream code close together (to enable easy switching between the two).

"Very positive experience with Marathon"
