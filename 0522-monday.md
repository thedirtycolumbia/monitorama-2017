# Monday

### The Tidyverse and the Future of the Monitoring Toolchain
_John Rauser (Snapchat)_

The data science toolchain is spectacular, while the monitoring toolchain is not.

R is a programming language, but also a larger statistical computing environment.

"The base R library has what I call PHP disease"

R is nearly as difficult as it used to be, and that's largely thanks to Hadley Wickham.

http://tidyverse.org

Development on these packages started in 2005, but it's really taken off in the past 3 years.

The tidyverse is already transforming data science. The _ideas_ in tidyverse are going to trasform data manipulation and visualization.

The ggplot2 package offers a DSL for describing graphics hosted in R. This DSL was based on the Grammar of Graphics.

ggplot2 is complete, compact, and expressive. You can go from an idea to an image in a handful of code faster than any other language.

A data frame is a flexible, uniform, table-like structure similar to a database table or a spreadsheet.

A data frame is like a Lego in that it enables composition. It is the common interface (I/O format) between most tools in R.

Data frames can be nested within each other to produce things like a data frame of timestamps mapped to histogram objects.

R has a pipe operator (via the magrittr package) that emulates the Linux command-line pipe operator.

The combination of dplyr and the pipe operator lets you create a readable composition of data manipulation operations.

R, like Lisp, has strong metaprogramming features that allow you to design programs from the bottom-up (Paul Graham).

### Martyrs on Film: Learning to hate the #oncallselfie
_Alice Goldfuss (GitHub)_

A talk about how Twitter saved me from on-call.

Being on-call brings with it a sense of duty and responbility. 

It hones your troubeshooting skills, forces you to see the weak parts of your system, and teaches what isn't ready for production.

It also creates a sense of community. Alice created the #oncallselfie to document her on-call experiences.

In early December of 2016 PagerDuty added an integration into its mobile app for taking on-call selfies.

In March of 2017, the great Amazon S3 outage caused a massive peak in #oncallseflie posts on Twitter.

People on the internet started reacting to Alice's on-call selfies and questioned how often she was paged.

Ops has a military culture (e.g. "fighting fires", "having a go bag", "teling war stories").

GitLab recently posted a live stream of a major outage related to their databases and backups.

We feel heroic when we respond to emergencies and help resolve them, but "actions scenes stop the plot" and "pages stop the plot of your career".

When companies hire they are more excited about what you have built and not about all of the pages you answered.

If you are often bumping thresholds, snoozing pages, and adding delays to pages those are symptoms of band-aids.

Visibility is key to resolving "Centralia infrastructure" (hidden fires). Not just system visibility, but team visibilty — people above you need to know that things are broken.

Actionable alerts = something breaks + customers notice + I am the best person to fix it + and it must be fixed now.

Put your developers on-call. The right tool for the job. The people who caused the problem are the best people to fix it.

There are a number of large companies that have recently audited their on-call experience included Etsy, Heroku, and GitHub.

"I'm at GitHub now and my team hasn't been paged in months."

The on-call selfie has built a community. "If it wasn't for the on-call selfie, I would've burned out before I even knew I was on fire."

### Monitoring in the Enterprise
_Bryan Liles (Capital One)_

Monitoring Things At Your Day Job: A Concise Guide

1. Pick a tool
2. Monitor things
3. Bicker about choices
4. GOTO step 1

"Nothing in the enterprise is concise."

How does your enterprise monitoring team know what to monitor?
Do they have access to tools?
Do they know who to notify?
Do they know what is important?
How do they know when changes happen?

Whatever they do won't be good enough. So you do it yourself.

What should you alert on? Who should you alert? What tools should you use?
So many good ones. In an enterprise why not just run all of them since cost is not an issue?

How do you monitor your monitoring tools?

In the enterprise you are a small cog in a big machine competing with everyone for everyone else's attention.

A service level indicator (SLI) is a measurement of some part of your service (e.g. error rate).
A service level objective (SLO) is a target value for a service measured by an SLI.
A service level agreement (SLA) is what you promise your customers you'll deliver.

"If you create a log, use a structured log." Logs are useless if they aren't aggregated. Your logs should tell a story.

Metrics are one or more numbers that give details about something. Metrics are often combined to create a time series.

A single request travels through multiple services. Use tracing to track requests and resource use through your entire stack.

Health endpoints allow you to monitor a service. If you have a health endpoint, it should snitch. Have it return useful data.

The enterprise monitoring team monitors all team SLAs.
They provide a central point of contact for alerts.
They research tooling and practices for teams.

Whenever you think about about monitoring, start with USE: utilization, saturation, and errors.
Brendan Gregg gave us RED: rate, error, duration.
Google's SRE book prescribes: latency, traffic, errors, saturation.

Development teams should make their metrics available for others to consume.

