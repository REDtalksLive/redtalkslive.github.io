---
title: 'REDtalks.live #37 - BigStats v0.4 released'
date: Wed, 05 Sep 2018 16:46:13 +0000
draft: false
tags: ['BigStats', 'code', 'telemetry', 'words']
---

BigStats v0.4 has been released! Yay!!! ![BigStats-300dpi](https://redtalkslive.files.wordpress.com/2018/09/bigstats-300dpi.png) Way back in [REDtalks.live #32 – Stats & Dashboards](http://redtalks.live/2018/05/25/redtalks-live-32-stats-dashboards/) (May 25th) I demonstrated a prototype solution for getting useful data out of a BIG-IP and into various telemetry pipelines... Well, that was version 0.1, and now we're up to version 0.4! So, what's happened since? Highlights from the RELEASE\_NOTES.md file:

1.  We now have a RELEASE\_NOTES.md file to track all the awesome!
2.  Support for Apache Kafka message brokers
3.  Support for both AS3 declarations AND traditional configurations (it originally only supported the AS3 declarative interface).
4.  Support for output sizing: Small (VIP Stats only), Medium (VIP + Pool stats), Large (working on this....)
5.  Support for Device Stats (RAM and CPU)
6.  Apache Kafka 'topic' config:
    1.  All data in one topic, or;
    2.  Separate topics per BIG-IP Tenant
7.  Enforced minimum polling interval of 10 seconds (play safe, kids)
8.  Added "config.enabled: true|false" to the config because, sometimes you want some peace and quiet.
9.  Provided a BigStatsSettings object schema (schema validator might be coming soon..)

Here's an sample stats object built from crawling the running config of a BIG-IP every 'config.interval: _n_' seconds:```
{
        "ip-172-31-1-20-us-west-1-compute-internal": {
                "services": {
                        "Tenant\_01/App1": {
                                "/Tenant\_01/App3/172.31.4.11:80": {
                                        "clientside\_curConns": 0,
                                        "clientside\_maxConns": 0,
                                        "clientside\_bitsIn": 0,
                                        "clientside\_bitsOut": 0,
                                        "clientside\_pktsIn": 0,
                                        "clientside\_pktsOut": 0,
                                        "/Tenant\_01/App1/web\_pool1": \[
                                                {
                                                        "172.31.10.112:80": {
                                                                "serverside\_curConns": 0,
                                                                "serverside\_maxConns": 0,
                                                                "serverside\_bitsIn": 0,
                                                                "serverside\_bitsOut": 0,
                                                                "serverside\_pktsIn": 0,
                                                                "serverside\_pktsOut": 0,
                                                                "monitorStatus": "down"
                                                        }
                                                },
                                                {
                                                        "172.31.10.111:80": {
                                                                "serverside\_curConns": 0,
                                                                "serverside\_maxConns": 0,
                                                                "serverside\_bitsIn": 0,
                                                                "serverside\_bitsOut": 0,
                                                                "serverside\_pktsIn": 0,
                                                                "serverside\_pktsOut": 0,
                                                                "monitorStatus": "down"
                                                        }
                                                },
                                                {
                                                        "172.31.10.113:80": {
                                                                "serverside\_curConns": 0,
                                                                "serverside\_maxConns": 0,
                                                                "serverside\_bitsIn": 0,
                                                                "serverside\_bitsOut": 0,
                                                                "serverside\_pktsIn": 0,
                                                                "serverside\_pktsOut": 0,
                                                                "monitorStatus": "down"
                                                        }
                                                },
                                                {
                                                        "172.31.10.114:80": {
                                                                "serverside\_curConns": 0,
                                                                "serverside\_maxConns": 0,
                                                                "serverside\_bitsIn": 0,
                                                                "serverside\_bitsOut": 0,
                                                                "serverside\_pktsIn": 0,
                                                                "serverside\_pktsOut": 0,
                                                                "monitorStatus": "down"
                                                        }
                                                }
                                        \]
                                }
                        },
                        "Common": {
                                "/Common/172.31.4.200:80": {
                                        "clientside\_curConns": 0,
                                        "clientside\_maxConns": 0,
                                        "clientside\_bitsIn": 0,
                                        "clientside\_bitsOut": 0,
                                        "clientside\_pktsIn": 0,
                                        "clientside\_pktsOut": 0,
                                        "/Common/noAS3\_POOL": \[
                                                {
                                                        "172.31.10.200:8080": {
                                                                "serverside\_curConns": 0,
                                                                "serverside\_maxConns": 0,
                                                                "serverside\_bitsIn": 0,
                                                                "serverside\_bitsOut": 0,
                                                                "serverside\_pktsIn": 0,
                                                                "serverside\_pktsOut": 0,
                                                                "monitorStatus": "down"
                                                        }
                                                },
                                                {
                                                        "172.31.10.201:8080": {
                                                                "serverside\_curConns": 0,
                                                                "serverside\_maxConns": 0,
                                                                "serverside\_bitsIn": 0,
                                                                "serverside\_bitsOut": 0,
                                                                "serverside\_pktsIn": 0,
                                                                "serverside\_pktsOut": 0,
                                                                "monitorStatus": "down"
                                                        }
                                                },
                                                {
                                                        "172.31.10.202:8080": {
                                                                "serverside\_curConns": 0,
                                                                "serverside\_maxConns": 0,
                                                                "serverside\_bitsIn": 0,
                                                                "serverside\_bitsOut": 0,
                                                                "serverside\_pktsIn": 0,
                                                                "serverside\_pktsOut": 0,
                                                                "monitorStatus": "down"
                                                        }
                                                }
                                        \]
                                }
                        }
                },
                "device": {
                        "memory": {
                                "memoryTotal": 7574732800,
                                "memoryUsed": 1525312880
                        },
                        "cpu0": {
                                "cpuIdle": 161495459,
                                "cpuIowait": 169763,
                                "cpuSystem": 292088,
                                "cpuUser": 973939
                        },
                        "cpu1": {
                                "cpuIdle": 160343033,
                                "cpuIowait": 68690,
                                "cpuSystem": 426881,
                                "cpuUser": 992052
                        }
                }
        }
}
```Here's the code/docs: [https://github.com/npearce/BigStats](https://github.com/npearce/BigStats) In addition to all this awesome, I've also shared my lab environment setup details! So, if you want to build a Graphite/Grafana demo like in episode #32, or maybe you want to test against a single node Kafka Broker but don't know how to do this, well, look no further. Here are all my lab setup instructions: [https://gist.github.com/npearce/](https://gist.github.com/npearce/) Thanks for listening!