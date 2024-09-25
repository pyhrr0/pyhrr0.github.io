---
title: "Linux Programming Interface"
date: 2024-09-25T13:33:37Z
---
## System Calls
  ### Process Control:
  - fork() - Creates a new child process by duplicating the calling process.
  - execve() - Replaces the current process image with a new program.
  - exit() - Terminates the calling process.
  - wait(), waitpid() - Waits for state changes in a child process.
  - getpid(), getppid() - Returns the process ID of the calling or parent process.
  - getuid(), geteuid() - Gets the real and effective user ID of the calling process.
  - getgid(), getegid() - Gets the real and effective group ID.
  - setuid(), seteuid() - Sets the user ID of the calling process.
  - setgid(), setegid() - Sets the group ID.
  - kill() - Sends a signal to a process or group of processes.
  - nice() - Changes the scheduling priority of the process.
  - ptrace() - Provides processes with the ability to observe and control other processes.

  ### File Operations
  - open(), openat() - Opens a file descriptor for a file.
  - close() - Closes an open file descriptor.
  - read() - Reads data from a file descriptor.
  - write() - Writes data to a file descriptor.
  - lseek() - Repositions the read/write file offset.
  - pread(), pwrite() - Reads or writes data at a given offset without changing the file offset.
  - stat(), fstat(), lstat() - Retrieves information about a file.
  - chmod(), fchmod() - Changes the permissions of a file.
  - chown(), fchown() - Changes the ownership of a file.
  - unlink(), unlinkat() - Deletes a name from the filesystem.
  - rename(), renameat() - Renames a file or directory.
  - mkdir(), mkdirat() - Creates a new directory.
  - rmdir() - Removes a directory.
  - link(), symlink(), readlink() - Creates or reads symbolic and hard links.
  - fsync(), fdatasync() - Synchronizes a file's in-memory state with storage device.
  - dup(), dup2(), dup3() - Duplicates a file descriptor.
  - fcntl() - Performs operations on file descriptors.
  - ioctl() - Device-specific input/output control operations.

  ### Directory Operations
  - opendir(), fdopendir() - Opens a directory stream.
  - readdir() - Reads a directory entry.
  - closedir() - Closes a directory stream.
  - rewinddir() - Resets the position of the directory stream.
  - scandir() - Scans a directory for matching entries.
  - chdir() - Changes the current working directory.
  - getcwd() - Retrieves the current working directory path.

  ### Memory Management
  - brk(), sbrk() - Changes the end of the data segment (allocates/deallocates memory).
  - mmap(), munmap() - Maps or unmaps files or devices into memory.
  - mprotect() - Changes protection on a region of memory.
  - mlock(), munlock() - Locks or unlocks memory in RAM.
  - shmat(), shmdt() - Attaches or detaches shared memory segments.

  ### Inter-Process Communication (IPC)
  #### Pipes and FIFOs
  - pipe(), pipe2() - Creates a unidirectional data channel (pipe).
  - mkfifo(), mkfifoat() - Creates a named pipe (FIFO).
  #### Message Queues
  - msgget() - Creates or accesses a message queue.
  - msgsnd() - Sends a message to a queue.
  - msgrcv() - Receives a message from a queue.
  - msgctl() - Performs control operations on a message queue.
  #### Semaphores
  - semget() - Creates or accesses a semaphore set.
  - semop() - Performs operations on semaphores.
  - semctl() - Performs control operations on a semaphore set.
  ##### Shared Memory
  - shmget() - Allocates shared memory.
  - shmat(), shmdt() - Attaches/detaches shared memory.
  - shmctl() - Controls shared memory operations.
  #### Sockets
  - socket() - Creates an endpoint for communication.
  - bind() - Binds a name to a socket.
  - listen() - Listens for socket connections.
  - accept(), accept4() - Accepts a socket connection.
  - connect() - Initiates a connection on a socket.
  - send(), recv() - Sends/receives messages on a socket.
  - sendto(), recvfrom() - Sends/receives messages to/from a socket.
  - socketpair() - Creates a pair of connected sockets.

  ### Signals
  - signal(), sigaction() - Installs a signal handler.
  - raise() - Sends a signal to the calling process.
  - kill() - Sends a signal to a process.
  - sigprocmask() - Examines or changes blocked signals.
  - sigpending() - Checks for pending signals.
  - sigsuspend() - Waits for a signal.
  - sigwait() - Waits for signals in a set.

  ### Timers and Clocks
  - time() - Returns the current time.
  - gettimeofday(), settimeofday() - Gets/sets the system time.
  - clock_gettime(), clock_settime() - Accesses high-resolution clocks.
  - nanosleep() - Suspends execution for a specified time.
  - alarm() - Schedules a SIGALRM signal.
  - setitimer(), getitimer() - Sets or gets the value of an interval timer.
  - timer_create(), timer_delete(), timer_settime(), timer_gettime() - Manages POSIX timers.

  ### Resource Management
  - getrlimit(), setrlimit() - Gets or sets resource limits.
  - getrusage() - Returns resource usage measures.

  ### User and Group Management
  - getpwnam(), getpwuid() - Retrieves password file entries.
  - getgrnam(), getgrgid() - Retrieves group database entries.

  ### Process Scheduling
  - sched_getscheduler(), sched_setscheduler() - Gets or sets the scheduling policy.
  - sched_getparam(), sched_setparam() - Gets or sets scheduling parameters.
  - sched_yield() - Yields the processor.
  - sched_get_priority_max(), sched_get_priority_min() - Gets priority limits.

  ###  Miscellaneous
  - uname() - Gets system information.
  - sysinfo() - Retrieves system statistics.
  - ptrace() - Traces processes (used for debugging).
  - sysctl() - Reads or writes system parameters.
  - umask() - Sets file mode creation mask.

