1. You have to setup a <u>**plain k8s**</u> cluster with **custom configuration** parameters for kubelet. The customer needs following options to be set:

	- kubelet is to be **dynamically configured**, i.e. there <u>**MUST**</u> be the option to reconfigure <u>**EACH**</u> node (master/worker) while running in production. A wrong configuration <u>**MUST NOT**</u> be applied so no node is out of service 
	- Every node needs resource configuration to prevent out of resource situations. Eviction policies <u>**SHOULD**</u> be adjusted with the reservation parameters. As an example of the setup think of master nodes (3x) being 4 Cores 8GB RAM each and worker nodes (3x) being 8 Cores 16GB each to use for your calculations.
	- All settings are to be set within a single configuration if possible
	- **kubeadm** or **systemd** are permitted to be used
	
	**QUESTIONS**:
	  - How would you solve the customer needs?
	  - Explain the strategy you would present to the customer related to out of resource handling.

2. You have multiple on-premise cluster setups (OpenShift or k8s, doesn't matter) within the organization. The customer needs to be able to run a "hybrid cloud" so he asks to connect a Cloud Provider (any) for <u>**running ONLY dynamic workloads**</u> (i.e. these resources are dynamically claimed within the cloud). Conditions are as follows:

	- Any cluster is within it's **own VLAN**
	- Weave or Calico are used
	- The connection between a cluster and the cloud <u>**MUST BE**</u> fully encrypted
	- Any new node within the cloud provider joins as a worker node

    **QUESTIONS**:
    - What is your idea about solving this?

3. The customer has a k8s cluster running. Currently a single CNI is used which results in a Pod only having one network interface given. The customer expects the following:
	
	- Pods can have multiple network interfaces
	- Each network interface is configurable as an **OWN VLAN** (networks **MUST** be independent)
	- If not possible to solve this within k8s, OpenShift can be introduced

    **QUESTIONS**:
    - Can a multi-network CNI-setup be applied within k9s?
    - If using OpenShift: explain a possible solution.

4. Customer's operation team provides a ready k8s cluster setup with following details:
	- <u>**any setup**</u> (unlimited and any possible storage mounts, CPU a.s.o.), but
	- **node's root FS with XFS and LVM**
	
	Now a new project has to be started and one main discussion point is:
    - Project Manager means that all the nodes **MUST** use **btrfs**.
    - The reason for this is that some project dependent jobs **MUST** use **btrfs**

    **QUESTIONS**:
    - How would you de-escalate the situation and implement both wishes?
    - What are the differences between XFS and BTRFS?

5. You (operation team) have to manage a continuously growing amount of K8s/OpenShift clusters within the organization. One big topic is access management. (this is a big discussion topic)
    
    **QUESTIONS**:
    - Which authentication/authorization mechanisms are possible?
    - How would you implement a centralized access management?

6. **Simple Questions**:
	- What is the difference between L2 and L3 networking?
		- Tell about some CNIs and their functionality.
		- Which CNIs support encryption?
	- What is the difference between the following:
		- Containers
		- Virtual Mashines
		- Python Virtual-Environment
	- What are cgroups and namespaces?
	- Explain the functionality of a Service (with different types) within k8s
	- Why this script does not work and how you can fix this (any solution acceptable)?
	```
	#!/bin/sh
	IFS=$'\n'
	set -xe
	: "${REACT_APP_API_GATEWAY_URL?Need an environment variable called REACT_APP_API_GATEWAY_URL}"
	#replace url variable with environment specific url

	FILE_PATHS=$(find /usr/share/nginx/html/static -name 'main.*.chunk.js')
	#loop over it
	for FILE_PATH in ${FILE_PATHS[@]}
	do
	    echo "processing file"
	    echo FILE_PATH
	    sed "s/%%/$/g" $FILE_PATH | envsubst '$REACT_APP_API_GATEWAY_URL' > $FILE_PATH.new
	    mv $FILE_PATH.new  $FILE_PATH
	done

	exec "$@"
	```
	- (kann nur gefragt werden, wenn die vorherige Frage behandelt worden ist)
	  what is the difference between `[@]` vs `[*]` in <u>**bash**</u>?

