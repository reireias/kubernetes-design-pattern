# Adapter Pattern

## Example 1. redis-exporter

```
    +------------+        +-------+
    | Prometheus |        |  app  |
    +------------+        +-------+
          | get metrics       |
+---------+----------------------------+
|         |                   |        |
| +----------------+   +-------------+ |
| | redis_exporter |---|    redis    | |
| |   <Container>  |   | <Container> | |
| +----------------+   +-------------+ |
|                <Pod>                 |
+--------------------------------------+
```

- [Prometheus](https://prometheus.io/) is pull type resource monitoring tool.

- redis don't have prometheus metrics.

- [redis_exporter](https://github.com/oliver006/redis_exporter) exports redis metrics.

### run on minikube
```sh
# create Pod and Service
kubectl apply -f redis-with-exporter.yml

# get metrics by redis-exporter
curl $(minikube service exporter --url)/metrics
```


## Example 2. mysql rich healthcheck

```
                             +--------+
                             | Client |
                             +--------+
                                  |
                                  | access to /
                                  |
+---------------------------------------------+
|                                 |           |
| +-------------+       +-------------------+ |
| |    MySQL    | query | mysql-healthcheck | |
| | <Container> |-------|    <Container>    | |
| +-------------+       +-------------------+ |
|                    <Pod>                    |
+---------------------------------------------+
```

- MySQL rich healthcheck using [reireias/mysql-healthcheck](https://github.com/reireias/mysql-healthcheck)

- You can execute a SQL by accessing this container.

### run on minikube
```sh
# create persistent volume
kubectl apply -f mysql-pv.yml

# create service and deployment
kubectl apply -f mysql-with-ritch-healthcheck.yml

# do healthcheck
curl $(minikube service healthcheck --url)
# return OK
```