Your central monitoring team helps define boundaries and heirarchies that enable communication in larger organizations.

### Yo Dawg: Monitoring Monitoring Systems at Netflix
_Roy Rapoport (Netflix)_

The Hero's Journey, every myth has common elements.

Figuring out how to monitor monitoring systems is turtles all the way down.

Monitoring is not alerting. Monitoring systems are platform that take a bunch of data and make it available for consumption.
In short, they take a bunch of data, and they output opinions.

Netflix has a time series database called Atlas. Their alerting service reads from Atlas and sends a request to an action service that then sends a notification to a third-party service like AWS, email, page. Requests are held in an SQS queue. They may also be sent to a stream processing service.

Netflix cares deeply about the query load on Atlas. There is a "hot checker" service that checks on SQS activity. There is also a "cold checker" service that checks the hot checker. Actually, they check on each other.

An Atlas deployment group (ADG) takes incoming metrics via a publish service, that sends metrics to the 6H tier (which stores data for 6 hours) and also sends them to S3. Data will persist through additional 3D (3-day) and 18D (18-day) tier. The short term tiers allow for the map reduce job that moves data into the long-term tiers to fail a few times without dropping data.

Each customer metrics ADG sends metrics to other ADGs (one in each data center) for internal monitoring.

Netflix experiences monitoring system failures every 18-24 months. They've determined this to be an acceptable trade-off.

Starting in May of 2011, Netflix had about 2 million metrics. They now have billions of metrics.

Roy has been at Netflix for about 8 years. A notable theme from this time period is the increasing stability of the system.
New device launches, new content launches, and adding new countries were all a big deal and have become routine operations.

### Our Many Monsters
_Megan Anctil (Slack)_

Slack has a small (~5 engineer) visiblity operations team that owns monitoring, alerting, and related infrastructure.

Slack currently stores 30 days of metrics data from 160 hosts. That is 1.5 million metrics/second and 90TB of storage.

Some vendors charge per host, some scale cost by the type of host, others charge per metric stored at 10s resolution.

Megan went over some of the current pricing from vendors. In the best case scenario vendors would charge $450,000/month.

Clients send data through an ELB, that hits a Graphite relay -> Graphite Carbon -> Graphite API <- Grafana.

Carbon Relay and Carbon cache have been replaced with Gocarbon: https://github.com/lomik/go-carbon

Slack uses Grafana with some custom hacks (e.g. metric exploration and ad-hoc graphing features) and custom dashboards.

Lesson learned: not having tags gets pretty hard and the workarounds get worse and worse.

"You cannot aggregate percentiles." Actually, you can, but you should not. The resulting data is janky garbage.

Slack has a fairly standard ELK stack storing 2 weeks of logs (250TB) on 450 nodes with a replication factor of 2.

Best case scenario for sending this data to a vendor: $1.2M/month.

Clients send data through an ELB to a Logstash sink -> ELB -> Logstash -> ES clients.

Structured logging is better. Filter out any unimporting logs. Always be tuning (ABT).

Slack's alerting stack (https://www.icinga.com/) handles 140K services, 6.3K hosts with a max check latency of ~ 3 seconds.

Clients send data to Icinga (nsca-wrapper -> nsca server -> Icinga <- xinetd) <- Thruk (https://www.thruk.org/).
Slack also uses ThousandEyes and CloudWatch.

Running these open-source solutions has allowed Slack to develop custom features and solve bugs quickly on their own.

### Tracing Production Services at Stripe
_Aditya Mukerjee (Stripe)_

Tracing is about more than HTTP requests.

An intelligent and context-aware metrics pipeline: https://veneur.org

It's possible to extract log data into a histogram, but you don't want to do that at 3am. A lot of log messages are only useful to identify the line of code that is failing. "If you need to look at logs, there's a gap in your system's observability tools." One of those missing tools is probably tracing.

Monitoring, like testing, depends heavily on a developer's ability to predict future failures.

It is a practical observation that the developer's who wrote the code are the best to debug it, and thus should be on-call. We don't this for other types of engineering and we shouldn't do this for software engineering.

Nobody wants to write the code to add tracing around every request because it might be useful for a failure later.

Logs, metrics, and traces are the fundamental particles of monitoring.

The performance requirements for efficiently emitting, storing, and querying logs, metrics, and traces are all different. The implementation details of each should not dictate the interface we emit them to.

A log is a metric with "longer' data. A trace is a metric that allows "inner joins".

Stripe created a standard sensor format (SSF). The pipeline can handle each particle and determine where to send them. Veneur handles the task of converting the data to a format that downstream services or vendors expect.

If your monitoring pipeline goes down you lose visibility. The pipeline needs to be distributed and highly-available. The problem is that not all metric aggregations can be distributed, but you don't want to have a central aggregator node. To avoid this problem, Veneur uses a consistent hashing scheme to send metrics to each node in the cluster.

https://github.com/tdunning/t-digest are a way to recursively calculate approximate percentiles in a distributed fashion.

### Linux debugging tools you'll love
_Julia Evans (Stripe)_

How I got better a debugging. Remember the bug is happing for a logical reason. Be confident. Know your debugging toolkit.

Be a wizard. Ask your operation system what your programs are doing. Operating systems will tell you the truth while your programs lie to you.

The case of the mystery configuration file. When you write a configuration file, start your program, and it doesn't work. strace lets you trace system calls (like an API for your OS). Unfortunately, strace can make your program run 50x slower. There are some faster system call tracing tools, notably perf trace (Linux), dtruss (OS X), and eBPF based tools like opensnoob (Linux).

The case of the slow program. Part one. Run the time command and find that the program wasn't using the CPU 95% of the time. So it was waiting on something. Use pgrep to find the program's PID. Then cat /proc/${PID}/stack. See that it's waiting on the network. Part two. Use time to find that the command is using a lot of CPU. Run perf top and find which function (in the interpreter) is using the CPU. See that the problem was a regular expression. Part three. Run the time commmand to find that the program is using a lot of CPU (but it's all in the kernel this time). Use dstat to find that the programming is writing to disk a lot, but why is that using a lot of CPU? Run perf top and see that the program is writing to any encrypted directory.

