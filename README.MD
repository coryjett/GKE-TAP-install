# Multicluster TAP 1.7.3
https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.7/tap/multicluster-installing-multicluster.html

## Create GKE clusters

### View
`gcloud container clusters create tap-view --region us-east1 --node-locations us-east1-c -m e2-standard-4 --num-nodes=4`

### Build
`gcloud container clusters create tap-build --region us-east1 --node-locations us-east1-c -m e2-standard-4 --num-nodes=4`

### Run
`gcloud container clusters create tap-run --region us-east1 --node-locations us-east1-c -m e2-standard-4 --num-nodes=4`

### Iterate
`gcloud container clusters create tap-iterate --region us-east1 --node-locations us-east1-c -m e2-standard-4 --num-nodes=4`

## Install Cluster Essentials

Install on each cluster.

https://docs.vmware.com/en/Cluster-Essentials-for-VMware-Tanzu/1.7/cluster-essentials/deploy.html

### Add TAP package repository

`export INSTALL_REGISTRY_HOSTNAME=registry.tanzu.vmware.com`

`export TAP_VERSION=1.7.3`

`export INSTALL_REPO="tanzu-application-platform"`

`tanzu package repository add tanzu-tap-repository \
  --url ${INSTALL_REGISTRY_HOSTNAME}/${INSTALL_REPO}/tap-packages:$TAP_VERSION \
  --namespace tap-install`

## Deploy TAP clusters

### Deploy view cluster
`kubectl config use-context gke_fe-cjett_us-east1_tap-view`

`tanzu package install tap -p tap.tanzu.vmware.com -v 1.7.3 --values-file values-view.yaml -n tap-install`

### Deploy build cluster
`kubectl config use-context gke_fe-cjett_us-east1_tap-build`

`tanzu package install tap -p tap.tanzu.vmware.com -v 1.7.3 --values-file values-build.yaml -n tap-install`

### Deploy run cluster
`kubectl config use-context gke_fe-cjett_us-east1_tap-run`

`tanzu package install tap -p tap.tanzu.vmware.com -v 1.7.3 --values-file values-run.yaml -n tap-install`

### Deploy iterate cluster
`kubectl config use-context gke_fe-cjett_us-east1_tap-iterate`

`tanzu package install tap -p tap.tanzu.vmware.com -v 1.7.3 --values-file values-iterate.yaml -n tap-install`

## Add clusters to Tanzu Developer Portal (TDP)
https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.7/tap/tap-gui-cluster-view-setup.html

## Deploy scan policy
https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.7/tap/scc-ootb-supply-chain-testing-scanning.html
https://community.pivotal.io/s/article/5008X00002PUnwo1693277190931?language=en_US

## Deploy testing pipeline
https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.7/tap/getting-started-add-test-and-security.html

## Setup dev namespace
https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.7/tap/install-online-set-up-namespaces.html

## Deploy workload to build cluster
https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.7/tap/multicluster-getting-started.html#start-the-workload-on-the-build-profile-cluster-1

## Deploy deliverable to run cluster
https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.7/tap/multicluster-getting-started.html#start-the-workload-on-the-build-profile-cluster-1

## Handy commands

### Packages
`tanzu package available list --namespace tap-install`

`tanzu package install tap -p tap.tanzu.vmware.com -v 1.7.3 --values-file values-build.yaml -n tap-install`

`tanzu package installed update tap -p tap.tanzu.vmware.com -v 1.7.3 --values-file values-build.yaml -n tap-install`

`tanzu package installed get tap -n tap-install`

`tanzu package installed list -A`

`tanzu package installed delete tap --namespace tap-install`

`kubectl describe packageinstall/NAME -n tap-install`

### Apps

`tanzu apps workload list -n dev`

`tanzu apps workload tail tanzu-java-web-app --since 10m --timestamp --namespace dev`

### Supply Chains

`tanzu apps cluster-supply-chain list`

`tanzu apps cluster-supply-chain get basic-image-to-url`

### Scanning

`k describe scanpolicy scan-policy -n dev`

### Pipelines

`k describe pipeline developer-defined-tekton-pipeline -n dev`