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
> Targets -- app, service, server(Apache, Windows, Linux)


> Prometheus Server 
>> * Time Series DB -- metrics data(CPU usage, N of exceptions, Memory usage, request count or duration)
>> * Data retrieval worker -- pulls metrics from targets
>> * HTTP Server / API -- accepts queries, returns metrics to external clients

> External clients -- Grafana UI, Alertmanager, etc.





## Usage

