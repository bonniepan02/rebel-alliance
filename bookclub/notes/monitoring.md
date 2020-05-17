# Monitoring and logging

New relic belongs to performance monitoring category of the tech stack, while sentry can be primarily classified under "Exception monitoring". 
Sentry is a great tool for logging and tracking exceptions in an application. It shows the entire stack trace and occurences to developers for the complete context. It also provide exception grouping and alerting integration. 

Kibana and prometheous can be categorized as "monitoting" tools. Developers describe Kibana as Explore and visualize your data. Kibana is an open source, browser based analytics and search dashboard for Elasticsearch. 
Prometheus is detailed as an open dource service monitoring system and time series database. It collects metrics from configured targets at given intervals, evaluate rule expressions, displays the results, and can trigger alerts if some condition is observed to be true. Some of its ecosystem components include Grafana and alertmanager (https://prometheus.io/docs/introduction/overview/)

Splunk and Sumo logic are both classified as "Log Management" tools.

Jaeger vs Zipkin: Developers describe Jaeger as Distributed tracing system released as open source by Uber. Zipkin is detailed as a distributed tracing system. It helps gather timing data needed to troubleshoot latency problems in service architectures. They both can be primarily classified as monitoring tools. 

## Moniotoring and observability
Observability is achieved when data is made available from within the system that you wish to monitor. Monitoring is the actual task of collecting and displaying this data.

References:
1. https://medium.com/@copyconstruct/effective-mental-models-for-code-and-systems-7c55918f1b3e
2. https://medium.com/@copyconstruct/monitoring-and-observability-8417d1952e1c