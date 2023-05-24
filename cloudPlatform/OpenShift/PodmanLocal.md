
# Podman

## Podman on Windows

https://github.com/containers/podman/blob/main/docs/tutorials/podman-for-windows.md
file:///C:/Program%20Files/RedHat/Podman/podman-for-windows.html

```
podman machine init
podman machine start
```

## RH Container Catalog
### Introduction

A container registry is a repository or collection of repositories for storing container images and container-based application artifacts.

``` 
podman pull <registry>[:<port>]/[<namespace>/]<name>:<tag>
```

The registries that Red Hat provides are: 			
- registry.redhat.io (requires authentication) 				
- registry.access.redhat.com (requires no authentication)
- registry.connect.redhat.com (holds Red Hat Partner Connect program images)      

You can display the container registries using the podman info --format command:    
```
podman info -f json | jq '.registries["search"]'
```

You can edit the list of container registries in the registries.conf configuration file. As a root user, edit the /etc/containers/registries.conf file to change the default system-wide search settings. 			

As a user, create the $HOME/.config/containers/registries.conf file to override the system-wide settings

## Local Settup 

