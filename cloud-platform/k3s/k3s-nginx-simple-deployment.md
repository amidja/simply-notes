
##K3S a Sample NGINX Deployment 

```bash
#create a deployment from an Nginx image with three replicas available on port _80_:\
kubectl create deployment nginx --image=nginx --port=80 --replicas=3

#Creating a ClusterIp service to the deployment
kubectl create service clusterip nginx --tcp=80:80
kubectl describe service nginx
```


Create Ingress to the service:


```bash
cat > nginx-ingress.yaml << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx
  annotations:
    ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx
            port:
              number: 80
EOF
```

```bash
kubectl apply -f nginx-ingress.yaml

kubectl describe ingress nginx
```

**Notes:** 
* The Kubernetes project recommends using [Gateway](https://gateway-api.sigs.k8s.io/) instead of [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/). The Ingress API has been frozen.
* This example was sourced from the following [URL](https://www.baeldung.com/ops/k3s-getting-started)

