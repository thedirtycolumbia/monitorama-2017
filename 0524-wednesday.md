### UX Design and Education for Effective Monitoring Tools
_Amy Nguyen (Pinterest)_

Pinterest is the world's first visual discovery engine. They do 150K req/sec.
They use Graphite, OpenTSDB (on HBase), Kafka, Storm, Spark, ELK (migrated to SumoLogic), Zipkin, etc.

100 TB logged per day, 2.5M metrics/sec.

Why care about UX?
Not everyone should have to be an expert at interpreting monitoring data.
Help people reach conclusions faster.
You don't know what questions people want to answer with their own data.

The real goal of UX: help people work correctly, quickly, and independently.

"The fastest way to become a 10x engineer is to help 10 other engineers do their jobs better."

The user experience isn't just about tools, it also concerns your team, communication, documentation, and more.
Don't overload people with too much information.
Use your expertise to determine the most helpful default behavior.
Make it hard to to do the wrong thing.

People have a lot of feelings about using your tools, and most of those feelings are negative.
Using your tools probably means that there is something broken that they have to fix.
Do whatever it takes to make your tool faster (e.g. aggregation, in-memory storage, caching).

On the front-end: don't reload existing data if the user changes the time window.
Prevent the user from requesting the data incessantly.
Lazy-load graphs on a dashboard.

Make it easy to try things without fear.
For example, maybe they've made a lot of configuration progress and they don't want to lose all of that.
"That's how you get tab hoarders."
Make sure the back button still works. Make sure that your URLs result in a consistent view of the data.

### Automating Dashboard Displays with ASAP (As Smooth As Possible)
_Kexin Rong (Stanford InfoLab)_

Kexin is currently a PhD student working with Peter Bailis on MacroBase, a system for diagnosing anomalies.

Short-term fluctuations in a plot can obscure long-term trends. Smoothing with a moving average can help with this.

Usage studies have shown that smoothing can lead to 38% more accuracy and 44% faster responses when diagnosing problems.

In many cases, plotting raw data is often noisy and spikes tend to dominate the plot and obscure important fluctuations.

Which smoothing function should be used?
Moving averages are good. You can control the amount of smoothing by adjusting the window size.
Signal processing theory tells us that moving averages are good at removing noise.

Point-to-point variance is a way to quantify smoothness. Increase window size to reduce point-to-point variance.

We want to avoid oversmoothing. We can measure the "outlyingness" of a plot to give us a boundary and avoid oversmoothing.
Luckily there is a statistical function for this metric, the "kurtosis" of a plot measures exactly that.
Kurtosis tell us when increasing our window size is no longer good if we want to preserve long-term deviations in our plot.

Optimizations:
* Pre-aggregate according to the available display resolution.
* Do not update data at the actual arrival date, just fast enough for a human to notice.
* Only search for window sizes that correspond to high periodicity in the time series.

http://futuredata.stanford.edu/asap/ (https://github.com/stanford-futuredata/ASAP)

https://github.com/kexinrong

### Monitoring That Cares (The End of User Based Monitoring)
_François Conil_

We've all missed alerts. It usually happens when you have no backup and it goes straight to your manager.

We all take on-call seriously, the problem is coverage. The dashboard is all green but the site is down.

We monitor because people use our services and we don't want them to catch us with our pants down.

"There's three types of lies: lies, damned lies, and service status pages."

There are three types of broken:
* It's broken and you don't care.
* It's broken for them and it's not supposed to work that way (update docs).
* It's broken and it needs to be fixed (this is where you should get paged).

If you don't know what what to monitor in an application, go back six months in time and talk to the developer early in the development cycle. Don't rush to add alerts on a service before stepping back to think about what's important to the users.

Talk to the product managers and owners, they will understand how the product is used and what is important to users.

"The key to not being woken up by monitoring alerts is to fix the cause for alerts." — someone on the internet, probably.

### Consistency in Monitoring with Microservices at Lyft
_Yann Ramin (Lyft)_

Is an engineering manager at Lyft (previously held that role at Twitter).

"C is still the best programming language."

How to handle production incidents with lots of services and diverse teams.

What are we trying to solve? Scaling operational mindfulness.
Examples:
* "I didn't realize it was logging errors all weekend."
* "The Jenkins build was green so I closed my laptop and walked away."

We approach monitoring as operations. We don't think about it an end-to-end. We need to build in monitoring from the start.

How can we make monitoring easy. Are there ways to absract or modularize it?

Teams are on-call for their services. There is no operations or SRE role per se.

The goal is to make it easy to produce metrics. Lyft uses the statsd protocol.

Lyft's pipeline looks like: github.com/lyft/statsrelay -> github.com/lyft/statsite -> TSDB (Wavefront and an internal thing)

Service level aggregates (histograms) centrally, per host data is processed locally.
Everything writes at the same time (either at the top of the second or minute) so aggregates are aligned everywhere.

Lyft processes billions of events per second. That turns into only ~200K metrics/second thanks to rollups.

