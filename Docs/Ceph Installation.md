# Ceph Installation

Ceph is important to achieve shared storage. It allows us to pool together local drives on each device, as long as they are on the same LAN.  This will allow us to achieve features like high availability, VM migration, and storage availability. If one drive fails, data will be saved due to replication.

Ceph installation was pretty simple by using the gui following this guide:

https://pve.proxmox.com/wiki/Deploy_Hyper-Converged_Ceph_Cluster


I only have one network interface per device, which isn't ideal for ceph but it's fine for this homelab scenario. 

### Installation options selected:
No subscription
Number of replicas: 3
Min number of replicas: 2

### OSDs:
Each host has a usb 3.1(10Gb throughput) to sata connector for external ssds. Since I'm using small form factor devices, I decided to go for an external storage solution due to the lack of space in each device.
3 of the hosts have 1x480GB SSD. 1 host has 1x2TB SSD. Having OSDs of different sizes is also not ideal, but I'm working with what I have. Ceph will assign a larger weight to this OSD device, so more of the data is written there than the other 3 drives.

### Ceph Pool:
Accept defaults for this. Used all available OSDs to create the pool. VMs should now be able to utilize this for storage.  

### Monitors:
Created 3 total as per the instructions of the documentation.

### Manager:
Created one total as per the instructions of the documentation

### Ceph FS:
This will allow us to have a distributed filesystem amongst the proxmox hosts. For example, this will allow us to have one file accessible from each host. Installation easily done from the GUI.
