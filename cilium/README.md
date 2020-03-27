# cEOSR/Kubernetes for Cilium

# Versions Supported
## Kubernetes Version
Kubernetes 1.16

## Linux Distro/Kernel
CentOS Linux release: 7.7/7.6
Kernel : 3.10

## Docker version:
Docker 18.06 with systemd cgroup driver

## Cilium
Cilium 1.7 or later

# How To Deploy

- Deploy Cilium following the [Cilium guide](https://cilium.readthedocs.io/en/stable/gettingstarted/k8s-install-default/)
- Download and make any desired changes to `cloudeos-cilium.yaml`
- Apply node annotations if desired
- Deploy `cloudeos-cilium.yaml`
```
    kubectl apply -f cloudeos-cilium.yaml
``` 
# User configurable Parameters 
Refer to the CloudEOS parameters in the [project root directory](https://github.com/aristanetworks/cloudeos-k8s).
