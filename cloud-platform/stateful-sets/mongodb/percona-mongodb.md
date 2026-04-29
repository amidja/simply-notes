
[Home Page](https://www.percona.com/mongodb/software/percona-server-for-mongodb)


## Installation 

[Quick Start](https://docs.percona.com/percona-operator-for-mongodb/quickstart.html)


## Delete

https://docs.percona.com/percona-operator-for-mongodb/delete.html

```bash 
#delete the Cluster deployment
kubectl get deploy -n psmdb
kubectl delete deploy percona-server-mongodb-operator -n psmdb

#delete the Operator CRDs
kubectl get crd | grep percona
kubectl delete crd perconaservermongodbbackups.psmdb.percona.com perconaservermongodbrestores.psmdb.percona.com perconaservermongodbs.psmdb.percona.com

#delete the namespace
kubectl create namespace psmdb
```


Ignore all this stuff just follow the quick start 
Installing using helm

```bas
$ helm repo add percona https://percona.github.io/percona-helm-charts/
$ helm repo update
```
```
 kubectl create namespace psmdb
```

```bash
helm install my-mongo-db-op percona/psmdb-operator --namespace psmdb

#  See if the operator Pod is running:
kubectl get pods -l app.kubernetes.io/name=psmdb-operator --namespace psmdb

#  Check the operator logs if the Pod is not starting:
export OP_POD=$(kubectl get pods -l app.kubernetes.io/name=psmdb-operator --namespace psmb-op --output name)
kubectl logs $OP_POD --namespace=psmb-op
```


``` Deploy the database cluster from psmdb-db chart


helm install my-mongo-db percona/psmdb-db --namespace=psmb
helm install my-mongo-db ./ --namespace=my-psmb
helm uninstall my-mongo-db --namespace=psmb

#Percona Server for MongoDB cluster is deployed now. Get the username and password:
ADMIN_USER=$(kubectl -n psmb-mongodb get secrets my-mongo-db-psmdb-db-secrets -o jsonpath="{.data.MONGODB_USER_ADMIN_USER}" | base64 --decode)
ADMIN_PASSWORD=$(kubectl -n psmb-mongodb get secrets my-mongo-db-psmdb-db-secrets -o jsonpath="{.data.MONGODB_USER_ADMIN_PASSWORD}" | base64 --decode)

#Connect to the cluster:
kubectl run -i --rm --tty percona-client --image=percona/percona-server-mongodb:7.0 --restart=Never \
  -- mongosh "mongodb://${ADMIN_USER}:${ADMIN_PASSWORD}@my-mongo-db-psmdb-db-mongos.psmb-mongodb.svc.cluster.local/admin?ssl=false"


https://artifacthub.io/packages/helm/percona/psmdb-db
https://github.com/percona/
https://github.com/percona/percona-helm-charts
https://github.com/percona/percona-helm-charts/tree/main/charts/psmdb-db


 