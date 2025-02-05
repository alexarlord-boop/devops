# What is Ingress
Usually, for testing or demo purposes an external service is used, like `NodePort` or `LoadBalancer`. The external url contains http, exposed IP address and a specific port. This is not user-friendly and not secure for production.

Ingress is a K8s component, that allows keeping IP address and port not exposed, forwarding external request from end-user to internal service, not external.

# YAML configuration
Ingress:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
#   namespace: my-namespace
spec:
  rules:
  - host: myapp.com
    http:
      paths:
      - backend:
            serviceName: myapp-internal-service
            servicePort: 8080

```
Internal service:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-internal-service
spec:
    selector:
        app: myapp
    ports:
        - protocol: TCP
        port: 8080
        targetPort: 8080
```


# Ingress Controller
Ingress configuration is not enough, we need an Ingress Controller, that will evaluate and implement the rules of the Ingress object.

Ingress Controller pod = entrypoint to the cluster.
Ingress Controller is a 3d party tool, like Nginx, Traefik, HAProxy, etc.

> Most Cloud providers have their own load balancer, like GCP, AWS, Azure, so you don't have to implement your own.

> But, on bare metal, you have to implement some kind of entrypoint to cluster.
> * External Proxy server - software / hardware solution with a public IP address and open ports.


# Minikube setup

Command below automatically starts the K8s NGINX implementation of Ingress Controller:
```bash
minikube addons enable ingress
kubens # verify that ingress-nginx namespace is created
kubectl get pod -n ingress-nginx
kubectl apply -f dashboard-ingress.yaml 
```
In my example we map view.com URL to the default minikube dashboard service.
Also we need to add <dashboard-ingress ip address>:view.com mapping in the /etc/hosts file.

# Default backend
```default-http-backend``` - can be used for custom error message responses when user hits the wrong/undefined URL.
* k8s default backend is a pod that serves a 404 page, and is used when no other Ingress rule matches.

To make it custom, you can create a custom pod with a custom error message and map it to the default backend with the following ingress configuration:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: default-http-backend
spec:
    selector:
        app: default-http-backend
    ports:
        - protocol: TCP
        port: 80
        targetPort: 8080
```

# TLS certificate - https://
Ingress can also be used to provide TLS termination, so that the user can access the service via https://.

Different controllers have different ways of handling TLS termination, but the most common way is to use a secret with the TLS certificate and key. [NGINX Controller TLS](https://kubernetes.github.io/ingress-nginx/user-guide/tls/)

To do this, you need to create a secret with the TLS certificate and key, and then reference it in the Ingress configuration, in tls section above rules:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tls-example-ingress
  # namespace: kubernetes-dashboard
spec:
    tls:
    - hosts:
        - myapp.com
        secretName: myapp-tls-secret  # reference to the secret
    rules:
    - host: myapp.com
        http:
        paths:
        - path: /
          backend:
                serviceName: myapp-internal-service
                servicePort: 8080
```

#### Secret configuration:

To create cert and key for TLS, use the following command:
```bash
# rsa cert and key will last for 365 days, but this cert won't be trusted by browsers.
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=yourdomain.com/O=yourdomain.com"
```

To generate base64 encoded cert and key, use the following command:
```bash
cat cert.crt | base64
cat key.key | base64
```
    
```yaml
apiVersion: v1
kind: Secret
metadata:
    name: myapp-tls-secret
    namespace: default  # same namespace as Ingress component.
data:
    tls.crt: base64 encoded cert  # actual value, not path
    tls.key: base64 encoded key
type: kubernetes.io/tls
```


A better way is to use a trusted certificate authority, like Let's Encrypt, and use cert-manager to automate the process.
<!-- ... TODO:- implement tls certificate for ingress with Cert-Manager -->
[Secure Your Kubernetes Ingress with TLS guide](https://medium.com/@tamerbenhassan/secure-your-kubernetes-ingress-with-tls-a-comprehensive-guide-47e315f5c517)

#### Cert-Manager - K8s tool for managing certificates
* automates the management and issuance of TLS certificates
```bash
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.5.3/cert-manager.yaml
kubectl get pods -n cert-manager
```

#### New kind: [ClusterIssuer / Issuer](https://cert-manager.io/docs/concepts/issuer/)
```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
  namespace: kubernetes-dashboard
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: test@gmail.com
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
    - http01:
        ingress:
          class: nginx
```

#### Requesting certificate
```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: tls-secret
  namespace: kubernetes-dashboard
spec:
  secretName: dashboard-tls-secret
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
  commonName: view.com
  dnsNames:
  - view.com
```

#### TLS Cert debugging
```bash
kubens kubernetes-dashboard

kubectl get clusterissuer 
kubectl get certificate 
kubectl describe certificaterequest 
kubectl get challenges 
kubectl describe challenge <challenge-name> 
```
