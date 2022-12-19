# Jellyfin on Kubernetes #

This project contains the required resources to deploy Jellyfin into
Kubernetes. It is adapted from the [Jellyfin on Openshift](https://github.com/home-cluster/jellyfin-openshift) project. The instructions provided here are for microk8s kubernetes running on Ubuntu 22.04 LTS.

## Pre-requisites ##

To deploy this project you will need:

- A working kubernetes cluster. See [here] for instructions on getting started with microk8s.
- An ingres controller and cert-manager (or something similar) for providing access to the jellyfin service and performing TLS termination. [This guide](https://microk8s.io/docs/addon-cert-manager) explains how to configure the cert-manager and ingress in microk8s.

Also note, this example deployment uses the fairly basic 'hostpath' storage for the media library. This will likely be suitable for a home media server running on microk8s or some other lightweight/single node implementation, where the media files are stored in a local directory, but is not suitable for multi-node clusters. Another storage option is to add media to the library via network shares.

## Kubernetes resources ##

The `base/` directory contains a `PersistentVolumeClaim` , a `Deployment`, `Service`, and `ingress` to deploy Jellyfin
into Kubernetes.

You will likely need to update the following:

- the ingress controller (see point 1 in the troubleshooting section below).
- the path to the folder on the local machine that contains your media files.

The examples in the project use [kustomize](https://kustomize.io/) to modify configuration parameters. Kustomize is included in recent versions of kubectl and provides a convenient way to adapt a base set of resources to multiple environments. I have included a [sample  overlay](./overlay/kustomization.yaml) which contains example patches for the above configuration parameters.

## Troubleshooting ##

This project is tested on a microk8s kubernetes cluster running on Ubuntu. It should be mostly portable across different kubernetes implementatoins, however keep in mind.

1. The [ingress controller configuration](./resources/ingress.yaml) provided in this project ustilises the **microk8s** `ingress` and `cert-manager` addons. If you are using a different kubernets implementation you may need to modify the ingress configuration beyond changing the hostname of your server.


## TODO List ##

- For the media persistant volume resource,`spec.claimRef.namespace` should be set using a kustomize patch.