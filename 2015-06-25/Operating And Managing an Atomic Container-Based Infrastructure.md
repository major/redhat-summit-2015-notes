## Operating and Managing an Atomic Container-Based Infrastructure
_Scott Collier, Lars Kellogg-Stedman, and Brett Thurber_

### What is Atomic?
* systemd + Kubernetes + Docker + SELinux
* JEOS for containers
* Everything must run in a container on Atomic unless you can prove that it can't (don't add anyhting to the OS itself)
* Orchestration is done via kubernetes
    * Be declarative with kubernetes
    * Tell it you want 5 copies running, it keeps 5 running
* Upgrades/Rollbacks
    * OSTree handles updates/rollbacks
    * You're always in a known-good state
    * Reboot into new updates, rollback if needed
* Super privileged containers
    * *Go watch Dan Walsh's talk from Wednesday*
    * RHEL tools, rsyslog, sadc containers are provided by Red Hat for easy install
        * sadc is part of the sar/sysstat package
        * Handles logging for atomic host
        * RHEL tools container has lots of utilities
            * git, tcpdump, strace, gdb, etc
* Atomic command
    * `atomic info rhel7/rhel-tools`
    * Labels in the Dockerfile provide metadata needed for the image
* Upgrading
    * `atomic host status`
    * `atomic host upgrade && reboot`
        * Nothing in your system is being changed -- new tree dropped beside current tree
    * `atomic host status`
* Can get it with RHEL, CentOS, and Fedora
* Flannel gives an overlay network that allows cross-host container communication
    * Required for kubernetes
* Kubernetes
    * KB master on the first node
    * Other nodes are just regular KB nodes

### Deployment Options
* Manual deployment for a single server or automated deployment using heat
* cloud-init runs on Atomic when it first boots
* 