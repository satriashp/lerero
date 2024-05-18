# Nest.js demo app

## Build docker image


```
cd ./project

docker build --tag lerero:0.1.0 .
```

## Run docker
Run nest.js container app with this command:

```
docker run --rm -p 3000:3000 lerero:0.1.0
```

Access nest.js project with this link http://localhost:3000

# Setup local kubernetes with k3d

## Prerequisite
* kubectl
* terraform
* terragrunt
* k3d


## Run and deploy local kubernetes cluster

This will deploy k3d along with local docker image repository.
```
k3d cluster create --config ./cluster/k3d.yaml
```
then check if cluster are running `kubectl cluster-info`

## Deploy infra app and Nest.js demo

1. Push nest.js demo project docker image into local registry
```
# tagging
docker tag lerero:0.1.0 registry.localhost:5000/lerero:0.1.0

# push
docker push registry.localhost:5000/lerero:0.1.0
```

2. Provision with terragrunt.
This command will install flux and neccessary file. Then flux will take care the rest.

```
terragrunt run-all apply --terragrunt-working-dir ./cluster/terraform

```

# Troubleshoot

## local hosts dns
This list of dns need to set to localhost/127.0.0.1:
```
127.0.0.1 registry.localhost
127.0.0.1 grafana.lvh.me
127.0.0.1 lerero.lvh.me
```

* for windows you can set this by editing this file `C:\Windows\System32\drivers\etc\hosts`