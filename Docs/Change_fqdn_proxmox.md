# Reason for Changing the fqdn of a proxmox host

The original fqdn I gave the proxmox hosts, <b>pve<1-4>.ErichStudios.local</b> has some issues with it.
<br>
<br>
While dns is case insensitve per RFC 4343, some systems, either proxmox or another services I may install in my lab, may be case sensitive.
Some program, may convert the current fqdn to lowercase, and pass it to one of these systems and cause errors. Standardizing on lowercase can help me avoid these edge cases in the future.
<br>
<br>
The final problem I have with my fqdn, is that I used the .local subdomain. This subdomain is reserved by mDNS.I will be changing this to .org
<br>
<br>
# How to change the fqdn of proxmox hosts in a cluster
Sources: 
-  https://pve.proxmox.com/wiki/Renaming_a_PVE_node
-  https://pve.proxmox.com/wiki/Cluster_Manager
-  https://forum.proxmox.com/threads/changing-name-of-nodes-in-cluster.136623/
-  https://forum.proxmox.com/threads/rename-a-cluster-not-a-node.34442/
-  https://www.thomas-krenn.com/en/wiki/Change_hostname_in_a_productive_Proxmox_Ceph_HCI_cluster
-  https://forum.proxmox.com/threads/wrong-ip-in-subject-alternative-names-section.179353/
-  https://forum.proxmox.com/threads/new-hostname-and-ssl-certificates.44736/

The fqdn will be changed from <b>pve<1-4>.ErichStudios.local</b> ->  <b>pve<1-4>.erichlab.org</b>

This will require use to change the cluster name, edit configs on each node, and reissue certificates
<br>
<br>
### Change the cluster name

1. On any node
    -  Run command, "nano /etc/pve/corosync.conf" and change clustername in <b>totem</b>, to desired name and increment config_version by 1.
2. On every node, restart corosync service.
3. On any node, restart pveproxy, refresh browser, restart pve-cluster, and refrash browser one last time.
<br>
<br>

### Change the fqdn of a host

1. Temporarily Disable HA
    - Stop PVE-HA-LRM service on all nodes, one at a time
    - Once PVE-HA-LRM is stopped on each node, stop PVE-HA-CRM on each node, one at a time.

2. Edit /etc/hosts on every host
    - change the domain name and sub-domain to erichlab.org
    
3. Update the myhostname value in /etc/postfix/main.cf on every host
    - remove ErichStudios.local and change it to erichlab.org
4.  Modify the search value in /etc/resolv.conf on every host
    - remove ErichStudios.local and change it to erichlab.org
5. Run this command to update aliases.db
    - newaliases
6. Remove the pve-ssl key and pem file for each node
    - rm -f /etc/pve/nodes/hostname/pve-ssl.pem
    - rm -f /etc/pve/nodes/hostname/pve-ssl.key
7. Remove the CA root pem and key
    - rm -f /etc/pve/pve-root-ca.pem
    - rm -f /etc/pve/priv/pve-root-ca.key
8. Reissue certs on each node
    - pvecm updatecerts -f
9. restart pveproxy and pvestatd on each node
    - systemctl restart pveproxy && systemctl restart pvestatd
