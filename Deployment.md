Comparison of Deployment tools usage percentage:

![Alt text](images/tools-usage.png?raw=true "Tools Usage")

- SaltStack: https://github.com/saurabhsurana/trove-installer/tree/master/saltstack
- SaltStack-based OpenStack: https://github.com/EntropyWorks/salt-openstack
- OpenStack with Puppet: https://wiki.openstack.org/wiki/Puppet/Deploy
- OpenStack with Chef: https://docs.chef.io/openstack.html
- OpenStack with Ansible: https://github.com/openstack/openstack-ansible
- OpenStack with PackStack: https://wiki.openstack.org/wiki/Packstack
- OpenStack with Juju: https://jujucharms.com/openstack
- OpenStack with Fuel: https://wiki.openstack.org/wiki/Fuel

List of openstack projects:
https://governance.openstack.org/tc/reference/projects/index.html

#### OpenStack Ansible:
https://github.com/openstack/openstack-ansible

# OpenStack-Ansible

OpenStack-Ansible is an official OpenStack project which aims to deploy production environments from source in a way that makes it scalable while also being simple to operate, upgrade, and grow.

For an overview of the mission, repositories and related Wiki home page, please see the formal Home Page for the project (http://governance.openstack.org/reference/projects/openstackansible.html).

For those looking to test OpenStack-Ansible using an All-In-One (AIO) build, please see the Quick Start guide (http://docs.openstack.org/developer/openstack-ansible/developer-docs/quickstart-aio.html).

For more detailed Installation and Operator documentation, please see the Deployment Guide :
#### (https://docs.openstack.org/project-deploy-guide/openstack-ansible).

If you have some questions, or would like some assistance with achieving your goals, then please feel free to reach out to us on the OpenStack Mailing Lists (particularly openstack-operators or openstack-dev) or on IRC in #openstack-ansible on the freenode network.

This guide refers to the following types of hosts:

- Deployment host, which runs the Ansible playbooks
- Target hosts, where Ansible installs OpenStack services and infrastructure components

![Alt text](images/process.png?raw=true "Deployment Process")

Install one of the following supported operating systems on the **deployment hosts**:

- Ubuntu server 16.04 (Xenial Xerus) LTS 64-bit
- Centos 7 64-bit

Configure at least one network interface to access the Internet or suitable local repositories.

We also recommend setting your locale to en_US.UTF-8. Other locales might work, but they are not tested or supported.


### Prepare the Deployment Host

#### Ubuntu
````
$ apt-get update
$ apt-get dist-upgrade
$ apt-get install aptitude build-essential git ntp ntpdate \
  openssh-server python-dev sudo
````

#### Centos 
````
$ yum upgrade
$ yum install https://rdoproject.org/repos/openstack-ocata/rdo-release-ocata.rpm
$ yum install git ntp ntpdate openssh-server python-devel \
  sudo '@Development Tools'
````
