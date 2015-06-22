## Kubernetes: Beyond The Basics
_Paul Bakker_

**Goal:** Automated, production ready Kubernetes cluster in 9 steps

### Step 0: Understanding Kubernetes
* Nodes
    * Master node has a REST API
    * Tell the master to do things for you
    * Physical machines
* Pods
    * Groups of containers
* Controllers
    * Defines how many replicas of a container should be running
    * Lives on master node
    * Schedules containers to pods
* Deployment 101
    * Have a Docker container
    * Define replication controller (JSON file)
    * `kubectl create -f <json file>`
    * Replication controller makes pods
    * Pods and replication controllers have labels
        * Makes it easier to refer to them
    * Scaling with `kubectl scale` -- specify replication count
    * Updating an application
        * Make a new replication controller for new container
        * `kubectl create`
        * Scale down the old replication controller

### Step 1: Simple automation
* `kubectl` requires a bunch of typing
    * Use the REST API to build replication controllers and destroy old clusters
* What about downtime or scheduled maintenance?
    * Not there yet in Kubernetes

### Step 2: Load balancing
* Pods come and go with dynamic IP addresses
* Service is a proxy to pods with a fixed IP
* Not quite right
    * SSL offloading?
    * Better load balancing?
    * Redirects/rewrites?
    * Fixed IP on an internal network that can't be reached?
* *Services are designed for inter-pod communication*
* **Don't use them as HTTP load balancers**
* Now add a custom load balancer
    * Vulcan proxy (w/etcd)
    * Proxy needs to know about where the pods are
    * Vulcan uses etcd to get its configuration
    * Can use haproxy/nginx with templating
* Doesn't work -- pods aren't accessible from load balancer

### Step 3: Weave (virtual networking)
* Each pod gets its own IP so you can access pods from outside Kubernetes environment
* Weave hands out IP addresses to pods

### Step 4: Load balancer registration
* How can the LB keep track of pods? They might scale up or scale down.
    * Something needs to watch the Kubernetes API
    * That thing can make adjustments in etcd
    * Vulcan sees changes in etcd and adjusts its configuration
* Amdatu vulcanized
    * Written in Go
    * Connects Kubnernetes/Vulcan
    * Available in a Docker container
    * It watches Kubnernetes API and adjusts etcd upon changes

### Step 5: Blue/Green Deployment
* Existing version already running, start running the new code in new containers
* Then flip LB from old to new in one movement
* Destroy old environment
* Watch out for database schema/migrations and container capacity
* How do we know if the new containers are healthy?
    * Make health checks available at `GET /health` and check the response
    * Build server can't reach the pods -- how do we do health checks?
    * Need a deployer server that does health checks, sets up replication controllers, and adjusts etcd
* *Gets a little crazy around here -- notes are incomplete -- check the session video*
