# Deescalate Situation

Customer's operation team provides a ready k8s cluster setup with following details:

- <u>**any setup**</u> (unlimited and any possible storage mounts, CPU a.s.o.), but
- **node's root FS with XFS and LVM**
	
Now a new project has to be started and one main discussion point is:
- Project Manager means that all the nodes **MUST** use **btrfs**.
- The reason for this is that some project dependent jobs **MUST** use **btrfs**

**QUESTIONS**:
- How would you de-escalate the situation and implement both wishes?
- What are the differences between XFS and BTRFS?
