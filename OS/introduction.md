## Flashback

Unix was written by Dennis Ritchie and Ken Thompson. It happened in 1969 because an earlier multiuser OS project - Multix was a
failure at Bell Labs. Unix was simple and shipped with code. This allowed multiple implementations of the OS.
BSD was one of them.

### Design
1. Unix was a simple OS. While other operating systems shipped with thousands of system calls, Unix had hundreds. And its
design goals were very clear. This led to widespread acceptance.

2. In Unix, everything is a file. Therefore, both data and devices can be manipulated/interfaced with using a set of simple system
calls like open(), read(), write(), close() etc.

3. It was written in C. This led to a good degree of portability among different architectures.

4. Unix offered fast process creation times and the unique fork() system call. It also had robust IPCs. This led to easy creation
of processes that *did one thing and did it well*. These processes could be strung together later to accomplish more complex tasks.

Unix owes its success to the design decisions made my Ritchie and Thompson. It evolves beautifully to accommodate needs of the
present day.

## The birth of Linux
Linus Torvalds wasn't happy that Unix was not a free operating system. He did not like DOS and the Minix that he used had some
license issues related to the distribution of modified code. So he wrote his own operating system in 1991 for the 80386 intel
processor that was popular at the time.

Link to Linus' announcement of Linux: http://www.thelinuxdaily.com/2010/04/the-first-linux-announcement-from-linus-torvalds/

Today Linux runs on desktops, servers, mobile phones, embedded systems, even supercomputers.

A Linux operating system comprises the kernel, C library, compiler toolchain, some basic system utilities like the shell
and the GUI (if applicable).

### The kernel

The kernel is the piece of software that provides services for all other systems, manages hardware and distributes system resources.

Typical subsystems:
1. Interrupt handlers
2. Schedulers
3. Memory management systems

The kernel has full access to hardware resources and has a protected memory space. This system state and memory are called "Kernel space".
All other applications run in userspace. The userspace only sees a subset of resources available and operate within certain limits.
When the kernel code is executed, the system is said to be in kernel mode. User mode in all other cases. Userspace interacts
with the kernel space using system calls. An example is the C library's printf() function which uses the write() system call offered
by the kernel under the hood to write data to buffers. The kernel is said to execute in process context when it executes these calls on
behalf of the callers.

![Image from Linux Kernel Devlopment by Robert Love](https://notes.shichao.io/lkd/figure_1.1.png)

#### Hardware interaction

The kernel is also responsible for interfacing with hardware. This is done by means of hardware interrupts. When the hardware
wants to communicate with the kernel, it sends a signal, known as an interrupt. This asynchronously interrupts the kernel. The kernel
then executes the handler associated with the number that the interrupt is identified by. The kernel operates in interrupt context
when this happens. This is not associated with any userspace process.

So at any moment, the processor is in one of these states -

1. Kernel space - process context
2. Kernel space - interrupt context
3. User space

Even when it is idle, it executes an idle process in the process context.

Kernel Design -  Monolithic vs Microkernel
> Monolithic - implemented entirely as single large process running in a single address space. All services exist
> and operate in a single kernel address space. Communication between components is quicker and the design is simple. This
> means better performance. Most Unix varieties are monolithic.
> Microkernels - functionality is broken down into separate processes called servers. Only the servers absolutely required are
> run in privileged mode. Servers run in different address spaces.
> Communication is via IPCs. The failure of one server does not mean failure of the entire system.
> Modularity allows swapping of subsystems. IPCs also contribute to latencies. And, switching between kernel and user space
> frequently is expensive. So most servers are made to run in kernel space, which defeats the purpose of microkernels. Windows
> NT and mach (OS X is based on this) are examples of microkernels.
> Linux is a monolithic kernel but borrows good things from microkernels. It has modular design with kernel pre-emption, support
> for kernel threads and the ability to dynamically load modules into the kernel. Everything runs in kernel mode with direct
> function invocation and not message passing. So it is performant.

### Major characteristics of Linux that differentiate it from other Unix variants
1. It supports dynamic loading of kernel modules.
2. The kernel is preemptive. It has the capability to kill any process even if it is running in kernel mode. Some Unix variants
    may have this but traditional Unix did not.
3. Does not differentiate between threads and processes. All of them are considered "tasks" sharing system resources.
4. FREEEEEEEE YAYAY

### Linux kernel versions

Two variants:
Stable and Development.

Stable - production level releases suitable for deployment.
Development - experimental, quickly changing.

Warning - the following info could be outdated:
Linux version naming scheme:
<major version>.<minor version>.<revision>
Add odd number of minor version denotes a development version whereas an even number denotes a stable version.
Eg, 2.5.3 is a development version whereas 2.6.7 is a stable version
