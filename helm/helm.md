# Helm
Based on [Helm Workshop from Softonic](https://github.com/softonic/helm-workshop)

## What is Helm
Tool to manage [Kubernetes](../kubernetes/kubernetes.md) using charts.

* Release: Specific installation of teh chart. It's a way to store a history of everything that's being executed in helm (The relase is like a tag)

## Helm charts
Collection of files that describe a related set of k8s resources.
* There're charts in public repos.
* They are a standard for k8s applications

### Structure
This is the defaulty structure that creates when we run `helm create <chart_name>`
* chart-name
  * /templates --> Template where is going to be used the values from `values.yaml`
  * /charts --> External dependencies (like node_modules)
  * `Chart.yaml` --> Like a package.json
  * `values.yaml` --> file where we store values that's going to be used in the templates


## Helm template
Allows to render templates in the console. It will only show, it won't install anything.

* `helm template --create-namespace -n <namespace> <release_name> <path_to_chart>`
  * i.e. `helm template --create-namespace -n helm-workshop helm-workshop helm-workshop`
  * Output:
  ```sh
  ---
  # Source: helm-workshop/templates/serviceaccount.yaml
  apiVersion: v1
  kind: ServiceAccount
  metadata:
    name: helm-workshop
    labels:
      helm.sh/chart: helm-workshop-0.1.0
      app.kubernetes.io/name: helm-workshop
      app.kubernetes.io/instance: helm-workshop
      app.kubernetes.io/version: "1.16.0"
      app.kubernetes.io/managed-by: Helm
  ---
  # Source: helm-workshop/templates/service.yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: helm-workshop
    labels:
      helm.sh/chart: helm-workshop-0.1.0
      app.kubernetes.io/name: helm-workshop
      app.kubernetes.io/instance: helm-workshop
      [...]
  ```


### Built-in objects
Objects that are passed into a template from the template engine
* Release
* Values
* Chart
* Files
* Capabilities
* Templates

We can access to that info like this:
```yaml
{{ .Values.whatever }}
```

Specificity:
We can override values in this order
1. `Values.yaml`
2. Parent `Values.yaml`
3. `--set` parameters

We can specify which files need to use in order to add more values to that Built-in object:
`helm template example-chart -f values-o1.yaml --set key1.key2=value`

So in the last command we will add to the built in object that value and we'll override a specific key-value:
Before
```yaml
key1:
  key2: anything
```
After running the command
```yaml
key1:
  key2: value
```

### Template syntax
Based on Golang templates
* Actions --> `{{` `}}`
* Whitespace handling --> `{{ var }}` those whitespaces will remain, if we want to deleted them we need to add `{{-` and `-}}`
  * i.e `{{23 -}} > {{- 45}}` --> `23 > 45`
* Comments
  * `{{/* comment */}}`
  * `*{{ comments }}*`
* Default values --> `port {{default 80 .Values.server.port}}` If the value or the key doesn't exist will add 80 as a default value. The key parent needs to be defined or it will propmt an error
* Pipelines syntax --> UNIX `hostname: {{ .Values.server.host  | trim | lower }}`
* Functions syntax --> `functionName arg1 arg2`
* Flow control --> `if/else`,  `with` (to enter specific keys), `range`
  * i.e. `{{ if eq .Values.favorite.drink "coffee" }}mug: true {{ end }}`
  * i.e. `{{ with .values.favorite }} drink: {{ .drink }}`
  * i.e. `{{ range .Values.pizzaToppings }} - {{ . | title | quote }} {{ end }}`
    * the `.` means the element itself

Important: the file is in lowercase `values.yaml` but the built-in object needs to be in uppercase `.Values.key`

### Template composition
* `define`: declares new named templates
  * Can be externalized in `_underscoredFiles.tpl`
    * i.e. `_mychart.labels.tpl`
* `template`: imports / injects the specified named template


## Helm install
Command to install charts
* `helm install --create-namespace -n <namespace> <release_name> <path_to_chart>`
  * i.e. `helm install --create-namespace -n helm-workshop helm-workshop helm-workshop`
    * Output: 
    ```sh
    shop helm-workshop helm-workshop
    NAME: helm-workshop
    LAST DEPLOYED: Wed May  5 10:59:08 2021
    NAMESPACE: helm-workshop
    STATUS: deployed
    REVISION: 1
    NOTES:
    1. Get the application URL by running these commands:
      export POD_NAME=$(kubectl get pods --namespace helm-workshop -l "app.kubernetes.io/name=helm-workshop,app.kubernetes.io/instance=helm-workshop" -o jsonpath="{.items[0].metadata.name}")
      export CONTAINER_PORT=$(kubectl get pod --namespace helm-workshop $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
      echo "Visit http://127.0.0.1:8080 to use your application"
      kubectl --namespace helm-workshop port-forward $POD_NAME 8080:$CONTAINER_PORT
    ```

## Helm upgrade
By default we start with one pod `kubectl --namespace helm-workshop get pods`
```sh
NAME                            READY   STATUS    RESTARTS   AGE
helm-workshop-57689ffd8-hmln8   1/1     Running   0          6m19s
```

With upgrade we can change any values from the values.yaml and change it in the release:
* `helm upgrade <release> <Chart> --namespace <namespace>`
  * i.e `helm upgrade helm-workshop helm-workshop --namespace helm-workshop`
  * Output: `Release "helm-workshop" has been upgraded. Happy Helming!`

If we get the pods again: `kubectl --namespace helm-workshop get pods`
```sh
NAME                            READY   STATUS    RESTARTS   AGE
helm-workshop-57689ffd8-hmln8   1/1     Running   0          8m53s
helm-workshop-57689ffd8-w8wpf   1/1     Running   0          96s
```

## Helm Secrets
Check [encryption part](../encryption/encryption.md).

## Helm packages
* artifacthub.io --> Repository of charts

A chart can have subcharts

We can add repos with: ` helm repo add <org> <url>`
* i.e. `helm repo add bitnami https://charts.bitnami.com/bitnami`

In the Chart.yaml we can have a dependencies keys with
* name
* version
* repository


## Other commands:
* `helm version`: Print the client version
* `helm create <chart_name>`: Create a new chart with the given name (already viewed) 
* `helm delete`: Delete a release
* `helm search repo <repo_name>`: to search for specific repos that we have installed