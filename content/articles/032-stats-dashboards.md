---
title: 'REDtalks.live #032 - Stats & Dashboards'
date: '2018-05-25'
draft: false
tags: ['Demo', 'Grafana', 'Graphite', 'StatsD', 'Observability', 'Telemetry']
---

We appear to be leaving the era of polling devices for stats and then processing them into something meaningful. This is what happy customers are telling me! So, how should we be operating in the future? Organizations are standardizing on app-centric performance monitoring and dash-boarding technologies. They're using open source technologies like 'StatsD', 'GraphiteDB', 'Grafana', and 'Prometheus'. And its up to the infrastructure vendors to present useful data to these systems, or get out of the way! To address such asks I set out on a new project called 'BigStats'. BigStats is an small piece of code installed onto an F5 BIG-IP App Services device that enables it to push application statistics into popular databases and dashboards. Watch this episode to see how simple this solution really is:

{{< youtube mnVtuQkQL5Y >}}

Here's the code: [GitHub](https://github.com/npearce/BigStats) Thanks for listening!