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
1.1. INSTALLING PODMAN
   Podman for creating and deploying containers
   $ sudo yum install -y podman
   $ sudo yum module install -y container-tools
   
2.1. INSTALL AND REGISTER THE RHEL SERVER
   $ subscription-manager register --username=<user_name> --password=<password>
   $ subscription-manager refresh
   $ subscription-manager list --available
   $ subscription-manager attach --pool=<pool_id>
   $ yum update -y
   
2.2. REGISTRY AUTHENTICATION
   Set up authentication to registry.redhat.io
   $ sudo podman login registry.redhat.io (Prove the Credentials)

2.3. FIREWALL CONFIGURATION
  If you have a firewall running on your system allow below ports.
 <br/> $ firewall-cmd --permanent --add-port=80/tcp \
    && firewall-cmd --permanent --add-port=443/tcp \
    && firewall-cmd --permanent --add-port=5432/tcp \
    && firewall-cmd --permanent --add-port=5433/tcp \
    && firewall-cmd --permanent --add-port=6379/tcp \
    && firewall-cmd --reload
    
2.4. IP ADDRESSING AND NAMING SERVICES
  Component Containers configured in Red Hat Quay so that they can communicate with each other.
   Using a naming service - using DNS(dnsname) to nake the containers so that they can be called by Names.
   
   Using the host network - 
   
   Configuring port mapping - Port mappings to expose ports on the host and then use these ports in combination with the host IP address or host name.
 <br/>  Component             #  Port Mapping             # Address
 <br/>  Quay                  # -p 80:8080 /-p 443:8443   # http://quay-server.example.com
 <br/>  Postgres for Quay     # -p 5432:5432              # quay-server.example.com:5432
 <br/>  Redis                 # -p 6379:6379              # quay-server.example.com:6379
 <br/>  Postgres for Clair V4 # -p 5433:5432              # quay-server.example.com:5433
 <br/>  Clair V4              # -p 8081:8080              # http://quay-server.example.com:8081

3.1. CONFIGURING PORT MAPPING FOR RED HAT QUAY 
Using port mappings to expose ports on the host and then use these ports in combination with the host IP address or host name to navigate to the Red Hat Quay endpoint.
 $ ip a
 $ cat /etc/hosts [local DNS Entry]

3.2. CONFIGURING THE DATABASE-
Red Hat Quay requires a database for storing metadata.
 $ mkdir -p $QUAY/postgres-quay [$QUAY- Installation folder]
 $ setfacl -m u:26:-wx $QUAY/postgres-quay [Apply Permissions]
 $ sudo podman run -d --rm --name postgresql-quay -e POSTGRESQL_USER=quayuser -e POSTGRESQL_PASSWORD=quaypass -e POSTGRESQL_DATABASE=quay -e POSTGRESQL_ADMIN_PASSWORD=adminpass -p 5432:5432 -v $QUAY/postgres \ quay:/var/lib/pgsql/data:Z registry.redhat.io/rhel8/postgresql-13:1-109
 $ sudo podman exec -it postgresql-quay /bin/bash -c 'echo "CREATE EXTENSION IF NOT EXISTS pg_trgm" | psql -d quay -U postgres'
 
3.3. CONFIGURING REDIS
Redis is a key-value store that is used by Red Hat Quay for live builder logs.
$ sudo podman run -d --rm --name redis -p 6379:6379 -e REDIS_PASSWORD=strongpassword registry.redhat.io/rhel8/redis-6:1-110

4.1. CREATING THE YAML CONFIGURATION FILE
$ touch config.yaml
BUILDLOGS_REDIS:
host: quay-server.example.com
password: strongpassword
port: 6379
CREATE_NAMESPACE_ON_PUSH: true
DATABASE_SECRET_KEY: a8c2744b-7004-4af2-bcee-e417e7bdd235
DB_URI: postgresql://quayuser:quaypass@quay-server.example.com:5432/quay
DISTRIBUTED_STORAGE_CONFIG:
default:
- LocalStorage
- storage_path: /datastorage/registry
DISTRIBUTED_STORAGE_DEFAULT_LOCATIONS: []
DISTRIBUTED_STORAGE_PREFERENCE:
- default
FEATURE_MAILING: false
SECRET_KEY: e9bd34f4-900c-436a-979e-7530e5d74ac8
SERVER_HOSTNAME: quay-server.example.com
SETUP_COMPLETE: true
USER_EVENTS_REDIS:
host: quay-server.example.com
password: strongpassword
port: 6379
$ mkdir $QUAY/config
$ cp -v config.yaml $QUAY/config
4.1.1. Configuring a Red Hat Quay superuser
Superusers have the following capabilities:
<br/> User management
<br/> Organization management
<br/> Service key management
<br/> Change log transparency
<br/> Usage log management
<br/> Globally-visible user message creation

Add the SUPER_USERS array to the config.yaml file:
 SERVER_HOSTNAME: quay-server.example.com
 SETUP_COMPLETE: true
 SUPER_USERS:- quayadmin
 
4.2. PREPARE LOCAL STORAGE FOR IMAGE DATA
 $ mkdir $QUAY/storage
 $ setfacl -m u:1001:-wx $QUAY/storage

4.3. DEPLOY THE RED HAT QUAY REGISTRY
$ sudo podman run -d --rm -p 80:8080 -p 443:8443 --name=quay -v $QUAY/config:/conf/stack:Z -v $QUAY/storage:/datastorage:Z registry.redhat.io/quay/quay-rhel8:v3.12.1

 CHAPTER 5. USING RED HAT QUAY
 <br/> GUI-(Using Browser)
 <br/> Access the Quay using the browser  at http://quay-server.example.com [updated same in /etc/hosts and config.yaml]
 <br/> Create Account and add a user, for example, quayadmin with a password password.
 <br/>CLI-
 <br/> $ sudo podman login --tls-verify=false quay-server.example.com
 
  5.1. PUSHING AND PULLING IMAGES ON RED HAT QUAY
 <br/> $ sudo podman pull busybox
 <br/> $ sudo podman images
 <br/> $ sudo podman tag docker.io/library/busybox quay-server.example.com/quayadmin/busybox:test [modify the Tag]
 <br/> $ sudo podman push --tls-verify=false quay-server.example.com/quayadmin/busybox:test [Push the Image]
  
