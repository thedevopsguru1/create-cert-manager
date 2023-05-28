# create-cert-manager

# Install Cert-manager
## 1- Install cert-manager’s CRDs
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.crds.yaml
```
## 2- Create a cert-manager namespace.
```
kubectl create namespace cert-manager
```
## 3- Add the Helm repository which contains the cert-manager Helm chart.
```
helm repo add cert-manager https://charts.jetstack.io
```
## 4- Update your Helm repositories.
```
helm repo update
```
## 5- Install the cert-manager Helm chart.
```
helm install \
my-cert-manager cert-manager/cert-manager \
--namespace cert-manager \
--version v1.8.0
```
### or
```
helm install my-cert-manager cert-manager/cert-manager --namespace cert-manager --version v1.8.0
```
### You should see an output similar to this:
![image](https://user-images.githubusercontent.com/85393914/222500555-3f0f0eac-ebb8-477e-bb53-d518bf190372.png)

## 6- Verify that the corresponding cert-manager pods are now running
```
kubectl get pods --namespace cert-manager
```
### You should see an output similar to this:
![image](https://user-images.githubusercontent.com/85393914/222500662-3bc885a0-2623-4728-af25-dd20c5f7f364.png)

## Create a Cluster issuer Resource.
### 1- create a yaml file with this: issuer.yaml
```
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
  namespace: default
spec:
  acme:
    email: user@yannickeboo.com #make sure to add right email here
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: letsencrypt-secret-prod
    solvers:
    - http01:
        ingress:
          class: nginx
```
```
 kubectl apply -f issuer.yaml
```
### Fixed the above by changing stuff like email and ingress class
### 2- Update the ingress file by adding these in red
![image](https://user-images.githubusercontent.com/85393914/222505425-e2133248-993c-441c-b372-b52957899214.png)

## After all we should have secure applications
![image](https://user-images.githubusercontent.com/85393914/222505679-bd478939-9004-40bd-b2bb-54353288ddd0.png)
## Double check that the certificate is good:
```
kubectl describe certificate namehere
```
```
kubectl describe certificate example-tls
```
