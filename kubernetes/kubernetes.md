# Kubernetes (k8s)

## What is Kubernetes?
Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.


## How to launch a process inside K8S
1. Start connection to kubernetes: `kubeon`
2. Enter to the cluster where the process is: `kubectx <cluster_name>` 
3. Enter to the namespace of the process: `kubens <namespace>`
4. (Optional) check the cronjob configured: `kubectl get cronjob`
5. Launch the process: `kubectl create job --from=cronjob/<cronjob_name>`
    1. It will create a pod and we can check the logs of that process: `kubectl get pods`
    2. Search for the one that is running/launching in order to check the logs of the process: `kubectl logs <pod_name> -f`
        * `-f` means follow in order to follow the logs


## Useful commands
* `kubeoff`: Stop connection to kubernetes cluster
* `kubectx`: Get the list of available clusters 
* `kubectx <cluster_name>`: To enter a specific cluster enter the name of it or its alias
* `kubectl`: Kubernetes API to interact with the cluster
* `kubens`: Get the list of all the available namespaces
* `kubens <namespace>`: To enter a specific namespace
* `kubectl get deploy`: List deploys inside namespace
* `kubectl get deploy -n <namespace>`: List deploys outside the namespace
* `kubectl describe deploy <deploy_name>`: Get info from deploy 


## Utilities:
* [k9s](https://github.com/derailed/k9s)
* kube-ps1
* kubernetes-cli
* kubectx
* kubernetes-helm
* autocomplete for k8s