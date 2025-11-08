Proxmox installation:

1. Install pve 9 installer iso from website:
2. https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso
3. Use RUFUS and usb to create an installer with iso
4. Plug in usb to each server
5. Fill out Time zone as new york
6. Put in email and create root password
7. Choose installation disk
8. Set IP/subnet, DNS, default gateway, FDQN
9. naming scheme: pve<1-4>.ErichStudios.local
10. Continue with the installation
11. Once installation is completed, restart server and remove usb
12. Take down device name, cpu type, ram, storage space, root password



Summary:

I didnt really run into any real issues with installing proxmox on the servers. The only issue I had was on pve4. When installing I received this: “error /boot/ file not found”. I retried the installation two more times. On the third installation it was successful. Not sure what changed but I guess the third time was the charm. The other minor issue I had was figuring out the FQDN of each host. Choosing the subdomain/hostname was easy, pve<1-4>. But choosing the domain name was the real challenge. Naming the domain for my home lab was a big decision for me. I ultimately decided on “ErichStudios”. For the top level domain I choose local. Here is the naming scheme: pve<1-4>.ErichStudios.local. I will need to go and change the host name of each device due to me using capital letters. But that is a problem for another day.
