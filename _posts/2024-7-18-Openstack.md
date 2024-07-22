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