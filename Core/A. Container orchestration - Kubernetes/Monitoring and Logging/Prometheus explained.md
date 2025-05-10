# Prometheus

> Automatic monitoring and alerting toolkit for microservices and containerized applications.

* monitoring system and time series database
* collects metrics from monitored targets by scraping metrics HTTP endpoints
* stores all scraped samples locally and runs rules over this data to either aggregate and record new time series from existing data or generate alerts




## Use cases
1. Constantly monitor the health of your services
2. Alerting based on certain conditions (crash, high CPU usage)
3. Identify problems before they affect users


## Architecture
[metrics types](https://prometheus.io/docs/concepts/metric_types/) | 
> Targets -- app, service, server(Apache, Windows, Linux)


> Prometheus Server 
>> <details><summary>Time Series DB -- metrics data(CPU usage, N of exceptions, Memory usage, request count or duration)</summary>
>> 
>> 1. ```HELP```: metrics description. 
>> 2. ```TYPE```: Counter(how many times x), Gauge(value if x), Histogram(how long/big), Summary()
>> </details>
>> <details><summary>Data retrieval worker (pull feature -- that is beneficial)</summary>
>> -- pulls metrics from targets, hostaddress/metrics http endpoint (must comply with format Prometheus can understand).
>>
>> -- service might provide /metrics endpoint by default, with use of Exporter, or with use of Prometheus client libraries to expose metrics (in custom apps).
>>
>> -- as example: mysql DB with a side Exporter container.
>> </details>
>> <details><summary>HTTP Server / API</summary>
>> -- accepts queries, returns metrics to external clients
>> </details>

> External clients -- Grafana UI, Alertmanager, etc.

Prometheus server reads alerts from ```alert.rules``` file, and sends them to Alertmanager:

> <details><summary>Alertmanager</summary>
> -- handles alerts sent by Prometheus server
>
> -- send out notifications to different services (Email, Slack, etc.)
> </details>

Prometheus server stores metrics data in its own time series database, but it can also store data in other databases (like InfluxDB, Graphite, etc.):
> <details><summary>Data Storage</summary>
> -- custom time series format, not suitable for relational databases
> -- could be switched to remota storage systems
> -- can be a bottleneck for scaling
> </details>

#### Pull system advantages
1. In case of push model, apps/services are forced to push metrics to the server, resulting in high load of network traffic, monitoring become a bottleneck.
2. Multiple Prometheus instances can pull metrics data.
3. Prometheus server is the only initiator of metrics scraping, so it gives better detection/insight if service is up.




## Usage
Which targets, at what interval -- everything in ```prometheus.yml``` config file.



```yml
global:
    scrape_interval: 15s
    evaluation_interval: 15s
rule_files:
    - "alert.rules"  # alerting rules
scrape_configs:
    - job_name: 'prometheus'   # self monitoring
        static_configs:
            - targets: ['localhost:9090']
    - job_name: 'node'
        scrape_interval: 1m  # rewrite global interval
        static_configs:
            - targets: ['localhost:9100']
```

You either write PromQL queries in Prometheus UI, or use Grafana to visualize data.
Learning PromQL, Configuring YAML files, Set up of Grafana dashboards -- comples, not well-documented, have a steep learning curve.

| Pros | Cons |
| --- | --- |
|stand alone and self-containing|difficult to scale|
|no extensive setup|limits monitoring|

Fully compatible with Docker and Kubernetes, and can be used to monitor containerized applications.

