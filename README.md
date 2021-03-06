# DellEMC BigDL Engine for Cloudera Data Science Workbench

## Overview
This repository contains the preconfigured engine for the Dell EMC Ready Solution for AI - Machine Learning with Cloudera Hadoop. The engine is configured with [Intel BigDL](https://bigdl-project.github.io/master/#whitepaper/) a distributed deep learning library for Apache Spark. Jumpstart examples are located in the [BigDL4CDSW](https://github.com/dell-ai-engineering/BigDL4CDSW) repository.

### Versions
- BigDL 0.5.0
- Spark 2.2
- Scala 2.11.8
- Java 8

#### Note on Java 7
Intel BigDL recommends Java 8 when using Spark 2.x as Java 7 may cause performance issues. If you are required to use Java 7 then follow the instructions to [build from source](https://bigdl-project.github.io/master/#ScalaUserGuide/install-build-src/#download-bigdl-source) using an environment with Java 7. Then replace the /opt/Intel directory with the Java 7 compiled BigDL and edit the spark.jars in spark-defaults.conf file if using the Jumpstart templates.

## How to use Dockerfile
#### Build image and push to a Docker repository
    sudo yum install docker
    sudo systemctl start docker
#### Build the container
    docker build --network=host -t dellrepo/bigdl:0.5.0 . -f Dockerfile
#### Test that it works
    docker run -it dellrepo/bigdl:0.5.0 /bin/bash
#### Push the container image
    docker push dellrepo/bigdl:0.5.0

#### If you need to run a docker registry: 
Run registry container, assumes certificate is in a certs directory under the dockerfile.
```
docker run -d \
  --restart=always \
  --name registry \
  -v `pwd`/certs:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -p 443:443 \
  registry:2
```
Change the docker repo from dellrepo to \<hostname\> when tagging images

A docker registry is secured using TLS and requires a certificate. [Docker registry configuration guide](https://docs.docker.com/registry/deploying/)
If you must use a self signed certificate follow the [Docker guide for an insecure registry server](https://docs.docker.com/registry/insecure/#use-self-signed-certificates)

### Add the engine to CDSW
1. Log in to CDSW as a site administrator
2. Go to Admin and then Engines
3. Name the engine and add the docker registry location

### Verify that the engine works
1. Log in to CDSW
2. Create a new project
3. Open a new workbench session
4. Before starting the workbench change the selection to the new BigDL engine
5. Start the engine
