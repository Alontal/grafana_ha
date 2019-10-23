# Grafana High Availability (HA) test setup

A set of docker compose services which together creates a Grafana HA test setup with capability of easily
scaling up/down number of Grafana instances.

Included services

- Grafana
- Mysql - Grafana configuration database and session storage
- Prometheus - Monitoring of Grafana and used as data source of provisioned alert rules
- Nginx - Reverse proxy for Grafana and Prometheus. Enables browsing Grafana/Prometheus UI using a hostname

## Prerequisites

### Build grafana docker container

Build a Grafana docker container from current branch and commit and tag it as grafana/grafana:dev.

## Start services

```bash
$ docker-compose up -d
```

Browse

- http://grafana.loc/
- http://prometheus.loc/

Check for any errors

```bash
$ docker-compose logs | grep error
```

### Scale Grafana instances up/down

Scale number of Grafana instances to `<instances>`

```bash
$ docker-compose up --scale grafana=<instances> -d
# for example 3 instances
$ docker-compose up --scale grafana=3 -d
```

## Test alerting

### Create notification channels

Creates default notification channels, if not already exists

```bash
$ ./alerts.sh setup
```

### Slack notifications

Disable

```bash
$ ./alerts.sh slack -d
```

Enable and configure url

```bash
$ ./alerts.sh slack -u https://hooks.slack.com/services/...
```

Enable, configure url and enable reminders

```bash
$ ./alerts.sh slack -u https://hooks.slack.com/services/... -r -e 10m
```

### Provision alert dashboards with alert rules

Provision 1 dashboard/alert rule (default)

```bash
$ ./alerts.sh provision
```

Provision 10 dashboards/alert rules

```bash
$ ./alerts.sh provision -a 10
```

Provision 10 dashboards/alert rules and change condition to `gt > 100`

```bash
$ ./alerts.sh provision -a 10 -c 100
```

### Pause/unpause all alert rules

Pause

```bash
$ ./alerts.sh pause
```

Unpause

```bash
$ ./alerts.sh unpause
```
