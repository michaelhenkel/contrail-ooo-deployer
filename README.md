# Install OSP10/11 with Contrail 3.2.x/4.0.x the easy way
## What it does
This ansible playbook    
- creates the undercloud VM
- installs the undercloud    
- configures the undercloud    
- configures the overcloud    
-- networking    
-- nic template    
## pre-requisites
The undercloud is installed as a VM. The ansible playbook should be    
executed on the KVM host hosting the undercloud VM.    
### virsh networks
On that KVM host at least two virsh networks must be present:    
a. one for the provisioning network, connecting the undercloud VM with the overcloud nodes    
b. one for the external/public API network in order to access the API endpoints    
### stack user    
A stack user must exist with a public key
### virtualized control plane    
In case the control plane will run in VMs, the pxe_ssh driver can be used.    
Therefore it is required to define the virsh templates on the KVM hosts.    
### RHEL guest image    
As of now the RHEL guest image for the undercloud VM cannot be retrieved automatically.    
It is required to download it manually to the KVM host hosting the undercloud VM.    
### Contrail package    
The Contrail install package must be downloadable from a webserver. The tgz format is needed.    

## Configuration
### General configuration    
General parameters are defined in 
```
cat contrail-ooo-deployer/inventory/group_vars/all.yml
---
RHEL_VERSION: 7.4
RHEL_QCOW: /root/rhel-server-7.4-x86_64-kvm.qcow2
IMAGE_DIRECTORY: /var/lib/libvirt/images
UNDERCLOUD_IMAGE_SIZE: 100G
UNDERCLOUD_NAME: ocataX
UNDERCLOUD_ROOT_PWD: contrail123
UNDERCLOUD_STACK_PWD: contrail123
UNDERCLOUD_DEFAULT_NW: default
UNDERCLOUD_PROV_NW: brbm
UNDERCLOUD_INT_API_NW: int-api
UNDERCLOUD_VCPU: 4
UNDERCLOUD_RAM: 16348
SATELLITE_FQDN: satellite.juniper.lab
SATELLITE_IP: 10.87.64.101
SATELLITE_KEY: rhel-7-osp10
SATELLITE_ORG: Juniper
RH_POOL_ID: $POOL_ID
OPENSTACK_VERSION: ocata
ROOT_SSH_KEY: /root/.ssh/id_rsa.pub
STACK_SSH_KEY: /home/stack/.ssh/id_rsa.pub
CONTRAIL_VERSION: 4.0.1.0
CONTRAIL_VERSION_BUILD: 32
CONTRAIL_PACKAGE_LOCATION: http://10.0.0.2/
```
### Ironic node configuration    
```
cat contrail-ooo-deployer/inventory/group_vars/ironic_nodes.yml
---
ironic_nodes:
  control-1-at-5b3s30:
    ip: 10.87.64.31
    mac: 52:54:00:16:54:d8
    role: control
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  compute-1-at-5b3s30:
    ip: 10.87.64.31
    mac: 52:54:00:2a:7d:99
    role: compute
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  tsn-1-at-5b3s30:
    ip: 10.87.64.31
    mac: 52:54:00:e0:54:b3
    role: contrail-tsn
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-controller-1-at-5b3s30:
    ip: 10.87.64.31
    mac: 52:54:00:d6:2b:03
    role: contrail-controller
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-analytics-1-at-5b3s30:
    ip: 10.87.64.31
    mac: 52:54:00:01:c1:af
    role: contrail-analytics
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-analytics-database-1-at-5b3s30:
    ip: 10.87.64.31
    mac: 52:54:00:4a:9e:52
    role: contrail-analytics-database
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  control-1-at-centos:
    ip: 10.87.64.32
    mac: 52:54:00:40:9e:13
    role: control
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  compute-dpdk-1-at-centos:
    ip: 10.87.64.32
    mac: 52:54:00:1d:58:4d
    role: compute-dpdk
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  compute-2-at-centos:
    ip: 10.87.64.32
    mac: 52:54:00:6d:89:2d
    role: compute
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-controller-1-at-centos:
    ip: 10.87.64.32
    mac: 52:54:00:a8:46:5a
    role: contrail-controller
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-analytics-1-at-centos:
    ip: 10.87.64.32
    mac: 52:54:00:b3:2f:7d
    role: contrail-analytics
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-analytics-database-1-at-centos:
    ip: 10.87.64.32
    mac: 52:54:00:59:e3:10
    role: contrail-analytics-database
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  control-1-at-5b3s32:
    ip: 10.87.64.33
    mac: 52:54:00:1d:8c:39
    role: control
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  compute-1-at-5b3s32:
    ip: 10.87.64.33
    mac: 52:54:00:9c:4b:bf
    role: compute
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  compute-2-at-5b3s32:
    ip: 10.87.64.33
    mac: 52:54:00:1d:a9:d9
    role: compute
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-controller-1-at-5b3s32:
    ip: 10.87.64.33
    mac: 52:54:00:cd:59:92
    role: contrail-controller
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-analytics-1-at-5b3s32:
    ip: 10.87.64.33
    mac: 52:54:00:2f:81:1a
    role: contrail-analytics
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
  contrail-analytics-database-1-at-5b3s32:
    ip: 10.87.64.33
    mac: 52:54:00:a1:4a:23
    role: contrail-analytics-database
    user: root
    password: c0ntrail123
    memory: 16348
    cpus: 4
    disk_gb: 40
    ipmi_driver: pxe_ssh
```    
This example imports a couple of nodes and adds profiling information to it (role)    
