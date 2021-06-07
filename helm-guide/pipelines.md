- [Back](https://github.com/dptorri/k8s_intro/blob/master/README.md)

## Template Functions and Pipelines
`username: {{ .Values.usernameDB | quote | repeat 2 }}`
```
data:
  myvalue: "Hello World"
  username: "superAdminDBsuperAdminDB"
```
`username: {{ .Values.usernameDB | default "tea" | upper }}`
```
// comment out # usernameDB: superAdminDB 
data:
  myvalue: "Hello World"
  username: "TEA"
```

### Add a conditional property

```
data:
  myvalue: "Hello World"
  username: {{ .Values.usernameDB | quote }}
  {{ if eq .Values.usernameDB "superAdminDB" }}
  mug: true
  {{ end }}

  myvalue: "Hello World"
  username: "superAdminDB"
  
  mug: true
```
### Fix whitespace
```
  {{- if eq .Values.usernameDB "superAdminDB" }}
  mug: true
  {{- end }}
```
