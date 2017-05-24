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
