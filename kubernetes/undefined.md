# 명령어

**Create an NGINX Pod**

```bash
kubectl run nginx --image=nginx
```

**Generate POD Manifest YAML file \(-o yaml\). Don't create it\(--dry-run\)**

```text
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

**Create a deployment**

```text
kubectl create deployment --image=nginx nginx
```

**Generate Deployment YAML file \(-o yaml\). Don't create it\(--dry-run\)**

```text
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

**Generate Deployment YAML file \(-o yaml\). Don't create it\(--dry-run\) with 4 Replicas \(--replicas=4\)**

```text
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

pod와 service를 동시에 실행. pod와 service는 묶여있음

```text
kubectl run httpd --image=httpd:alpine --port 80 --expose
```

