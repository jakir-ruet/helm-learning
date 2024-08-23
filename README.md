[![LinkedIn][linkedin-shield-lapissoft]][linkedin-url-lapissoft]
[![Facebook-Page][facebook-shield-lapissoft]][facebook-url-lapissoft]
[![Youtube][youtube-shield-lapissoft]][youtube-url-lapissoft]

## Visit Us [Lapis Soft](http://www.lapissoft.com)
### Welcome Helm Learning
#### Introduction
Helm is a package manager for Kubernetes, which is an open-source platform for automating the `deployment`, `scaling`, and management of `containerized` applications. Helm helps you `define`, `install`, and `upgrade` even the most complex Kubernetes applications. It uses a packaging format called `charts`, which are collections of files that describe a related set of Kubernetes resources.

It simplifies the process of deploying and managing applications on Kubernetes clusters. Helm consists of **two** main components: the `Helm client` and the `Helm server (Tiller)`. The client is used to interact with the server and manage charts, while the server contains all the necessary information about available charts.
![Helm](/img/helm.png)

**Function of these components**
- **Client:** The client (CLI), resides in the local workstation.
- **Server (Tiller):** The server (Tiller), resides in the Kubernetes cluster to execute what’s needed.

#### [Installing Helm](https://helm.sh/docs/intro/install/)
#### [Cheat Sheet](https://helm.sh/docs/intro/cheatsheet/)

#### [Charts](https://helm.sh/docs/topics/charts/)
Helm uses a packaging format called charts. A chart is a collection of files that describe a related set of Kubernetes resources. A single chart might be used to deploy something simple, like a memcached pod, or something complex, like a full web app stack with HTTP servers, databases, caches, and so on.
```bash
demo-chart/
│
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
└── charts/
```
**Chart Architecture**
![Helm](/img/helm-architecture.png)

**Chart Types**
- Application: It is the `default` type and it is the standard chart which can be operated on fully. These charts are designed to deploy a specific application or a set of closely related applications.
  
  For example: A Helm chart for deploying an `Nginx` web server, a `PostgreSQL` database, or a complete web application stack (e.g., frontend, backend, database).
- [Library](https://helm.sh/docs/topics/library_charts/): Its provides utilities or functions for the chart builder. These are charts that are not meant to be deployed directly. Instead, they are intended to be used as dependencies by other charts. Library charts provide reusable snippets or helper templates that can be included in other charts.
  
  For example: A library chart might provide common configuration templates for `logging`, `monitoring`, or `security` settings that other charts can use.

#### [Chart Hooks](https://helm.sh/docs/topics/charts_hooks/)
Helm provides a hook mechanism to allow chart developers to intervene at certain points in a release's life cycle. For example, you can use hooks to:
- Load a `ConfigMap` or `Secret` during install before any other charts are loaded.
- Execute a Job to back up a database before installing a new chart, and then execute a second job after the upgrade in order to restore data.
- Run a Job before deleting a release to gracefully take a service out of rotation before removing it.

Hooks work like regular templates, but they have special annotations that cause Helm to utilize them differently. In this section, we cover the basic usage pattern for hooks.

#### [The Workflow](https://helm.sh/docs/topics/provenance/)
This section describes a potential workflow for using provenance data effectively.

Prerequisites:
- A valid [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) keypair in a binary (not ASCII-armored) format
- The helm command line tool
- [GnuPG](https://gnupg.org/) command line tools (optional)
- Keybase command line tools (optional)

## Courtesy of Jakir

[![LinkedIn][linkedin-shield-jakir]][linkedin-url-jakir]
[![Facebook-Page][facebook-shield-jakir]][facebook-url-jakir]
[![Youtube][youtube-shield-jakir]][youtube-url-jakir]

### Have a good day, stay with me
<!-- Personal profile -->

[linkedin-shield-jakir]: https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url-jakir]: https://www.linkedin.com/in/jakir-ruet/
[facebook-shield-jakir]: https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white
[facebook-url-jakir]: https://www.facebook.com/jakir-ruet/
[youtube-shield-jakir]: https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white
[youtube-url-jakir]: https://www.youtube.com/@mjakaria-ruet/featured

<!-- Company profile -->

[linkedin-shield-lapissoft]: https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url-lapissoft]: https://www.linkedin.com/company/lapis-soft/
[facebook-shield-lapissoft]: https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white
[facebook-url-lapissoft]: https://www.facebook.com/GoLapisSoft/
[youtube-shield-lapissoft]: https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white
[youtube-url-lapissoft]: https://www.youtube.com/@LapisSoft/featured
