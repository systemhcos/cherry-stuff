ian@ian-XPS-8500:~/Documents/dev/hcos-backend$ ssh root@93.115.27.203
The authenticity of host '93.115.27.203 (93.115.27.203)' can't be established.
ECDSA key fingerprint is: SHA256:Q8be4h1Rykl6BDDYQ0VW1QMHIFUyeHfnTQqS7NT766M
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '93.115.27.203' (ECDSA) to the list of known hosts.
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
(root@93.115.27.203) Password: 
The time and date of this login have been sent to the system logs.

WARNING:
   All commands run on the ESXi shell are logged and may be included in
   support bundles. Do not provide passwords directly on the command line.
   Most tools can prompt for secrets or accept them from standard input.

VMware offers powerful and supported automation tools. Please
see https://developer.vmware.com for details.

The ESXi Shell can be disabled by an administrative user. See the
vSphere Security documentation for more information.
[root@more-poodle:~] cd /etc/
[root@more-poodle:/etc] ls | grep netplan
[root@more-poodle:/etc] ls | grep networj
[root@more-poodle:/etc] ls | grep network
[root@more-poodle:/etc] esxcli network nic list
Name    PCI Device    Driver  Admin Status  Link Status  Speed  Duplex  MAC Address         MTU  Description
------  ------------  ------  ------------  -----------  -----  ------  -----------------  ----  -----------
vmnic0  0000:03:00.0  ixgben  Up            Up           10000  Full    a4:bf:01:4e:37:14  1500  Intel(R) Ethernet Controller X540-AT2
vmnic1  0000:03:00.1  ixgben  Up            Down             0  Half    a4:bf:01:4e:37:15  1500  Intel(R) Ethernet Controller X540-AT2
[root@more-poodle:/etc] esxcli network vswitch standard list
vSwitch0
   Name: vSwitch0
   Class: cswitch
   Num Ports: 6400
   Used Ports: 4
   Configured Ports: 128
   MTU: 1500
   CDP Status: listen
   Beacon Enabled: false
   Beacon Interval: 1
   Beacon Threshold: 3
   Beacon Required By: 
   Uplinks: vmnic0
   Portgroups: VM Network, eno1
[root@more-poodle:/etc] esxcli network vswitch standard portgroup list
Name        Virtual Switch  Active Clients  VLAN ID
----------  --------------  --------------  -------
VM Network  vSwitch0                     0        0
eno1        vSwitch0                     1        0
[root@more-poodle:/etc] sxcli network ip interface ipv4 get
-sh: sxcli: not found
[root@more-poodle:/etc] esxcli network ip interface ipv4 get
Name  IPv4 Address   IPv4 Netmask   IPv4 Broadcast  Address Type  Gateway      DHCP DNS
----  -------------  -------------  --------------  ------------  -----------  --------
vmk0  93.115.27.203  255.255.255.0  93.115.27.255   STATIC        93.115.27.1     false
[root@more-poodle:/etc] esxcli network vswitch standard portgroup add --portgroup-name="bond0" --vswitch-name=vSwitch0
[root@more-poodle:/etc] esxcli network vswitch standard portgroup set --portgroup-name="bond0" --vlan-id=LT-3100
Error: While processing '--vlan-id'. Argument type mismatch. Expecting integer value. Got 'LT-3100'

Usage: esxcli network vswitch standard portgroup set [cmd options]

Description: 
  set                   Set the vlan id for the given port group

Cmd options:
  -p|--portgroup-name=<str>
                        The name of the port group to set vlan id for. (required)
  -v|--vlan-id=<long>   The vlan id for this port group. This value is in the range (0 - 4095)
[root@more-poodle:/etc] esxcli network vswitch standard portgroup set --portgroup-name="bond0" --vlan-id=3100
[root@more-poodle:/etc] esxcli network vswitch standard uplink add --uplink-name=vmnic1 --vswitch-name=vSwitch0
[root@more-poodle:/etc] esxcli network vswitch standard uplink add --uplink-name=vmnic1 --vswitch-name=vSwitch0
