# Open Shift Installation on Disconnected network
Required machines for cluster installation
  One temporary bootstrap machine
  Three control plane machines
  At least two compute machines, which are also known as worker machines.

Minimum resource requirements for cluster installation
Bootstrap     - RHCOS - 4CPU - 16GB RAM - 100 GB HDD
Control Plane - RHCOS - 4CPU - 16GB RAM - 100 GB HDD
Compute       - RHCOS - 2CPU -  8GB RAM - 100 GB HDD


User-provisioned DNS requirements
The Kubernetes API 
  api.<cluster_name>.<base_domain>. 
  api-int.<cluster_name>.<base_domain>.
  
The OpenShift Container Platform application wildcard
  *.apps.<cluster_name>.<base_domain>.
  
The bootstrap, control plane, and compute machines
  bootstrap.<cluster_name>.<base_domain>.
  <control_plane><n>.<cluster_name>.<base_domain>.
  <compute><n>.<cluster_name>.<base_domain>.

  api.ocp4.example.com.		IN	A	192.168.1.5 
  api-int.ocp4.example.com.	IN	A	192.168.1.5 
  *.apps.ocp4.example.com.	IN	A	192.168.1.5 
  bootstrap.ocp4.example.com.	IN	A	192.168.1.96 
  control-plane0.ocp4.example.com.	IN	A	192.168.1.97 
  control-plane1.ocp4.example.com.	IN	A	192.168.1.98 
  control-plane2.ocp4.example.com.	IN	A	192.168.1.99 
  compute0.ocp4.example.com.	IN	A	192.168.1.11 
  compute1.ocp4.example.com.	IN	A	192.168.1.7 

Generating a key pair for cluster node SSH access-
$ ssh-keygen -t ed25519 -N '' -f <path>/<file_name>
$ eval "$(ssh-agent -s)" [check ssh-agent process is running]
ssh-agent is a program to hold private keys used for public key authentication (RSA, DSA, ECDSA, ED25519).
$ ssh-add <path>/<file_name> [Add your SSH private key to the ssh-agent ~/.ssh/id_ed25519]

Manually creating the installation configuration file
Prequesite-
You have an SSH public key on your local machine to provide to the installation program.
You have obtained the OpenShift Container Platform installation program and the pull secret for your cluster.
Obtain the imageContentSources section from the output of the command to mirror the repository.
Obtain the contents of the certificate for your mirror registry.

