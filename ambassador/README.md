# Ambassador Pattern

## Example 1. twemproxy
twemproxy ambassador

```
                     +-----------------+
                     | +-------------+ |
                     | |    Nginx    | |
                     | | <Container> | |
                     | +-------------+ |
                     |        |        |
                     | +-------------+ |
                     | |  twemproxy  | |
                     | | <Container> | |
                     | +-------------+ |
                     |        |  <Pod> |
                     +-----------------+
                              |
            +-----------------+-----------------+ 
            |                 |                 |
  +---------+-----------------+-----------------+---------+
  |         |                 |                 |         |
  | +---------------+ +---------------+ +---------------+ |
  | | Redis Shard 0 | | Redis Shard 1 | | Redis Shard 2 | |
  | |     <Pod>     | |     <Pod>     | |     <Pod>     | |
  | +---------------+ +---------------+ +---------------+ |
  |                     <StatefulSet>                     |
  +-------------------------------------------------------+
```

- ambassador pattern is container for external access.

- [twemproxy](https://github.com/twitter/twemproxy) is a fast, light-weight proxy for memcached and redis.

- The twemproxy distributes redis request by hash of key.

### run on minikube

```sh
# create configMap
kubectl create configmap twem-config --from-file=./nutcracker.yml

# create Pod and StatefulSet
kubectl apply -f twemproxy.yml
```

### check twemproxy behavior

```sh
# access to nginx container
kubectl exec -it ambassador-example --container nginx bash 
# install redis-cli
apt update
apt install -y redis-tools

# set value using twemproxy
redis-cli
127.0.0.1:6379> set hoge 1
127.0.0.1:6379> set fuga 2

# get value using twemproxy
127.0.0.1:6379> get hoge

# check keys in redis container
kubectl exec -it sharded-redis-0 --container redis redis-cli
127.0.0.1:6379> keys *
kubectl exec -it sharded-redis-1 --container redis redis-cli
127.0.0.1:6379> keys *
kubectl exec -it sharded-redis-2 --container redis redis-cli
127.0.0.1:6379> keys *
```
