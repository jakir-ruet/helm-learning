Check repo
```bash
helm repo list
```
Add repo
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
Create redis chart
```bash
helm install my-redis bitnami/redis --version 20.0.3
kubectl get pod
kubectl get all
```
Search repo
```bash
helm search repo bitnami
```
Search repo in hub
```bash
helm search hub bitnami
helm search hub bitnami | wc -l
```
Remove repo
```bash
helm repo remove repoName
```