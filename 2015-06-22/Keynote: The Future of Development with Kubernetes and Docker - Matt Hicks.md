## Keynote: The future of development with Kubernetes and Docker
_Matt Hicks_

### Intro
* OpenShift used to work with just control groups, namespaces, SELinux
    * Docker/LXC wasn't available at the time
* Originally worked in development, later ops, carried a pager for his own code when it broke
* Looks at new technology through the lens of development/operations
* Kubernetes/Docker are most exciting tech seen in years
* Haven't seen a fundamental change in programming languages in quite some time, plenty of promises made with each one
* Frameworks came along with promises (Rails, Django, Node, etc)
    * Didn't make fundamental change either
    * Why? _Didn't change how operations worked._
* Then came process change (Kanban, agile, CI/CD, etc)
    * Mostly limited in scope
    * They don't impact the wide scope of developed applications

### The World of IT
* Developers and IT Ops
    * Different groups, different reporting structures
    * Becomes warring factions that don't work together
* Developers know code, best practices, algorithms
    * Live in the world of software
* Operations knows software meeting infrastructure
    * iptables rules to block DDoS
* Motivated by different things
    * Devs motivated by innovation, producing next change, disruption
    * Ops motivated by stability, reliability, reduce impact of changes
    * Ops usually pays the bills and control costs
    * Ops owns security/compliance _(the worst)_

### Developers
* Your work represents a little bit about yourself (talents, capabilities)
    * Not just about finishing something
    * Delivering a powerful change, highly disruptive
* Forklift upgrades: you get the whole thing or nothing
    * Fix: break it up
    * Micro-services or Service Oriented Architecture (SOA)
    * Didn't come out as clean as people predicted
    * Comes close to resembling the application, but not entirely the same
* Can't expect engineers to know everything about development and operations
* Think about your code, your dependencies, and dependencies of those dependencies
    * Can you trace a line from your rails code back to glibc?
    * Very difficult to have an expert level knowledge of distant dependencies
* Devs know code, ops knows infrastructure
    * Where they meet is a critical point
    * It's an area where both groups aren't experts
* *Good anecdote of lack of operations at a startup -- refer to session video*

### When Code Meets Operations
* Dev builds a fast horse but looks like a camel when it deploys to production
    * *Mismatch of expectations creates gaps*
* **Can't compete** unless ideas make it into production as they were intended

### Enter containers
* We need innovation in the grey area between operations and development
    * Containers help with this
* Segment the running workloads from the underlying environment
    * Sounds like VM's, but this layering is different
    * Dependencies are packaged with the container
    * Keep OS to a minimum inside the container
    * Operations teams don't want tons of packages added to their operating system
    * Let ops team maintain the base OS that goes into the container
    * Let developers choose when to update a deployment
* Containers reduce friction and give developers plenty of choice
    * Developers can use newer tools to build their application
    * Won't impact operations teams as they're trying to do their jobs

### Orchestration
* Every application scales differently (apache vs. MySQL)
* Kubernetes introduces concepts to allow you to describe patterns of how your application should work
    * Pods: colocated containers
    * Application controllers: describe how to scale
    * Allows developers to describe how they want their code operated
* Operations teams can choose from multiple tools to solve challenges from developers
    * Example: overlaid networks using OVS, vlans, etc
* Can't keep developing applications the same way when going to Docker or Kubernetes
    *  Requires learning and a shift in thinking
    *  Requires an investment to learn
    *  **These technologies will separate companies by their operational efficiency**
* Timing is perfect
    * Never had access to unlimited infrastructure before
    * More tools available for developers every day
    * Data goes everywhere -- mobile, desktop, etc
* Innovative companies will cross the gap
* Build something great ;)
