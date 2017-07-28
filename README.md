# ELK stack based on Docker Compose

Run enhanced ELK stack (Elasticsearch, Logstash, Kibana, X-Pack, Curator) on top of Docker Swarm.

It will give you the ability to analyze any data set by using the searching/aggregation capabilities of Elasticsearch
and the visualization power of Kibana.

Based on official Elasticsearch Docker images plus custom Curator:

* [curator](https://github.com/valeriano-manassero/docker-curator-centos)

## Contents

1. [Requirements](#requirements)
   * [Host setup](#host-setup)
2. [Usage](#usage)
   * [Bringing up the stack](#bringing-up-the-stack)
3. [Initial Setup](#initial-setup) 
   * [How can I tune the Logstash configuration?](#how-can-i-tune-the-logstash-configuration)
   * [How can I tune the Curator configuration?](#how-can-i-tune-the-curator-configuration)
4. [Storage](#storage)
   * [Where Elasticsearch data is persisted?](#where-elasticsearch-data-is-persisted)
5. [Scale](#scale)
   * [How can I scale this Stack?](#how-can-i-scale-this-stack)

## Requirements

### Host setup

1. Install [Docker] version **17.06.0+** in Swarm Mode
2. Clone this repository

## Usage

### Bringing up the stack

Start the ELK stack using `docker` and giving it a name:

```bash
$ docker stack deploy -c docker-compose.yml <STACK NAME>
```

Give Kibana about 2 minutes to initialize, then access the Kibana web UI by hitting
[http://<HOSTNAME>:5601](http://<HOSTNAME>:5601) with a web browser.

By default, the stack exposes the following ports:
* 5044: Logstash Beats input.
* 5601: Kibana with default X-Pack credentials (user: elastic, password: changeme)

## Initial setup

### How can I tune the Logstash configuration?

The Logstash configuration is stored in `docker-configs/logstash/logstash.yml`.

**NOTE**: logstash.yml is configured to receive data by a Beats client on port 5044 without filters. Probably you'll need to configure this file according yours needs.

### How can I tune the Curator configuration?

The Curator configuration is stored in `docker-configs/curator/curator.yml` and `docker-configs/curator/actions.yml`.

**NOTE**: Curator is configured to purge ant logstash-* index older than 30 days. Probably you'll need to configure these files according yours needs.

## Storage

### Where Elasticsearch data is persisted?

The data stored in Elasticsearch will be persisted in a docker volume named `elastic_data`.

## Scale

### How can I scale this Stack?

This Docker Stack is easly scalable with scale capability of Docker Swarm.

This is an example of scale procedure:

```bash
$ docker service scale elk_logstash=3
```

**BEWARE before scaling elasticsearch instance:** make sure do you have enough free nodes. You cannot have two instance of elasticsearch on the same node due to same data volume conflict.