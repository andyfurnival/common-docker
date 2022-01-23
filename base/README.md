# Docker images for cp-base-new

This repo provides build files for the Confluent Platform Docker images.


## Properties

This project contains a Dockerfile for building *cp-base-new*, the common base image for Confluent's Docker images.

Properties are inherited from a top-level POM. Properties may be overridden on the command line (`-Ddocker.registry=testing.example.com:8080/`), or in a subproject's POM.

- *docker.skip-build*: (Optional) Set to `false` to include Docker images as part of build. Default is 'false'.
- *docker.skip-test*: (Optional) Set to `false` to include Docker image integration tests as part of the build. Requires Python 2.7, `tox`. Default is 'true'.
- *docker.registry*: (Optional) Specify a registry other than `placeholder/`. Used as `DOCKER_REGISTRY` during `docker build` and testing. Trailing `/` is required. Defaults to `placeholder/`.
- *docker.tag*: (Optional) Tag for built images. Used as `DOCKER_TAG` during `docker build` and testing. Defaults to the value of `project.version`.
- *docker.upstream-registry*: (Optional) Registry to pull base images from. Trailing `/` is required. Used as `DOCKER_UPSTREAM_REGISTRY` during `docker build`. Defaults to the value of `docker.registry`.
- *docker.upstream-tag*: (Optional) Use the given tag when pulling base images. Used as `DOCKER_UPSTREAM_TAG` during `docker build`. Defaults to the value of `docker.tag`.
- *docker.test-registry*: (Optional) Registry to pull test dependency images from. Trailing `/` is required. Used as `DOCKER_TEST_REGISTRY` during testing. Defaults to the value of `docker.upstream-registry`.
- *docker.test-tag*: (Optional) Use the given tag when pulling test dependency images. Used as `DOCKER_TEST_TAG` during testing. Defaults to the value of `docker.upstream-tag`.
- *docker.os_type*: (Optional) Specify which operating system to use as the base image by using the Dockerfile with this extension. Valid values are `ubi8`. Default value is `ubi8`.
- *docker.skip-security-update-check*: (Optional) The Dockerfile.ubi8 will run a security update check. If left to the default (false) and there is a pending security update that is not installed, then the build will fail, enforcing good security practices. If set to true, the check will pass no matter what. NOT ADVISABLE USE AT YOUR OWN RISK. Default value is `false`.

## Build arguments

- *CONFLUENT_VERSION*: (Required) Specify the full Confluent Platform release version. Example: 5.4.0
- See Dockferfile for more.


## Building

This project uses `maven-assembly-plugin` and `dockerfile-maven-plugin` to build Docker images via Maven.

To build SNAPSHOT images, configure `.m2/settings.xml` for SNAPSHOT dependencies. These must be available at build time.

```
mvn clean package -Pdocker -DskipTests # Build local images
```


Manual Docker Build
```
docker build -f Dockerfile.ubi8 --build-arg OPENSSL_VERSION='-1.1.1k-5.el8_5' --build-arg WGET_VERSION='-1.19.5-10.el8' --build-arg NETCAT_VERSION='-7.70-6.el8' --build-arg PYTHON36_VERSION='-3.6.8-38.module+el8.5.0+12207+5c5719bc' --build-arg TAR_VERSION='-1.30-5.el8' --build-arg PROCPS_VERSION='-3.3.15-6.el8' --build-arg KRB5_WORKSTATION_VERSION='-1.18.2-14.el8' --build-arg IPUTILS_VERSION='-20180629-7.el8' --build-arg HOSTNAME_VERSION='-3.20-6.el8' --build-arg ZULU_OPENJDK_VERSION='17' --build-arg PYTHON_PIP_VERSION='==21.*' --build-arg PYTHON_SETUPTOOLS_VERSION='==54.*' --build-arg ARTIFACT_ID='cp-base-new' --build-arg PROJECT_VERSION='7.0.1' -t confluent/cp-base-new .

```