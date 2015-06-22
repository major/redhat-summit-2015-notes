## Red Hat Enterprise Linux C++ Toolchains: 10 Tips to Drive Your Development
_Matt Newsome_

Link to slides: http://bit.ly/dtstalks

### Tip 1: DTS
* Versions of GCC in RHEL are supported for 10 years
* Stability is key
* Ideal scenario is a single source of code built for RHEL 6 and 7
    * Developer Toolset (DTS) does this
    * Separate toolset from the stuff in RHEL
    * Build with DTS and it works on current and current+1 versions of RHEL
    * x86_64 only
* Built on software collections
    * Everything in /opt, activated by script
    * Multiple versions installed in parallel
    * Independent lifecycle from RHEL
* GCC 4.9
    * C++14 support, new standards
    * AVX-512 support
* Enable it with `scl` tool after installation

### Tip 2: Sanitizer
* AddressSanitizer alerts you to overflows of the stack buffer
* ThreadSanitizer finds race conditions if you don't use a mutex for handling global variables
* UndefinedBehaviorSanitizer gives more feedback on poorly done code

### Tip 3: New language standards for C++11/14
* Lambdas (C++11)
* C++14 is experimental, coming in DTS 4.x

### Tips 4: Security ASLR/PIE
* Counters ROP attacks/exploits
* Randomizes position of binaries
* Review performance impact versus security benefit
* Can test randomization with PIE

### Tip 5: RELRO
* Counters dynamically linked function addresses
* Global offset table hods the addresses, someone could mess with it
* Reorders writable data to follow internal data structures
    * Helps reduce damage of buffer overflows
* Check performance costs here

### Tip 6: MEMSTOMP
* memcpy() calls with overlapping arguments
* Often causes segfaults
* memstomp will give feedback on code where the bad memcpy() exists
* You will have to fix memcpy() calls, or use memmove() (performance hit)

### Tip 7: Containers (Docker)
* DTS 3.1 has Dockerfiles available for make it easier
* Docker image integration w/Eclipse
* Do your builds easily inside a Docker container
* Very portable means of compiling/developing

### Tip 8: Overflow builtins
* GCC 5 feature, coming to DTS this fall
* Compiler can check for overflows and alert the user
* Variants are available for different variable types
* Use Fedora in Docker to use GCC 5 right now in RHEL 7
* Experimental

### Tip 9: Performance & Debug Tools
* gdb features growing
* SystemTap does live application analysis (only in use space, not kernel space)
* PAPI - interface to performance counter hardware
* Oprofile finds where time is spent in your application
* Valgrind can look for memory leaks

### Tip 10: Contribute back
* Need feedback from users
* Bug reports
* Community discussions
* Patches/features
* Release more software under open licenses
