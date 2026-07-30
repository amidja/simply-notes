
## RedHat Registry 

[RedHat Registry Authentication](https://access.redhat.com/articles/RegistryAuthentication)
[RedHat Documentation](https://docs.redhat.com/en)

## Red Hat Enterprise Linux

[Using-shared-system-certificates](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/securing_networks/using-shared-system-certificates)

To add a new certificate to the truststore:

- For trusted certificates, copy the certificate to `/etc/pki/ca-trust/source/anchors/`.
- For distrusted certificates, copy the certificate to `/etc/pki/ca-trust/source/blocklist/`.
- Enter the `update-ca-trust` command, or use the `trust anchor` subcommand.

See the `update-ca-trust(8)` and `trust(1)` man pages on your system for more information.
```bash
cp <~/certificate-trust-examples/Cert-trust-test-ca.pem> /usr/share/pki/ca-trust-source/anchors/
update-ca-trust extract
```


```bash
# list certs in a keystore
keytool -list -v -keystore /etc/pki/java/cacerts -storepass changeit
```

## RH UBI Base Image

[Introduction](https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image)
[Universal Base Images (UBI): Images, repositories, and packages](https://access.redhat.com/articles/4238681)
[All You Need to Know About Red Hat Universal Base Image](http://crunchtools.com/all-you-need-to-know-about-red-hat-universal-base-image/)
[FAQ - Universal Base Images](https://developers.redhat.com/articles/ubi-faq)


The Universal Base Image is designed and engineered to be the base layer for all of your containerized applications, middleware, and utilities.
Use the following to get images from a RedHat Container registry:

```text
oc import-image ubi8/ubi:8.8-854 --from=registry.access.redhat.com/ubi8/ubi:8.8-854 --confirm
#or
oc import-image ubi8/ubi-minimal:8.8-860 --from=registry.access.redhat.com/ubi8/ubi-minimal:8.8-860 --confirm
```

```text
podman pull registry.access.redhat.com/ubi8/ubi:8.8-854
#or
podman pull registry.access.redhat.com/ubi8/ubi-minimal:8.8-860
```

```
# Start/Run the container in Docker
docker run -it registry.access.redhat.com/ubi9/openjdk-17:1.24  /bin/bash
# Switch into root user
docker run -u root -it registry.access.redhat.com/ubi9/openjdk-17:1.24  /bin/bash
```

#### S2i

Source-to-Image (S2I) is a framework that makes it easy to write images that take application source code as an input and produce a new image that runs the assembled application as output. 

[GitHub - openshift/source-to-image: A tool for building artifacts from source and injecting into container images](https://github.com/openshift/source-to-image)  

For more information on building java images using s2i see: [Java - Source-to-Image (S2I) | Using Images | OpenShift Container Platform 3.11](https://docs.openshift.com/container-platform/3.11/using_images/s2i_images/java.html)  
The java configuration can be customised using following Env Variables: [Java - Source-to-Image (S2I) | Using Images | OpenShift Container Platform 3.11](https://docs.openshift.com/container-platform/3.11/using_images/s2i_images/java.html#s2i-images-java-environment-variables)

  
From <[https://docs.openshift.com/container-platform/3.11/using_images/s2i_images/java.html#s2i-images-java-environment-variables](https://docs.openshift.com/container-platform/3.11/using_images/s2i_images/java.html#s2i-images-java-environment-variables)>
### Red Hat UBI Java (OpenJDK) Images

- [OpenJDK 17 Runtime Image](https://catalog.redhat.com/en/software/containers/ubi9/openjdk-17-runtime/61ee7d45384a3eb331996bee)
- [OpenJDK 17 Build Image (S2i)](https://catalog.redhat.com/en/software/containers/ubi9/openjdk-17/61ee7c26ed74b2ffb22b07f6 )

These images does not include ***dnf*** as they are built on top of the ***ubu-minimal*** base image. This image uses ***microdnf*** as their package manager.

```dockerfile
#Install additional packages into a minimal UBI image

FROM ://redhat.com  
#1. Switch to root user to allow package installation 
USER root 

# 2. Use microdnf instead of dnf to install packages 
RUN microdnf install -y shadow-utils && \
 microdnf clean all 

# 3. Switch back to the standard non-root user 
USER 185
```

To switch into the *root* user in UBI java container:
```bash
docker exec -u root -it registry.access.redhat.com/ubi9/openjdk-17:1.24 /bin/bash
# or 
docker run -u root -it registry.access.redhat.com/ubi9/openjdk-17:1.24 /bin/bash
```

[What is new in OpenJDK Containers - Java 17](https://developers.redhat.com/articles/2022/04/19/java-17-whats-new-openjdks-container-awareness)

	
https://catalog.redhat.com/en/software/containers/ubi9/openjdk-17-runtime/
https://developers.redhat.com/blog/2018/12/10/install-java-rhel8
##### JVM Diagnostic 

[How to use jcmd](https://www.baeldung.com/running-jvm-diagnose)

Resources:
- [Images, repositories, packages and source code](https://access.redhat.com/articles/4238681)
- [Adding Software to a UBI container](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/assembly_adding-software-to-a-ubi-container_building-running-and-managing-containers)
- [Adding Software to RHEL8 UBIs](https://www.youtube.com/watch?v=TUjqcpwxvSA)
