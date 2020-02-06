# cEOSR/Kubernetes for Calico Release 4.23.0.1FX Notes

# Versions Supported
## Kubernetes Version
Kubernetes 1.16

## Linux Distro/Kernel
CentOS Linux release: 7.7/7.6
Kernel : 3.10

CoreOS Container Linux 4.19

## Docker version:
Docker 18.06 with systemd cgroup driver

## Calico
Calico v3.8. We support policy only and IPAM type host-local with subnet as ‘usePodCidr’. Manifest could be downloaded at:
https://docs.projectcalico.org/v3.8/manifests/calico-policy-only.yaml

Calico v3.10. We support policy only and IPAM type host-local with subnet as ‘usePodCidr’.  Manifest could be downloaded at:
https://docs.projectcalico.org/v3.10/manifests/calico-policy-only.yaml

# How To Deploy

- Deploy Calico policy only yaml per above references
- Download and make any desired changes to cloudeos.yaml
- Apply node annotations if desired
- Deploy `cloudeos.yaml`

	kubectl apply -f cloudeos.yaml
 
# User configurable Parameters 
Refer to the CloudEOS parameters in the [project root directory](https://github.com/aristanetworks/cloudeos-k8s).
