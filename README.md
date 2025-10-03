CIS 452 Lab 7: System Resource Utilization
------------------------------------------------------------------------

### Overview

Resource management is a primary responsibility of the operating system.
Mismanagement of resources (including CPU cycles) can cause inefficiencies and malfunctioning drivers;
and rogue processes can be responsible for a denial
of service to other users. 
The purpose of this assignment is to become familiar with several methods used
to determine the limits placed on system resources.
Both static and dynamic mechanisms will be introduced.

### Activities

* Submit limit values for the resources listed in the table *and* include
  either:
    * a code snippet used to determine the value, or
    * the name of the file in which you found the value

### System Resource Limitations

When working with user or system resources such as processes, files,
pipes, shared memory and semaphores,
it is prudent to be aware of any system limits placed upon ownership of these
resources.
This lab introduces several different methods for discovering the values
of system limits for specific machine/operating system combinations.
Note: these methods are translatable to any operating system.

**Static**

In this context, static means "not active"
(i.e. the limit is determined via a simple lookup).
As usual, the first place to start is the man pages and associated include
files (*always* a good idea for a developer).
General information about system resources can be found in `limits.h` and
`param.h`, but it may not be machine specific.
Be sure to examine
`posix1_lim.h` and `posix2_lim.h` in the event your specific system
configuration supports the Posix standard for that resource.

Note 1: These files may be located in different subdirectories. Use `find` to find them.

Note 2: Limits obtained statically may not reflect actual runtime
values;
the source code defaults may have been overridden during compilation or changed
by a sysadmin in the course of a local installation. Please note the location 
(and existence) of these files may differ from system to system.

**Dynamic**

Many user limits can be obtained dynamically via the `getrlimit()` system call
and the `sysconf()` function.
System limits can be determined via the `sysinfo()` system call.
All of them work by querying the kernel for the actual maximum value of a
resource.
They pass a flag to the system that specifies the resource of interest;
the appropriate value is then either returned directly or written into a
structure for subsequent access.
Read the man pages for details on how to use these functions at run-time
(i.e. from within an executing program).

**Empirical**

Finally, some limits are not defined in any include file or available using
query.
In this case, it is still possible to determine a maximum value experimentally.
Recall that all system calls return error codes indicating their
success/failure (and that you should be checking those values!).
Obviously, a carefully-designed program could "push the limit" and use the
return value to determine the point at which a system call fails,
and, hence, the limit of the resource it is requesting.
For example, the `shmget()` function takes a parameter specifying the size of
the shared memory segment desired.
It returns a segment ID if successful, or -1 on failure.
The size argument can be manipulated within your test program to determine the
maximum-sized shared memory segment allowed on your system.

### Problem Specification

* Use the specified method to determine the values for the following system
  resources.
    * Indicate what machine you are using to perform your tests and provide details
      regarding your method (name of include file, system call used, etc.)
    * For file size, report the maximum as allowed by _both_ the file system and the operating system. (The values should be different.)
    * When using `getrlimit`, watch for a value of `RLIM_INFINITY`.  Don't just blindly report a huge number. 


  | **System Object** |     | **Method** | &nbsp; &nbsp; &nbsp; &nbsp; | **Value** | &nbsp; &nbsp; &nbsp; &nbsp; | **Details** |
  | ----------------- | --- | ---------- | --------------------------- | --------- | --------------------------- | ----------- |
  | Maximum # of semaphores per process | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| static | | | |
  | Maximum value of a (counting) semaphore | | static | | | | |
  | Maximum value of a (counting) semaphore | | empirical | | | | |
  | Max. size of a shared memory segment | | empirical | | | | |
  | Page size (bytes) | | dynamic | | | | |
  | Physical pages in system | | dynamic | | | | |
  | Maximum # of processes per user | | dynamic | | | | |
  | Maximum # of open files: hard limit | | dynamic | | | | |
  | Maximum # of open files: soft limit | | dynamic | | | | |
  | Clock resolution | | dynamic | | | | |
  | Maximum filesize (in GB) according to OS | | dynamic | | | | |
   | Maximum filesize (in GB) according to file system | | dynamic | | | | |



Submit 
 * a writeup that includes a table similar to the above.
 * your source code.
