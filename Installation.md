Create Mirror from RedHat to Local.

1. Download the mirror-registry.tar.gz package for the latest version of the mirror registry for Red Hat OpenShift found on the OpenShift console Downloads page.
2. Install the mirror-registry
   [root@localmirror ~]# ./mirror-registry install --quayHostname localmirror.mylab.local --quayRoot /opt/mirror-registry

   OCP_RELEASE=4.16.4
LOCAL_REGISTRY='localmirror.mylab.local:8443'
LOCAL_REPOSITORY='ocp4/openshift4'
PRODUCT_REPO='openshift-release-dev'
LOCAL_SECRET_JSON='/home/mislam/pull-secret.json'
RELEASE_NAME="ocp-release"
ARCHITECTURE=x86_64
REMOVABLE_MEDIA_PATH=/opt/media

oc adm release mirror -a ${LOCAL_SECRET_JSON} --from=quay.io/${PRODUCT_REPO}/${RELEASE_NAME}:${OCP_RELEASE}-${ARCHITECTURE} --to=${LOCAL_REGISTRY}/${LOCAL_REPOSITORY} --to-releaseimage=${LOCAL_REGISTRY}/${LOCAL_REPOSITORY}:${OCP_RELEASE}-${ARCHITECTURE} --dry-run


4.
