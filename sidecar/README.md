# Sidecar Pattern

git-sync sidecar

```
  +--------+           +--------+
  | client |           | GitHub | html file
  +--------+           +--------+
       |                   |
       | access            | git pull
       |                   |
+-------------+     +-------------+
|    Nginx    |     |   git-sync  |
| <Container> |     | <Container> |
+-------------+     +-------------+
       |                   |
       | mount             | mount
       |                   |
       |  +-------------+  |
       +--| source code |--+
          |  <Voluem>   |
          +-------------+
```

- [git-sync](https://github.com/kubernetes/git-sync) syncs source code from git repository to local.

- Both Nginx and git-sync containers mounts same volume stored html files.

- git-sync pulls html file repository when pushed code to repository.

## run on minikube

```sh
# create service and deployment
kubectl apply -f git-sync.yml

# open in browser
minikube service git-sync
```
