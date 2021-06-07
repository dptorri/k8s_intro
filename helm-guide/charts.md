- [Back](https://github.com/dptorri/k8s_intro/blob/master/README.md)

### Create a Helm chart

`helm create mychart`
```
Creating mychart
.
├── Chart.yaml
├── charts
├── templates
│   ├── NOTES.txt
│   ├── _helpers.tpl
│   ├── deployment.yaml
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── service.yaml
│   ├── serviceaccount.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml

```
If you take a look at the mychart/templates/ directory, you'll notice a few files already there.

- NOTES.txt: The "help text" for your chart. This will be displayed to your users when they run helm install.
- deployment.yaml: A basic manifest for creating a Kubernetes deployment
- service.yaml: A basic manifest for creating a service endpoint for your deployment
- _helpers.tpl: A place to put template helpers that you can re-use throughout the chart

`rm -rf mychart/templates/*`

Removes all files inside templates.
### Create a ConfigMap
We recommend using the suffix .yaml for YAML files and .tpl for helpers. With this template we have an installable chart

```
// mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart-configmap
data:
  myvalue: "Hello World"
```
`helm install full-coral ./mychart`
```
NAME: full-coral
LAST DEPLOYED: Fri Jun  4 16:01:43 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```
Using Helm, we can retrieve the release and see the actual template that was loaded.

`helm get manifest full-coral`
```
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart-configmap
data:
  myvalue: "Hello World"
```
`helm uninstall full-coral`
```
release "full-coral" uninstalled
```

## Add a template call

We might want to generate a name field of the k8s resource by inserting the release name.

A template directive is enclosed in `{{ and }}` blocks.

This reads: 
"start at the top namespace, find the Release object, then look inside of it for an object called Name".
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
```
Now when we install our resource, we'll immediately see the result of using this template directive:

`helm install clunky-serval ./mychart`

```
NAME: clunky-serval
LAST DEPLOYED: Mon Jun  7 10:32:16 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None

// check the manifest: 

# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: clunky-serval-configmap
data:
  myvalue: "Hello World"
```

## Check rendered templates without installing them
`helm install --debug --dry-run goodly-gumpy ./mychart`

This will render the templates. But instead of installing the chart, it will return the rendered template to you so you can see the output:
```
install.go:173: [debug] Original chart version: ""
install.go:190: [debug] CHART PATH: .../k8s_intro/mychart

NAME: clunky-serval
LAST DEPLOYED: Mon Jun  7 10:50:31 2021
NAMESPACE: default
STATUS: pending-install
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
{}

COMPUTED VALUES:
affinity: {}
autoscaling:
...
HOOKS:
MANIFEST:
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: clunky-serval-configmap
  ```

