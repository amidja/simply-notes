# kubectl Context Management

Managing contexts in `kubectl` allows you to switch between multiple Kubernetes clusters and namespaces easily.

## View Current Context
```bash
$kubectl config current-context
```

## Configure Context

```bash
#For the current active context:
$kubectl config set-context --current --namespace=<namespace-name>

#For a specific named context:
$kubectl config set-context <context-name> --namespace=<namespace-name>
```

## List All Contexts
```bash
$kubectl config get-contexts
```

## Switch Context
```bash
$kubectl config use-context <context-name>
```

## Rename a Context
```bash
$kubectl config rename-context <old-name> <new-name>
```

## Delete a Context
```bash
$kubectl config delete-context <context-name>
```

## Steps to Duplicate a Context
```bash
#Identify the source context**
$kubectl config get-contexts
#
kubectl config view -o jsonpath='{.contexts[?(@.name=="source-context")]}'

$kubectl config set-context helm3 \
  --cluster=default \
  --user=default \
  --namespace=helm3
```


## Example: Switching Contexts

For more details, refer to the [kubectl documentation](https://kubernetes.io/docs/reference/kubectl/).

