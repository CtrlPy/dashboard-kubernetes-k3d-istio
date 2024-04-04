# dashboard-kubernetes-k3d-istio

st1. **Create a cluster and disable Traefik with the following command:**

```zsh 
k3d cluster create --api-port 6550 -p '9080:80@loadbalancer' -p '9443:443@loadbalancer' --agents 2 --k3s-arg '--disable=traefik@server:
```
` k3d cluster list`

st2. **Once you are done setting up a k3d cluster, you can proceed to install Istio with Helm 3 on it.**

```zsh
kubectl create namespace istio-system
helm install istio-base istio/base -n istio-system --wait
helm install istiod istio/istiod -n istio-system --wait

```
```zsh
kubectl label namespace istio-system istio-injection=enabled
helm install istio-ingressgateway istio/gateway -n istio-system --wait
```