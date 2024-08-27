### Welcome to Chart Template
#### Introduction
An object in Helm is a structured data format, often represented as a `dictionary` or `map`, that holds **key-value pairs**. These objects can contain nested objects, `arrays`, `strings`, `integers`, and other types. Functions of Objects;
  - Objects are passed into a template from the template engine.
  - Objects can be simple, and have just one value or they can contain other objects or functions.
  - For example: the Release object contains several objects (like .Release.Name) and the Files object has a few functions.

##### Helm [Builtin Object](https://helm.sh/docs/chart_template_guide/builtin_objects/) Type
- Release
  - Release.Name
  - Release.Namespace
  - Release.IsUpgrade
  - Release.IsInstall
  - Release.Revision
  - Release.Service
- Values
- Chart
  - Subcharts
- Files
  - Files.Get
  - Files.GetBytes
  - Files.Glob
  - Files.Lines
  - Files.AsSecrets
  - Files.AsConfig
- Capabilities
  - Capabilities.APIVersions
  - Capabilities.APIVersions.Has
  - Capabilities.KubeVersion
  - Capabilities.KubeVersion.Major
  - Capabilities.KubeVersion.Minor
  - Capabilities.HelmVersion
  - Capabilities.HelmVersion.Version
  - Capabilities.HelmVersion.GitCommit
  - Capabilities.HelmVersion.GitTreeState
  - Capabilities.HelmVersion.GoVersion
- Template
  - Template.Name
  - Template.BasePath

**Sample Chart**
```bash
helm create mydemochat
```
Remove all content from `NOTES.txt`
```bash
helm install mydemoapp . --dry-run
```
