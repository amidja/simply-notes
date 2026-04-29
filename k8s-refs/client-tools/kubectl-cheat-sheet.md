
## Working with Pods
```bash
#- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
# Start a unix pod interactively (this pod will stop as soon as you exit the bash cli)
#- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
$kubectl run -i --tty my-ubuntu --image=ubuntu:latest --restart=Never -- bash -il
$kubectl run -i --tty busybox --image=busybox --restart=Never -- sh
#- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
#
#- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
$kubectl delete pod busybox

$kubectl run my-ubuntu --image=ubuntu:latest --restart=Never

```


## Working with Secrets
```bash
$kubectl get secret <secret-name> -o jsonpath="{.data.aidn}" | base64 -d
```
