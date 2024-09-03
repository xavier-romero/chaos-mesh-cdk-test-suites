# Set up
### Requirements
- Minikube installed and running
- Kurtosis installed and working
- ```kubectl``` installed and configured for minikube
- ```helm``` installed

### Steps
```bash
git clone https://github.com/crytic/attacknet.git
# I had to modify go.sum to use the right Kurtosis-API version matching my enginer
cd attacknet && go build ./cmd/attacknet
# copy resulting attacknet binary to any folder in your $PATH
kubectl create ns chaos-mesh
helm repo add chaos-mesh https://charts.chaos-mesh.org
helm install chaos-mesh chaos-mesh/chaos-mesh -n=chaos-mesh --version 2.6.3
kurtosis cluster set minikube
kurtosis gateway
```

At this point you can deploy your Kurtosis stack, for instance:
```kurtosis run --enclave cdk .```


# Running tests
### Requirements
- Have the Kurtosis stack to test running on minikube
- Having Kurtosis Gateway running for Minikube

### Executing test
On the root folder of this repo, check that you have a soft link ```test-suites``` pointing to this root folder, like that:
```
lrwxrwxrwx  1 user user    1 ago 29 15:10 test-suites -> .
```
That's because attacknet finds for tests inside test-suites relative to the current path.

Now you just run to invoke attacknet with the desired test:

```
attacknet start cdk-network
```

# Clean up
```
kurtosis clean -a
kurtosis cluster set docker
helm uninstall chaos-mesh -n=chaos-mesh
helm repo remove chaos-mesh
minikube stop
```

# References
- [Attacknet](https://github.com/crytic/attacknet) + [DOC](https://github.com/crytic/attacknet/blob/main/docs/DOCUMENTATION.md)
- [Chaos Mesh](https://chaos-mesh.org/docs/)
    - [Chaos Mesh Stress](https://chaos-mesh.org/docs/simulate-heavy-stress-on-kubernetes/), etc
