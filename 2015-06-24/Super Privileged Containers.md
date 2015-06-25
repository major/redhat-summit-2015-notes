## Super Privileged Containers
_Dan Walsh_

<iframe width="1280" height="720" src="https://www.youtube.com/embed/dM2Fc53Dtd4" frameborder="0" allowfullscreen></iframe>

*The session video contains plenty of great SELinux jokes other humor.*

### Problem: Want to add something to Atomic platform
* No yum install, keeps the system very small
* Everyone wants an application added to Atomic image
    * Prove that the application can't run in a container first
    * Almost applications can

### Problem: Need to ship a container to manage the host
* What if my application's job is to manage the host or other containers
* What if I want to run gdb or strace?
* *Plug: Dan has a talk tomorrow about securing containers*

### Super privileged containers (SPC)
* Can manipulate content on the host
* Get started by turning off the security
    * Seriously ;)
* Running docker with --privileged turns off SELinux for that container -- ***DANGER***
    * Disables seccomp/user namespace
    * Disables SELinux separation
    * Disables mounting of filesystems readonly
    * Allows creation of Linux devices
* Disable namespace separation
    * `docker run --net=host` (use host's network devices directly)
        * Can run a confined docker container with this switch as well
    * `docker run --ipc=host` (share host's IPC namespace)
    * Kubernetes does this automatically (shared net/ipc)
    * `docker run --pid=host` (container can see all host processes)
* Mount namespace
    * `docker run -v /run:/run` (bind mounts host's `/run` in container)
        * Now you can manipulate systemd or docker from within a container
    * `docker run -v /dev:/dev` (shares device nodes w/container)
        * Allows libvirt to run in containers
        * Look at the "Kolla" (sp?) project which containerizes OpenStack software (including libvirt)
    * `docker run -v /:/host -e HOST=/host`
        * Full access to host
        * Able to `chroot /host` and access the whole host
        * ***BE CAREFUL***

### Demo time
*Check the session video for more. Dan often posts these videos as screencasts later on as well.*

* Container processes labeled with `spc_t`
    * `spc_t` can be thought of as `unconfined_t`
* Dan mounted /run, /dev, and / inside the SPC
    * `chroot /host`
    * Created a file called `/dan` on the host's OS from within the SPC
    * Can manipulate host services, talk to docker daemon, etc

### SPC (continued)
* SPC's require a really really long command line
* Don't get mad when Atomic won't let you use yum
* Now there's a RHELTools image available
    *  Has extra software: strace, ping, sosreport, man pages, gdb
    *  It's a fat container
*  There's a new `atomic` command
    *  Run `atomic --spc <docker_img>` and jump right into the RHELTools container in one step
* Atomic host is managed by rpm-ostree (use atomic for quick access)
    * `atomic host-status`
    * `atomic host-upgrade`
    * `atomic host-rollback` 

### My application only needs one or two more privileges
* Use `--cap_add SYS_TIME` (not ideal)
* No need for full blown SPC (that's going wide open w/security)
* Introducing container image labels
    * Labels patch -- developers can add content to image json data
    * `LABEL RUN docker run -d -n ntpd --cap_add SYS_TIME <image>`
    * Then do `atomic run ntpd`

### Look at container images differently
* How do you get a packaged image onto multiple Atomic machines?
* How do I tell my customer how to run my container?
* RPM's have post-install script but Docker doesn't
* Where do I put configs or install scripts?
    * `LABELS INSTALL` specifies how to do post-install and pre-uninstall scripts
* Meta container images
    * Lots of daemons involved for OpenStack
        * One `atomic` command pulls down multiple containers and starts them
        * `atomic run openstack`
    * Working on the same automation for FreeIPA
* ***Demo time***
    * Built an Apache container with `LABEL INSTALL`
    * `atomic info spc` << gets all labels in container
    * `atomic install spc` << creates systemd unit file for container
    * `atomic uninstall <container>` to uninstall
* Currently working with CoreOS and docker for standardizing labeling

### Q&A
* Could I get docker logs from inside the container out to an rsyslog server?
    * Not by default
    * Patches incoming for docker for direct logging to journald
    * Docker versions are released into RHEL about every 6 weeks
* If I have a bunch of system mgmt tools, should I put them in one big container or many smaller ones?
    * Go for one big container (in the vein of RHELTools image)
    * SPC should not be your default means of running containers ;)
* Are these capabilities RHEL 7 only?
    * Yes
    * RHEL 6 containers are supported on RHEL 7's platform, though
    * No RHEL 6 atomic, won't ever happen
    * Everyone upgrade!