## Standard Library Functions (glibc)
  ### String Manipulation
  - strlen(), strnlen() - Calculates the length of a string.
  - strcpy(), strncpy() - Copies strings.
  - strcat(), strncat() - Concatenates strings.
  - strcmp(), strncmp() - Compares strings.
  - strchr(), strrchr() - Locates characters in a string.
  - strstr(), strcasestr() - Finds a substring.
  - sprintf(), snprintf() - Writes formatted output to a string.
  - sscanf() - Reads formatted input from a string.

  ### Memory Functions
  - malloc(), calloc(), realloc(), free() - Allocates and manages dynamic memory.
  - memcpy(), memmove() - Copies memory areas.
  - memset() - Fills memory with a constant byte.
  - memcmp() - Compares memory areas.

  ###  Input/Output
  - printf(), fprintf() - Prints formatted output.
  - scanf(), fscanf() - Reads formatted input.
  - getc(), fgetc(), getchar() - Reads a character from a file or stdin.
  - putc(), fputc(), putchar() - Writes a character to a file or stdout.
  - fgets() - Reads a string from a file.
  - fputs(), puts() - Writes a string to a file or stdout.
  - fopen(), freopen(), fdopen() - Opens a file stream.
  - fclose() - Closes a file stream.
  - fread(), fwrite() - Reads or writes data to/from a file.
  - fseek(), ftell() - Repositions or obtains the position of the file pointer.
  - fflush() - Flushes a file stream.

  ### Mathematical Functions
  - abs(), labs(), llabs() - Computes absolute value.
  - ceil(), floor() - Rounds a number up or down.
  - sqrt(), cbrt() - Calculates square or cube roots.
  - pow(), exp(), log(), log10() - Exponential and logarithmic functions.
  - sin(), cos(), tan() - Trigonometric functions.
  - asin(), acos(), atan(), atan2() - Inverse trigonometric functions.

  ### System Utilities
  - system() - Executes a shell command.
  - getenv(), putenv(), setenv(), unsetenv() - Gets or sets environment variables.
  - getopt(), getopt_long() - Parses command-line options.
  - exit(), _exit(), atexit() - Exits a program and registers functions to be called at exit.
  - qsort(), bsearch() - Sorts and searches arrays.

  ### Date and Time
  - time() - Gets the current time.
  - ctime(), asctime() - Converts time to a string representation.
  - localtime(), gmtime() - Converts time to broken-down time.
  - mktime() - Converts broken-down time to time since the Epoch.
  - strftime(), strptime() - Formats and parses date and time.

  ### POSIX Threads (pthread)
  #### Thread Management
  - pthread_create() - Creates a new thread.
  - pthread_exit() - Terminates the calling thread.
  - pthread_join() - Waits for thread termination.
  - pthread_detach() - Marks a thread for detachment.
  - pthread_self() - Returns the calling thread's ID.
  - pthread_equal() - Compares thread IDs.
  - pthread_cancel() - Sends a cancellation request to a thread.
  - pthread_setcancelstate(), pthread_setcanceltype() - Sets thread cancellation state and type.
  #### Thread Attributes
  - pthread_attr_init(), pthread_attr_destroy() - Initializes/destroys thread attributes object.
  - pthread_attr_setdetachstate(), pthread_attr_getdetachstate() - Sets/gets detach state.
  - pthread_attr_setschedpolicy(), pthread_attr_getschedpolicy() - Sets/gets scheduling policy.
  - pthread_attr_setschedparam(), pthread_attr_getschedparam() - Sets/gets scheduling parameters.
  #### Mutexes
  - pthread_mutex_init(), pthread_mutex_destroy() - Initializes/destroys a mutex.
  - pthread_mutex_lock(), pthread_mutex_trylock(), pthread_mutex_unlock() - Locks/unlocks a mutex.
  - pthread_mutexattr_init(), pthread_mutexattr_destroy() - Initializes/destroys mutex attribute object.
  - pthread_mutexattr_settype(), pthread_mutexattr_gettype() - Sets/gets the mutex type.
  #### Condition Variables
  - pthread_cond_init(), pthread_cond_destroy() - Initializes/destroys a condition variable.
  - pthread_cond_wait(), pthread_cond_timedwait() - Waits on a condition variable.
  - pthread_cond_signal(), pthread_cond_broadcast() - Signals a condition variable.
  #### Read-Write Locks
  - pthread_rwlock_init(), pthread_rwlock_destroy() - Initializes/destroys a read-write lock.
  - pthread_rwlock_rdlock(), pthread_rwlock_wrlock(), pthread_rwlock_unlock() - Locks/unlocks a read-write lock.
  #### Thread-Specific Data (TSD)
  - pthread_key_create(), pthread_key_delete() - Creates/deletes a thread-specific data key.
  - pthread_setspecific(), pthread_getspecific() - Sets/gets thread-specific data.
  #### Barriers
  - pthread_barrier_init(), pthread_barrier_destroy() - Initializes/destroys a barrier.
  - pthread_barrier_wait() - Synchronizes threads at a barrier.

  ### Networking (Sockets API)
  #### Socket Creation and Configuration
  - socket() - Creates a socket.
  - socketpair() - Creates a pair of connected sockets.
  - bind() - Binds a socket to an address.
  - listen() - Listens for connections on a socket.
  - accept(), accept4() - Accepts a connection on a socket.
  - connect() - Initiates a connection on a socket.
  - getsockname(), getpeername() - Gets the local or remote address of a socket.
  - getsockopt(), setsockopt() - Gets/sets options on sockets.
  #### Data Transmission
  - send(), recv() - Sends/receives data on a socket.
  - sendto(), recvfrom() - Sends/receives messages with addresses.
  - sendmsg(), recvmsg() - Sends/receives messages with ancillary data.
  - shutdown() - Disables sends or receives on a socket.
  #### Address and Name Resolution
  - gethostbyname(), gethostbyaddr() - Gets host entry by name/address.
  - getaddrinfo(), freeaddrinfo() - Translates hostnames and service names to socket addresses.
  - inet_aton(), inet_addr(), inet_ntoa(), inet_ntop(), inet_pton() - Converts IP addresses between binary and text.
  #### Network Byte Order Conversion
  - htons(), htonl() - Converts values from host to network byte order.
  - ntohs(), ntohl() - Converts values from network to host byte order.

  ### Inter-Process Communication (Advanced IPC)
  #### Event Mechanisms
  - select(), pselect() - Monitors multiple file descriptors.
  - poll(), ppoll() - Waits for events on file descriptors.
  - epoll_create(), epoll_ctl(), epoll_wait(), epoll_pwait() - Scalable I/O event notification mechanisms.
  #### Inotify (Filesystem Event Monitoring)
  - inotify_init(), inotify_init1() - Initializes inotify instance.
  - inotify_add_watch(), inotify_rm_watch() - Adds/removes watches to monitor filesystem events.
  #### Signalfd
  - signalfd() - Creates a file descriptor for accepting signals.
  #### Timerfd
  - timerfd_create(), timerfd_settime(), timerfd_gettime() - Creates and manages timers that notify via file descriptors.
  #### Eventfd
  - eventfd(), eventfd_read(), eventfd_write() - Provides event notification mechanisms via file descriptors.

  ### Filesystem Interface
  #### Extended Attributes
  - setxattr(), getxattr() - Sets/gets extended attributes of files.
  - listxattr(), removexattr() - Lists/removes extended attributes.
  #### Filesystem Operations
  - mount(), umount(), umount2() - Mounts/unmounts filesystems.
  - sync(), syncfs() - Synchronizes filesystem data on disk with memory.
  - statvfs(), fstatvfs() - Retrieves filesystem statistics.
  #### File Locking
  - flock() - Applies or removes an advisory lock on an open file.
  - fcntl() with locking commands (F_GETLK, F_SETLK, F_SETLKW) - Performs record locking operations.

  ### Device and Terminal Control
  #### Terminal I/O (termios)
  - tcgetattr(), tcsetattr() - Gets/sets terminal parameters.
  - cfgetispeed(), cfsetispeed() - Gets/sets input baud rate.
  - cfgetospeed(), cfsetospeed() - Gets/sets output baud rate.
  - tcsendbreak() - Transmits a break.
  - tcdrain() - Waits until all output is transmitted.
  - tcflush() - Discards data written or received.
  - tcflow() - Suspends/resumes data transmission or reception.
  - tcgetsid() - Gets the session ID.
  #### ioctl Commands
  - Various ioctl() commands - Device-specific operations (e.g., controlling hardware devices).

  #### Dynamic Linking and Loading
  - dlopen() - Opens a shared library.
  - dlsym() - Looks up the address of a symbol.
  - dlclose() - Closes a dynamic library.
  - dlerror() - Returns error messages from dynamic linking.

  ### Real-Time Extensions
  #### Message Queues
  - mq_open(), mq_close() - Opens/closes a message queue.
  - mq_unlink() - Removes a message queue.
  - mq_send(), mq_receive() - Sends/receives messages.
  - mq_notify() - Registers for message queue notification.
  - mq_getattr(), mq_setattr() - Gets/sets message queue attributes.
  #### Semaphores
  - sem_open(), sem_close() - Opens/closes a named semaphore.
  - sem_unlink() - Removes a named semaphore.
  - sem_post(), sem_wait() - Increments/decrements a semaphore.
  - sem_trywait(), sem_timedwait() - Non-blocking or timed semaphore operations.
  - sem_getvalue() - Gets the semaphore value.
  #### Real-Time Signals
  - sigqueue() - Sends a signal with accompanying data.
  
  ### Capability Interfaces
  - cap_get_proc(), cap_set_proc() - Gets/sets process capabilities.
  - cap_init(), cap_free() - Initializes/frees capability data structures.
  - cap_get_flag(), cap_set_flag() - Gets/sets capability flags.

  ### Advanced Memory Management
  - mlockall(), munlockall() - Locks/unlocks all pages mapped into the address space.
  - madvise(), posix_madvise() - Advises kernel about memory usage patterns.
  - mremap() - Remaps a virtual memory address.
  - remap_file_pages() - Creates a nonlinear file mapping.

  ### Error Handling
  #### Error Variables and Functions
  - errno - Global variable indicating the error code.
  - perror() - Prints a descriptive error message.
  - strerror(), strerror_r() - Returns a string describing an error code.
  - err(), warn() - Error handling functions for reporting.

  ### Atomic Operations
  #### Built-in Functions
  - __sync_fetch_and_add(), __sync_fetch_and_sub() - Atomic arithmetic operations.
  - __sync_lock_test_and_set(), __sync_lock_release() - Atomic test and set operations.
  #### Atomic Types and Functions
  - atomic_flag, atomic_bool, atomic_int, etc. - Atomic data types for thread-safe operations.
  - atomic_thread_fence(), atomic_signal_fence() - Memory synchronization operations.
  
  ### Cryptographic Functions
  - crypt() - Performs password encryption.
  - getentropy() - Obtains random data.
  - getrandom() - Retrieves random bytes from the kernel random number generator.

  ### Locale and Internationalization
  - setlocale() - Sets the program's locale.
  - localeconv() - Gets locale-specific formatting information.
  - nl_langinfo() - Retrieves information about language-specific settings.

  ### Threading Extensions
  #### Thread Cancellation
  - pthread_cancel() - Requests cancellation of a thread.
  - pthread_setcancelstate(), pthread_setcanceltype() - Sets cancellation state/type.
  #### Spin Locks
  - pthread_spin_init(), pthread_spin_destroy() - Initializes/destroys a spin lock.
  - pthread_spin_lock(), pthread_spin_trylock(), pthread_spin_unlock() - Controls a spin lock.
  #### Thread Barriers
  - pthread_barrier_init(), pthread_barrier_destroy() - Initializes/destroys a barrier.
  - pthread_barrier_wait() - Synchronizes threads at a barrier.

  ### Debugging and Profiling
  - gprof() - Profiles program execution.
  - mtrace(), muntrace() - Traces dynamic memory allocation.
  - backtrace(), backtrace_symbols() - Obtains a backtrace (stack trace).

  ### Miscellaneous Functions
  #### Environment and Configuration
  - setreuid(), setregid() - Sets real and effective user/group IDs.
  - getresuid(), getresgid() - Gets real, effective, and saved user/group IDs.
  #### Process Groups and Sessions
  - setsid(), getsid() - Creates/gets a session ID.
  - setpgid(), getpgid(), getpgrp() - Sets/gets process group IDs.
  #### Resource Usage and Limits
  - prlimit() - Gets/sets resource limits.
  #### Random Number Generation
  - rand(), srand() - Generates pseudo-random numbers.
  - random(), srandom(), initstate(), setstate() - Advanced random number functions.
  #### File System Information
  - statfs(), fstatfs() - Gets file system statistics.

  ### Linux-Specific System Calls
  - clone() - Creates a new process with shared parts.
  - unshare() - Disassociates parts of the process execution context.
  - setns() - Joins an existing namespace.
  - eventfd(), eventfd_read(), eventfd_write() - Provides event notification via file descriptors.
  - signalfd() - Creates a file descriptor for accepting signals.
  - fanotify_init(), fanotify_mark() - Monitors filesystem events.

  ### Namespace and Control Groups (cgroups)
  #### Namespace Operations
  - unshare() - Starts a new namespace.
  - setns() - Enters a namespace.
  #### Control Groups Interfaces
  - Interactions via filesystem API (e.g., reading/writing to /sys/fs/cgroup/) - Controls resource allocation and usage limits for process groups.
