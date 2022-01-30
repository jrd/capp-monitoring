Capp monitoring
===============

# Generate SSH private/public keys

You should already have your own (for gitlab, …)

But in case, you generate a new pair like this:

```sh
ssh-keygen -t rsa
```

# Register the public key on the capp server

```sh
ssh <server-name> pubkeys add '"'"$(cat ~/.ssh/id_rsa)"'"'
```

Give the private key location when creating the release with `create-release`.

Specificities of this project
=============================

Although heavily based on [stefanprodan's dockerprom](https://github.com/stefanprodan/dockprom), this project integrates some specificities:
* adaptations for the creation of dca archives for easy deployment on a capp server.
* deletion of caddy reverse proxy: capp nginx proxy will replace it on deployment.


# Configurations

Two configurations are provided:
* `example` to copy as `staging` or `prod` environment.
* `dev` for local testing. For local testing, please make sure that docker-compose has access to the `./confs/dev/context` directory, as seen in the following chapter.

For each configuration, please **update the password and username data** in `confs/ENV/context/config` file before deployment.

First run:

```shell_session
./create-docker-envs-json
```

# Usage for `dev`

For dev environment, a reverse proxy is needed.
We use caddy, that we added in `context/docker-compose.dev.yml`. Launch the service by combining the two compose files:

```shell_session
TODO
cp -r context/healthchecker ../confs/dev/
docker-compose -f docker-compose.yml -f docker-compose.dev.yml --project-directory $PWD/../confs/dev/context up -d
```

General documentation
=====================

A monitoring solution for Docker hosts and containers with [Prometheus](https://prometheus.io/), [Grafana](http://grafana.org/), [cAdvisor](https://github.com/google/cadvisor),
[NodeExporter](https://github.com/prometheus/node_exporter), [Capp Healthchecker](https://gitlab.com/systra/qeto/infra/prometheus-capp-healthchecker) and alerting with [AlertManager](https://github.com/prometheus/alertmanager).

Containers:

* [Prometheus](https://prometheus.io/) (metrics database)
* [NodeExporter](https://github.com/prometheus/node_exporter) (host metrics collector)
* [cAdvisor](https://github.com/google/cadvisor) (containers metrics collector)
* [Capp Healthchecker](https://gitlab.com/systra/qeto/infra/prometheus-capp-healthchecker) (container health status collector)
* [AlertManager](https://github.com/prometheus/alertmanager) (alerts management)
* [Grafana](http://grafana.org/) (visualize metrics)

## Setup Grafana

Navigate to `https://monitoring.<server-name>` and login with user ***admin*** password ***admin***. You can change the credentials in the compose file or by supplying the `ADMIN_USER` and `ADMIN_PASSWORD` environment variables on compose up.
