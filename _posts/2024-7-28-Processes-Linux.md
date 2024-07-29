**Process**
A process is simply an instance of one or more related tasks (threads) executing on your computer. It is not the same as a program or a command. A single command may actually start several processes simultaneously. Processes use many system resources, such as memory, CPU (central processing unit) cycles, and peripheral devices, such as network cards, hard drives, printers, and displays. The operating system (especially the kernel) is responsible for allocating a proper share of these resources to each process and ensuring overall optimized system utilization.

**Different Process Types, Descriptions and Examples**
**a) Interactive Processes**
Need to be started by a user, either at a command line or through a graphical interface such as an icon or a menu selection.
e.g.bash, firefox, top, Slack, Libreoffice

**b) Batch Processes**
Automatic processes which are scheduled from and then disconnected from the terminal. These tasks are queued and work on a FIFO (First-In, First-Out) basis.
e.g. updatedb, Idconfig

**c) Daemons**
Server processes that run continuously. Many are launched during system startup and then wait for a user or system request indicating that their service is required.
e.g. httpd, sshd, libvirtd, cupsd

**d) Threads**
Lightweight processes. These are tasks that run under the umbrella of a main process, sharing memory and other resources, but are scheduled and run by the system on an individual basis. An individual thread can end without terminating the whole process and a process can create new threads at any time. Many non-trivial programs are multi-threaded.
e.g. dconf-service, gnome-terminal-server

**e) Kernel Threads**
Kernel tasks that users neither start nor terminate and have little control over. These may perform actions like moving a thread from one CPU to another, or making sure input/output operations to disk are completed.
e.g. kthreadd, migration, ksoftirqd


**Process Scheduling**
A critical kernel function called the scheduler constantly shifts processes on and off the CPU, sharing time according to relative priority, how much time is needed and how much has already been granted to a task.

When a process is in a so-called running state, it means it is either currently executing instructions on a CPU, or is waiting to be granted a share of time (a time slice) so it can execute. All processes in this state reside on what is called a run queue and on a computer with multiple CPUs, or cores, there is a run queue on each.

However, sometimes processes go into what is called a sleep state, generally when they are waiting for something to happen before they can resume, perhaps for the user to type something. In this condition, a process is said to be sitting in a wait queue.

There are some other less frequent process states, especially when a process is terminating. Sometimes, a child process completes, but its parent process has not asked about its state. Amusingly, such a process is said to be in a zombie state; it is not really alive, but still shows up in the system's list of processes.

**Processes and Thread IDs**
At any given time, there are always multiple processes being executed. The operating system keeps track of them by assigning each a unique process ID (PID) number. The PID is used to track process state, CPU usage, memory use, precisely where resources are located in memory, and other characteristics.

**PID Types and Descriptions**
- **Process ID**
Uniques Process ID number.

- **Parent Process ID**
Process (Parent) that started this process. If the parent dies, the PPID will refer to an adoptive parent; on modern kernels, this is kthreadd which has PPID=2.

- **Thread ID (TID)**
Thread ID number. This is the same as the PID for single-threaded processes. For a multi-threaded process, each thread shares the same PID, but has a unique TID.


**Terminating a Process**
To terminate a process, you can type kill -SIGKILL `<pid>` or kill -9 `<pid>`.


