# Hybrid Cloud Setup

You have multiple on-premise cluster setups (OpenShift or k8s, doesn't matter) within the organization. The customer needs to be able to run a "hybrid cloud" so he asks to connect a Cloud Provider (any) for <u>**running ONLY dynamic workloads**</u> (i.e. these resources are dynamically claimed within the cloud). Conditions are as follows:

- Any cluster is within it's **own VLAN**
- Weave or Calico are used
- The connection between a cluster and the cloud <u>**MUST BE**</u> fully encrypted
- Any new node within the cloud provider joins as a worker node

**QUESTIONS**:
- What is your idea about solving this?