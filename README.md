# Minio

MinIO's high-performance, software-defined object storage suite
enables customers to build cloud-native data infrastructure for
machine learning, analytics and application data workloads.


## On-premises 
I have K8s cluster deployed in my local datacenter enviornment, with one Master and 3 worker nodes.
For Persistence Volumes I am us Portworx as storage solution, you will find lot of storage solution available in market. Portworx is paid solution but they too have Freemium product
 as PX-Essentials which meets requirements for smaller clusters. There are other opensource soultions like OpenEBS, Rook, Robin etc. It's your choice what suits you best.

As setup is on-prem you would need an S3 base object store for taking backups. We will be using as `Minio` which is S3 compitable object store which is widely used.

## Perquisites
* K8s installed (1.15 Tested)
* Portworx PX-Essential (or Enterprise) installed 
* Make sure your Portworx is up and running 


## Install Minio
There are multiple ways of installing Minio
* StatefulSet Spec
* Helm
* Operator 

More info can be found on : https://docs.min.io/docs/deploy-minio-on-kubernetes.html

## Two ways of install
[Standalone]
[Distributed]
