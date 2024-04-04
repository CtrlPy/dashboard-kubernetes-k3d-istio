# dashboard-kubernetes-k3d-istio

  note: install all necessary versions of tools to work through asdf and the file .tool-ver...
`asdf install`

st1. **Create a cluster and disable Traefik with the following command:**

```zsh 
k3d cluster create --api-port 6550 -p '9080:80@loadbalancer' -p '9443:443@loadbalancer' --agents 2 --k3s-arg '--disable=traefik@server:*'
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
st3. **To deploy Dashboard, run the following command:**

```zsh
 curl -LO https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/dashboard.yaml
```

```zsh
kubectl apply -f dashboard.yaml
```
   note: Verify that Dashboard is deployed and running.
`kubectl get pod -n kubernetes-dashboard`

st4. **Create a ServiceAccount and ClusterRoleBinding to provide admin access to the newly created cluster.**
```zsh
kubectl create serviceaccount -n kubernetes-dashboard admin-user
kubectl create clusterrolebinding -n kubernetes-dashboard admin-user --clusterrole cluster-admin --serviceaccount=kubernetes-dashboard:admin-user

```
st5. **To log in to your Dashboard, you need a Bearer Token. Use the following command to store the token in a variable.**

```zsh
token=$(kubectl -n kubernetes-dashboard create token admin-user)
```
note: echo command and copy it to use for logging in to your Dashboard
```zsh
echo $token
```

st6. **You can access your Dashboard using the kubectl command-line tool by running the following command:**

```zsh
kubectl proxy
```
_Starting to serve on 127.0.0.1:8001_

Kubernetes Dashboard to view your deployments and services.

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login


