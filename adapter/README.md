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
