 ## Install OpenShift Local 
 
 https://github.com/crc-org
 https://console.redhat.com/openshift/create/local
 https://access.redhat.com/documentation/en-us/red_hat_openshift_local/2.20

 crc config set consent-telemetry no
 crc config set memory 16384
 crc setup
 
 crc config set preset openshift
 crc start 
 crc console 


## Started the OpenShift cluster.

The server is accessible via web console at:
  https://console-openshift-console.apps-crc.testing

Log in as administrator:
  Username: kubeadmin
  Password: CeKp9-eGJSU-Vbypu-4o3TB

Log in as user:
  Username: developer
  Password: developer

Use the 'oc' command line interface:
  PS> & crc oc-env | Invoke-Expression
  PS> oc login -u developer https://api.crc.testing:6443 (https://api-int.<cluster_name>.<base_domain>)


### OpenShift Cluster Registry

https://docs.openshift.com/container-platform/4.9/registry/accessing-the-registry.html

oc get nodes
oc debug nodes/<node_name>
oc login -u kubeadmin -p CeKp9-eGJSU-Vbypu-4o3TB https://api.crc.testing:6443:6443
podman login -u kubeadmin -p $(oc whoami -t) image-registry.openshift-image-registry.svc:5000



## Agnohost 

The image was created for testing purposes, reducing the need for having different test cases for the same tested behaviour.

The agnhost binary has several sub-commands which are can be used to test different Kubernetes features; their behaviour and output is not affected by the underlying OS.

https://pkg.go.dev/k8s.io/kubernetes/test/images/agnhost


https://github.com/mriosalido/agnhost

kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname

./agnhost dns-suffix
./agnhost dns-server-list
./agnhost etc-hosts

## dsfdsss  