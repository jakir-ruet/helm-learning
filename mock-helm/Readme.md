### Welcome to Helm Mock Exam
Add Repo
```bash
https://artifacthub.io/packages/helm/bitnami/nginx
helm repo add LocalRepoName https://charts.bitnami.com/bitnami
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo list
helm search repo nginx
helm search repo mysql
helm search repo wildfly
helm search repo wildfly --versions
helm search repo wildfly --version 13.4.1
helm search repo wildfly
helm search repo apache
```

Create Repo
```bash
helm repo update
helm install ReleaseName LocalRepoName/ChartName
helm install my-nginx mybitnami/nginx
helm list/ls
helm list/ls --output=yaml
kubectl get pod
```
