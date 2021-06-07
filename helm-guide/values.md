
- [Back](https://github.com/dptorri/k8s_intro/blob/master/README.md)
# Values files

### Update `./mychart/values.yaml`
```
usernameDB: superAdmin
```

Update `./mychart/template/configmap.yaml`

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  username: {{ .Values.usernameDB }}
```

`helm install miami-heat ./mychart --dry-run --debug`
```
install.go:173: [debug] Original chart version: ""
install.go:190: [debug] CHART PATH: ...k8s_intro/mychart

NAME: miami-heat
LAST DEPLOYED: Mon Jun  7 14:27:44 2021
...
COMPUTED VALUES:
usernameDB: superAdminDB
...
kind: ConfigMap
metadata:
  name: miami-heat-configmap
data:
  myvalue: "Hello World"
  username: superAdminDB
```
Because favoritusernameDBeDrink is set in the default values.yaml file to coffee, that's the value displayed in the template. We can easily override that by adding a --set flag in our call to helm install:

`helm install miami-heat ./mychart --dry-run --debug --set usernameDB=powerUser`
```
// mychart/templates/configmap.yaml
...
data:
  myvalue: "Hello World"
  username: powerUser
```
## Deleting a default key

If you need to delete a key from the default values, you may override the value of the key to be null, in which case Helm will remove the key from the overridden values merge.

