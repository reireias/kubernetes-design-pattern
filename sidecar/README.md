# Sidecar Pattern

## Example 1. git-sync

git-sync sidecar

```
     +--------+           +--------+
     | client |           | GitHub | html file
     +--------+           +--------+
          |                   |
          | access            | git pull
          |                   |
+---------------------------------------+
|         |                   |         |
|  +-------------+     +-------------+  |
|  |    Nginx    |     |   git-sync  |  |
|  | <Container> |     | <Container> |  |
|  +-------------+     +-------------+  |
|         |                   |         |
|         | mount             | mount   |
|         |                   |         |
|         |  +-------------+  |         |
|         +--| source code |--+         |
|            |  <Voluem>   |            |
|            +-------------+            |
|                 <Pod>                 |
+---------------------------------------+
```

- [git-sync](https://github.com/kubernetes/git-sync) syncs source code from git repository to local.

- Both Nginx and git-sync containers mount same volume stored html files.

- git-sync pulls html file repository when pushed code to repository.

### run on minikube

```sh
# create service and deployment
kubectl apply -f git-sync.yml

# open in browser
minikube service git-sync
```

## Example 2. topz

topz sidecar

```
+---------------------------------------------+
|  +---------------------------------------+  |
|  |  +-------------+     +-------------+  |  |
|  |  |    Nginx    |     |     topz    |  |  |
|  |  | <Container> |     | <Container> |  |  |
|  |  +-------------+     +-------------+  |  |
|  |            <PID namespace>            |  |
|  +---------------------------------------+  |
|                    <Pod>                    |
+---------------------------------------------+
```

- topz container and Nginx container share PID namespace.

- topz is monitoring container process using share PID namespace.

- You can see process resource in browser.

### run on minikube

```sh
# create service and deployment
kubectl apply -f topz.yml

# show services
minikube service list
|-------------|----------------------|-----------------------------|
|  NAMESPACE  |         NAME         |             URL             |
|-------------|----------------------|-----------------------------|
| default     | nginx                | http://192.168.39.196:31831 |
| default     | topz                 | http://192.168.39.196:30532 |
|-------------|----------------------|-----------------------------|

# Access to nginx
minikube service nginx

# Show topz url (and access in browser yourself).
echo "$(minikube service topz --url)/topz"
```
