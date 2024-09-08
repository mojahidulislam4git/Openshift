Mirror Registry Tools
https://console.redhat.com/openshift/downloads#tool-oc-mirror-plugin
  1. mirror-registry.tar.gz
  2. oc-mirror.rhel8.tar.gz (RHEL-8)
  3. oc-mirror.rhel9.tar.gz (RHEL-9)

$ ./mirror-registry install --quayHostname <host_example_com> --quayRoot <example_directory_name>
$ podman login -u init -p <password> <host_example_com>:8443> --tls-verify=false 

Web- https://<host.example.com>:8443
