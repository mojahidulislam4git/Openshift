## CHAPTER 1. 
DEPLOYING RED HAT QUAY ON PREMISE
The following image shows examples for on premise configuration, for the following types of deployments:
<br/> a. Standalone Proof of Concept
<br/> b. Highly available deployment on multiple hosts
<br/> c. Deployment on an OpenShift Container Platform cluster by using the Red Hat Quay Operator


![image](https://github.com/user-attachments/assets/daa8aa66-6188-4730-9eac-93027494457b)

Proof of Concept deployment -
 Red Hat Quay runs on a machine with image storage, containerized database, Redis, and optionally, Clair security scanning.


PREREQUISITES
1. One Machine(Physical/VM)
   Supported Architecure
   <br/> a. amd64/x86_64  (I am using x86_64)
   <br/> b. s390x
   <br/> c. ppc64le 
2. Red Hat Enterprise Linux (RHEL) 9
3. An active subscription to Red Hat
4. Two or more virtual CPUs
5. 4 GB or more of RAM
6. Approximately 30 GB of disk space on your test system
   <br/>    a. 10 GB of disk space for the RHEL OS.
   <br/>    b. 10 GB of disk space for Docker storage for running three containers.
   <br/>    c. 10 GB of disk space for Red Hat Quay local storage
