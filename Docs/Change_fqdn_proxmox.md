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

I will be change the fqdn from <b>pve<1-4>.ErichStudios.local</b> ->  <b>pve<1-4>.erichlab.org</b>
<br>
<br>
First, I will change the cluster name, which is currently ErichStudios to erichlab. 
<br>
I will then change the 
<br>
<br>
### Change the cluster name

1. On any node

    -  Run command, "nano /etc/pve/corosync.conf" and change clustername in <b>totem</b>, to desired name and increment config_version by 1.

<br>

2. On every node, restart corosync service.
3. On any node, restart pveproxy, refresh browser, restart pve-cluster, and refrash browser one last time.
<br>
<br>

### Change the fqdn of a host
<br>
Note, steps 2 onward are to be done one at a time for each host.
<br>
<br>

1. Temporarily Disable HA
    - Stop PVE-HA-LRM service on all nodes, one at a time
    - Once PVE-HA-LRM is stopped on each node, stop PVE-HA-CRM on each node, one at a time.

2. Edit /etc/hosts on a single host
    - change the domain name and sub-domain to erichlab.org
    
3. Update the myhostname value in /etc/postfix/main.cf on a single host
    - remove ErichStuios.local and change it to erichlab.org
4. Restart corosync service on every host
5. Reboot host that you changed the fqdn for.
6. Reissue certs on each node
    - pvecm updatecerts -f
7. restart pveproxy and pvestatd on each node
