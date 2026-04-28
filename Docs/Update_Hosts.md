### Updating Proxmox Hosts

Proxmox uses the APT package manager.
<br>
<br>
#### APT sources currently being used
- http://deb.debian.org/debian/
- http://deb.debian.org/debian/
- http://download.proxmox.com/debian/ceph-squid
- http://download.proxmox.com/debian/pve

<br>

#### Before/After Upgrading
- Check uptime
- Check available disk space
- Check free memory
- Check for failed Services


<br>
<br>

#### Manually adding the no-subscription repository
<img width="2564" height="1283" alt="image" src="https://github.com/user-attachments/assets/5e4adfbd-1b65-48a7-a765-9bc9151f6d04" />

- Click add
- Choose No Subscription from Repository drop down
- Since this is a homelab, there is no worries about the no subscription repo
<br>
<br>

#### Manually Upgrading Host Packages
<img width="833" height="385" alt="image" src="https://github.com/user-attachments/assets/4671a971-f500-4b70-965e-4ff28b8d83a3" />

- Click Refresh to check for new updates
- Click Upgrade
- Services do restart after upgrading packages. 

<br>
<br>

#### Kernel Update
<img width="1009" height="622" alt="image" src="https://github.com/user-attachments/assets/3a25e2fc-d6fc-4924-a365-4b786a0e13ea" />

- Restart after Kernel Update

