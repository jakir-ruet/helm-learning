[![LinkedIn][linkedin-shield-lapissoft]][linkedin-url-lapissoft]
[![Facebook-Page][facebook-shield-lapissoft]][facebook-url-lapissoft]
[![Youtube][youtube-shield-lapissoft]][youtube-url-lapissoft]

## Visit Us [Lapis Soft](http://www.lapissoft.com)
### Welcome Helm Learning
#### Introduction
Helm is a package manager for Kubernetes, which is an open-source platform for automating the `deployment`, `scaling`, and management of `containerized` applications. Helm helps you `define`, `install`, and `upgrade` even the most complex Kubernetes applications. It uses a packaging format called `charts`, which are collections of files that describe a related set of Kubernetes resources.

#### [Installing Helm](https://helm.sh/docs/intro/install/)
#### [Cheat Sheet](https://helm.sh/docs/intro/cheatsheet/)

#### [Charts](https://helm.sh/docs/topics/charts/)
Helm uses a packaging format called charts. A chart is a collection of files that describe a related set of Kubernetes resources. A single chart might be used to deploy something simple, like a memcached pod, or something complex, like a full web app stack with HTTP servers, databases, caches, and so on. For an example, chart name is `hello-world`.
```bash
.
└── hello-world
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

#### [Provenance and Integrity](https://helm.sh/docs/topics/provenance/)
Helm has provenance tools which help chart users verify the integrity and origin of a package. Using industry-standard tools based on Public Key Infrastructure (PKI), [GnuPG](https://gnupg.org/), and well-respected package managers, Helm can generate and verify signature files.

#### [The Workflow](https://helm.sh/docs/topics/provenance/)
This section describes a potential workflow for using provenance data effectively.

Prerequisites:
- A valid [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) keypair in a binary (not ASCII-armored) format
- The helm command line tool
- [GnuPG](https://gnupg.org/) command line tools (optional)
- Keybase command line tools (optional)

#### [Repository](https://helm.sh/docs/topics/chart_repository/)
A Helm repository is a storage location for Helm charts, making it easy to distribute, share, and manage applications packaged as Helm charts. Users can pull charts from repositories to install applications on their Kubernetes clusters or publish their own charts for others to use. The distributed community Helm chart repository is located at [Artifact Hub](https://artifacthub.io/)

Prerequisites:
The following prerequisites are required for a successful and properly secured use of Helm.
- A Kubernetes cluster
- Deciding what security configurations to apply to your installation, if any
- Installing and configuring Helm.

Key Components:
- Index File (`index.yaml`)
- Packaged Charts (`.tgz`)
- Repository URL

#### [OCI-based registries](https://helm.sh/docs/topics/registries/)
Beginning in Helm 3, you can use container registries with OCI support to store and share chart packages. Beginning in Helm v3.8.0, OCI support is enabled by default.

OCI (Open Container Initiative)-based registries are a type of container registry that follows the OCI standards for storing, distributing, and managing container images, as well as other types of artifacts like Helm charts, Open Policy Agent (OPA) bundles, and more. These registries offer a standardized, interoperable way to handle various types of software artifacts.

**Use hosted registries**
There are several hosted container registries with OCI support that you can use for your Helm charts. For example:
- Amazon ECR
- Azure Container Registry
- Docker Hub
- Google Artifact Registry
- Harbor
- IBM Cloud Container Registry
- JFrog Artifactory

#### [Helm Architecture](https://helm.sh/docs/topics/architecture/)
Helm is a package manager for Kubernetes that simplifies the deployment, management, and scaling of applications. Helm has evolved over time, and its architecture reflects this progression. Below is an overview of Helm’s architecture, particularly focusing on Helm v3, which is the latest major version.

It simplifies the process of deploying and managing applications on Kubernetes clusters. Helm consists of **two** main components: the `Helm client` and the `Helm server (Tiller)`. The client is used to interact with the server and manage charts, while the server contains all the necessary information about available charts.
![Helm](/img/helm.png)

**Function of these components**
- **Client:** The client (CLI), resides in the local workstation.
- **Server (Tiller):** The server (Tiller), resides in the Kubernetes cluster to execute what’s needed.

1. Helm Client (CLI)
- Role: The Helm client is a command-line interface (CLI) tool that users interact with directly. It handles local operations such as chart creation, packaging, and interaction with the Kubernetes API server.
- Functions:
  - Install/Upgrade/Delete Releases: The client can install, upgrade, or delete applications (known as releases) on a Kubernetes cluster.
  - Template Rendering: Before applying charts to the cluster, the client renders the templates locally, substituting values as specified.
  - Manage Repositories: Users can add, update, and remove chart repositories.
  - Package and Share Charts: The client can package charts and upload them to a chart repository.
  - Rollback: It can rollback releases to previous versions if necessary.
  - Interact with Kubernetes API: The Helm client communicates directly with the Kubernetes API server to manage resources.
2. Helm Chart
- Role: A Helm chart is a package that contains all the resource definitions necessary to run an application or service inside a Kubernetes cluster.
- Components:
  - Chart.yaml: Metadata about the chart, including the name, version, and description.
  - Values.yaml: Default configuration values for the chart, which can be overridden by the user.
  - Templates: Kubernetes resource templates (e.g., Deployments, Services, Ingress) that are rendered during installation.
  - Helpers (_helpers.tpl): Reusable template snippets to avoid repetition in templates.
  - Charts Directory: A place to add dependencies (sub-charts).
  - README.md: Optional documentation for the chart.
3. Kubernetes Cluster [Server (Tiller)]
- Role: The Kubernetes cluster is where Helm deploys and manages applications. It is the environment that runs the containerized applications.
- Components:
  - Kubernetes API Server: The central management entity that handles requests to create, update, or delete Kubernetes resources. Helm interacts with the API server to manage the lifecycle of applications.
  - Kubernetes Objects: Helm interacts with Kubernetes objects like Deployments, Services, ConfigMaps, and Secrets by sending the rendered templates to the Kubernetes API server.
4. Helm Repository
- Role: A Helm repository is a storage location for Helm charts, allowing users to share and distribute charts. Helm clients can download charts from these repositories to install or upgrade applications.
- Components:
  - Index.yaml: A file that lists all available charts in the repository along with their versions and metadata.
  - Packaged Charts: The .tgz files that contain the chart data.
  - Repository URL: The address of the repository, which can be added to Helm clients for easy access to charts.
5. Releases
- Role: A release is an instance of a chart that has been deployed to a Kubernetes cluster. Each release is associated with a specific version of a chart and a set of configuration values.
- Components:
  - Release Name: A unique identifier for each deployment of a chart.
  - Release History: Helm maintains a history of releases for a given chart, allowing users to roll back to previous versions if necessary.

**Helm Chart Architecture**
![Helm](/img/helm-architecture.png)

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
