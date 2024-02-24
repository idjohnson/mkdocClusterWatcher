# mkdocClusterWatcher
Using mkdocs images to monitory a cluster for changes

# Installation

Example
```
$ helm upgrade --install mkdocs --create-namespace -n mkdocs ./mkdocsWatcher/
Release "mkdocs" has been upgraded. Happy Helming!
NAME: mkdocs
LAST DEPLOYED: Sat Feb 24 15:48:53 2024
NAMESPACE: mkdocs
STATUS: deployed
REVISION: 2
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace mkdocs -l "app.kubernetes.io/name=mkdocswatcher,app.kubernetes.io/instance=mkdocs" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace mkdocs $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace mkdocs port-forward $POD_NAME 8080:$CONTAINER_PORT
```

## Ingress

Here is an example Ingress yaml
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    cert-manager.io/cluster-issuer: azuredns-tpkpw
    ingress.kubernetes.io/ssl-redirect: "true"
    kubernetes.io/ingress.class: nginx
    kubernetes.io/tls-acme: "true"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.org/websocket-services: mkdocs-mkdocswatcher
  name: mkdocs-mkdocswatcher
spec:
  rules:
  - host: mkdocwatcher.tpk.pw
    http:
      paths:
      - backend:
          service:
            name: mkdocs-mkdocswatcher
            port:
              number: 8000
        path: /
        pathType: ImplementationSpecific
  tls:
  - hosts:
    - mkdocwatcher.tpk.pw
    secretName: mkdocs-mkdocswatcher-tls
```
