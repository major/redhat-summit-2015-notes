## Performance Analysis and Tuning of Red Hat Enterprise Linux
_Jeremy Eder, D. John Shakshober, Larry Woodman, and Bill Gray_

***These slides are packed full of notes.  Get the slides.  Seriously.***

* Benchmarks are about latency and throughput/bandwidth
    * Highway
    * Latency is the speed limit on the highway
    * Bandwidth is the width of the highway
* Tuned
    * Makes a bunch of performance adjustments based on profiles
    * RHEL 7 has the highest performance settings by default (RHEL6 used balanced)
    * Thoughput performance profile gives better performance tests with all filesystems

### NUMA
* Benchmarks done with SPEC CPU 2006
* Moving memory closer to CPU's increases performance
* NUMA nodes communicate via interconnects -- that causes delays due to contention and distance traveled
* Most servers have one NUMA node per socket (some AMD's have 2)
* Tools
    * `lspcu` -- lists numa nodes
    * `numactl --hardware`
        * Shows all NUMA nodes
        * Also shows SLIT data (cost to get memory from another NUMA node for this particular server)
    * `lstopo` -- shows NUMA nodes, caches, hyperthreading, etc
* Tips for performance
    * Never disable NUMA (keep interleave memory off)
        * Makes your OS see only one NUMA node -- ***terrible***
    * Know your usage patterns
* Lots of per-NUMA node resources exist (CPU's, caches, memory, etc)
* Multiple kswapd's per NUMA node
* Three buckets of memory
    * First 16MB -- DMA (old, for ISA)
    * Next 4GB -- DMA32
    * Remainder -- Normal Zone
    * DMA + DMA32 are stuck on NUMA node 0
    * Memory goes active -> inactive -> free
        * Replicated per NUMA-node
        * *Active swapping/paging could happen on one node with another node having plenty of memory*
        * System normally tries to balance these resources but workloads can cause problems
* swappiness -- how aggressively system reclaims anonymous memory
    * Shouldn't need to tune swappiness very often in RHEL 7 (but sometimes needed in RHEL 5/6)
* Memory reclaim watermarks
    * Three levels: high, low and min
    * Free memory over high -- no action taken
    * Free memory falls below low watermark? swapd shows up to reclaim until pages make it back up to high
    * `min_free_kbytes` determines the number of KB must be available (go below that and only the kernel will run)
        * Allocated evenly among all NUMA nodes
    * `zone_reclaim_mode`
        * Tells kernel what to do when a NUMA node can't allocate memory
        * Either reclaim on same node or go get memory from another node (slow)
        * Poor setting choice can lead to ~ 40% slower performance
        * How do you figure out what to set it to?
            * Is NUMA data locality more important than having lots of cache data?
                * Large monolithic databases -- leave zone_reclaim_mode off (you need that cache and you can't tune much)
                * Multiple processes (including hypervisors) -- enable zone_reclaim_mode
* *Good NUMA checklist/tool slide from the slides*
* `numactl` can help you run a process with static numa memory/execution alignment
* Virtual guests are great places to start NUMA tuning
* RHEL 6.4 introduced `numad`
    * Tries to move the process to the node that has the resources available
    * libvirt queries `numad` to figure out where to place VM's
* RHEL 7 added NUMA balancing
    * Can move execution threads to memory
    * Can move memory to execution threads
    * 25% gain with auto NUMA balancing over time (w/SAP HANA)
    * 2x gain w/NUMA alignment on JBB 2005 (and smoother performance curve)

### Linux scheduler (and tuned)
* Scheduler has rbtrees for each socket/core that it maintains
* Bigger quantum: higher latency but higher throughput  (2-3x larger)
    * `sched_min_granularity_ns` and `sched_wakeup_granularity_ns` << quantum values
* `sched_migration_cost`
    * scheduler tries to keep all CPU's busy by shifting around processes to idle CPUs (can have a bad impact on caching at the CPU)
    * Increase 2-10x to decrease load balancing on systems running virtual machines
* fork() behavior
    * RHEL 5 -- child runs first
    * RHEL 6 and 7 -- parent runs first

### Page sizes
* Intel supports 4KB, 2MB, 1GB page sizes (hugepages)
    * 4KB is default but tuned pushes it to use larger pages by default
    * TLB can contain fewer entries with larger page sizes
    * 2MB standard size for RHEL
    * Can adjust them per NUMA node
    * 1GB hugepages were reserved at boot time previously
    * RHEL 7 allows 1GB hugepages allocation/deallocation live w/o reboot
    * Transparent hugepages -- for stack/heap/data/BSS pages (2MB)
        * Will chop pages into smaller pages if memory is constrained
        * On by default for anonymous memory
        * Some processes will be allocated small pages if they touch small amounts of memory
* Why not go to 1GB hugepages everywhere?
    * TLB is non-standard between processors
    * Fewer 1GB hugepages in physical hardware
* Use sysfs if you want to set hugepage allocation per NUMA node
* Using /proc will allocate across all NUMA nodes
    * Most startup scripts will do this for you automatically
    * *Look here first if you have lots of NUMA collisions*
* RHEL 7 allows live allocation/deallocation of 1GB hugepages via sysfs
* Enabling transparent_hugepages can almost double performance bump when allocating lots of RAM on a NUMA node
* *Bill Gray wrote `numad` and likes feedback*

### cgroups
* Filesystem in the kernel which allows you to make subsets of your system (NUMA nodes, CPU's, memory, network, disk I/O)
* `numastat` shows NUMA hits/misses
* `cpu.shares` allows throttling shares of the CPU
    * `cat cpu.shares` to find out the total share count
* cgroup oomkills
    * If you restrict a cgroup to a certain memory size and you overcommit, the system will swap/page
    * Kernel will kill off processes just as if it was running on a system with less memory

### Atomic host
* Tuned profile inheritance from host -> guest -> container
* Containers have faster round trip TCP/UDP perf than KVM

### Network performance (NFV) realtime
* Realtime kernel available for RHEL 7 w/tuned
* Power controls in BIOS increase latency, let it be adjustable in the OS level
* NIC offloads favor throughput
* NFV tuned raised throughput from ~ 300Gbps to 421Gbps (12x40Gb NICs)
* 218M pps on one system (12x40Gb NICs)
* Jitter under 20ms on realtime tuned profile (will be improved further)
* 5 microseconds round trip latency with OpenOnload and containers
* RHEL 7 `nohz_full`
    * Stop interrupting userspace tasks
    * Move timekeeping to non-latency-sensitive cores
    * Potentially no ticks in a guest nor in the host
* RHEL 7 `BUSY_POLL`
    * Must be driver enabled (Intel, Mellanox, SolarFlare, etc)
    * Remove interripts off network interface path
    * Much faster round trip times
* `turbostat` -- idle states and freq on Intel CPUs

### Disk I/O
* tuned profiles apply to disk I/O
* pdflush -- per filesystem flush daemon
* Go over dirty_background_ratio? async flush
* Go over dirty_ratio? sync flush
