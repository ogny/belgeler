There are several things that might cause an OOM event other than the system
running out of RAM and available swap space due to the workload. 
* The kernel might not be able to utilize swap space optimally due to the type
  of workload on the system. 
* Applications that utilize mlock() or HugePages have memory that can't be
  swapped to disk when the system starts to run low on physical memory.
* Kernel data structures can also take up too much space exhausting memory on
  the system and causing an OOM situation. 
* Many NUMA architecture–based systems can experience OOM conditions because of
  one node running out of memory triggering an OOM in the kernel while plenty
of memory is left in the remaining nodes.
