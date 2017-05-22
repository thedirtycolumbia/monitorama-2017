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
