# Setup a play Kubernetes Cluster

You have to setup a <u>**plain k8s**</u> cluster with **custom configuration** parameters for kubelet. The customer needs following options to be set:

- kubelet is to be **dynamically configured**, i.e. there <u>**MUST**</u> be the option to reconfigure <u>**EACH**</u> node (master/worker) while running in production. A wrong configuration <u>**MUST NOT**</u> be applied so no node is out of service 
- Every node needs resource configuration to prevent out of resource situations. Eviction policies <u>**SHOULD**</u> be adjusted with the reservation parameters. As an example of the setup think of master nodes (3x) being 4 Cores 8GB RAM each and worker nodes (3x) being 8 Cores 16GB each to use for your calculations.
- All settings are to be set within a single configuration if possible
- **kubeadm** or **systemd** are permitted to be used
	

**QUESTIONS**:

- How would you solve the customer needs?
- Explain the strategy you would present to the customer related to out of resource handling.