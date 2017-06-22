## Prepare Target Hosts

Install one of the following supported operating systems on the target host:
- Ubuntu server 16.04 (Xenial Xerus) LTS 64-bit
- Centos 7 64-bit

We recommend adding the Secure Shell (SSH) server packages to the installation on target hosts that do not have local (console) access.

We also recommend setting your locale to en_US.UTF-8. Other locales might work, but they are not tested or supported.


#### Ubuntu 
````
apt-get update
apt-get dist-ugrade
reboot

# Ensure that the kernel version is 3.13.0-34-generic or later
uname -r

apt-get install bridge-utils debootstrap ifenslave ifenslave-2.6 \
  lsof lvm2 ntp ntpdate openssh-server sudo tcpdump vlan
  
# Add the appropriate kernel modules to the /etc/modules file to enable VLAN and bond interfaces:

echo 'bonding' >> /etc/modules
echo '8021q' >> /etc/modules

# Configure Network Time Protocol (NTP) in /etc/ntp.conf to synchronize with a suitable time source and restart the service:
service ntp restart
````

#### CentOS
````
yum upgrade
yum install bridge-utils iputils lsof lvm2 \
  ntp ntpdate openssh-server sudo tcpdump

# Add the appropriate kernel modules to the /etc/modules file to enable VLAN and bond interfaces:
echo 'bonding' >> /etc/modules-load.d/openstack-ansible.conf
echo '8021q' >> /etc/modules-load.d/openstack-ansible.conf

# Configure Network Time Protocol (NTP) in /etc/ntp.conf to synchronize with a suitable time source and start the service:
systemctl enable ntpd.service
systemctl start ntpd.service

# Reboot the host to activate the changes and use the new kernel.
reboot
````