perf was used on programs that translate to C functions. You can get JS or Java (both JIT compiled) to tell perf which functions they are running.

The case of the intermittently (like 5 queries/hour) failing DNS queries. Use tcpdump to save the network requests for later analysis. Analyze the packets with wireshark and see that the problem is timeouts. Increase the timeout threshold.

Read zines. http://jvns.ca/zines/

### Instrumenting SmartTVs and Smartphones in the Netflix app for modeling the Internet
_Guy Cirino (Netflix)_

Making the internet fast is slow. Going international presented many hurdles for Netflix (e.g. 3G, spotty connections, etc.).

Netflix needs to know what the internet looks like, not an average, but the full distribution.

Logging anti-patterns: averages can't see the distribution, outliers distorted, 0, negatives, errors, etc.
Sampling results in missing data, rare events, and problems aren't equal in the population.

Histograms are Guy's favorite aggregation. You can take the ECDF of a histogram, which can then be used to calculate other useful metrics. Unfortunately, the long tail has the same resolution as the rest of the distribution. One solution to this problem is exponenentially spaced buckets which are cheap to compute and result in flat memory use.

Data is better than opinions. It doesn't really matter what you pick. Architecture is hard. It helps to make measuring cheap in order to be able experiment live, in the wild. It helps to know what "that" is before you engineer around it. Make getting "that" cheap so you can make decisions based on data. Don't guess, measure.

Making a reality lab. Guy played around with tc and network shaping to similate the network provided by major ISPs.

### Monitoring: A Post Mortem
_Charity Majors (Honeycomb)_

Hates monitoring. Founded Honeycomb (not a monitoring company).

RE: Monitoring is Dead talk by @grepory last year.

There is a seismic shift underway. A large percentage of the talks this year are about how we deal with complex systems.

Build better tools. Think about distributed systems even if you're not build distributed systems. All of the CS work around distributed systems is about complexity and dealing with failure gracefully.

Observability is how we make complex systems tractable.

Metrics and logs were designed for a very different era. Any system with a schema and an index is trying to get you to guess what your data is going to look like. Dashboards are terrible. You cannot pregenerate the right dashboard for every situation where you'll need one. Every time you see a dashboard showing all green it's a lie. We start to believe that if we haven't graphed it, it doesn't exist or doesn't matter. We don't have the detail we need to answer arbitrary questions. Demand more from your tools.

Monitoring is dead. Raw events are the future. Aggregates destroy your precious details.

Quit debugging with your eyeballs and start debugging by asking questions of your data.

Structure raw event data at the source. Logs are just baby events, just give them structure and make them real events. It's useless if it isn't structured. Outliers are the whole damn point. Events carry context and capture causation, both of which make events easier to reason about. Arbitrarily wide events allow you carry more and more context.

You can't hunt for outliers if your tool can't handle outliers.

Unknown-unknowns: engineering problems, open-ended time scale, require creativity
known-unknowns: support problem, closed time scale, use a dashboard and then automate it out of existence

Prometheus is awesome. It's the latest, best RRDTool that will ever be built, but I don't like that model.

We need tools that allow for exploratory analysis and asking questions.

Watch it run in production. If you're not looking at it when it's normal, you don't know what it looks like when it's normal.

Honeycomb and scuba provided a breadth-first approach. Open tracing provides a depth-first approach.

Debugging is a social experience. Our tools must tap into our sense of joy, play, performance, community, and solidarity.

Cultivate a team of SWEs who value operational excellence.

Observability is TDD for production.
