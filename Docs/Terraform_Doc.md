Terraform has allowed me to create a rhel template. This template will be preconfigured with the settings that I see as ideal for this environment. Clones made from this template can edit the name of the VM, the node on which the VM runs, and the amount of cpu cores, memory, and disk space the VM has configured. 
<br>
<br>
Note: Terraform provider uses local user created below via adduser, to ssh into the host to create the cloud-init snippet. Pam user is used to create all other resources.


Documentation:
- https://registry.terraform.io/providers/bpg/proxmox/latest/docs
- https://www.trfore.com/posts/using-terraform-to-create-proxmox-templates/
- https://cloudinit.readthedocs.io/en/latest/reference/examples.html

<br>

### 1. Enable snippets on proxmox
Datacenter > storage > cephfs > edit > contents > Click on snippets and save


### 2. Create terraform user:
pveum user add terraform@pam

### 3. Create terraform role:
pveum role add Terraform -privs "Realm.AllocateUser, VM.PowerMgmt, VM.GuestAgent.Unrestricted, Sys.Console, Sys.Audit, Sys.AccessNetwork, VM.Config.Cloudinit, VM.Replicate, Pool.Allocate, SDN.Audit, Realm.Allocate, SDN.Use, Mapping.Modify, VM.Config.Memory, VM.GuestAgent.FileSystemMgmt, VM.Allocate, SDN.Allocate, VM.Console, VM.Clone, VM.Backup, Datastore.AllocateTemplate, VM.Snapshot, VM.Config.Network, Sys.Incoming, Sys.Modify, VM.Snapshot.Rollback, VM.Config.Disk, Datastore.Allocate, VM.Config.CPU, VM.Config.CDROM, Group.Allocate, Datastore.Audit, VM.Migrate, VM.GuestAgent.FileWrite, Mapping.Use, Datastore.AllocateSpace, Sys.Syslog, VM.Config.Options, Pool.Audit, User.Modify, VM.Config.HWType, VM.Audit, Sys.PowerMgmt, VM.GuestAgent.Audit, Mapping.Audit, VM.GuestAgent.FileRead, Permissions.Modify"

### 4. Assign role to user:
pveum aclmod / -user terraform@pam -role Terraform

### 5. Create api token:
pveum user token add terraform@pam provider --privsep=0


### 6. Add a local user to each target host. 
adduser --home /home/terraform --shell /bin/bash terraform

### 7. Add user to sudoers
usermod -aG sudo terraform

### 8. Add local user to all host sudo files
visudo -f /etc/sudoers.d/terraform
<br>
Then add  these line:
<br>
terraform ALL=(root) NOPASSWD: /usr/bin/tee /mnt/pve/cephfs/*
<br>
terraform ALL=(root) NOPASSWD: /sbin/pvesm
<br>
terraform ALL=(root) NOPASSWD: /sbin/qm
<br>
terraform ALL=(root) NOPASSWD: /usr/bin/tee /var/lib/vz/*

### 9. On dev machine, create ssh key pair and add public key to proxmox server:
Run this once
<br>
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/terraform_id_ed25519 -C "USER_EMAIL"
<br>
Run this against every host in cluster
<br>
ssh-copy-id -i ~/.ssh/terraform_id_ed25519.pub terraform@<PVE_SERVER_ADDRESS>


### 10. Run these commands to create environment variables in your dev environment. Or add them to tfvars file:
Must escape the exclamation in the token id
<br>
export TF_VAR_pve_token_id=terraform@pve\!token
<br>
export TF_VAR_pve_token_secret=Example
<br>
export TF_VAR_pve_api_url=https://10.0.0.13:8006/
