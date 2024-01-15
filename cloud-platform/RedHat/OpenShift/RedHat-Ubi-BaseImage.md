# RH UBI Base Image

The Universal Base Image is designed and engineered to be the base layer for all of your containerized applications, middleware and utilities.

Use the following to get images from a RedHat Container registry:

```text
oc import-image ubi8/ubi:8.8-854 --from=registry.access.redhat.com/ubi8/ubi:8.8-854 --confirm

oc import-image ubi8/ubi-minimal:8.8-860 --from=registry.access.redhat.com/ubi8/ubi-minimal:8.8-860 --confirm
```

```text
podman pull registry.access.redhat.com/ubi8/ubi:8.8-854

podman pull registry.access.redhat.com/ubi8/ubi-minimal:8.8-860
```

Resources:

- [Images, repositories, packages and source code](https://access.redhat.com/articles/4238681)
- [Adding Software to a UBI container](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/assembly_adding-software-to-a-ubi-container_building-running-and-managing-containers)
- [Adding Software to RHEL8 UBIs](https://www.youtube.com/watch?v=TUjqcpwxvSA)
