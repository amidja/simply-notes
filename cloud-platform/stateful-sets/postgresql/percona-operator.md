# Percona PostgreSQL Operator

[Documentation](https://docs.percona.com/percona-operator-for-postgresql/2.0/index.html)


[OpenShift Instal](https://docs.percona.com/percona-operator-for-postgresql/2.0/openshift.html#install-the-operator-via-the-command-line-interface)


```bash
git clone -b v2.4.0 https://github.com/percona/percona-postgresql-operator
cd percona-postgresql-operator

kubectl apply --server-side -f deploy/crd.yaml
#customresourcedefinition.apiextensions.k8s.io/crunchybridgeclusters.postgres-operator.crunchydata.com serverside-applied
#customresourcedefinition.apiextensions.k8s.io/perconapgbackups.pgv2.percona.com serverside-applied
#customresourcedefinition.apiextensions.k8s.io/perconapgclusters.pgv2.percona.com serverside-applied
#customresourcedefinition.apiextensions.k8s.io/perconapgrestores.pgv2.percona.com serverside-applied
#customresourcedefinition.apiextensions.k8s.io/perconapgupgrades.pgv2.percona.com serverside-applied
#customresourcedefinition.apiextensions.k8s.io/pgadmins.postgres-operator.crunchydata.com serverside-applied
#customresourcedefinition.apiextensions.k8s.io/pgupgrades.postgres-operator.crunchydata.com serverside-applied
#customresourcedefinition.apiextensions.k8s.io/postgresclusters.postgres-operator.crunchydata.com serverside-applied

kubectl create namespace postgres-operator

kubectl apply -f deploy/rbac.yaml -n postgres-operator
#serviceaccount/percona-postgresql-operator created
#role.rbac.authorization.k8s.io/percona-postgresql-operator created

kubectl apply -f deploy/operator.yaml -n postgres-operator
#deployment.apps/percona-postgresql-operator created

#Step 6: Install cluster
kubectl apply -f deploy/cr.yaml -n postgres-operator

#Step 7: Verify that cluster is runnng 
kubectl get pg -n postgres-operator


## Verifya installation
kubectl get secret cluster1-pguser-cluster1 -n postgres-operator --template='{{.data.password | base64decode}}{{"\n"}}'
#m-*W7)GDB7t=|QUG-ti6<@j

kubectl run -i --rm --tty pg-client --image=perconalab/percona-distribution-postgresql:16 --restart=Never -- bash -il

PGPASSWORD='cm-*W7)GDB7t=|QUG-ti6<@j' psql -h cluster1-pgbouncer.postgres-operator.svc -p 5432 -U cluster1 cluster1
```

