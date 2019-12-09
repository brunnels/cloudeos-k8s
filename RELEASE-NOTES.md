# CloudEOS for Kubernetes Release Notes
## Version 4.23.0FX
### Supported Software Versions Supported
#### Kubernetes 
Kubernetes 1.16
#### Linux Distro/Kernel
CentOS Linux release: 7.7/7.6
Kernel : 3.10.0-1062.1.2.el7.x86_64 #1 SMP Mon Sep 30 14:19:46 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
#### Docker
We support docker 18.06 with systemd cgroup driver only.
#### Calico
We support calico policy only deployment mode using v3.8. Also make sure to run IPAM type host-local with subnet as ‘usePodCidr’ value in the calico-policy-only.yaml file.  

