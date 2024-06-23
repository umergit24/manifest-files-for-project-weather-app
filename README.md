# argo-cd-project-for-weather-app
argo cd with minikube

## Install Argo CD In Kubernetes Cluster

### Install
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Check the services and pods if they are ready:



### Run the UI
```
kubectl port-forward -n argocd svc/argocd-server 8080:443
```

### Get the Password
```
kubectl -n argocd get secret argocd-initial-admin-secret -o yaml
```


### Decode the Password
```
echo  <password_hash> | base64 --decode
```

> [!NOTE]
> username: admin
> password: <decoded_password>

### Create Argo CD Config File
app.yaml
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: myapp
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/umergit24/manifest-files-for-project-weather-app
    targetRevision: HEAD
    path: code
  destination:
    server: https://kubernetes.default.svc
    namespace: myapp

  syncPolicy:
    syncOptions:
      - CreateNamespace=true

    automated:
      selfHeal: true
      prune: true
```

```
kubectl apply -f application.yaml
```

![Pasted image 20240623200634](https://github.com/umergit24/argo-cd-project-for-weather-app/assets/172115564/2032d6d7-608c-4593-a5ea-ebe1dc228e30)

The application has been updated.

A look inside the configuration:
![Pasted image 20240623202013](https://github.com/umergit24/argo-cd-project-for-weather-app/assets/172115564/b0bb40a4-f265-4e0e-be50-84fa08f9d91a)

Argo CD has created the service and the deployment automatically. This is very impressive.

Now, let's change the nodeport to 5000 and see how long it takes for the update to be applied.
update: it did(Dont have ss)
