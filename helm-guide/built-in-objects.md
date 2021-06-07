- [Back](https://github.com/dptorri/k8s_intro/README.md)

### Built-in Objects

`Release`: This object describes the release itself. It has several objects inside of it:

- `Release.Name`: The release name
- `Release.Namespace`: The namespace to be released into (if the manifest doesn’t override)
- `Release.IsUpgrade`: This is set to true if the current operation is an upgrade or rollback.
- `Release.IsInstall`: This is set to true if the current operation is an install.
- `Release.Revision`: The revision number for this release. On install, this is 1, and it is incremented with each upgrade and rollback.
- `Release.Service`: The service that is rendering the present template. On Helm, this is always Helm.

`Values`: Values passed into the template from the values.yaml file and from user-supplied files. By default, Values is empty.

`Chart`: The contents of the Chart.yaml file. Any data in Chart.yaml will be accessible here. For example `{{ .Chart.Name }}-{{ .Chart.Version }}` will print out the mychart-0.1.0.
```
apiVersion: The chart API version (required)
name: The name of the chart (required)
version: A SemVer 2 version (required)
kubeVersion: A SemVer range of compatible Kubernetes versions (optional)
description: A single-sentence description of this project (optional)
type: The type of the chart (optional)
keywords:
  - A list of keywords about this project (optional)
home: The URL of this projects home page (optional)
sources:
  - A list of URLs to source code for this project (optional)
dependencies: # A list of the chart requirements (optional)
  - name: The name of the chart (nginx)
    version: The version of the chart ("1.2.3")
    repository: (optional) The repository URL ("https://example.com/charts") or alias ("@repo-name")
    condition: (optional) A yaml path that resolves to a boolean, used for enabling/disabling charts (e.g. subchart1.enabled )
    tags: # (optional)
      - Tags can be used to group charts for enabling/disabling together
    import-values: # (optional)
      - ImportValues holds the mapping of source values to parent key to be imported. Each item can be a string or pair of child/parent sublist items.
    alias: (optional) Alias to be used for the chart. Useful when you have to add the same chart multiple times
maintainers: # (optional)
  - name: The maintainers name (required for each maintainer)
    email: The maintainers email (optional for each maintainer)
    url: A URL for the maintainer (optional for each maintainer)
icon: A URL to an SVG or PNG image to be used as an icon (optional).
appVersion: The version of the app that this contains (optional). Needn't be SemVer. Quotes recommended.
deprecated: Whether this chart is deprecated (optional, boolean)
annotations:
  example: A list of annotations keyed by name (optional).
```

`Files`: This provides access to all non-special files in a chart. While you cannot use it to access templates, you can use it to access other files in the chart. See the section Accessing Files for more.
- `Files.Get` is a function for getting a file by name (.Files.Get config.ini)
- `Files.GetBytes` is a function for getting the contents of a file as an array of bytes instead of as a string. This is useful for things like images.
- `Files.Glob` is a function that returns a list of files whose names match the given shell glob pattern.
- `Files.Lines` is a function that reads a file line-by-line. This is useful for iterating over each line in a file.
- `Files.AsSecrets` is a function that returns the file bodies as Base 64 encoded strings.
- `Files.AsConfig` is a function that returns file bodies as a YAML map.

`Capabilities`: This provides information about what capabilities the Kubernetes cluster supports.
- `Capabilities.APIVersions` is a set of versions.
- `Capabilities.APIVersions.Has $version` indicates whether a version (e.g., batch/v1) or resource (e.g., apps/v1/Deployment) is available on the cluster.
- `Capabilities.KubeVersion and Capabilities.KubeVersion.Version` is the Kubernetes version.
- `Capabilities.KubeVersion.Major` is the Kubernetes major version.
- Capabilities.KubeVersion.Minor` is the Kubernetes minor version.

`Template`: Contains information about the current template that is being executed
- `Template.Name`: A namespaced file path to the current template (e.g. mychart/templates/mytemplate.yaml)
- `Template.BasePath`: The namespaced path to the templates directory of the current chart (e.g. mychart/templates).