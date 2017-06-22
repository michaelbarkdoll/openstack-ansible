OpenStack-Ansible deploys Linux containers (LXC) and uses Linux bridging between the container and the host interfaces to ensure that all traffic from containers flows over multiple host interfaces. This appendix describes how the interfaces are connected and how traffic flows.

For more information about how the OpenStack Networking service (neutron) uses the interfaces for instance traffic, please see the OpenStack Networking Guide.

https://docs.openstack.org/ocata/networking-guide/


## Bonded network interfaces

In a typical production environment, physical network interfaces are combined in bonded pairs for better redundancy and throughput. Avoid using two ports on the same multiport network card for the same bonded interface, because a network card failure affects both of the physical network interfaces used by the bond.

## Linux bridges

The combination of containers and flexible deployment options requires implementation of advanced Linux networking features, such as bridges and namespaces.

- Bridges provide layer 2 connectivity (similar to switches) among physical, logical, and virtual network interfaces within a host. After a bridge is created, the network interfaces are virtually plugged in to it.

OpenStack-Ansible uses bridges to connect physical and logical network interfaces on the host to virtual network interfaces within containers.

- Namespaces provide logically separate layer 3 environments (similar to routers) within a host. Namespaces use virtual interfaces to connect with other namespaces, including the host namespace. These interfaces, often called veth pairs, are virtually plugged in between namespaces similar to patch cables connecting physical devices such as switches and routers.

Each container has a namespace that connects to the host namespace with one or more veth pairs. Unless specified, the system generates random names for veth pairs.

The following image demonstrates how the container network interfaces are connected to the host’s bridges and physical network interfaces: