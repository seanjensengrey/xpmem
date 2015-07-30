XPMEM is a Linux kernel module that enables a process to map the memory of another process into its virtual address space.  Source code can be obtained by cloning the Mercurial repository or by downloading a tarball from the Downloads link above.

The XPMEM API has three main functions:

```
  xpmem_make()    
  xpmem_get()
  xpmem_attach()
```

A process calls xpmem\_make() to export a region of its virtual address space.  Other processes can then attach to the region by calling xpmem\_get() and xpmem\_attach().  After a memory region is attached, it is accessed via direct loads and stores.  This enables upper-level protocols such as MPI and SHMEM to perform single-copy address-space to address-space transfers, completely at user-level.

XPMEM regions are free to have "holes" in them, meaning virtual memory regions that are not allocated.  This makes XPMEM somewhat more flexible than mmap().  A process could, for example, export a region via XPMEM starting at address 0 and extending 4 GB.  Accesses to allocated (valid) virtual addresses in this region proceed normally, and pages are mapped between address spaces on demand.  A segfault will occur if the source process or any other process mapping the region tries to access an unallocated (invalid) virtual address in the region.