# Red Hat Quay architecture #
Red Hat Quay is a distributed and highly available container image registry for your enterprise.

Red Hat Quay container registry platform provides-
Secure storage
Distribution
Access controls
Georeplications
Repository mirroring
Governance of containers
Cloud-native artifacts

![Screenshot 2024-09-08 131441](https://github.com/user-attachments/assets/b8e6f3ad-4e04-4c5a-b124-6ce21201596f)

## 1.1. SCALABILITY AND HIGH AVAILABILITY (HA)
The code base used for Red Hat Quay is the same as the code base used for Quay.io, which is the highly available container image registry hosted by Red Hat. 
Quay.io and Red Hat Quay offer a multitenant SaaS solution.

## 1.2. CONTENT DISTRIBUTION
Content distribution features in Red Hat Quay include the following:
  ### Repository mirroring
  Lets you mirror images from Red Hat Quay and other container registries, like JFrog Artifactory, Harbor, or Sonatype Nexus Repository.
  ### Geo-replication (async)
  Geo-replication allows multiple, geographically distributed Red Hat Quay deployments to work as a single registry from the perspective of a client or user.
  ### Deployment in disconnected or air-gapped environments
  Red Hat Quay is deployable in a disconnected environment in one of two ways:
    a. Red Hat Quay and Clair connected to the internet, with an air-gapped OpenShift Container Platform cluster accessing the Red Hat Quay registry through an 
       explicit, allowlisted hole in the firewall.
    b. Using two independent Red Hat Quay and Clair installations. One installation is connected to the internet and another within a disconnected, or firewalled, 
       environment. Image and vulnerability data is manually transferred from the connected environment to the disconnected environment using offline media.

  
  


