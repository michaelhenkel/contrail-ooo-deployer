# Install OSP10/11 with Contrail 3.2.x/4.0.x the easy way
## pre-requisites
The undercloud is install as a VM. The ansible playbook should be    
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

