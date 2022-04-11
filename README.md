
## Run Container

```
minikube start --cpus 4 --memory 16384 \
--kvm-gpu -p juno --kubernetes-version=v1.20.11 \
--addons default-storageclass storage-provisioner
```

## Docker Login Credentials for Private Registry
```
docker login ghcr.io
config=~/.docker/config.json
fullpath=$(realpath $config)
kubectl create secret generic regcred \
--from-file=.dockerconfigjson=$fullpath \
--type=kubernetes.io/dockerconfigjson
```

## Install Nodes
```
cd juno && helm install juno . -f values.yaml && cd ..
```

## Port Forwards for local development.
```
kubectl port-forward svc/juno-service 26657 &
```


## Clean up

```
pkill kubectl
helm uninstall juno
minikube delete -p juno
```
