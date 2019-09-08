# Kubernetes Networking

The customer has a k8s cluster running. Currently a single CNI is used which results in a Pod only having one network interface given. The customer expects the following:
	
- Pods can have multiple network interfaces
- Each network interface is configurable as an **OWN VLAN** (networks **MUST** be independent)
- If not possible to solve this within k8s, OpenShift can be introduced

**QUESTIONS**:
- Can a multi-network CNI-setup be applied within k9s?
- If using OpenShift: explain a possible solution.