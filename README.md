# Visual(ED) Dashboard Demo

## What is this

All thing needed to quicly spin up a fully fucntional demo site [Visual(ED) Dashboard](http://demo.visualed.org/) on your own computer.

A few notes:

1. The demo site is deployed on a Azure VM.
2. All data displayed here is fully synthetic.

## Prerequisites to run this dashboard on your server

### Operating system and infrastructure

This has been tested on Azure VM (Ubuntu 18.04), Amazon VM (CentOS 7), Macbook Pro (MacOS 10.15.4). we have not tested in Windows system yet, but in theory everything should work similar.

### Virtulization Software

#### docker

See [Installation guide](https://docs.docker.com/get-docker/) according to your system.

#### minikube

See [Installation guide](https://kubernetes.io/docs/tasks/tools/install-minikube/) according to your system.

#### Verify everything is set

##### on mac

```bash
$ minikube start --driver=docker --extra-config=apiserver.service-node-port-range=80-60000
😄  minikube v1.9.2 on Darwin 10.15.4
✨  Using the docker driver based on user configuration
👍  Starting control plane node m01 in cluster minikube
🚜  Pulling base image ...
🔥  Creating Kubernetes in docker container with (CPUs=2) (2 available), Memory=3890MB (3938MB available) ...
🐳  Preparing Kubernetes v1.18.0 on Docker 19.03.2 ...
    ▪ apiserver.service-node-port-range=80-60000
🌟  Enabling addons: default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube"
```

##### on Linux

```bash
$ minikube start --driver=none --extra-config=apiserver.service-node-port-range=80-60000
```

## How to deploy this on my laptop/desktop/cloud server?

With this one liner command.

```bash
curl https://gist.githubusercontent.com/xiaolinked/3e0fd1b94526b1fec465e11c492d097d/raw/kustomized_file.yaml | kubectl apply -f -
```

Check [prerequisites](#Prerequisites) if run into problem.

#### To view result

```bash
minikube service nginx-svc --url
```

## Delete everything

```bash
minikube delete
```

### What is happening under the hood?

1. It leverages the latest virtuliazation technology which create all components (proxy server, web server, time-series data store, ETL job) in different container and put them together.
2. The script downloaded a [gist](https://gist.githubusercontent.com/xiaolinked/3e0fd1b94526b1fec465e11c492d097d/raw/kustomized_file.yaml) file, which is a full description for all components needed in this system. Kubenetes take this description and create all components inside the minikube environment.

## How to update the gist for the one liner

only needed if change

```bash
kustomize build . > kustomized_file.yaml
```

Then copy the content of kustomized_file.yaml into any gist.

If don't have kustomize, follow [installation guide].(https://github.com/kubernetes-sigs/kustomize/blob/master/docs/INSTALL.md)
