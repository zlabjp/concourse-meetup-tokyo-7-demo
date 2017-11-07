# Concourse CI/CD Meetup Tokyo #7

This repository contains demo scripts for "Continous Delivery with Concourse and Kubernetes" which talks at Concourse CI/CD Meetup Tokyo #7. On this demo, we use [zlabjp/kubernetes-resource](https://github.com/zlabjp/kubernetes-resource), which is a concourse resource for controlling the Kubernetes cluster.

- URL https://www.meetup.com/Concourse-CI-Tokyo-Meetup/events/244165635
- Slide: https://bit.ly/concourse-and-k8s

## Preparing

1. Create `stage` and `prod` namespaces
1. Create `concourse` serviceaccount in both namespaces
1. Create `concourse` rolebinding which is bound `edit` clusterrole in both namespaces

```
for ns in stage prod; do
  kubectl create ns $ns
  kubectl create sa concourse -n $ns
  kubectl create rolebinding concourse --clusterrole edit --serviceaccount ${ns}:concourse -n $ns
done
```

1. Clone https://github.com/zlabjp/kubernetes-scripts
1. Copy a kubeconfig of `concourse` serviceaccount of each namespace using `create-kubeconfig` in kubernetes-scripts repository
1. Paste a kubeconfig to `stage-kubeconfig` and `prod-kubeconfig` in `ci/vars.yml`

```
$ git clone https://github.com/zlabjp/kubernetes-scripts.git && cd kubernetes-scripts
$ ./create-kubeconfig concourse --namespace stage
$ ./create-kubeconfig concourse --namespace prod
```

## License

All files in this repository are released under the MIT License, see LICENSE.txt.
