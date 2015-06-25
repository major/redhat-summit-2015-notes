## Red Hat Enterprise Virtualization Hypervisor: KVM now & in the future
_Karen Noel (Red Hat)_

### Intro
* KVM from Red Hat is the best out there (claimed by speaker) ;)
    * Because of what Red Hat does to KVM
* Red Hat is the maintainer of almost all RHEV-based applications
    * Most development focus is on qemu
    * 75% of RHEV devs work on qemu
    * libvirt contributions are up since OpenStack began using it
    * Guest functions: virtio, guest clock, etc
    * qemu-ga is the guest agent and talks to libvirt via serial
        * Helps with snapshot support, freeze/thaw, among other things
    * Red Hat maintains the virtio-win drivers for Windows guests
        * Talks to the same backends as qemu
    * What is Red Hat Enterprise Hypervisor?
        * Part of the RHEV product
        * Part of RHEL OpenStack Plaform
        * RHEV-H and RHEV-M are different

### RHEV Hypervisor 7.1
* New kernel development and qemu development gets backported into RHEV
    * qemu 2.1.2 in RHEL 7 with bugfixes from QEMU 2.2
    * Some devices/features get disabled in QEMU in RHEL
        * Most removed items are not being maintained or not needed in RHEL
        * Some of those items aren't good for security
        * Lovingly referred to as "crap" features
* QEMU 2.3 coming in 7.2
* QEMU 3.0 in 7.3

### RHEV Performance & Scaling
* RHEV team works closely with performance teams that focus on kernel
* SPECvirt_sc2013 results show RHEV as 10 of top 14 results
* VMWare and FusionSphere on the lower end of perf ratings

### RHEV 7 Future Roadmap
* Live migration
    * RHEL 6 to RHEL 7 migration is possible
    * Migration between different versions of 7 is possible
    * Can run RHEL 6 machine types on 7
* Live snapshot active merge
    * Live snaps -- capture disks/memory at a point in time and store in qcow2 volume chains
    * Uses block mirroring
* Network Functions Virtualization (NFV)
    * Config 1: NUMA pinning/topology awareness, SR-IOV, DPDK
    * Config 2: DPDK + OpenvSwitch, vhost-user
    * Future: OPNFV
    * Real-time KVM - low avg/max latency requirements
* Libvirt knows about NUMA topology now
* Hugepage NUMA awareness is available
    * 2MB + 1GB hugepages with NUMA awareness
* VFIO device assignment (PCI passthrough)
* vCPU pinning keeps the VM's cpus pinned to a single NUMA node
* DPDK + VFIO
    * Hand off an Intel XL710 to a guest through IOMMU/VFIO
    * Guest uses dpdk-lib and DPDK-aware apps to talk to the device
    * Soon the card can be assigned to openvswitch instead of to the guest
* Real-time KVM for NFV
    * Goal: Bring latency down without spikes
    * New tuned profiles for hosts/guests in 7.2
    * New RHEL-RT kernel in host/guest (includes KVM)
    * KVM + kernel patches + libvirt support planned for 7.2
* Virtual storage improvements (future)
    * Single VM scaling w/IOthreads
        * Fine grained locking in QEMU
        * virtio dataplane
        * IOthread pinning
        * thread per disk/initiator or host cpu
    * Multiqueue block layer in qemu
    * RHEV for little endian (coming in 7.2)