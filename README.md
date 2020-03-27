# CloudEOS for Kubernetes

## Files/Directories
* [cilium](https://github.com/aristanetworks/cloudeos-k8s/tree/master/cilium) - Configuration files for deploying in a Cilium environment
* [calico](calico) - Configuration files for deploying in a Calico environment
* [RELEASE-NOTES.md](RELEASE-NOTES.md) - Release notes for this version
* [leaf-switch.config](leaf-switch-config) - config example for BGP Peer switch

## Design Goals
There are a few design goals with CloudEOS:
* Provide a Kubernetes native deployment model
* Allow for easy deployment of cluster nodes
* Provide an operationally consistent networking model for administrators
* Provide the benefits of EOS from a management, observability, feature, and supportability in a Kubernetes networking environment.

CloudEOS achieves these goals by the following:

* Using a daemonset to deploy CloudEOS containers on each node in the cluster
* Providing configuration options directly in the deployment YAML file
* Allowing standard EOS configuration
* Using the same binary as any other EOS based device
* Support for streaming telemetry and management through Arista CloudVision

# Configuration Options
## Supported Annotations for CloudEOS

Annotations have a higher precendence than environment variables

|Parameter | Description | Example |Optional|
|----------|-------------|---------|--------|
|arista/bgp-peer-ip-1|IP address of BGP Peer | arista/bgp-peer-ip-1="172.20.3.1"|no|
|arista/bgp-peer-ip-2|IP address of second BGP Peer for redundant links| arista/bgp-peer-ip-2="172.20.4.1"|yes|
|arista/bgp-local-as |ASN for the Kubernetes nodes.  If using iBGP this should match the ToR| arista/bgp-local-as="65003"|no|
|arista/bgp-remote-as-1 |ASN for the BGP Peer if not using iBGP| arista/bgp-remote-as-1="65001"|yes|
|arista/bgp-remote-as-2 |ASN for the second BGP Peer if not using iBGP| arista/bgp-remote-as-2="65002"|yes|
|NODE_NAME | Hostname of CloudEOS | NODE_NAME="kube153" | yes |

## Supported Environment Variables for CloudEOS

Environment Variables will be used if Annotations are not present for BGP Settings

|Parameter | Description | Example |
|----------|-------------|---------|
|BGP_AS    | Local or remote BGP AS number.| - name: "BGP_AS" </br> value: "65130"| 
|INTERFACE_MTU| Integer value used to configure app container interface MTUs|-name: "INTERFACE_MTU" </br> value: 9000|
|DISABLE_EGRESS_NAT|Set to “true” to disable egress NAT for traffic originating from POD network to outside the cluster.  Set to “false” to enable egress NAT from the POD network. Default is false. |-name: "DISABLE_EGRESS_NAT" </br> value: True|
|startupconfig-data|Users can append any EOS specific configuration fragment. Do NOT put `end` in the snippet|-name: "startupconfig-data" </br> value: "ntp server 1.1.1.1"|
|CNI_PROVIDER| (required) CNI Policy provider ("cilium" or "calico")| - name: "CNI_PROVIDER" </br> value: "cilium"|
## ConfigMap support for clusterwide configuration

(optional) If you need to apply a chunk of static config to every pod you can also create a configmap in Kubernetes that will store and configure all CloudEOS pods with generic configuration.  NOTE: These commands must be fully expanded (i.e. `interface Ethernet 1`, not `int eth 1`), and no syntax checking will occur.  If there are syntax errors with the commands it may prevent the pod from starting.  In this example I'm adding a few lines of static config for SSH to be remapped to port 8022 and adding an NTP server.  This config will be placed on every CloudEOS pod.

    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: cloudeos-configmap
      namespace: default
    data:
      cloudeos-config-data: |
        ntp-server 172.22.22.50
        management ssh
        server-port 8022

You will then need to add this configmap to your YAML specification for CloudEOS, namely as a volumeMount under cloudeos-init and a volume under the pod.  These are noted in the following snippets with a comment:

Entry for the cloudeos-init container:

      initContainers:
      - image: aristanetworks/cloudeos-init:0.9
        name: cloudeos-init
        command: ["/ceos-init", "-cni=false"]
        env:
          - name: "CLOUDVISION_IP"
            value: "10.90.224.175"
        volumeMounts:
        - name: kickstart-config
          mountPath: /config/
        # Add the following:
        - name: configmap-config
          mountPath: /config/cloudeos-configmap

Entry for the volumes section:

    volumes:
    - name: kickstart-config
      hostPath:
        path: /opt/cni/cloudeos
    # Add the following:
    - name: configmap-config
      configMap:
        name: cloudeos-configmap


## Deployment Guides
* [Deployment with Cilium](cilium)
* [Deployment with Calico](calico)

## Example Configuration
[Example Config](EXAMPLE.md)
