# PersistentVolumn system
- an API for users and administrators that abstracts details of how storage is provided(PersistentVolume) from how it is consumed.(PersistentVolumeClaim)

## PersistentVolume
- a piece of storage in the cluster that has been provisioned by an administrator.
- PVs are volume plugins like Volumes, but have a lifecycle independent of any individual pod that uses the PV
- captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system

## PersistentVolumeClaim
- a request for storage by a user
- Pods consume node resources and PVCs consume PV resources
- Claims can request specific size and access modes (e.g., can be mounted once read/write or many times read-only

## StorageClass
- Cluster administrators need to be able to offer a variety of PersistentVolumes that differ in more ways than just size and access modes, without exposing users to the details of how those volumes are implemented.
- Volume implementations such as gcePersistentDisk are configured through StorageClass resources
- GKE creates a default StorageClass for you which uses the standard persistent disk type. The default StorageClass is used when a PersistentVolumeClaim doesn't specify a StorageClassName.

##The interaction between PVs and PVCs follows ↓ lifecycle
 - provitioning
 - binding
 - using
 
##pod use gce persistence disk as volumns by persistenceVolumnClaim
- On GKE, PersistentVolumes are typically backed by Compute Engine persistent disks.
- Unlike Volumes, the PersistentVolumes lifecycle is managed by Kubernetes.
- PersistentVolumes are cluster resources that exist independently of Pods. 
- This means that the disk and data represented by a PersistentVolume continue to exist as the cluster changes and as Pods are deleted and recreated
- A PersistentVolumeClaim is a request for and claim to a PersistentVolume resource
- Pods use claims as Volumes. The cluster inspects the claim to find the bound Volume and mounts that Volume for the Pod.
- Portability is another advantage of using PersistentVolumes and PersistentVolumeClaims. You can easily use the same Pod specification across different clusters and environments because PersistentVolume is an interface to the actual backing storage.
- ReadWriteMany: The Volume can be mounted as read-write by many nodes. 
  PersistentVolumes that are backed by Compute Engine persistent disks don't support this access mode.


#use case
- Deployments are designed for stateless applications and therefore all replicas of a Deployment share the same Persistent Volume Claim. Since the replica Pods created will be identical to each other, only Volumes with modes ReadOnlyMany or ReadWriteMany can work in this setting
- 
