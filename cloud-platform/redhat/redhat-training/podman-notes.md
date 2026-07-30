
## Podman Network

[Podman Doc - Basic Networking](https://github.com/podman-container-tools/podman/blob/main/docs/tutorials/basic_networking.md)

[Podman Container Networking (redhat - blog) ](https://www.redhat.com/en/blog/container-networking-podman)
[Podman Container IP addresses (redhat - blog)] (https://www.redhat.com/en/blog/container-ip-address-podman)
### Communication between a rootles container and the host


To communicate between the container host and a rootless container, you can simply use port mapping and direct traffic to the port Podman assigns:
```shell
[amidja@rhel-0 ~]$ podman run -dt --rm -P nginx
[amidja@rhel-0 ~]$ podman port -l # to see all use `-a` 
80/tcp -> 0.0.0.0:36883
[amidja@rhel-0 ~]$curl http://localhost:38997
```

[Podman port command](https://docs.podman.io/en/stable/markdown/podman-port.1.html)  lists port mappings for a container

With `-p` the desired port mapping can be specified:
```shell
[amidja@rhel-0 ~]$podman run --replace -dt -p 8081:80 --name my-web-app nginx
[amidja@rhel-0 ~]$ podman port -l
80/tcp -> 0.0.0.0:8081
```

A  more straightforward is to use `--network host` flag

The `--network host` flag in Podman removes network isolation and lets a container share the host computer's network stack directly. The container uses the host's IP address, sees all physical network interfaces, and binds directly to host ports without needing `-p` port mapping

```shell
podman run -it --rm --network host nginx bash
podman run -it --rm --network host nicolaka/netshoot
```

[How to use host network with a podman container](https://oneuptime.com/blog/post/2026-03-17-use-host-networking-podman/view)
 
### Communicate between two rootless containers 

There are few of non-trivial ways to accomplish communication between two rootless containers.

