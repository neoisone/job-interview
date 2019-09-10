# Kubernetes Networking

The customer has a k8s cluster running. Currently a single CNI is used which results in a Pod only having one network interface given. The customer expects the following:
	
- Pods can have multiple network interfaces
- Each network interface is configurable as an **OWN VLAN** (networks **MUST** be independent)
- If not possible to solve this within k8s, OpenShift can be introduced

**QUESTIONS**:
- Can a multi-network CNI-setup be applied within k9s?
- If using OpenShift: explain a possible solution.


By default the pods are the part of the subnet which the node is a part of and it is connected to only a single subnet.
This is the case with both kubernetes as well as openshift.

However Kubernetes supports a CNI plugin calles MULTUS. With openshift 3.x there was no support for the same (Reason why I couldnt play with it earlier).
But with the release of Openshift 4.1, multus will be a supported feature.

- Pods can gave multiple network interfaces
The answer is yes they can have and I have my findings below.


Multus CNI is useful in cases where network isolation is required e.g. Control Plane. This also adds an advantage to secure the network using different channels for sensitive data.

In openshift we can create a custom resource definition or CR and the we can make an annotated pod which can use that CR.

Sample definition

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
apiVersion: "k8s.cni.cncf.io/v1"
kind: NetworkAttachmentDefinition
metadata:
  name: macvlan-conf
spec:
  config: '{
      "cniVersion": "0.3.0",
      "type": "macvlan",
      "master": "eth0",
      "mode": "bridge",
      "ipam": {
        "type": "host-local",
        "subnet": "192.168.1.0/24",
        "rangeStart": "192.168.1.200",
        "rangeEnd": "192.168.1.216",
        "routes": [
          { "dst": "0.0.0.0/0" }
        ],
        "gateway": "192.168.1.1"
      }
    }'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


                                                                   +++++++++++++++++++ 
                                                                   +Kubernetes Server+   
(Secure nw)              +++++++++++++++++                         +++++++++++++++++++  
(NW1)--------------------+      POD      +                                     |(API comms)
(NW2)--------------------+net0           +                                     |
(user traffic)           +net1       eth0+--------------------------------------
                         +++++++++++++++++



Sample Pod
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
apiVersion: v1
kind: Pod
metadata:
  name: thor
  annotations:
    k8s.v1.cni.cncf.io/networks: macvlan-conf
spec:
  containers:
  - name: samplepod
    command: ["/bin/bash", "-c", "sleep 2000000000000"]
    image: centos/tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[stark@master-0 ~]$ oc create -f thor.yaml
[stark@master-0 ~]$ oc create -f thor.yaml 
pod/thor created

[stark@master-0 ~]$ oc exec -it thor  -- ip -4 addr			// verify network interface is created & attached to pod

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000 
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
3: eth0@if6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP  link-netnsid 0 
    inet 10.244.1.4/24 scope global eth0
       valid_lft forever preferred_lft forever
4: net1@if2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN  link-netnsid 0 
    inet 192.168.1.203/24 scope global net1
       valid_lft forever preferred_lft forever

- Each network interface is configurable as an **OWN VLAN** (networks **MUST** be independent)
Once saperate interfaces have been established we can have multiple VLANS as well.


