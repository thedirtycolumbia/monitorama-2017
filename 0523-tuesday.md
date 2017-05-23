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

### I volunteer as tribute: the future of oncall
_Bridget Kromhout (Pivotal)_

Is not on-call right now. Organizes DevOpsDays. Has a podcast called Arrested DevOps.

How do we go from talking about specific [monitoring] tools to planning for failures.

"In a world that celebrates pioneers, be the settlers instead."
Maintaining things also matters. Don't YOLO things into production.
When you get back to work after the conference. Go look at that thing in your infrastructure that has always bugged you.

We have this stuff that we're trying to monitor, make available, and scale up, but that's not the only thing that matters.
Communication matters. Knowing what to do in an outage doesn't come from documentation, it comes from cross-team collaboration between dev and ops. Blaming a person or type of person for production outages doesn't help.

There's a lot of places where people get excited about getting rid of all things (e.g. Serverless and NoOps), but there are still servers and ops people somewhere. We need to find empathy and improve communcation, it's not just shipping features.

"Most of tech is tribalism and fashion." It's very easy to get caught up in hype-driven development because it's exciting. It's worth stopping to think about what problems you're solving for your organization.

A lot of ops culture is in our heads or in decoupled documentation.
Be detailed at the time you write the config file or the commit message and thank yourself for it later.

Ops: you can't just put devs into the on-call rotation and say "good luck". Automate things, document, and share across teams.
Devs: build for operability (observability, debugging, etc). Day one is really short, day two is forever.
Developers and ops engineers are seeing the same problem from different sides, neither side is necessarily "wrong".

We're not going to solve our technical problems by throwing human misery at them. Instead of volunteering as tribute for the on-call rotation, invest in your organization's operability.

Read James Turnbull's book https://artofmonitoring.com/
