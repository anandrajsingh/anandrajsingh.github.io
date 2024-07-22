Openstack is a cloud service which offers primarily IaaS to users. It is piece of software when installed on cluster of Physical Servers, offers the users to create, run and manage Virtual Machines(VM) on top of it along with support to attach the connected storage along with the networking services in a virtual format known as Infrastructure as a Service.

**Nova**
Nova is OpenStack project that provides a way to provision. It supports creating virtual machines, baremetal servers (through the use of ironic), and has limited support for system containers. It runs as a set of daemons on top of existing Linux servers to provide that service.

It requires following additional Openstack services for basic function

- Keystone
- Glance
- Neutron
- Placement

Openstack Compute consists of the following areas and components:
1. nova-api service: Accepts and responsds to end user compute api calls.
2. nova-api-metadata service: Accepts metadata requests from instances.
3. nova-compute service: worker daemon to talk with hypervisor.
4. nova-scheduler service: decides compute host
5. nova-conductor module: Mediates interaction between nova-compute service & DB
6. nova-novncproxy daemon: Proxy to access VM console GUI via VNC.
7. nova-spicehtml5proxy: daemaon: Proxy to access VM console GUI via SPICEHTML.
8. The queue: Central Hub fro passing messages between daemons.
9. SQL database: Openstack Compute can support any database that SQLAlchemy supports 
SQLite3 for Research and Development work
MySQL, MariaDB & PostgreSQL for commercial setup


**Neutron**
Openstack Neutron is an SDN networking project focused on delivering network-as-a-service (Naas) in virtual compute environments.

Openstack Networking enables projects to create advanced virtual network topologies which may include services such as firewall, and a Virtual Private Network. It also supports security groups to block or unblock, port ranges or traffic types for that VM.

It requires Keystone for basic function.

Components in Neutron Service:
1. Neutron Server: Provides API, manages database etc.
2. Plugins: Manages Agent.
3. Agents: Provides Layer 2/3 connectivity to instances, Handles physical-virtual network transition, Handles metadata, etc.
a) Layer 2 (Ethernet and Switching)
=> Linux Bridge Agent
=> OVS Agent -> Compute & Controller both
b) Layer 3 (IP and Routing)
=> L3 Agent -> Controller
=> DHCP Agent -> Controller
c) Miscellaneous
=> Metadata Agents -> Controller


**Keystone**
API client authentication, service discovery, and distributed multi-tenant authorization by implementing Openstack's Identity API. It supports LDAP, OAuth, OpenID Connect, SAML and SQL. It is organized as a group of internal services exposed on one or many endpoints. For example, an authenticate call will validate user/ project credentials with the Identity service and upon success, create and return a token with the Token service.

**Glance**
Glance image service include discovering, registering and retrieving virtual machine images. It has RESTful API that allows querying of VM image metadata as well as retrieval of actual image. VM image made available through Glance can be stored in a variety of locations from simple filesystems to object-storage systems like the OpenStack Swift Project.

It requires Keystone for basic function.

**Horizon**
It is the canonical implementation of Openstack's dashboard, which is extensible and provides a web based user interface to Openstack services.

It requires Keystone for basic function.