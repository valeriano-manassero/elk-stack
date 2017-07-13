# ELK stack based on Docker Compose

Run ELK (Elasticsearch, Logstash, Kibana, Curator, NGINX) stack with Docker and Docker Compose.

It will give you the ability to analyze any data set by using the searching/aggregation capabilities of Elasticsearch
and the visualization power of Kibana.

Based on the my Centos 7 Docker images:

* [elasticsearch](https://github.com/valeriano-manassero/docker-elasticsearch-centos)
* [logstash](https://github.com/valeriano-manassero/docker-logstash-centos)
* [kibana](https://github.com/valeriano-manassero/docker-kibana-centos)
* [curator](https://github.com/valeriano-manassero/docker-curator-centos)
* [NGINX](https://github.com/valeriano-manassero/docker-httpproxy-centos)

## Contents

1. [Requirements](#requirements)
   * [Host setup](#host-setup)
2. [Usage](#usage)
   * [Bringing up the stack](#bringing-up-the-stack)
3. [Initial Setup](#initial-setup) 
   * [Default Kibana index pattern creation](#Default-Kibana-index-pattern-creation)
   * [How can I tune the Logstash configuration?](#how-can-i-tune-the-logstash-configuration)
4. [Storage](#storage)
   * [Where Elasticsearch data is persisted?](#where-elasticsearch-data-is-persisted)

## Requirements

### Host setup

1. Install [Docker] version **17.03.0+**
2. Install [Docker Compose] version **1.12.0+**
3. Clone this repository

## Usage

### Bringing up the stack

Start the ELK stack using `docker-compose`:

```bash
$ docker-compose up -d
```

Give Kibana about 2 minutes to initialize, then access the Kibana web UI by hitting
[http://localhost:5601](http://localhost:5601) with a web browser.

By default, the stack exposes the following ports:
* 5044: Logstash Beats input.
* 1234: Kibana proxyed by NGINX for authentication)

## Initial setup

### Default Kibana index pattern creation

When Kibana launches for the first time, it is not configured with any index pattern.

**NOTE**: You need to inject data into Logstash before being able to configure a Logstash index pattern via the Kibana web
UI. Then all you have to do is hit the *Create* button.

### How can I tune the Logstash configuration?

The Logstash configuration is stored in `logstash/logstash.yml`.

**NOTE**: logstash.yml is configured to receive data by a Beats client on port 5044 without filters. Probably you 'll need to configure thhis file according yours needs.

## Storage

### Where Elasticsearch data is persisted?

The data stored in Elasticsearch will be persisted in a docker volume named `elasticsearch_data`.
