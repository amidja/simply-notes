
This example is from https://skupper.io/examples/index.html
## Skupper Podman Example
[Podman Example's Github Repository](https://github.com/skupperproject/skupper-example-podman)

### Podman Docs/References

- [Podman Docs](https://podman.io/docs)
- [Podman Tutorials](https://github.com/podman-container-tools/podman/blob/main/docs/tutorials/README.md)
- [Podman Desktop Doc](https://podman-desktop.io/docs/intro)

How to check is Podman running in rootless mode:
```bash
podman info --format '{{.Host.Security.Rootless}}'
```
To find a Podman container's IP addres:
```bash
#podman inspect <container_name> --format '{{.NetworkSettings.IPAddress}}'
podman inspect default-skupper-router --format '{{.NetworkSettings.IPAddress}}'

#
podman run --name basic_httpd -d -p 8080:80/tcp docker.io/nginx
podman inspect basic_httpd --format 'IP address: {{.NetworkSettings.IPAddress}}'
#IP address:

sudo podman run --name basic_httpd -d -p 8080:80/tcp docker.io/nginx
sudo podman inspect basic_httpd --format 'IP address: {{.NetworkSettings.IPAddress}}'
#IP address: 10.88.0.2

curl http://localhost:8080

sudo podman run -it fedora:latest /bin/bash
curl http://10.88.0.2:8080

```

sudo podman run -d --name my-container -p 80:80 nginx
podman pull fedora:latest 
sudo podman run it fedora:latest --name fedora /bash

#### Check Podman Env

https://docs.redhat.com/en/documentation/red_hat_service_interconnect/2.2/html/using_service_interconnect/system-creating-site-cli

```bash
podman version
systemctl status podman.socket

#enable the Podman socket
systemctl --user enable --now podman.socket
```

##### Start the user systemd socket for a rootless service
```bash
systemctl --user start podman.socket
systemctl --user enable --now podman.socket
loginctl enable-linger <USER>
systemctl --user status podman.socket
```

##### Podman Remote

- [Podman Remote Doc](https://podman-desktop.io/docs/podman/podman-remote)
- [Podman Remote Guide](https://github.com/podman-container-tools/podman/blob/main/docs/tutorials/remote_client.md)

```bash
podman system connection ls
podman system connection add redhat-el-0 --identity ~/.ssh/id_ed25519 amidja@192.168.0.80/run/user/1000/podman/podman.sock
podman system connection default redhat-el-0
```

### Prerequisites 

Skupper CLI https://skupper.io/docs/install/index.html#installing-the-skupper-cli

### Environment Variables 

SKUPPER_PLATFORM=[podman|docker|linux]
SKUPPER_SYSTEM_RELOAD_TYPE=[auto]

#### Installing Skupper CLI

```bash
curl https://skupper.io/v2/install.sh | sh
#Path:     /home/amidja/.local/bin/skupper
```

To uninstall Skupper CLI, use:
```bash
curl https://skupper.io/uninstall.sh | sh
```
#### Installing Skupper Controller

```bash
export SKUPPER_SYSTEM_RELOAD_TYPE=auto
export SKUPPER_PLATFORM=podman
#skupper system install --reload-type auto -p podman
skupper system install -p podman

#Pulled system-controller image: quay.io/skupper/system-controller:2.2.1
#Platform podman is now configured for Skupper
```
This runs a container to support site, link and service operations.

#### Create a Skupper Site

By default, all sites are created with the namespace `default`. On non-Kubernetes sites, you can create multiple sites per-user by specifying a _namespace_, for example:

```bash
#export SKUPPER_PLATFORM=podman
skupper site create north -p podman
#File written to /home/amidja/.local/share/skupper/namespaces/default/input/resources/Site-north.yaml
```

While the site is created, the site is not running at this point. To run the site:
```console
skupper system start -p podman
#Sources will be consumed from namespace "default"
#Site "north" has been created on namespace "default"
#Platform: podman
#Definition is available at: /home/amidja/.local/share/skupper/namespaces/default/input/resources
```

Check the status of the site:
```
skupper site status
#NAME    STATUS  MESSAGE
#north   Ready   OK
```


#### Linking sites using a link resource

On K3S Site:
```
kubectl config set-context --current --namespace west
skupper link generate > link.yaml
```

Podman:
```bash
export SKUPPER_PLATFORM=podman
skupper system apply -f link.yaml

#File written to /home/amidja/.local/share/skupper/namespaces/default/input/resources/Link-link-west-skupper-router.yaml
#Link link-west-skupper-router added
#File written to /home/amidja/.local/share/skupper/namespaces/default/input/resources/Secret-link-west.yaml
#Secret link-west added
#Custom resources are applied.

skupper link status
```

#### Adding  a Skupper Listener

```bash
skupper listener create backend 8080 --host 192.168.0.101
skupper listener create backend 8080 --host 0.0.0.0
#File written to /home/amidja/.local/share/skupper/namespaces/default/input/resources/Listener-backend.yaml
#skupper system apply -f /home/amidja/.local/share/skupper/namespaces/default/input/resources/Listener-backend.yaml
skupper system reload
#
skupper listener status

```


### How to access Skupper connector from a Podman container

[Initial Issue record](https://github.com/skupperproject/skupper/discussions/2538)
[Jie Skupper CLI Instructions](https://github.com/jhua04/knowledge/blob/main/Kubernetes/Installation%20and%20management/skupper-cli-installation.md)

## RedHat Service Interconnect 

[Service Interconnect](https://docs.redhat.com/en/documentation/red_hat_service_interconnect/2.2/html/using_service_interconnect/index)
[Service Interconnect - Creating Site with CLI](https://docs.redhat.com/en/documentation/red_hat_service_interconnect/2.2/html/using_service_interconnect/system-creating-site-cli)