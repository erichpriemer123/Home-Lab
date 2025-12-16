# Template Configuraiton
For template creation we will use Cloud-init. Cloud-init will allow me to automate the process of configuring a virtual machine and the installation of an operating system.
We will be using a RHEL cloud image (qcow2). These images are already set up for use with cloud-init and are made to be templated. They are also optimized to run as virtual machines and are more storage efficient. When the creation of the virtual machine template is done, every cloned VM from this template will quickly boot and we will have a unique, ready to use virtual machine.

This is the video I followed to configure the RHEL cloud image template that I will be using for all virtual machines within my environmentL
https://www.youtube.com/watch?v=1Ec0Vg5be4s

## Steps: 
1. Create VM (choose ceph everytime)
    1. OS TAB -> do not use any media 
    2. System TAB -> Machine type q35 (has a native PCIe bus)
    3. System TAB -> OVMF (UEFI)
    4. System TAB -> Qemu Agent
    5. Disk TAB -> Remove Default disk
    6. Confirm TAB -> Start after created should not be selected
2. Remove CD/DVD drive from VM
3. Add CloudInit Drive
    - bus/device: IDE,2
4. Open Proxmox shell and run commands:
    - wget https://access.cdn.redhat.com/content/origin/files/sha256/e8/e89e0a3e28c3e7fb73a97483b60130d5a3e4bba9481ae282ec6e0551ccf30047/rhel-9.7-x86_64-kvm.qcow2?user=f8b6f90bb552c86770abc0d12de8a4a3&_auth_=1763846673_381503cbea69854975645d98d61ec199
5. Resize image to your liking using this command:
    - Qemu-img resize <rhel_img_name> <# of GB>G
6. Attach disk to vm using this command:
    - qm importdisk <vm-id> <rhel_img_name> <storage-location>
    - Storage location = ceph pool
7. After attaching the disk, edit disk settings and enable discard. This will allow the guest OS run trim commands, freeing unused space back to the host
8. In vm options, edit Boot Order, and disable network boot, and enable disk boot
9. Convert the VM to a template
10. Edit cloud-init settings, add default user and under IP config choose dhcp
11. You should be able to clone virtual machines from these templates. Use full clones.
12. Restart clone after first boot
