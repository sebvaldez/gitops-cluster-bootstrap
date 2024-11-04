# Notes

# create a minikube cluster
minikube start --cpus=4 --memory=8192

( there was an error in the storage-provisioner )
# version its creating: k8s version: 1.30.0

# install argocd
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


# Get initial login secret
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode; echo

# ex:
Fy3OmqNPDnrhXU2M

# enable argocd ui
>By default, the Argo CD API server is not exposed with an external IP. To access the API server, choose one of the following techniques to expose the Argo CD API server:
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

# recommended for minikube
kubectl port-forward svc/argocd-server -n argocd 8080:443

# ( theres a ingresss config)

kubectl port-forward svc/argocd-server -n argocd 8080:443

```bash
argoui() {
    if [ -f /tmp/argoui.pid ]; then
        # If PID file exists, kill the process and remove the PID file
        kill $(cat /tmp/argoui.pid) && rm /tmp/argoui.pid
        echo "Argo CD port forwarding stopped."
    else
        # Start port-forwarding and save the PID to a file
        kubectl port-forward svc/argocd-server -n argocd 8080:443 > /dev/null 2>&1 &
        echo $! > /tmp/argoui.pid
        echo "Argo CD port forwarding started."
    fi
}
```



# Deploy bootstrap cluster
## Test changes with helm template .

## deploy

k apply -f bootstrap.yaml

