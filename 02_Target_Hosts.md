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

### Deploying Secure Shell (SSH) keys

Ansible uses SSH to connect the deployment host and target hosts.

Copy the contents of the public key file on the **deployment host** to the /root/.ssh/authorized_keys file on each target host.

Test public key authentication from the deployment host to each target host by using SSH to connect to the target host from the deployment host. If you can connect and get the shell without authenticating, it is working. SSH provides a shell without asking for a password.
For more information about how to generate an SSH key pair, as well as best practices, see GitHub’s documentation about generating SSH keys.

https://help.github.com/articles/connecting-to-github-with-ssh/

OpenStack-Ansible deployments require the presence of a /root/.ssh/id_rsa.pub file on the **deployment host**. The contents of this file is inserted into an authorized_keys file for the containers, which is a necessary step for the Ansible playbooks. You can override this behavior by setting the lxc_container_ssh_key variable to the public key for the container.

### Configure Storage
Logical Volume Manager (LVM) enables a single device to be split into multiple logical volumes that appear as a physical storage device to the operating system. ***The Block Storage (cinder) service***, and the LXC containers that run the OpenStack infrastructure, ***can optionally use LVM for their data storage***.

**OpenStack-Ansible automatically configures LVM on the nodes, and overrides any existing LVM configuration**. If you had a customized LVM configuration, **edit the generated configuration file** as needed.

To use the **optional** Block Storage (cinder) service, create an LVM volume group named **cinder-volumes** on the ***storage host***. Specify a metadata size of 2048 when creating the physical volume. For example:

````
# pvcreate --metadatasize 2048 physical_volume_device_path
# vgcreate cinder-volumes physical_volume_device_path
````

Optionally, create an LVM volume group named **lxc** for container file systems. If the lxc volume group does not exist, containers are automatically installed on the file system under /var/lib/lxc by default.


### Network configuration

![Alt text](images/network-bridges.png?raw=true "Network Bridges")

For a detailed reference of how the host and container networking is implemented, refer to :

Appendix E: Container networking :

https://docs.openstack.org/project-deploy-guide/openstack-ansible/draft/app-networking.html#network-appendix

For use case examples, refer to Appendix A: Example test environment configuration :

https://docs.openstack.org/project-deploy-guide/openstack-ansible/draft/app-config-test.html#test-environment-config

Appendix B: Example production environment configuration

https://docs.openstack.org/project-deploy-guide/openstack-ansible/draft/app-config-prod.html#production-environment-config




