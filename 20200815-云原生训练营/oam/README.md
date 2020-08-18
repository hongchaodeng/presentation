# Using OAM to Deploy Flask App

Set up:

```bash
bash create-cluster.sh

# Install ingress controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml

# build flask app image
docker build -t flask-sample:v1 .
kind load docker-image flask-sample:v1

kubectl create namespace oam-system
helm repo add crossplane-master https://charts.crossplane.io/master/
helm install oam --namespace oam-system crossplane-master/oam-kubernetes-runtime --devel

kubectl apply -f config/
```

Verify:

```bash
$ curl localhost/
Hello World from Flask
```


## Clean up

```
kubectl delete -f config/
kind delete cluster
```

## References

- [Setting up ingress in KinD](https://kind.sigs.k8s.io/docs/user/ingress/)