Instrument loggers so that you have metrics by log severity and can easily page somebody if a service logs critical.

Lyft also has Envoy (https://lyft.github.io/envoy/), a layer 7 proxy and communcation bus, which "exposes a multitude of statistics to aid in system visibility and debugging as well as distributed tracing via thirdparty providers."

Lyft also has Orca which is a Salt module that provisions remote resources a service needs during deploy.

They also have an okay Go stats library: https://github.com/lyft/gostats

Using different systems for monitoring is distracting and delays debugging.

In Lyft's central monitoring and alerting hub, dashboards map to Salt states.
Every service gets a default dashboard with base alarms.
Services define extra resources they need in a Salt pillar.

We shouldn't expect users to become experts in order to use our monitoring and alerting tools.

### Critical to Calm: Debugging Distributed Systems
_Ian Bennett (Twitter)_

Writing around 4.3 billion unique metrics per minute.

Yourkit is great for debugging JVM problems.

Methodology. How we approach problems. In general: measure, analyze, adjust, verify. Basically, do science.

Start with the easier and/or cheaper options first. Peeling the onion (start outside, work in):

    ( metrics ( tuning ( tracing and logs ( profiling (custom instrumentation or code change

When to peel
Make a single change at one layer. Repeat.
Avoid making multiple changes at once and verify you haven't changed behavior.
Remember to re-rank and prioritize.

Profiling.
CPU: samples, stack traces, lock/wait analysis
Memory: heap allocation, GC collection
Disk: I/O, latency, errors

Performance fixes should be as isolated as possible.

When the pager goes off, don't create more problems by skipping steps or guessing. Your gut can and will be wrong.

Context is helpful. Zipkin and LogLens don't have enough info. Logging needs to be bolstered.
Finagle (https://github.com/twitter/finagle) is awesome because each service is a function and filters are easy to create.

### Managing Logs with a Serverless Cloud
_Paul Fisher (Lyft)_

Many of us started with monitoring the monolith: Adding logging to an application. Tailing the logs.

As we scale, we try to stick with logs but scaling log aggreagation, storage, and search is hard.

So instead, we start implementing a stats pipeline.
Using stats for metrics, tracing for debugging and sampling, and logs for incident triage and text debugging.

Please don't log every single 200 status OK request that your service handles.

Lyft's constraints were: AWS, trying to remain vendor neutral (open source), and trying to maintain stateless infrastucture.
Lyft's infrastructure scales up and down dynamically depend on load (e.g. more on a Friday night, less on a Sunday morning).

Lyft uses Heka for log shipping. Heka streams data into Kinesis Firehose.

Lambda logging at Lyft is CPU bound. Log fanout happens from S3 to `os.tmpfile()`, and the file is decompressed, parsed into JSON into another `os.tmpfile()` and then on to ElasticSearch. The lambda also talks to SQS and DynamoDB for backfill and rate-limiting, respectively. Bundle names are written to SQS during failure. Service rates are written to a DynamoDB table.

Lyft uses Kibana for searching of the logs stored in ElasticSearch.

Running the ElasticSearch service on AWS requires some additional configuration for security:
https://aws.amazon.com/blogs/security/how-to-control-access-to-your-amazon-elasticsearch-service-domain/

Lyft uses Yelp's ElastAlert for alerting: https://github.com/Yelp/elastalert

On AWS, it's better to start with a larger ElasticSearch cluser than you need and scale down.
Lyft's original ES cluster was 10 x r3.8xlarge data nodes. That gave them about 7-10K JSON docs/second of throughput.

Use metrics where you can instead of logs. If you have to use logs, serverless / lambda architecture works pretty well.

### Distributed Tracing at Uber Scale: Creating a Treasure Map for your Monitoring Data
_Yuri Shkuro (Uber)_

Co-founded the OpenTracing initiative. Works on https://uber.github.io/jaeger/ (similar to Zipkin and Dapper)

Would you like some tracing with your monitoring?

Tracing provides features that are orthogonal to other types of observations like events, logs, and metrics.

Tracing is a good source of data when you are working on performance and latency optimization across services.
It also helps with root cause analysis, service dependency analysis, and propagates context.

If tracing is so awesome, why isn't everyone doing tracing. A show of hands in the room was probably like 5-15%.

Tracing is hard. The instrumentation required is difficult and boring.
In-process context propagation particularly hard in some languages.

In Golang and Node.js there is no such thing as thread-local storage, so you have to pass a context object explicitly.

"At Google, we require that Go programmers pass a Context parameter as the first argument to every function on the call path between incoming and outgoing requests." via https://blog.golang.org/context

OpenTracing to the rescue! Uber has 2000-3000 microservices. If you want to implement tracing in a large organization, you need to show each team that they want it. The way to do that is to sell the service dependency analysis ability it offers. The other killer feature of distributed tracing is that it allows for the propagation of arbitrary baggage across services.

Example uses for baggage are for tenancy (e.g. set an env at the top and send it all the way down to the DB) and authorization tokens.

OpenTracing: http://opentracing.io
