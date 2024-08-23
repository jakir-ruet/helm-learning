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
A workflow in the context of Helm and Kubernetes refers to the sequence of steps or processes involved in deploying, managing, and maintaining applications on Kubernetes clusters. This typically includes tasks like provisioning infrastructure, deploying applications, configuring environments, and managing updates. Below is an example of a typical workflow for deploying an application using Helm, along with CI/CD integration, monitoring, and maintenance.

Prerequisites:
- A valid [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) keypair in a binary (not ASCII-armored) format
- The helm command line tool
- [GnuPG](https://gnupg.org/) command line tools (optional)
- Keybase command line tools (optional)

##### Step Helm Workflow
1. Setup and Initialization: Ensure Helm is installed on your local machine or CI/CD environment.
```bash
brew install helm  # On macOS
```
Add the necessary Helm chart repositories.
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```
Configure Kubernetes Context: Make sure your Kubernetes context is set up and configured to point to the correct cluster.
```bash
kubectl config use-context my-cluster
```
2. Development and Customization (Create or Customize Helm Charts): Develop or customize Helm charts to define your application and its dependencies. Modify the `values.yaml` file to configure the deployment.
Example values.yaml:
```yaml
image:
  repository: my-app
  tag: "1.0.0"
replicaCount: 3
resources:
  limits:
    cpu: 500m
    memory: 512Mi
```
- Use Helm Hooks:
- Implement Helm hooks to run specific actions during different phases of the release lifecycle, such as pre-install or post-upgrade tasks.
3. Deployment (Install the Application): Deploy your application to the Kubernetes cluster using Helm.
```bash
helm install my-release ./my-chart -f values.yaml
```
- Verify Deployment: Confirm that the application has been deployed correctly by checking the Kubernetes resources.
```bash
kubectl get pods,svc -n my-namespace
```
1. Continuous Integration/Continuous Deployment (CI/CD): Set up a CI/CD pipeline using tools like Jenkins, GitLab CI, GitHub Actions, or CircleCI to automate the deployment process.
```yaml
name: CI/CD Pipeline
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Set up Helm
        uses: aws/setup-helm@v1
      - name: Install dependencies
        run: helm dependency update ./my-chart
      - name: Deploy to Kubernetes
        run: helm upgrade --install my-release ./my-chart -f values.yaml
```
Automate Testing: Use Helm plugins like helm unittest to automate testing of your Helm charts during the CI/CD pipeline.
1. Monitoring and Logging: Deploy monitoring tools such as Prometheus and Grafana using Helm charts to monitor the health of your application and Kubernetes cluster.
```bash
helm install prometheus bitnami/prometheus
helm install grafana bitnami/grafana
```
Integrate Logging: Use tools like Elasticsearch, Fluentd, and Kibana (EFK stack) for centralized logging, also deployable via Helm charts.
```bash
helm install efk bitnami/efk
```
6. Updating and Upgrading (Rolling Updates): Update your application by modifying the values.yaml file or upgrading the chart version, and then use Helm to apply the changes.
```bash
helm upgrade my-release ./my-chart -f values.yaml
```
Rollback if Necessary: If an upgrade fails or introduces issues, roll back to a previous release.
```bash
helm rollback my-release 1
```
7. Maintenance
- Regular Backups: Implement regular backups for critical data and Kubernetes configurations.
- Security Patching: Ensure that both the Kubernetes cluster and the deployed applications are regularly patched and updated for security vulnerabilities.
- Resource Scaling: Adjust resource allocations and replica counts as needed to handle varying loads on your application.
```bash
helm upgrade my-release ./my-chart --set replicaCount=5
```
Chart Versioning: Maintain version control over your Helm charts to track changes and ensure consistency across environments.
8. Disaster Recovery
- Backup and Restore: Use Helm to manage backup and restore processes. For example, schedule database backups and store them securely.
- High Availability: Deploy applications in a highly available configuration, such as multi-zone or multi-region deployments, using Helm.

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
- aws Container Registry
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

#### [Advanced Helm Techniques](https://helm.sh/docs/topics/advanced/)
Advanced Helm techniques can significantly enhance how you manage Kubernetes applications, providing more flexibility, automation, and robustness in your deployment workflows. Below are some advanced Helm techniques:

1. Subcharts and Global Values
- Subcharts: Subcharts are Helm charts that are included as dependencies of a parent chart. They allow you to manage complex applications with multiple components. For instance, a parent chart could represent an entire application, while subcharts represent individual microservices.
- Global Values: In complex charts with multiple subcharts, global values can be used to set values that are shared across all subcharts. This helps in maintaining consistency and reducing redundancy.
```yaml
# values.yaml
global:
  image:
    registry: myregistry.com

subchart1:
  image:
    name: service1
    tag: v1.0.0

subchart2:
  image:
    name: service2
    tag: v2.0.0
```

2. Helmfile
Helmfile is a tool for managing multiple Helm charts with a declarative approach. It allows you to define and manage Helm releases in a single file, making it easier to handle complex deployments.
- Use Cases:
  - Managing multiple environments.
  - Coordinating the deployment of multiple charts.
  - Ensuring consistency across multiple clusters.
```yaml
# helmfile.yaml
releases:
  - name: my-app
    namespace: default
    chart: ./charts/my-app
    values:
      - values.yaml
  - name: my-database
    namespace: database
    chart: stable/postgresql
    values:
      - db-values.yaml
```
You can then deploy everything with:
```bash
helmfile sync
```

3. Functions Templating and Pipelines
Helm templates support advanced functions and pipelines that allow you to manipulate values and strings in sophisticated ways.
- Common Templating Functions:
  - `default`: Provides a default value if the original value is empty.
  - `required`: Ensures a value is provided, otherwise an error is thrown.
  - `trim`, `quote`, `replace`: String manipulation functions.
  - `lookup`: Allows you to query the Kubernetes cluster for resources during the rendering process.
```yaml
# templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}
spec:
  replicas: {{ .Values.replicas | default 1 }}
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
          env:
            - name: ENV
              value: {{ .Values.environment | quote | default "production" }}
```

4. Helm Hooks
Hooks allow you to run specific Kubernetes resources or Helm templates at particular points in a release lifecycle (e.g., before install, after install, before delete).
- Use Cases:
  - Running database migrations before deploying an application.
  - Cleaning up resources after a release is deleted.
  - Performing custom validation or pre-checks before an installation.
```yaml
# templates/job-migrate.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: "{{ .Release.Name }}-migrate"
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation
spec:
  template:
    spec:
      containers:
        - name: migrate
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          command: ["./migrate.sh"]
      restartPolicy: OnFailure
```

5. Helm Test
Helm allows you to define and run tests for your Helm releases. This can be used to verify that a deployment is functioning as expected.
- Use Cases:
  - Post-deployment validation (e.g., ensuring a web service is reachable).
  - Running smoke tests to verify core functionality.
Creating Tests: Add templates with the `helm.sh/hook: test` annotation to define what should be tested.
```yaml
# templates/test-connection.yaml
apiVersion: v1
kind: Pod
metadata:
  name: "{{ .Release.Name }}-test-connection"
  annotations:
    "helm.sh/hook": test
spec:
  containers:
    - name: wget
      image: busybox
      command: ['wget']
      args: ['{{ template "my-app.fullname" . }}:8080']
  restartPolicy: Never
```
You can then run the test with:
```bash
helm test my-release
```

6. Helm Chart Linting
- Linting your Helm charts helps catch common issues, ensuring that your charts follow best practices and are correctly formatted.
- The `helm lint` command can be used to validate the syntax and structure of your charts before deploying them.
```bash
helm lint my-chart/
```

7. Helm Plugins
Helm supports plugins, which extend its functionality with custom commands. You can create your own plugins or use those created by the community.
- Use Cases:
  - Automating repetitive tasks.
  - Integrating Helm with other tools.
  - Extending Helm’s capabilities.
Installing a Plugin:
```bash
helm plugin install https://github.com/databus23/helm-diff
```

8. OCI Registry for Helm Charts
Helm v3 supports OCI (Open Container Initiative) registries, allowing you to store Helm charts in container registries like Docker Hub, AWS ECR, or aws ACR.
- Use Cases:
  - Storing charts in a secure, centralized location.
  - Leveraging existing container registries for chart distribution.
```bash
# Push a chart to an OCI registry
helm chart save ./my-chart oci://myregistry.com/my-repo
helm chart push oci://myregistry.com/my-repo/my-chart:1.0.0
# Pull a chart from an OCI registry
helm chart pull oci://myregistry.com/my-repo/my-chart:1.0.0
```

9. Continuous Integration/Continuous Deployment (CI/CD) with Helm
Helm can be integrated into CI/CD pipelines to automate the deployment of applications.
- Use Cases:
  - Automatically deploying charts to staging or production environments.
  - Running Helm tests as part of the pipeline.
  - Rolling back releases automatically if tests fail.
Example: Using tools like Jenkins, GitLab CI, or GitHub Actions to deploy charts.

Example GitLab CI configuration:
```yaml
deploy:
  stage: deploy
  script:
    - helm upgrade --install my-release ./my-chart
  environment:
    name: production
    url: https://my-app.com
```

10. Helm Secrets Management
Managing sensitive information like passwords and API keys is critical in Kubernetes deployments. Helm supports integrating secrets management tools.
Tools: Helm Secrets, SOPS, Sealed Secrets, or using Kubernetes Secret resources.
- Use Cases:
  - Encrypting sensitive values in `values.yaml`.
  - Storing secrets in a secure backend like AWS KMS, HashiCorp Vault, or aws Key Vault.
Example with Helm Secrets:
```bash
# Encrypt a values file
sops -e secrets.yaml > encrypted-secrets.yaml
# Use the encrypted file in your Helm deployment
helm secrets upgrade --install my-release ./my-chart -f encrypted-secrets.yaml
```
**NB:**
These advanced techniques can greatly enhance your use of Helm, providing more control, automation, and security in your Kubernetes environments.

#### [Kubernetes Distribution](https://helm.sh/docs/topics/kubernetes_distros/)
A Kubernetes distribution is a packaged version of Kubernetes that includes additional tools, services, and sometimes custom features or configurations to simplify or enhance the Kubernetes experience. These distributions are often tailored for specific use cases, environments (on-premises, cloud, hybrid), or organizational needs. Some popular Kubernetes distributions mention below;
- Amazon Elastic Kubernetes Service (EKS)
- Google Kubernetes Engine (GKE)
- aws Kubernetes Service (AKS)
- Red Hat OpenShift

#### [Role-based Access Control](https://helm.sh/docs/topics/rbac/)
Role-Based Access Control (RBAC) in Helm is used to manage permissions for users and applications when interacting with Kubernetes resources via Helm. Since Helm interacts with the Kubernetes API to install, upgrade, and manage applications (Helm charts), RBAC is crucial for controlling who can perform these actions.

##### How RBAC Applies to Helm
###### Helm Commands and Kubernetes API:
- Helm commands such as `helm install`, `helm upgrade`, `helm rollback`, and `helm delete` result in API calls to the Kubernetes cluster. These calls need appropriate permissions, which are managed using RBAC.
- For example, installing a Helm chart typically requires the ability to create various resources like Deployments, Services, and ConfigMaps.

###### Service Accounts and Permissions:
- Helm typically operates using a service account in the Kubernetes cluster. The permissions of this service account determine what operations Helm can perform.
- By default, if you do not specify a service account, Helm uses the default service account in the namespace where it is operating. This account's permissions can be modified to restrict or expand what Helm can do.

##### Creating RBAC Resources for Helm
To properly use Helm with RBAC, you usually need to create a Role or ClusterRole and bind it to a ServiceAccount using a RoleBinding or ClusterRoleBinding.
- Creating a Service Account: You can create a dedicated service account for Helm in a specific namespace.
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: helm-service-account
  namespace: default
```
- Create a Role or ClusterRole: Define a `Role` or `ClusterRole` that specifies the permissions needed by Helm. For instance, if Helm needs to manage resources like Deployments, Pods, and Services.
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: helm-role
rules:
- apiGroups: [""]
  resources: ["pods", "services", "configmaps"]
  verbs: ["get", "list", "watch", "create", "update", "delete"]
- apiGroups: ["apps"]
  resources: ["deployments", "replicasets"]
  verbs: ["get", "list", "watch", "create", "update", "delete"]
```
If you need the role to apply across all namespaces, you would use a `ClusterRole`.
- Bind the Role to the Service Account: Bind the `Role` or `ClusterRole` to the Helm service account using a `RoleBinding` or `ClusterRoleBinding`.
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: helm-role-binding
  namespace: default
subjects:
- kind: ServiceAccount
  name: helm-service-account
  namespace: default
roleRef:
  kind: Role
  name: helm-role
  apiGroup: rbac.authorization.k8s.io
```
- Using the Service Account with Helm: When running Helm commands, you can specify the service account to use.
```bash
helm install my-release my-chart --namespace default --service-account helm-service-account
```

###### Common Use Cases for Helm with RBAC
- Restricted Environments:
  - In environments with strict security requirements, you might need to limit the permissions of Helm so that it can only operate on specific namespaces or resources.
- Multi-Tenant Clusters:
  - In multi-tenant Kubernetes clusters, each tenant might have their own Helm service account with RBAC policies that restrict them to their own namespace.
- Automation and CI/CD Pipelines:
  - When using Helm in CI/CD pipelines, you often create a service account with the exact permissions needed to deploy applications, avoiding unnecessary privileges.
- Namespace Isolation:
  - Helm can be used in a way that it can only deploy resources within a single namespace, preventing it from affecting resources in other namespaces.

#### [Helm Plugins](https://helm.sh/docs/topics/plugins/)
Helm plugins are extensions that add new functionality to Helm, allowing users to customize and enhance the Helm toolchain. Plugins can introduce new commands or modify existing behaviors, making Helm more flexible and powerful for specific use cases.

##### Key Concepts of Helm Plugins
- Installation: Helm plugins can be easily installed from a Git repository, local path, or Helm plugin index.
- Development: Plugins are typically written in Go, Bash, or any scripting language that can be executed on the command line.
- Usage: Once installed, a plugin's commands are available directly via the helm command-line interface.

###### Managing Helm Plugins
| Command                                       | Work                            |
| :-------------------------------------------- | :------------------------------ |
| helm plugin list                              | Check list of installed plugins |
| helm plugin install <URL/>path>               | Plugin install                  |
| helm plugin update <plugin-name>              | Update plugin                   |
| helm plugin remove <plugin-name>              | Remove plugin                   |
| helm plugin show <plugin-name>                | Show plugin                     |
| helm diff upgrade <release-name> <chart-path> | Compares a Helm release's       |

###### Benefits of Helm Plugins
- Customization: Extend Helm’s functionality to meet specific project or organizational needs.
- Automation: Automate repetitive tasks and integrate Helm with other tools and workflows.
- Community Contributions: Leverage and contribute to the wide range of open-source plugins available in the Helm ecosystem.

#### [Permissions management for SQL storage backend](https://helm.sh/docs/topics/permissions_sql_storage_backend/)
Managing permissions for an SQL storage backend in the context of Helm typically involves deploying a Helm chart that includes database configurations and ensuring that the appropriate permissions are set up for the database users. Helm can automate the deployment of these configurations across different environments, ensuring consistency and security.

##### Steps for Managing SQL Permissions with Helm
- Define Database Credentials in `values.yaml`
```yaml
database:
  name: my_database
  user: app_user
  password: secure_password
  rootPassword: root_secure_password
roles:
  - name: read_only
    permissions:
      - type: table
        table: my_table
        actions: ["SELECT"]
  - name: admin
    permissions:
      - type: database
        actions: ["ALL"]
```
- Create Kubernetes Secrets for Sensitive Data in `chart-yaml`
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  db-user: {{ .Values.database.user | b64enc }}
  db-password: {{ .Values.database.password | b64enc }}
  db-root-password: {{ .Values.database.rootPassword | b64enc }}
```
- Deploy Database with Helm (If exist)
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-release bitnami/postgresql -f values.yaml
```
- Create SQL Scripts to Set Up Roles and Permissions in `init.sql`
```bash
CREATE ROLE read_only;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO read_only;

CREATE ROLE admin;
GRANT ALL PRIVILEGES ON DATABASE {{ .Values.database.name }} TO admin;

CREATE USER {{ .Values.database.user }} WITH ENCRYPTED PASSWORD '{{ .Values.database.password }}';
GRANT read_only TO {{ .Values.database.user }};
```
- Execute SQL Scripts During Deployment in `deployment.yaml`. Use Helm hooks to execute these SQL scripts when the database is deployed or upgraded. Hooks like `post-install` or `post-upgrade` can be utilized.
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: db-init
  annotations:
    "helm.sh/hook": post-install,post-upgrade
spec:
  template:
    spec:
      containers:
        - name: init-db
          image: postgres:13
          env:
            - name: PGPASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-credentials
                  key: db-root-password
          command: [ "psql" ]
          args:
            - "--host=localhost"
            - "--username=postgres"
            - "--dbname={{ .Values.database.name }}"
            - "--file=/scripts/init.sql"
          volumeMounts:
            - name: script-volume
              mountPath: /scripts
              subPath: init.sql
      volumes:
        - name: script-volume
          configMap:
            name: init-scripts
      restartPolicy: OnFailure
```
- Manage Updates and Rollbacks
```bash
helm upgrade my-release bitnami/postgresql -f values.yaml
```
- Use Helm Secrets or Helm Secrecy Tools: Consider using Helm plugins like `helm-secrets` to manage sensitive data securely if you need to keep secrets out of source control.
```bash
helm-chart/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── secret.yaml
│   ├── deployment.yaml
│   ├── configmap.yaml
│   └── job.yaml
└── scripts/
    └── init.sql
```

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
