# Ceph
**cloud-native storage orchestrator** that operates as a pod inside a k8s cluster

### Overview: Distributed storage system providing object, block, and file storage.

##### Key Attack Surfaces:

- **Ceph Dashboard**: Web interface; check for default credentials or weak auth.
- **Ceph Monitors & Managers**: Critical for cluster coordination; ensure they're not exposed.
- **RADOS Gateway (RGW)**: S3-compatible API; test for insecure bucket policies or auth bypass.

### Resources:

[Ceph Storage](https://rook.io/docs/rook/v1.9/ceph-storage.html)

[User Management — Ceph Documentation](https://docs.ceph.com/en/latest/rados/operations/user-management/)

[Past vulnerabilities — Ceph Documentation](https://docs.ceph.com/en/latest/security/cves/)

> **Default creds**
> If you do not specify a user name, Ceph will use `client.admin` as the default user name.
> 
>  If you do not specify a keyring, Ceph will look for a keyring via the `keyring` setting in the Ceph configuration. For example, if you execute the `ceph health` command without specifying a user or a keyring, Ceph will assume that the keyring is in `/etc/ceph/ceph.client.admin.keyring` and will attempt to use that keyring.

# Look for:
- Lack of encryption on disk and in transit
- Access to rook operator 

## **Ceph Attack Checklist**
### Reconnaissance

- [ ]  Identify Ceph Dashboard (default port: 8443)
- [ ]  Locate RADOS Gateway (S3-compatible API)
- [ ]  Enumerate Ceph Monitors and Managers
### Exploitation

- [ ]  Test for default credentials on dashboard
- [ ]  Attempt S3 bucket enumeration and access
- [ ]  Exploit weak bucket policies or ACLs
- [ ]  Abuse exposed Ceph REST API endpoints
### Post-Exploitation

- [ ]  Extract stored data from pools/buckets
- [ ]  Modify or delete objects to test impact
- [ ]  Pivot to other services using credentials or data

Irrespective of the type of Ceph client, for example, block device, object store, file system, native API, or the Ceph command line, Ceph stores all data as objects within pools. Ceph users must have access to pools in order to read and write data. Additionally, administrative Ceph users must have permissions to execute Ceph’s administrative commands.

### [3.2.2. Using the Ceph command interface interactively](https://docs.redhat.com/en/documentation/red_hat_ceph_storage/4/html/administration_guide/monitoring-a-ceph-storage-cluster#using-the-ceph-command-interface-interactively_admin) 

[Ceph CLI](https://docs.ceph.com/en/latest/rados/operations/monitoring/)

You can interactively interface with the Ceph storage cluster by using the `ceph` command-line utility.

**Prerequisites**

- A running Red Hat Ceph Storage cluster.
- Root-level access to the node.

**Procedure**

1. To run the `ceph` utility in interactive mode.
2. **Bare-metal** deployments:

**Example**
```plaintext
[root@mon ~]# ceph
ceph> health
ceph> status
ceph> quorum_status
ceph> mon_status
```
3. **Container** deployments:
**Red Hat Enterprise Linux 7**
```plaintext
docker exec -it ceph-mon-MONITOR_NAME /bin/bash
```
**Red Hat Enterprise Linux 8**
```plaintext
podman exec -it ceph-mon-MONITOR_NAME /bin/bash
```
- _MONITOR_NAME_ with the name of the Ceph Monitor container, found by running the `docker ps` or `podman ps` command respectively.
**Example**
```plaintext
[root@container-host ~]# podman exec -it ceph-mon-mon01 /bin/bash
```
This example opens an interactive terminal session on `mon01`, where you can start the Ceph interactive shell.
3. To list all socket files for the Ceph processes:
```plaintext
[root@mon ~]# ls /var/run/ceph
```