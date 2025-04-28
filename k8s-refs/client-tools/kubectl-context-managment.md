# kubectl Context Management

Managing contexts in `kubectl` allows you to switch between multiple Kubernetes clusters and namespaces easily.

## View Current Context
```bash
kubectl config current-context
```

## List All Contexts
```bash
kubectl config get-contexts
```

## Switch Context
```bash
kubectl config use-context <context-name>
```

## Rename a Context
```bash
kubectl config rename-context <old-name> <new-name>
```

## Delete a Context
```bash
kubectl config delete-context <context-name>
```

## Set a Default Namespace for a Context
```bash
kubectl config set-context --current --namespace=<namespace>
```

## Example: Switching Contexts
```bash
kubectl config use-context my-cluster
```

For more details, refer to the [kubectl documentation](https://kubernetes.io/docs/reference/kubectl/).

